# From zero to FB offer and mentoring other people (my way to 750+ problems)

I’ve spent several years solving leetcode and doing all the algorithm-related stuff. It was a long way. Many lessons were learned, a lot of new useful skills acquired, many changes happened in my life (which made me rethink why I’m doing what I’m doing), and many good people met along the way.
So I want to share my experience with you, and the approach which I developed during this journey.

For those looking for TLDR, here are the key points of what’s written below:
1. I’ve spent 3 years doing LC and my life will never be the same again :)
2. How many LC problems should I solve in order to get to FAANG: just enough to be comfortable solving most of the LC medium without too much effort.
3. Consistency is the key
4. Having LC-buddies increases your motivation 10x. The most progress I made in collaboration with other people
5. Mistakes I’ve made: I should’ve cared more about my mental health

My leetcode path
================
4 years ago I realized I’m doing something wrong with my career and I can do better. Back then I was a manager of a small team in Yandex (a Russian tech giant). I was dreaming of working at FAANG eventually. Just because it seemed cool to me and also some of my friends were already there, which made me think “Am I worse than them?”.

So I didn’t know anything about leetcode or algorithms, except for the small course at the university. But I didn’t remember anything from there at that point.
I’ve begun with CLRS. I started to read through every chapter and solve the follow-up tasks.
Here is the directory in my repo with everything I have done https://github.com/prius/learning/tree/master/cormen-introduction
This turned out to be a good starting point, as it gave me some idea of the data structures and set some foundation for my further learning and research. I would recommend everybody to start with the basic algorithm course before jumping into leetcode. You’ll waste a lot of time on simple tasks without this foundation.

A bit later I learned from my colleagues about leetcode. So I’ve started solving LC problems. I’ve spent the first 6 months actively solving tasks every day. I’ve done 180 problems. And actually, it was enough for me to get into FB.

That might have been the end of the story, as the goal has been achieved. Indeed for most people, you don’t really want to do more. You might get unlucky of course and get only hard tasks during your interview, but you can’t prepare for everything anyway.

However, this story is far from the end. Actually, I discovered I really liked solving those tasks and learning more about the computer science field. Also, I kind of wanted to prove to myself that I’m capable of studying something complicated, even though I missed it during my years in university. Finally, I have discovered, this process actually makes me better in other aspects of life as well. I’ve realized that I can apply the same principles I use to do LC to many other activities. For example this year I started working out in the gym the same way I did leetcode, and actually got some great results already. My body looks much better now and I feel better.

Another thing that boosted up my LC progress was COVID and lockdowns. I think it is the same for many people here though. With nothing to do during the lockdown, I’ve started to spend a lot of time reading articles, watching lectures on youtube, and solving LC problems. It was quite easy back then, as it was nothing to do outside anyway.

So by the end of 2020, I’ve reached 700 solved LC tasks. This was a big milestone for me. Actually, I never thought I would manage to do that. And I finally got my LC t-shirt at that point. The most expensive t-shirt in my life, haha.

After reaching that point I didn’t really have enough motivation to continue at the same pace. But I didn’t want to lose my expertise either. At this point, I’ve decided to gather a group to study algorithms together. It turned out to be a very good idea and the group still functioning. During the group's lifetime, all of the participants progressed a lot. Especially big progress is visible for people who were at the beginner level. After 6 months all of them are already capable of solving most of the medium tasks.

This is where I am right now.

Platinum question: how many problems do I have to solve?
========================================================

Obviously, there is no unambiguous answer to that question. It varies greatly and depends on your background. If you have a previous CS degree you probably would just need to brush up on what you already know. So you might not need to do LC at all. However, that’s not the case for most people.
My advice which I usually give to people when they ask is not based on the task count. From my perspective, you’re ready to go to the interview, when you are capable of solving most of the medium LC tasks without much effort and without looking at the solution. This gives you quite a good indicator you’ll be able to do the same during the interview. Of course, there is a risk you’ll get only hard tasks on the interview. But I’d say you have better ways to waste your time than preparing for situations like that.

My approach to grinding
=======================
Actually just grinding won’t help you too much. I’ll share my approach here. It is pretty common among people here, but still might be helpful knowing there are other people around doing the same. Also, it might be helpful for those, who’re just starting.

So first of all *consistency is the key*. It applies to many areas in your life. You don’t do great things in one day. It just doesn’t work like that (except for Hollywood movies). There is a theory that in order to become good at something your brain has to spend 10000 hours doing that. I’m not in a position to judge how much it is true. However, it looks about right. 10k hours means years of work. Also, there are nuances in how your brain builds connections between neurons. I highly recommend the course “Learning how to learn” on Coursera. It gives you a much better perspective on the subject. But anyway in order to develop the algorithmic skill you have to train your brain consistently. Every day. Even if it is just 20 min a day. After a few months of doing that you’ll notice the results, and will be quite happy and proud of yourself. You don’t need to spend 10k hours though. Even 1k hours is already too much for just a simple FAANG interview. If you spend 1k hours and still can’t manage to pass the interview, you’re likely missing something in your approach.

#### How to pick problems?

The general rule here for myself is to always do something which is one level above your current ability. If you’re just starting, then once you realize you can solve easy tasks, switch to another level immediately. Then, when you realize you are kind of comfortable with medium problems, take more and more hard problems. The same applies to the topics you’re learning. I’d recommend going topic by topic (DP, string matching, disjoint sets, etc), but at the very moment, you realize you got the idea and solved a couple of tasks, move to the next topic. To help myself manage that it automated a process. For that, I have written a swagger that generates a client for leetcode API (https://github.com/prius/python-leetcode) and then I generate statistics about the number of solved tasks by topic, which is used to decide what to focus on next.

#### Revisit problems

This might sound like an obvious suggestion, but it wasn’t for me. This is a very important part of the whole learning process because your brain just forgets most of the information if you don’t revise it. As a result, I had to learn many things twice.

And once again I don’t like to do things manually, so I automated the process partially.

Using the same leetcode API client I generate Anki cards for all the leetcode problems (here is the code https://github.com/prius/leetcode-anki). I really like flashcards and they proved to be very effective. I use them for many things now. For learning languages, system design topics, algorithmic theory, OOP, small talk ideas (not a joke). Basically for every new piece of information you acquire you can make a note a put it to Anki to revisit it later. 
![image](https://assets.leetcode.com/users/images/45c5e325-2c2f-47b5-b330-11e6c45a4389_1632501849.3554325.gif)


#### Mock interviews

Again sounds obvious, but again many people ignore that. I know it is bound to a certain level of anxiety. Like you think “my level is not that great yet”, or “my English is not good”. However, once you overcome that it gives you more and more profit. Without mock interviews, your months of grind are worth nothing. During such an interview you train not only your problem solving and coding skills but communication and verification as well. Also, you learn how to code quicker, which is very important for the interviews.

#### Online competitions

Another great source of coding training is online competitions. Like Google CodeJam, Facebook HackerCup, etc.
This makes you see your weak areas in terms of coding speed, etc

#### Track your progress

It is very nice to see your progress through time. I use my personal Github repo for that https://github.com/prius/learning. I really like to see with my own eyes statistics like this one
![image](https://assets.leetcode.com/users/images/04cac772-758b-4b88-aecf-a98373a76ae4_1632492399.3386445.png)

The number of LOC for all my solutions to leetcode problems is at 43k right now (including tests), which is already a pretty impressive number considering it has been done by a single person in after hours.

#### Theory

At some point you’ll find yourself stuck, unable to proceed any further. Often the reason for that is that you are missing some basic knowledge. Of course, you always can come up with most of the algorithms yourself, given you have enough time. But why waste your time, when somebody has already done it for you. Some less-known algorithms (like Tarjan’s algorithm) are not easy to come up with, and there is literally no chance to get the problem without knowing it. So I recommend studying theory in parallel. There are lots of resources for that. I used MIT courses on youtube and CLRS. 

#### Problem-solving skill

Problem-solving is actually a skill and you can actually train it. It might not seem that obvious, but at some point, you realize you actually became better at that. I highly recommend the book I read “How to Solve It, A New Aspect of Mathematical Method” by G. Polya. This describes problem-solving for mathematical tasks and focused more on educating students, but I found most of what’s written there applicable for studying algorithms as well.

#### Grind buddy

What I found extremely useful is to have a close friend doing LC with you. This way you’ll be pushing each other to work harder and you’ll always have somebody to discuss your wins and failures with. And you don’t really have to be on the same level for that. Just somebody motivated enough will do.

#### Learning group

To push grind buddy practice even further, I’ve organized a learning group. With 5 other people, we meet twice a week. When we’ve just started we were discussing a topic a week. Somebody made a presentation and explained it to others. Then when we ran out of topics, we started to discuss hard tasks during these meetings or some particular topics of interest (last time it was system design, for example).
This group actually keeps me going. Even when I think I’m tired and can’t do anything this week, I still have commitments in the group, so I know that I have to do something anyway. This is about gaining momentum in your learning process, so you can’t really stop easily.

#### Motivation

Of course, all of that won’t work without enough motivation. I found that the motivation to get into some tech company doesn’t work very well, because, in the end, you realize all of those companies are similar in some way and it is not really worth it to dedicate a significant amount of your time prepping for getting into some corporation. Financial motivation doesn’t work either, because as an SWE you are already making quite a lot even if you're not in FAANG yet.
The motivation I found most useful for myself grows from the fact that I discovered that I actually like solving those algorithmic tasks. So I want to become better at that. I like mentoring people, I like learning more and more, I like training my brain to do complicated things. In the long term, I see myself mentoring (or even teaching) people and actually doing something nice in this area to help other people (I have some ideas, but I won’t share them yet). If not that, I would’ve stopped somewhere at the point where I solved 180 problems and got into FB.

To sum up: I see the long-term goals for myself in this area, this is what keeps me going.

Mistakes
========

Most of the mistakes I describe here are related to my mental health. I ignored this for so long, but then covid and heavy grinding made all of those problems come to the surface. So I had to learn to deal with them and adjust my life values accordingly.

#### Wrong goals
As I said before, correctly chosen goals are very important. There was a point when I made a goal for myself to get into FAANG. After I got into FB I set another goal, to get into Google (I’ve failed this before). After a year I realized, I spent a lot of time and effort (and a year of my still young life) on something that might not really be worth it. This got me a little bit depressed and led me into some kind of a life crisis. It took me a while to re-adjust my values to return myself back on track both in life and in the learning process.
So I strongly advise you not to make a long-term goal for yourself to get into one of the FAANG companies. It is simply not worth it to spend your life chasing that.

#### Making the price of failure too high
This actually correlated with the previous paragraph. If it is all or nothing, when you fail it’ll hit you really hard. This is what happened to me after the first rejection from Google. I was dreaming about that job, and I had no other options in my head other than working there. So always have a backup plan which is at least not worse. This way no matter what happens your mental health will remain intact.

#### Ignoring WLB

Again, related. I’ve spent 1 year almost without the “life” aspect. Partially because of covid, partially because of grinding. You really have to switch activities. Work and leetcode shouldn’t take all your time (like it did to me). Go to meet your friends often, take some time for your hobby, work out, travel. Yes, you can optimize your life for achieving something, so you do it faster. But in the end, there is a good chance of regretting it later. You can regret that you were missing out on something in your life during that period. This leads to anxiety, so be careful.

#### Ignoring the importance of other people

It is common for engineers to think they can achieve everything in life with just their own effort. But in real life, this is not the case. You’ll be surprised how much you were missing out by doing something on your own. I was. It was an eye-opening experience when I actually start talking to people about their approach to things I’m interested in. It turned out there are a lot of people out there that are smarter than me and possess information that is impossible (or extremely hard) to acquire other than talking to them.

Chats, grind buddies, learning groups, your colleagues, meetups - are just a few tools to talk to people. Use them and your life will become much easier and interesting.

#### Doing not enough mock interviews

Well, this is the last thing here. This one is quite obvious. Mock interviews are very important. Try to overcome your anxiety and actually do 10-20. Then it’ll become easy. As I said before, there are common fears, like “My English is not good enough”, “What if I meet a peer who is much smarter than me?”, etc. But you’ll realize that everybody else out there is exactly the same. So you have nothing really to fear. Also, meditation really helps to ignore those fears, I found meditation really helpful.

## Final notes
My leetcode journey was (and still is) a lot of fun. I hope somebody will find my experience helpful. Just wanted to share it here, because I found posts like that useful for myself in the past as well. It helps to see you're not alone and there are actually people out there doing the same. Also some pieces of advice I found really helpful, cause I missed them myself (even though they're kind of obvious).
So I wish you the best of luck if you've read till here. Keep grinding and improving yourself.
