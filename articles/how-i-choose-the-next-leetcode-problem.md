# How I choose the next leetcode problem to solve

Do you just pick an arbitrary problem to solve or have some systematic approach to that?

I generally do it both ways.

* I pick the tasks at random, using an Anki deck (described in [this article](https://medium.com/@pv.safronov/anki-studying-3-14e8f8f2f4)) and also [here](https://www.reddit.com/r/leetcode/comments/pzfh2z/leetcode_anki_cards_generator_python_library/)
* I pick the tasks with the lowest solved/total ratio

The latter approach I will describe here.

I like solving leetcode problems by topics. It helps you concentrate on a certain type of problems. After solving 5-10 problems on a given topic, you usually:
* Developing your own template for this type of problem which helps you later to code faster and concentrate on the whole solution rather than the details
* Start noticing a pattern. Later, when you see other problems, you might notice that you start recognizing those patterns behind the descriptions of the problems. If you jump from one topic to another, it is much harder to learn those patterns

But unfortunately, on leetcode, there is no straightforward way to say how much progress you have made on the particular topic.

I use my own [leetcode client](https://pypi.org/project/python-leetcode/) for that.

To generate such statistics, you have to follow a few simple steps.

First set up a virtualenv
```bash
virtualenv -p python3 leetcode
. leetcode/bin/activate
pip3 install python-leetcode
```

Then in python shell (or file) initialize the client
```python
import leetcode

# Get the next two values from your browser cookies
csrf_token = "xxx"
leetcode_session = "yyy"

configuration = leetcode.Configuration()

configuration.api_key["x-csrftoken"] = csrf_token
configuration.api_key["csrftoken"] = csrf_token
configuration.api_key["LEETCODE_SESSION"] = leetcode_session
configuration.api_key["Referer"] = "https://leetcode.com"
configuration.debug = False

api_instance = leetcode.DefaultApi(leetcode.ApiClient(configuration))
```

Now once the client is initialized, you can start performing actual queries. Let's try to write some code to calculate the necessary statistics.

First of all, we have to acquire a list of all the problems we solved.

```python
api_response = api_instance.api_problems_topic_get(topic="algorithms")

slug_to_solved_status = {
    pair.stat.question__title_slug: True if pair.status == "ac" else False
    for pair in api_response.stat_status_pairs
}
```

Now for each problem, we want to get its tags.

```python
import time

from collections import Counter


topic_to_accepted = Counter()
topic_to_total = Counter()


# Take only the first 10 for test purposes
for slug in list(slug_to_solved_status.keys())[:10]:
    time.sleep(1)  # Leetcode has a rate limiter
    
    graphql_request = leetcode.GraphqlQuery(
        query="""
            query getQuestionDetail($titleSlug: String!) {
              question(titleSlug: $titleSlug) {
                topicTags {
                  name
                  slug
                }
              }
            }
        """,
        variables=leetcode.GraphqlQueryVariables(title_slug=slug),
        operation_name="getQuestionDetail",
    )

    api_response = api_instance.graphql_post(body=graphql_request)
    
    for topic in (tag.slug for tag in api_response.data.question.topic_tags):
        topic_to_accepted[topic] += int(slug_to_solved_status[slug])
        topic_to_total[topic] += 1

print(
    list(
        sorted(
            ((topic, accepted / topic_to_total[topic]) for topic, accepted in topic_to_accepted.items()),
            key=lambda x: x[1]
        )
    )
)
```

The output will look like this:

```python
[('memoization', 0.0),
 ('number-theory', 0.0),
 ('binary-search-tree', 0.0),
 ('quickselect', 0.0),
 ('recursion', 0.0),
 ('suffix-array', 0.0),
 ('topological-sort', 0.0),
 ('shortest-path', 0.0),
 ('trie', 0.0),
 ('geometry', 0.0),
 ('brainteaser', 0.0),
 ('combinatorics', 0.0),
 ('line-sweep', 0.0),
 
 ...
 
 ('union-find', 0.3076923076923077),
 ('linked-list', 0.3333333333333333),
 ('string-matching', 0.3333333333333333),
 ('segment-tree', 0.4),
 ('data-stream', 0.5),
 ('strongly-connected-component', 0.5),
 ('minimum-spanning-tree', 0.6666666666666666),
 ('merge-sort', 1.0),
 ('doubly-linked-list', 1.0)]
```

So it is clearly visible which topics we should focus on in our preparation.
In this case, the memoization topic is one of the targets for improvement, so I can go to [https://leetcode.com/tag/memoization/ ](https://leetcode.com/tag/memoization/) and choose a new memoization problem. Or use python to automate the process.

Well, and this is how I do that. You can use this API for many purposes, and what I just showed is just one of the simplest things you can achieve. I hope this article finds its reader, as I'm pretty sure there many people out there who want to automate their leetcode grinding workflow as well.
