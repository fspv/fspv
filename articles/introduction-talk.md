# Introduction talk
I usually give an introduction talk during each interview, which is the same all the time. So I thought, why can't I publish it? It will be nice to make it visible to everybody who wants to know more about my career.

## Early years
I've started coding at university. Back then, I studied physics at [Moscow State University](https://en.wikipedia.org/wiki/Moscow_State_University) but quickly realized coding was more interesting to me. So this led me to find my first couple of engineering jobs and eventually led me into Yandex.

## Yandex
I spent six years at Yandex. That was a fun time. I worked for Yandex's Payments service, which was responsible for handling every payment in the company.

To give you a bit more context (for those who're not from ex-USSR), I'll explain what Yandex is.

Yandex in Russia is effectively replacing all the western services:
* Google (Search, Maps, Gmail, Calendar, etc.)
* Uber
* Spotify
* Amazon (somewhat)
* Netflix
* IMDB
and many more.

So you can imagine payments are on the critical path for the most of the services. When payments service breaks, people can't take a taxi, buy cinema tickets, etc.

I've started there as a junior [SRE](https://en.wikipedia.org/wiki/Site_reliability_engineering) and ended up leading a team there. The team was responsible for the entire payments infrastructure.

Examples of significant projects I did there:
* PCI DSS environment for securely storing user's credit card data. There is a set of requirements from Visa and Mastercard telling you how to store such data securely. It is not that easy to implement those requirements. It took about two years to build the system. We made it in collaboration with the application development team, security team, network operations team, data center team, and external auditors team. By the information I possess at the moment of writing this article, my team and I managed to build the most secure PCI DSS environment in Russia (and probably in the world). Here is a presentation by my colleague (in Russian) about how we made this https://www.youtube.com/watch?v=9ZsH6Ze8wno
* Automation of all of the operations and processes. When I just came to Yandex, it was a relatively small company (about 3k people). And over the time of my employment, it grew significantly. Both in terms of people and in terms of the size of the services it runs. So the old operations-based approach didn't work anymore. So I led an initiative to automate everything possible. We automated (and I did a significant part of it myself):

    * Server deployment and configuration, which removed engineer from those processes completely and reduced human error possibility and toil significantly

    * Development process (VCS, CI, code reviews, style guidelines, tests, etc.), which reduced toil as well

    * Failover of databases, which led to less downtime and toil

    * Onboarding of new services, which helped us to make it faster and with fewer errors and manual operations

    * Monitorings (alerts) creation process, which became unmanageable at some point, and we had tens of thousands of them

    * Application deployment process (it took 80% of on-call engineer time before and 1% after)

    And much more. I think I can now write a book on how to approach a transition from manual operations to automation.
* I also supported a small team of three people as a manager for the last four years of my employment. I was more like a tech lead manager. Along with being a people manager responsible for motivation, growth, performance, I was also a technical leader who helped the team make the right technical decisions and deliver a high-quality product in time. Finally, my automation efforts became recognized in neighbor teams as well. So I started a cross-team effort to integrate services we developed internally into those neighbor teams.

## Facebook
I spent the last two years at Facebook. I have been working there on the world's largest (to the best of my knowledge) MySQL installation. This MySQL was the one where entire Facebook stores its data. The most significant parts of it are UDB (user database, storing the social graph) and XDB (a DBaaS for Facebook).

The MySQL team is huge. There are a lot of outstanding people working there. Because of the size, it is split into several sub-teams. The sub-team I have been working at is called Fleet Management. It is a very subtle name, so I'll give you some examples of what the team does (it is not an NDA at all, you can find all the same info in these talks by my colleagues https://youtu.be/NfS5ZLNPxS4 https://www.youtube.com/watch?v=_DebqFNWJ-k):
* Balancing: we have to ensure the data is spread as evenly as possible across the fleet. We have several layers of balancing, which calculate the best moves that satisfy all the constraints we have.
* Instance state machine. It is described here https://engineering.fb.com/2013/10/22/core-data/under-the-hood-mysql-pool-scanner-mps/. Fleet management is responsible for most of it.
* Copies, OLMs - services that move data from one place to another (for the purposes of balancing or maintenance, for example).
* OS provisioning/upgrades (https://www.youtube.com/watch?v=EajAjFCZz4Q), maintenance handling, and much more

Here I usually give examples of the projects I did in FB. But I'm hesitant about posting it publicly, as it provides more details about the systems, which are a bit harder to find on the Internet (though still possible). So I'll leave it empty. I just will mention that those are super challenging and interesting projects. At that scale, every problem becomes much more challenging and interesting. If there is a possibility something can go wrong in your code, given the number of servers Facebook has - be sure it will go wrong.

# Final notes
That is what I usually tell during the introduction talk during the interview. Sometimes there are follow-up questions. There was even an interview when we discussed my FB experience for the whole hour and didn't even get to real technical questions (and the feedback for this interview was positive, to my surprise). But on average, I try to keep it short to preserver more time for the actual interview.
