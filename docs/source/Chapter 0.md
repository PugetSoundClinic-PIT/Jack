
## Weapon of Mass Disconnection

# Credits

- Puget Sound Clinic, UW iSchool
	- Nic Weber
	- Jack Sui
- Stefan Kremser, Spacehuhn Technologies
	- [ESP8266 Deauther](https://github.com/SpacehuhnTech/esp8266_deauther)

# Preface

The year was 2006. No one in my apartment complex had internet until China's state-owned railway telecom operator *Tietong* installed its networking equipment in a utility shed downstairs in midst of China's rapid tech infrastructure boom ahead of the 2008 Beijing Olympics. And no one in my home had internet until, after a deal was struck between me and my father, I learned to play *The Blue Danube* on piano in second grade. Even though my performance never went beyond the first four bars, my father went above and beyond and got me an entire custom PC setup with a *Tietong* internet contract. Though the plasma display, the skeuomorphic Microsoft Internet Explorer 6 interface, the flickering ADSL modem device, and the shiny, blinking, and loudly whirring *Tietong* boxes in the network cabinet eighty feet below my study room, I stared into the internet.

The internet stared back at me so hard that my mother had to start hiding the mouse, the keyboard, then the power cord[^0] to limit my computer usage. When I traveled back to my grandparents' home during school breaks, I played board games all day with my grandfather to help reorient myself to an analog life. It changed the next summer, when my father brought with him a work laptop, though. I discovered the [Aircrack-ng](https://www.aircrack-ng.org) toolkit for cracking Wi-Fi passwords and began trying to bust into neighbors' networks which had the default NETGEAR, LINKSYS, and DLINK names. In retrospect, most residential Wi-Fi networks I saw in those days had terrible security. I somehow never succeeded in gaining access to a single one, which I blame on weak signals and scarcity of information on those tools on the newborn Mandarin internet.

< TODO >

# You all deserve my many thanks

**Nic Weber**, director of the Puget Sound Clinic for Public Interest Technology and my research supervisor at the University of Washington. You literally willed this guide into existence, and this wish is my command! In the ten weeks that gave birth to this guide, I had so much fun drafting my words, building hardware setups, and talking about electric race cars with you. The other end of my support comes from...

**My parents**, for supporting my education and, when asked me "what do you want to do when you graduate?", enduring my fire hose rambles on "what it means to be a systems programmer and embedded software developer oh and by the way this is what I mean by hardware and software and firmware and operating systems also why they matter for your smartphone and..." Yet these interests of mine all started with...

**Joey Chen**, my high school friend, who asked me to build an Internet-of-Things setup so that his art class final project - an electric lamp - can somehow be interactive and Wi-Fi capable. I ended up getting to know the ESP8266 microcontroller, which will soon become *the* name you're most familiar with throughout this guide. This is the hardware piece of the puzzle that makes up this guide, while the security piece comes from...

**The tutoring company five minutes away from my home** when I was in middle school, which my parents signed me up with because I was horrible at math. Exploring their poorly protected Wi-Fi network and insecure network devices became the way I kill my time on breaks. This company was the spark of my interest in cybersecurity. It later kindled into ethical hacking and computer Capture-the-Flag contest participation. To my knowledge, the company does not tutor computer science or cybersecurity subjects. And finally...

**You** - o reader mine - I hope you enjoy it as much as I did making it.

# First things first

Dear reader, what's in front of your eyes is a guide on computing hardware and Wi-Fi security. While I can't wait to get knee-deep in these exciting topics with you, there are a couple of (boring yet important) reminders that we should all take note of.

- This guide teaches you how to hack Wi-Fi networks.
- Wi-Fi networks are *everywhere*.

It seems right to connect the dots and go, "lovely! I can take what I learn from this and use it to hack my {parent|roommate|coworker}'s broadband to give them a {birthday gift|prank|really bad day}!" Well, let's not do that. Messing with someone's network connection might sound really fun at times, but there are a lot of reasons to why that's an awful idea. To name a few:

- These days, most home and office Wi-Fi routers come with pretty secure settings, straight out-of-the-box. This means they can be quite difficult to hack, if not impossible. (And that's a really good thing for people that rely on these Wi-Fi networks.) In the case of offices, companies pay network administrators to build, protect, and monitor their networks around the clock. Testing your luck on these secure setups can likely be a waste of your time.
- Interrupting someone's internet connection can be a major annoyance. This is especially true if said someone is in a video game match, taking an online test, working remotely or simply enjoying their time on YouTube. We've all been there so let's agree to be nice human beings and not do bad things.
- It goes without saying that hacking - or interfering with - wireless networks without approval is very illegal. This is true no matter if you're in the United States, Germany, China, or anywhere in the world really. People *have* [gone to jail](https://arstechnica.com/tech-policy/2011/07/wifi-hacking-neighbor-from-hell-gets-18-years-in-prison/) for hacking Wi-Fi and that doesn't sound fun. Actually, much less fun than reading and following this guide.

With these out of the way, don't be afraid to test out things in this guide yourself - in an ethical way! In fact, a big part of this guide is about setting up a test network and hacking it yourself. If you follow this guide with a friend - even better! You can try to hack each other's setup.

You may also be reading this guide, or an adapted version of it, in a classroom setting. In this case, your instructor will likely have you covered.

Now that we know what we should and shouldn't do, let's hop in!

# Footnotes

[^0]: What she didn't realize, though, is that the computer uses the same type of power cord as the rice cooker.