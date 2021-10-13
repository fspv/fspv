# Graph traversal python patterns that help you think less and code faster

For some types of algorithmic problems, the amount of code you have to write is quite significant. Sometimes it is even impossible to write a complete solution in time unless you cut some corners. So the habit I developed is to always look for coding patterns in whatever problem I solve. Today I'll show you one of them.

## Matrix traversal problem

Let's consider a problem that involves some kind of graph traversal. We start with a simple one, [Number of islands](https://leetcode.com/problems/number-of-islands/). I assume the solution for this task is a well-known one, so I leave the algorithm itself without an explanation.

After coming up with the algorithm and asking clarifying questions, I usually write the skeleton for the solution straight away. We'll be using a straightforward DFS solution. There is also a UnionFind solution, but it doesn't illustrate the idea that well.

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # Pattern 1
        rows, cols = len(grid), len(grid[0]) if grid else 0

        visited: List[List[bool]] = [[False] * cols for _ in range(rows)]

        def dfs(row: int, col: int) -> None:
            # TODO
            pass

        count = 0

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == "1" and not visited[row][col]:
                    dfs(row, col)
                    count += 1

        return count
```

Here you may see a very high-level solution. The `dfs()` function is not implemented yet. However, we already have something to show to the interviewer. This code illustrates the idea you explained them before starting coding. Nothing too complicated here yet. You've just put what you have said into the code.

And there is already one pattern here. I set `rows` and `cols` variables that I can later reuse throughout the code. It helps a lot to have those variables straight away, so I don't have to think about them later.

The next step will be to start implementing the `dfs()` function. Again we won't be implementing it completely. Instead, we'll go down by 1 level of abstraction.

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # Pattern 1
        rows, cols = len(grid), len(grid[0]) if grid else 0

        def neighbors(row: int, col: int) -> Iterator[Tuple[int, int]]:
            # TODO
            pass

        def dfs(row: int, col: int) -> None:
            visited[row][col] = True

            # Pattern 2
            for neigh_row, neigh_col in neighbors(row, col):
                dfs(neigh_row, neigh_col)

        count = 0

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == "1" and not visited[row][col]:
                    dfs(row, col)
                    count += 1

        return count
```

The next pattern you see is the `neighbors(row, col)` function. This function gives you all neighbors you have for each vertex of your graph. You don't need to think straight away of all the corner cases you might encounter. You just say, "there will be a `neighbors()` function that will handle that, and we'll implement it later". So now you can focus on the application logic instead of struggling with calculating all neighbors' positions and figuring out if they are out of boundaries and if we can use them.

It looks like we almost have a solution. It is left to implement neighbors function. Again `rows` and `cols` variables save us some time.

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        rows, cols = len(grid), len(grid[0]) if grid else 0

        visited: List[List[bool]] = [[False] * cols for _ in range(rows)]

        def neighbors(row: int, col: int) -> Iterator[Tuple[int, int]]:
            # Pattern 3
            for neigh_row, neigh_col in (
                (row + 1, col),
                (row, col + 1),
                (row - 1, col),
                (row, col - 1),
            ):
                if 0 <= neigh_row < rows and 0 <= neigh_col < cols:  # Pattern 3
                    if grid[row][col] == "1":
                        if not visited[neigh_row][neigh_col]:
                            yield neigh_row, neigh_col

        def dfs(row: int, col: int) -> None:
            visited[row][col] = True

            for neigh_row, neigh_col in neighbors(row, col):
                dfs(neigh_row, neigh_col)

        count = 0

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == "1" and not visited[row][col]:
                    dfs(row, col)
                    count += 1

        return count
```

Also, there is another small pattern here. The list of adjacent cells is almost always the same, as well as edge case check `0 <= neigh_row < rows and 0 <= neigh_col < cols`. So most of the time, you can write those lines straight away without giving it too much thinking.

And this is our solution. As you might notice, even at stages 1 and 2, there is already something to show to the interviewer. And `neighbors()` function, once implemented, takes 10 SLOC (out of 24 total). So by not implementing it in place, you save yourself almost 50% of the time. Also, it is pretty nice to show the interviewer you can decompose your code into functions. It will be definitely noted with a "+" sign.

## Actual graph

When you traverse an actual graph, you're usually given edges in some way that is not really usable, and you can't just write your `neighbors()` function using the given input.

I like to define a function called `build_adj_list()`, which (as the name suggests) builds an adjacency list. As you know from the CS course, a graph can be represented using either an adjacency list or an adjacency matrix. Adjacency matrix representation is quite rare, so I'll show only the list one.

I'll be using [Minimum Number of Vertices to Reach All Nodes](https://leetcode.com/problems/minimum-number-of-vertices-to-reach-all-nodes/) as an example. Edges are given here as a source and destination pairs. I once again will abuse the ability to make nested functions in python. In the real world, I'd instead create a class for that. But we're short on time, so whatever works will suit.

```python
class Solution:
    def findSmallestSetOfVertices(self, n: int, edges: List[List[int]]) -> List[int]:
        def build_adj_list() -> List[List[int]]:
            # TODO
            pass

        adj_list = build_adj_list()
        visited = [False] * n
        roots = [False] * n

        def dfs(vertex: int) -> None:
            # TODO
            pass

        for vertex in range(n):
            if not visited[vertex]:
                # Assume the vertex is root
                roots[vertex] = True

                # Visit all the vertices from here 
                dfs(vertex)

        return [vertex for vertex in range(n) if roots[vertex]]
```

I omit the algorithm details again. This article is about structuring your code. You see here that again we have a rough solution already. What's left to implement here is just 2 functions: `build_adj_list()` and `dfs()`. So after verifying your logic is correct so far, you can jump into implementing those functions. I would implement `dfs()` first because it is part of the algorithm. While `build_adj_list()` is just a straightforward function, it is fine if we don't finish it in time.

```python
class Solution:
    def findSmallestSetOfVertices(self, n: int, edges: List[List[int]]) -> List[int]:
        def build_adj_list() -> List[List[int]]:
            # TODO
            pass

        adj_list = build_adj_list()
        visited = [False] * n
        roots = [False] * n

        def dfs(vertex: int) -> None:
            visited[vertex] = True

            for next_vertex in adj_list[vertex]:
                if visited[next_vertex]:
                    # Can reach this vertex from somewhere else => not root
                    roots[next_vertex] = False
                else:
                    dfs(next_vertex)

        for vertex in range(n):
            if not visited[vertex]:
                # Assume the vertex is root
                roots[vertex] = True

                # Visit all the vertices from here 
                dfs(vertex)

        return [vertex for vertex in range(n) if roots[vertex]]
```

And now, finally, `build_adj_list()`, when we have already got the entire solution.

```python
class Solution:
    def findSmallestSetOfVertices(self, n: int, edges: List[List[int]]) -> List[int]:
        def build_adj_list() -> List[List[int]]:
            adj_list: List[List[int]] = [[] for _ in range(n)]

            for src, dst in edges:
                adj_list[src].append(dst)

            return adj_list

        adj_list = build_adj_list()
        visited = [False] * n
        roots = [False] * n

        def dfs(vertex: int) -> None:
            visited[vertex] = True

            for next_vertex in adj_list[vertex]:
                if visited[next_vertex]:
                    # Can reach this vertex from somewhere else => not root
                    roots[next_vertex] = False
                else:
                    dfs(next_vertex)

        for vertex in range(n):
            if not visited[vertex]:
                # Assume the vertex is root
                roots[vertex] = True

                # Visit all the vertices from here 
                dfs(vertex)

        return [vertex for vertex in range(n) if roots[vertex]]
```

This is how you gradually get to the solution layer by layer.


## Final thoughts

Ideally, you shouldn't think at all when you write your code. And this layer-by-layer approach helps you do that. If you go deep straight away when you write your solution, you have to switch back and forth between different layers of abstraction. And when you do it more BFS-like way, you always stay on the same level. And once one layer is finished, you can go ahead and code the next one.

It is always worth to think if you can split your code into functions. This way, you postpone implementing insignificant parts of your code while focusing on what's really important.  It is even better when you develop those ways to split your code long before the interview. On the interview, time is a scarce resource.

This skill is very useful in real-world applications as well. So all of that leetcoding does have some real-world application in the end. Even though sometimes it seems like it doesn't.
