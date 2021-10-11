# How I built a web service in appengine over the weekeng

10 years ago, when I just started my carreer there was almost no automation available. In order to build a simple service you would almost certainly end up purchasing a VPS, configuring nginx, installing all the necessary packages, thinking about security, installing and configuring databases etc. What I did over this weekend still looks like a magic to me. Spin up a service with caches, queues, balancers, etc it is a matter of few minutes these days.

Today I'll tell you about the service I built over the last weekend. No rocket science here, the service it quite small. But still it serves as a proof, that one can build such a service in just 2 days. This means that with the certain level of motivation and spare time, you can put your ideas into code, and one of the modern cloud services will take care of the rest.

But enough on that for now, let's jump to the implementation details.

## Task
Long time ago I had this idea, that I can optimize my study plan by picking each day the Leetcode problem from the topic I didn't touch before. In this [article](https://medium.com/@pv.safronov/how-i-choose-the-next-leetcode-problem-to-solve-e0d9ae9bc0f) I described a manual approach for that. Now I want to automate things even more, so I wanted to create a web service for that, which everybody can use.

Specs for the service:

1. Have an input form that accepts output from here [https://leetcode.com/api/problems/algorithms/](https://leetcode.com/api/problems/algorithms/).
2. On form submit I should see the list of all the topics with the progress for each one of them.
3. Additionaly the service should suggest me a problem to solve based on my progress.

## Localhost solution

For every problem I like to follow this framework:
1. Make it work.
2. Make it right.
3. Make it fast.

This step is about making it work. For that I've built a flask app locally. On each request this app made a series of requests to leetcode and constructed the statistics I need.

From this stage we were ready to move to the next step, since I know for sure I have all the necessary libraries and those libraries provide all the information I need.

In particular:

1. For each problem I was able to get the list of tags.
2. For each problem I got the information if it is solved or not (provided in the input)
3. List of problems from the input is complete.
4. For each problem I can get its human readable name.


## Scaling the solution

Now to make the solution usable I had to overcome the following issues:

1. Leetcode has a built in rate limiter. So I can do only one request per second to the leetcode API.
2. I can't construct an output fast enough even if leetcode didn't have rate limiter. On each request I have to resolve the information about ~2000 problems, which obviously won't work.

So in order for that code to be able to serve request I had to come up with some sort of database or cache that let's me avoid fan out to the requests to leetcode API on every user request. This is when Google AppEngine comes to play and you really start realizing how cool that is.

It took me some time to figure out how to use Google Cloud. I'd say I've spent good several hours figuring out which services I need and how to actually work with them. But this time investment is definitely worth it. Long time ago I spent months of time learning how balancers, reverse proxies, caches, firewalls, etc work. Now it is just a matter of figuring out which button to press in the UI, or which python library to use.

I started with using Cloud Run. The main problem for me was where to store the cached results for the data from leetcode. Both Postgres database and Memcache service were like $40 per month, which is not worth it for a pet project. Later I realized that if you run an app in the AppEngine it provides you a shared memcache installation for free. This installation provides you no guarantees at all, but again, for pet project that's fine. And the best part is that in order to use memcache you just have to import the python library. That's it. No jokes. Just literally import library and to `memcache.set(key, val)` straight away. Quite impressive IMO.

The final solution comprises of the following components:

1. Default app that serves HTTP requests.
2. Worker app that fetches data from Leetcode.
3. Balancer to route requests between default app and worker app.
4. Shared memcache where worker stores leetcode data.
5. Distributed queue that stores tasks to invalidate the cache.
6. Cron job (provided by app engine) that creates tasks.

And all of that is available to you out of the box. Could you imagine anything like that 10 years ago?

The workflow for the worker is the following:

1. Cron job asks Leetcode API for the information about all known problems.
2. For each problem cron job creates a task in the distributed queue.
3. Distributed queue is configured to feed one task a second to a worker (so we don't go over Leetcode's rate limit).
4. When worker is spinned up with the particular problem, it fetches the necessary information about it from the leetcode API and stores this information in the memcache (or does nothing if this information alerady exists).

TODO: IMG LINK

Workflow for the user-facing part:

1. Read the form input and validate it.
2. For each problem from the input fetch its tags from the memcache.
3. Gather statistics per tag and display this statistics to user (so the least-solved topics will appear first).
4. Pick an arbitrary problem which covers the most least-solved topics and show it to the user.

TODO: IMG LINK

In the end you'll have a result that looks like this

TODO: IMG LINK

## Security considerations
### Validate the input
I had to validate the user input. Because having such a form exposed to public is a great source of all kinds of code injection. So before publishing the service I've made a validator for the form, to ensure all the information I use from there is safe to use.

### Split user-facing page and leetcode crawlers
Another reason to split service to "worker" and "user-facing" part was to prevent external users from accessing the sensitive data and issuing calls directly to my leetcode account. It is not like a lot can be done having my leetcode token, but still.

Of course it is never enough security. But it is always tradeoffs. For this simple service I decided I made enough to feel comfortable about publishing the service to the Internet.

## Make it look nice
Now when it all works and I am almost ready to publish my service, there are a couple of small steps left.

First of all, I don't want for the service to look ugly (as it does with plain HTML). I remember there was a twitter bootstrap, that does the job for you, once imported. But by now it looks a bit outdated to my taste. So I asked a frontend-engineer friend of mine, what is popular these days. He recommended me [Material-UI](https://www.muicss.com/), which is another UI based on Google's guidelines. I'm quite happy with what I've got. One more time this weekend Google made my life much easier.

When it's done, I had to also resolve correctly the titles of the tags and problems. It took me a while to add some code to populate the cache with this data. Now you can see human-readable names and titles instead of just leetcode's internal identifiers.

## TODO
There is still a huge room for optimization here. So far I just made requests to the memcache from the user-facing app asyncronous, since for each user request I have to make like 4000 memcache calls, which produces a lot of IO. Of course this is still sounds crazy even if it is asyncronous. I need to aggregate the results in cache and store them so they can be resolved in batches. Making so many IO requests per one user request for such a simple application is no good.

So "make it fast" phase is still ahead. However, for the current state, when the service has just 1 user (myself) it is not worth it to put so much effort in this service.

## Praise for technical progress and advancement
After my service finished, I had a strong feeling that Google does a lot to facilitate lifes not only for ordinary people, but for developers as well.

This is not a marketing post and not a post praising Google though. Should I choose some other cloud provider, like Amazon or Yandex, I'm pretty sure I would have had a similar experience, beacuse those folks are competing with each other intensively. It is more about how development process was 10 years ago and how it is now. Back then to create a similar service with similar infrastructure I would spend like 2 weeks, I guess. And some things I would never be able to achieve at all (like secure key management, load balancers (cause I need an access to BGP for that), etc). Today it is so simple. Just click a couple of buttons and here you go.

Still I believe that this knowledge I acquired when I had to work on a much lower level of abstraction helps me to be more effective now. I know how all that stuff works under the hood, so I know exactly what I'll get when I press those buttons and how to use it. This saves time. But anyway I'm happy for the young developers who don't need to mess around with all that low level crap and can spend their time actually doing something useful.

And finally, the lesson I learned this weekend is that if you have some idea, you can just sit for a couple of days and implement it. Instead of ruminating this idea for months. It is so easy today. Just import some libraries and write some code. Cloud provider will take care of the rest.
