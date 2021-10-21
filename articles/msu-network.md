# Moscow state university network built by students

![image](https://user-images.githubusercontent.com/1616237/138260822-7771ecba-68a9-4921-b497-d0f78ae55a83.png)

A long time ago, the Internet connection wasn’t a thing you would expect to be available in every place you find yourself by default. Internet connection was a privilege available to the luckiest people. It was like that in Russia until the late 2000s, when the situation started to change.
Back then, I was studying at one of the top Russian Universities: Moscow State University (MSU). It is famous not only for its high level of education but also for a huge main building which you see above. People outside the University don’t know, but the whole building is a giant students’ dormitory (except for the middle part with the spire). Even though it is the most prominent University in Russia, its administration didn’t provide an internet connection to its students. What a shame. But there were the most intelligent people in the country studying there, so they’ve found the way out. They had built a network on their own, which provided students Internet connection from 2002 till 2013 (when the administration effectively legalized this network).

I was lucky to have a chance to work as a support engineer for this network. That was my first job, and I had a lot of fun maintaining and building the network. And now I’m going to share with you some details about it.

# The network topology

The administration of the University considered illegal the network built by students, which put lots of restrictions on the available resources.

All the network equipment had to be located in the students’ dormitory rooms.

There was no way to place network cables inside the building.
No help from those in charge (security, floor governors, etc.). Actually, we always expected quite the opposite; they were fighting this network as hard as they could (without providing any alternative at all).
In the beginning, there were only 10 Mbps hubs available. They were extremely slow because the packet arriving at one port was mirrored to all other ports, flooding the network with unnecessary traffic. Later they were replaced by L2 switches, that are smart enough to choose the destination port for the packet.
Having two competing networks was a cherry on the top of it. They were cutting each other’s cables, executing DDoS attacks, stealing equipment, etc.

The top-level topology looked like this (the scheme of MSU main building from above):

![image](https://user-images.githubusercontent.com/1616237/138260865-8188507c-d13e-4d18-9301-ef0bb1c77765.png)

The Core router was sitting in one of the Ph.D. students’ laboratories. That was the 19th floor of sector A (the central sector of the building). To connect this router to the other parts of the network, we laid fiber cables through secret passages in the walls, ceilings, floors, and ventilation shafts. Sometimes even I was surprised how well those cables were hidden. For example, there was a network switch in the ventilation shaft, and in order to reach it, you had to get into the hole in the wall and climb a couple of meters. There you’ll see it. But where does this switch get power to work? There is no socket nearby. Well, the socket is two floors above, and there is a wire going from that socket to the switch. Check the pictures below if you don’t believe me.

![image](https://user-images.githubusercontent.com/1616237/138260928-fd81757e-8da5-461b-9d3d-baeb842890e8.png)
![image](https://user-images.githubusercontent.com/1616237/138260959-c7dc34d9-b045-42d6-a78f-6907999cfe42.png)

The whole network was a mess. Network equipment was randomly distributed across the main building of the MSU. We connected most of the clients via cables going through their windows. Well, not really windows. In the dormitory, below each window, there was a narrow ventilation hole. You could use the handle to open or close it. Here is how it looked like.

This is a “single” room for Ph.D. and last year students:

![image](https://user-images.githubusercontent.com/1616237/138260985-f4e27fb0-a67e-4be9-aa37-f20a062bf3ab.png)

And if you come closer to the window, you’ll notice this:

![image](https://user-images.githubusercontent.com/1616237/138261002-d557d5a2-6083-42bc-921e-d118028ce075.png)

(see the handle above the radiator and cables going into the hole?)

Some rooms in the dormitory were multi-purpose. Apart from being the place to live, they were also playing the role of machine rooms. Close to the hole under the window mentioned above, there was a network switch installed. Usually, that were 24-port managed switches (D-Link DES-3526 or EdgeCore ES3526XA). Those switches were connected to each other via separate gigabit ports available. The 100 Mbit ports were used to connect the clients. So 26 cables were going through this ventilation hole. Students living in those rooms were privileged ones. They were provided with a good discount on the internet connection. And the network support prioritized fix of their issues.

Check out some images of such “switch rooms”, as we called them:

![image](https://user-images.githubusercontent.com/1616237/138261027-73ed3ee1-074f-482e-996a-9ce570065a52.png)
![image](https://user-images.githubusercontent.com/1616237/138261043-dfeb6833-93ff-41c7-9b10-8b8605982b79.png)
![image](https://user-images.githubusercontent.com/1616237/138261050-82ed1e41-fcc9-4cad-a56b-77091b7701ed.png)
![image](https://user-images.githubusercontent.com/1616237/138261061-9ef17394-bfc9-485d-9578-c9667394a725.png)
![image](https://user-images.githubusercontent.com/1616237/138261067-414e352e-aca6-4b62-ae8f-0b55a7a3100c.png)
![image](https://user-images.githubusercontent.com/1616237/138261084-434824df-a162-4929-8343-b54b81471b5d.png)

And this is how it looked from outside:

![image](https://user-images.githubusercontent.com/1616237/138261108-95eea964-673f-4462-8b4c-cb5b555d59d7.png)
![image](https://user-images.githubusercontent.com/1616237/138261121-21d7a74c-21f7-4f90-a083-15c9da393391.png)
![image](https://user-images.githubusercontent.com/1616237/138261129-4c7a0845-633a-4777-9c28-ab25fc3af3ce.png)
![image](https://user-images.githubusercontent.com/1616237/138261145-2377351b-3198-407d-b8a4-b48ccab61dae.png)

I even found one of such room’s owners posing behind the cables:

![image](https://user-images.githubusercontent.com/1616237/138261155-0b0fb02f-c2d9-44e6-8bb8-1131c2ff7d35.png)

This is a regular view from the MSU dormitory room window:

![image](https://user-images.githubusercontent.com/1616237/138261171-b18eecd6-80f4-43e8-9064-f304b73e9fd7.png)

Most of the time, we managed to reduce the number of cables coming out of the window down to ~15. We could do this because of a simple trick: the regular ethernet cable (Cat 5e) has 8 wires. And to transmit 100 Mbps, you need just 4 of them. All 8 wires are required only for a 1 Gbps connection. So using a single cable, you can connect 2 clients who live close to each other. We always tried to do that if possible because the hole can’t fit so many cables.

Sometimes the room didn’t have a ventilation hole, or it was painted/rusty/locked, so we can’t have access to it. Then we had to come up with other ways:

![image](https://user-images.githubusercontent.com/1616237/138261195-e7b48e83-688f-49bc-8116-ee6a5c7b8a10.png)

Some rooms were not equipped with provider-grade switches and had just consumer-grade 5/8-port switches. There was no way to tell if that switch was up or down, which client connected to which port, etc. They were a real pain in the ass for the network engineers.

![image](https://user-images.githubusercontent.com/1616237/138261217-d91da572-18cd-40e8-993b-c0213cd7a9da.png)

On the outside, the whole network looked just awful. The net of Cat 5e cable covered one of the most beautiful buildings in Russia.

Here is the topology of the network as of 2012. Sectors B and V are missing because they had already had an “official” network by that moment.

![image](https://user-images.githubusercontent.com/1616237/138261239-d4bb900b-0a3b-4e9b-baf4-630cf529956c.png)

## Building the network

The client connection procedure was not the easiest one as well. If a student didn’t have a cable in their room already, you had to make a new one. So you go to the closest “switch room” and hope there is somebody there and they’ll let you in. If nobody was living there anymore, you had to go to the administration and try to convince them to give you the keys. Often they didn’t let you in. That’s when you had to resort to the last option: using lockpicks. Yes, you heard it correctly. Network engineers had lockpicks and were trained to use them.

![image](https://user-images.githubusercontent.com/1616237/138261275-b815a985-a58c-47a4-a951-9e6e758a9d83.png)

After you finally had access to the switch, you had to estimate how much cable you needed from this switch to the student’s room. That wasn’t an easy task as well, and some math skills were involved in cutting the right length. But after a few weeks of work and a few mistakes, you are rarely wrong. Also, you have to be sure the cable is less than 100 meters long. 100 meters is the limit for the ethernet, after which the signal weakens so much, the receiving side can’t decode it anymore.

![image](https://user-images.githubusercontent.com/1616237/138261371-ea105ff5-9bae-4556-9756-33f9b08bba67.png)

In the end, you had a cable hanging from the hole in the window. A connector is attached to one side of the cable and connected to the switch. I hope I didn’t forget to tie the cable to the radiator so it doesn’t slip outside. Now you have to get this cable to the destination. You thought it would be an easy task? Of course not. If it is not too high, you could throw another cable out of the destination window, tie them together downstairs and pull it to the destination room. If it is like the 17th floor, this trick won’t work. You had to work in couple with somebody. You attach something heavy to a free-hanging end of the cable. Then one engineer sways the cable while another one is catching it downstairs. That seemed dangerous at the beginning. But later you are getting used to it and not scared at all.

We tried to add many cables at once and leave them hanging, so we don’t have to bother “switch room” inhabitants for every new client we connect.

![image](https://user-images.githubusercontent.com/1616237/138261407-78038cbd-ec3c-435e-a8ca-3a50ca92991f.png)

Sometimes we needed to get to the roof to pass the cable from one side of the building to another. Getting on the roof was illegal, of course, and required a bit of climbing walls. But who cared? Delivering an internet connection to our friends and other students was much more important.

Check out some more pictures of the network built this way.

![image](https://user-images.githubusercontent.com/1616237/138261434-afd1b693-d1fc-4a1b-a1d5-c70794bb7153.png)
![image](https://user-images.githubusercontent.com/1616237/138261440-32135d43-35aa-4ad0-8f6f-492154aab036.png)
![image](https://user-images.githubusercontent.com/1616237/138261456-0d78a511-66b2-4c0a-b703-8b94a1b14d2c.png)
![image](https://user-images.githubusercontent.com/1616237/138261461-6e2f9109-b5b7-4924-984c-fb690dc8168c.png)
![image](https://user-images.githubusercontent.com/1616237/138261506-aacc2e36-d888-4529-a8b2-e72d07edf984.png)
![image](https://user-images.githubusercontent.com/1616237/138261516-9b4ffd51-548c-4251-8e61-ed9c6cf191a4.png)

## Issues

Every day we were firefighting and fixing the network. All sorts of issues were happening there. It is not a regular network, where all cables are carefully hidden, users have no access to the network equipment, and there is full access to the equipment by the engineers.

* Users were powering off the switches because they needed to connect their electric teapot somewhere. The administration was switching off the equipment and preventing engineers from getting access to it. Because “this network is illegal” and “Why do you need this internet anyway? I don’t have it, and I’m fine”.
* Network loops were a common issue (there was no visibility over the network topology, and sometimes either engineers or users were connecting something that shouldn’t have been connected).
Cables were wearing off, resulting in a poor connection and hard to debug issues (the port went from 100 Mbps to 10 Mbps, or MAC-address transmitted with errors, resulting in flooding a switch L2 routing table with tens of thousands of incorrect MAC addresses).
* Since it was an L2 network, there were cases when people were trying to spoof the network by running their own DHCP server. For the same reason, it was hard to locate those people. *Some rooms were impossible to connect because there was no switch available within the 100 meters radius.

## Wars

Back in 2002, they were like 5 or 6 separate small networks. There was a constant war between those. Later the Great Unification happened, leaving just 2 big networks: SNTO (where I was working) and Hackers. The war between those continued.
A lot was happening during this competition, and that was not a fair competition. They cut each other’s cables, hacked each other, stole equipment. As far as I am aware, one of those conflicts resulted in a criminal court case. I don’t know the details, unfortunately.

I saw with my own eyes 2 acts of aggression between those networks:
* My server installed in the ventilation shaft had been stolen. I had friends in the security, so we managed to locate them via CCTV and return the server.
* There was an operation to cut our competitors’ upstream channels executed at night. I didn’t take part, but those who were there told me they wore a uniform to disguise themselves as road workers, opened manholes, climbed down, and cut the cables. Where did the management get the information about which cables to cut — I have no idea.
In 2013 the MSU’s administration finally decided to build an “official” network. That put an end to this war. The “official” network was just a legalized SNTO network. Effectively they allowed SNTO to build a proper network in MSU, using their own money. Why couldn’t they do it 10 years earlier? Who knows. But at least now, the students of one of the top Russian universities have a stable internet connection.

## What did I learn?

This was my very first job. I spent there almost 2 years, and I used every opportunity to learn.

I learned:
* To configure managed network equipment via CLI. This might sound easy, but it is not. It is very different from just working with the Linux terminal. Also, you have to understand the important concepts of networking. For example, concepts of VLAN, routing, L2 discovery, SNMP, and much more.
* To code python. I made my first Django project during my employment there. It was a project that facilitated network monitoring and the work of the support engineer in general. You can find the description in this repo https://github.com/prius/freeports
* To promote my ideas and innovate. For that python project, I had to convince decision-making people we needed the tool I was about to build. That was hard. In the end, we agreed that I build the tool, and they decide they want to pay for that or not. In the end, everybody loved the tool and used it extensively. I still wasn’t paid, though, but that is not because the tool was not good enough. This was another lesson for me. But I was happy anyway because I would have built it even if I knew I wouldn’t be paid. It was about having fun and not the money.
* Some soft skills. There was a need to convince people to give you access somewhere, help you with something, do some crazy things (like climbing roofs), etc.
* A lot about Windows. People constantly offered me side jobs to fix their PCs or laptops. It was an excellent way to make easy money, so I never refused.
* I became a local internet provider as well. I was smuggling the internet to my friends (I’ll tell more in another article). So I learned to set up my servers, work with Linux, configure VPN servers, compile kernel modules, etc.
And finally, I made a lot of good friends there, with whom I have a good relationship till today.

## Engineers always find a way
It is hard to imagine people building such a massive network without any funding while still being students and studying hard. Nevertheless, that’s what happened at the Russian largest University in the 2000s. They were just having fun. That is how true geeks entertain themselves (it is also worth mentioning that the network first appeared in the sectors where Physics, Math and Computer Science students resided).

Also, this teaches us that something you build doesn’t have to be perfect to fulfill the user’s requirements. This network was built with lots of antipatterns, was made of whatever was available at the moment. But still, it worked and delivered a lot of happiness to people back then. This is also my experience working at “Big Tech” companies nowadays (FB, Yandex). Many times during the day, you ask yourself, “how does it even suppose to work”? But it works. And it does its job. We should always strive for the ideal, but we should also be able to stop. And with that, I stop writing this article cause it was already a long read. It doesn’t cover everything that happened during those days, but I’ll cover the rest in separate articles.

Thank you for reading this far, and stay tuned!
