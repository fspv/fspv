# How I sped up my python project 150 times

Sorry for the clickbaity title. It is not like I found some silver bullet to speed up all of the python projects. This article is a short story on how I changed the architecture of my app, so it runs 30 seconds instead of 1 hour 20 minutes.

## About the app
The app is my pet project that generates Anki flashcards for all the leetcode problems. Check out the repo if you want to learn more about it [https://github.com/prius/leetcode-anki](https://github.com/prius/leetcode-anki).

The way it worked before, it fetched all the leetcode problems' details one by one to later generate Anki flashcards using this info. It took 1 hour and 20 minutes to fetch all the data from Leetcode API using the initial version of the code.

The logic looked like this:
```python
# List of problem slugs by topic
# Example:
# {"algorithms": ["two-sum", "tree-sum"]}
problem_slugs: Dict[str, List[str]] = {}

for topic in for topic in ["algorithms", "database", "shell", "concurrency"]:
    problem_slugs[topic] = fetch_problems_for_topic(topic)

# List of detailed problem data along with its topic
# Example:
# [
#   ("algorithms", ProblemData(description="Two Sum description", title="Two Sum")),
#   ("algorithms", ProblemData(description="Three Sum description", title="Three Sum")),
# ]
problems: List[Tuple[str, ProblemData]] = []

for topic, problem_slugs in problems.items():
    for problem_slug in problem_slugs:
        problem_data = fetch_problem_data(problem_slug)  # Slow
        problems.append((topic, problem_data))

deck = AnkiDeck()

for topic, problem in problems:
    deck.add_note(generate_anki_note(topic, problem))

deck.write_to_file()
```

The issue here is that for each leetcode problem, we fetch its data separately.

Until recently, I thought this was the only way to fetch the data about the problem because this is what leetcode itself does when it loads a problem page. And downloading 2000+ problems one by one is slow. So that's why it took so long for the generator to run.

## Solution
While messing around with the browser network debugger on different leetcode pages, I noticed a GraphQL method `problemsetQuestionList` to generate the page with the list of all the problems [https://leetcode.com/problemset/all/](https://leetcode.com/problemset/all/). I didn't pay attention to that one before because I thought it returned too little information to generate the flashcard (only title, slug, status, frequency, and a couple of other fields). But what if I request more fields from this method? I gave it a go, and, to my surprise, this method returned everything I asked it for. 

So I could issue the following request, which returns all the problems (paginated by limit and skip parameters)
```graphql
query problemsetQuestionList($categorySlug: String, $limit: Int, $skip: Int, $filters: QuestionListFilterInput) {
  problemsetQuestionList: questionList(
    categorySlug: $categorySlug
    limit: $limit
    skip: $skip
    filters: $filters
  ) {
    total: totalNum
    questions: data {
        questionId
        questionFrontendId
        boundTopicId
        title
        titleSlug
        categoryTitle
        frequency
        freqBar
        content
        translatedTitle
        isPaidOnly
        difficulty
        likes
        dislikes
        isLiked
        isFavor
        similarQuestions
        contributors {
          username
          profileUrl
          avatarUrl
          __typename
        }
        langToValidPlayground
        topicTags {
          name
          slug
          translatedName
          __typename
        }
        companyTagStats
        codeSnippets {
          lang
          langSlug
          code
          __typename
        }
        stats
        acRate
        codeDefinition
        hints
        solution {
          id
          canSeeDetail
          __typename
        }
        hasSolution
        hasVideoSolution
        status
        sampleTestCase
        enableRunCode
        metaData
        translatedContent
        judgerAvailable
        judgeType
        mysqlSchemas
        enableTestMode
        envInfo
        __typename
    }
  }
}
```

So I issued requests in pages of size 100 and got all the problems in like 20 requests. I couldn't increase page size because, otherwise, the server could not generate the data within the time limit and returned the 503 error. But anyway, this change alone brought the generation time down to 4 minutes, which is already 20x faster than the initial code. Already a huge win, which I wanted to celebrate (until another idea came to my mind).

But before we go further, here is the pseudocode illustrating the new version of the app.

```python
problem_count = fetch_problem_count()
page_size = 100

problems: List[Tuple[str, ProblemData]] = []

for page in range(math.ceil(problem_count / page_size)):
    data = fetch_problems_page(skip=(page * page_size), limit=page_size)

    for problem in data:
        problems.append((problem.topic, problem))

deck = AnkiDeck()

for topic, problem in problems:
    deck.add_note(generate_anki_note(topic, problem))

deck.write_to_file()
```

## Make it even faster
Looking at the GraphQL query above, you might notice it queries a lot of fields. And most of those are not used to generate a flashcard. So what if I remove unused fields and make a page size larger?

Check out the revised query:

```graphql
query problemsetQuestionList($categorySlug: String, $limit: Int, $skip: Int, $filters: QuestionListFilterInput) {
  problemsetQuestionList: questionList(
    categorySlug: $categorySlug
    limit: $limit
    skip: $skip
    filters: $filters
  ) {
    questions: data {
        questionFrontendId
        title
        titleSlug
        categoryTitle
        freqBar
        content
        isPaidOnly
        difficulty
        likes
        dislikes
        topicTags {
          name
          slug
        }
        stats
        hints
    }
  }
}
```

With this new query, I could finally increase the page size. Actually, at some point, I realized I could even fetch all the problems in one go if I didn't fetch unnecessary data. And when I do that, the runtime of the script is 30 seconds. I couldn't believe my eyes. I thought there were no ways to speed it up even 2 times. And I reached 160x speedup with just a couple of simple changes.

## What would I do differently
When I came down this way, I realized I wasted several days of work by not doing a deeper investigation prior to coding. If I had spent some time at the beginning digging into leetcode requests, I would have saved a lot of time. So lesson learned, and it was fun going down this way anyway.
