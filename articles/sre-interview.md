# SRE interview preparation resources

Have you ever wondered why there are so few materials on the web describing the process of preparation for an SRE interview?
I had. From what I see, there are still quite a few articles available, and they mostly describe the interview process but not how to prepare for that.

I see 2 main reasons for that:
1. There are not so many SRE out there in comparison to SWE
2. The interview process is not standardized, so by preparing for Google, you might not end up prepared for Facebook

As for the first point, even SRE often choose to go via the SWE interview path because it is easier to prepare for the coding interview (you "just" grind leetcode). Also, there is no way to avoid having real-world experience with what's being discussed during the interview for the SRE role. Some interview types as "troubleshooting" are just impossible to pass without having hands-on debugging experience.

To elaborate on the second point, here is the interview process I got in 3 different companies (excluding behavioral rounds):

**Google**
1. Linux internals
2. Troubleshooting
3. Non-abstract large scale system design (NALSD)
4. Practical coding (== no leetcode)

**Facebook**
1. Linux internals
2. Networks
3. System design
5. Coding (leetcode easy/medium)

**Citadel**

* Just casual talks about everything (Linux internals, network, troubleshooting, system design) over 4 rounds + practical coding mixed in.

So you might notice, even between close competitors (FB and G), only one round matches. Everything else is different.

Given all that, I actually would prefer an SWE interview today, as I know algorithms quite well. But things are not that bad for the SRE interview either. There are still things that are common among all the companies, like fundamental things that are asked almost everywhere.

So now, I'll try to better the situation and dump the primary resources one must use to prepare for an SRE interview.

### Linux internals
Preparation for this interview is strictly necessary, even if you have decades of SRE experience. This interview primarily tests your academic knowledge about the Linux kernel, syscalls, and the behavior of userspace tools. It is impossible to acquire this knowledge via practice unless you're a kernel hacker.

The most important resource for preparing for this interview is a book by Michael Kerrisk, [The Linux programming interface](https://man7.org/tlpi/) (TLPI). The book covers almost every syscall on the system and gives you an insight into how the Linux kernel works. You must read it all. It is a huge book, so dedicate at least a month to read it.

An additional book I'll recommend is [Modern operating systems](https://www.amazon.com/Modern-Operating-Systems-Andrew-Tanenbaum/dp/013359162X) by Andrew Tannenbaum. That is a classic book. Most topics there are covered in TLPI  as well. So I'd recommend skimming over this book after TLPI but not reading it completely.

### Networking
TLPI, together with "Modern operating systems", covers most of the networking you need. But if you want to go deeper, then [TCP/IP Illustrated, Vol. 1: The Protocols](https://www.amazon.com/TCP-Illustrated-Vol-Addison-Wesley-Professional/dp/0201633469) is something you're looking for. It is recommended by Facebook recruiters as well.

### Troubleshooting
There is simple no way to excel in troubleshooting other than through experience. So make sure you do a lot of debugging at work. Usually, this comes naturally in case you work on more "sysadmin"-like position. Otherwise, be sure not to spend most of your time coding. Being oncall is the perfect way to do lots of debugging and troubleshooting.

### System Design
This part is simple, and the preparation is almost the same as for the SWE system design. However, in some cases (such as Google), a system design interview is much harder and requires you not only to build a system but also to do a lot of computations (more detailed than the ones you do for the SWE system design interview).

[This link](https://sre.google/workbook/non-abstract-design/) will make you understand better what's the difference is.

Also, I highly recommend reading this workshop. It gives you an idea of what's ideally expected from you. Though it is impossible to come up with such a detailed description during the interview, so don't be freaked out by the amount of information in this workshop.

Finally, I'd recommend you to learn latency numbers and practice back-of-the-envelope calculations. Here is an Anki deck with all the numbers [link](https://ankiweb.net/shared/info/36912554).

### Coding
Sometimes for SRE, you'll encounter a "practical coding" interview.

Example of question I've seen:
*Given a rsync server log, calculate an avg number of bytes transferred from one IP to another

As you see, there is no algorithm involved here at all. So it is hard to say how to prepare for this type of interview. I'd say the only way to do that is lots of coding practice, and you'll be fine.

### General
If you're applying for Google, reading Google's [SRE book](https://sre.google/books/) is a must. Many questions on the interviews are from this book. But even if you don't apply to Google, the book is useful anyway.
Then, practice is required everywhere, not only for troubleshooting. If you've read the entire TLPI and know each chapter by heart, it won't help you unless you worked with all the subsystems yourself. Your answers just won't sound confident enough.

## Final thoughts
I personally think the SRE interview path is much harder than the SWE interview. There are a lot of preparation resources available out there for SWEs (leetcode, pramp, etc.). But there is nothing like that for SREs. Even if there were, there will be no activity there due to the low popularity of this position in comparison to SWE.

But don't be frightened by this fact. If SRE is something you know better, then go this path. Just be sure to invest time into preparation if you go to FAANG, as it is not like just your hands-on experience will be enough unless you're fortunate.
