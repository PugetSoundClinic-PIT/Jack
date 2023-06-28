
Taking a break from planning our ESP8266 setup, we should take a moment to ponder what makes Wi-Fi vulnerable and what kind of disruption we can cause to insecure wireless networks or devices. Some of these facts aren't going to make sense at first; for example, password-protected Wi-Fi should be immune to attacks since the password will block unauthorized devices from the network... right? Well, not always. Wi-Fi and its underlying technology has been around for more than twenty years now. Compared to the 2000s, although Wi-Fi keeps getting faster and faster, the number of its security issues doesn't steadily drop towards zero. Like many other security topics in computer science, Wi-Fi security is a tough problem to solve with new security holes constantly being discovered and fixed.

> If you know the enemy and know yourself, you need not fear the result of a hundred battles. - Sun Tzu, in *The Art of War*

Wi-Fi is complicated technology, so are its security measures. We're not here to dig through all the technicalities (there are entire books, lessons, and exams around wireless networking!) but to rather discuss fundamental mechanisms that make up a Wi-Fi network. For example:

- How do devices join or leave a Wi-Fi network?
- How do devices (attempt to) securely exchange information over a Wi-Fi network?
    - ...and with this knowledge - How can we (attempt to) break this security?

Protecting a Wi-Fi network from attacker interference is somewhat like locking a bicycle in public. Many factors determine the safety of the bike (lock, chain, bike rack...). Similarly, many algorithms and mechanisms make up the security design of Wi-Fi. If you bike, you will know that depending on your bicycle, lock and location, it can be anywhere from easy to near impossible to make sure your bike is still at where it should be when you return. Just [watch](https://youtu.be/PcV4LVhSRLg?t=95) [these](https://cyclingmagazine.ca/sections/news/thieves-cut-down-tree-to-steal-bike-in-montreal/) [thieves](https://www.newsflare.com/video/82278/crime-accidents/man-caught-cutting-a-tree-down-to-steal-a-bike) chopping down an entire tree... to steal a bicycle. The same goes for Wi-Fi: there's no single magic spell to build a completely bulletproof network or break into every single one you come across.

# Terminology

When I try to explain a tech topic, coming up with proper ways to introduce concepts and acronyms always take up a big chunk of my time. (I hope I didn't lose you already in the last chapter, where we walked through the definition of embedded systems.) There are some terms one just can't avoid when talking about Wi-Fi and wireless networking in general, so let's be all on the same page, shall we?

A **Wi-Fi network** is a bunch of wireless devices talking to each other using the **Wi-Fi** *wireless protocol*. A *protocol* is an agreed upon way for devices (or humans) to exchange information, just like the "English protocol" I'm using right now to convey my thoughts to you on this page. All devices on a Wi-Fi network must send and receive information in, well, the Wi-Fi protocol in order to understand each other. These devices include everything you can find on your home, school, work or public Wi-Fi network, such as the router, phones, laptops, game consoles, sometimes printers, smart doorbells and whatnot. Wi-Fi devices are formerly called **stations**. There are two kinds of stations: **access points (AP)** and **clients**. From the list of Wi-Fi stations above, the router would be an AP and everything else are clients.

It turns out Wi-Fi is not just one protocol - but a whole family of protocols! Recall that Wi-Fi has been around for quite some time and it wouldn't have kept up with our internet demands had we not given the original protocol a couple of updates. The Wi-Fi Alliance and the Institute of Electrical and Electronics Engineers (IEEE) lead the design of a set of technical standards called **IEEE 802.11**. These standards are highly technical documents that describe how exactly a Wi-Fi protocol works and the precise format of data that a Wi-Fi device should send or receive. You can think of Wi-Fi as a "trade name" for the underlying 802.11 technology: the former is certainly a much more catchy name than "eight-oh-two-dot-eleven".

You may also hear Wi-Fi networks being called **WLAN**, or Wireless Local Area Networks. Just like a LAN (Local Area Network) can connect multiple devices with physical ethernet cable, a WLAN can do so wirelessly. Since Wi-Fi or 802.11 are the most popular protocols of WLAN, the terms are often used interchangeably.

# 802.11 Authentication and association

With terms out of the way, let's start by analyzing the simplest form of Wi-Fi from a security perspective - an open, unprotected network. This is often the kind you see at public places like hotels, libraries or airports.

Web developers often get a classic question on job interviews: "what happens when I type google.com in my browser and hit enter?" We can ask ourselves a similar question: "what happens when I tap to join an open Wi-Fi on my phone?"

The answer to both questions is, well, "a lot happens." Your tap just kicked off a flurry of data exchange between your phone and the access point. Among that exchange are two important mechanisms: **authentication** and **association**. These mechanisms happen on every Wi-Fi (re)connect and it would be a real shame if a security issue exists in this fundamental mechanism, right?

## 802.11 authentication

When a client wants join a WLAN, **authentication** is the first thing to happen. It needs to prove to the AP that it is authorized to be part of the network because it has the right password. Isn't it strange that an open Wi-Fi network requires an *authentication* phase? 802.11 has thought of that. When the client sends AP an authentication request, it asks for a particular *authentication algorithm*:

- Open System, represented by a zero-value
- Shared Key, represented by a one-value

You may have guessed that Open System works great for an unprotected WLAN. Once the AP receives the client's request, the AP will check that its network indeed doesn't require a password. If that is the case, the AP will send the client a response, telling it that it has passed authentication! For an open network, this mechanism is like an unlocked door.  When IEEE originally designed the authentication protocol, it was not meant to control access to a network but instead be a quick check to make sure that the client is capable of normally communicating with the AP.

## 802.11 association

**Association** begins right after authentication. During 802.11 authentication, the AP greeted the client but isn't quite ready yet to let the client onto the network. 802.11 gives WLAN stations a lot of freedom to send data in the way they want: clients and APs alike can pick from many data rates, timeout conditions and many other low-level 802.11 parameters! If the client and AP happen to land on incompatible preferences, they're going to have trouble sending data to each other. It's like struggling to understand an accent. Therefore, 802.11 association is a chance for the joining client and the AP to "settle their differences" and pick a set of 802.11 parameters they're both happy with. For an open network, once association completes the AP can finally welcome the client to the network.

## Deauthentication and disassociation

After authentication and association, your phone can begin exchanging data on the open Wi-Fi. What if it needs to leave for some reason? Say, you are asking it to join another Wi-Fi or turning its Wi-Fi off. Sure, the phone can simply stop sending data to the current Wi-Fi AP, and the AP will realize your phone is no longer there after a while, but there's a better way to say goodbye. Enter **de**authentication and **dis**association. They are as simple as sending a message with three important elements:

- Source address (who's sending this message?)
- Destination address (who is this message for?)
- Reason code (why does the deauthentication/disassociation happen?)

If your phone wants to politely leave a Wi-Fi, it can fill out a deauthentication message like this with itself as source, AP as destination, and a reason code that basically means "I have left this network." Similarly, an AP can also deauthenticate its clients at any time. Here are a couple of possible reasons:

- The client hasn't sent anything in a while (are you still there?)
- Insufficient resources (I'm too busy)

An AP deauthenticates in the same way a client would: fill out a *deauth* message with itself as source, a client as destination, and a reason code. An interesting case is when an AP is shutting down: since it is the host of a Wi-Fi, it needs to tell all of its clients to either disconnect or move to some other AP. Sure, the AP can send many deauth messages with each client taking turns as the destination, but there is a magical destination called "**broadcast**" that means *everybody*. A broadcast deauth works the same way you think it will: all clients on a Wi-Fi listens to broadcast messages and will disconnect themselves when receiving a broadcast deauth from the AP.

## A wild attack appears!

Authentication, association, disassociation and deauthentication sound like great ways to manage clients joining and leaving a WLAN. However, chaos ensues when **anyone with appropriate Wi-Fi hardware can deauthenticate any station on any WLAN.** It's as easy as 1-2-3:

- Listen for Wi-Fi radio traffic around you to figure out addresses of the AP and client(s) you want to attack
- Fill out a deauth message (also called a deauth *frame*) with desired source and destination addresses, and any reason code ("unspecified" is good enough)
- Send the frame into the air!

A client receiving a deauth frame wouldn't know if it came from the genuine AP or an attacker, since the attacker can simply copy the AP's source address. Likewise, an AP receiving a deauth frame can't tell if it's from an actual client because the source address is really an address of a valid client. The result? Either the client leaves a Wi-Fi mistakenly thinking the real AP told it to get off, or the AP drops a client mistakenly thinking the client decided to go. This attack is called the **deauthentication attack**. In cybersecurity taxonomy, Wi-Fi deauth attack is a type of **denial-of-service attack** because in this case, it denies the client from using the Wi-Fi "service".

Maybe this is where we wish we'd had physical wires: on physical ethernet we at least know we'll always be talking to that one box on the other end of the wire. (Unless someone swaps out that box, that is.) On a wireless system, how can we really be sure that a message in thin air came from the same machine we've been talking to?

## What this means for us

The [ESP8266 Deauther](https://github.com/SpacehuhnTech/esp8266_deauther) project you downloaded in the last chapter is a firmware that takes advantage of this management frame vulnerability and allows you to launch deauthentication attacks from an ESP8266. The deauth attack is a very approachable cybersecurity topic because a great majority of Wi-Fi networks as of the time this guide was written (2023) can still fall for this attack. It is also quite inexpensive to demonstrate; all it takes are some $50 of electronics and some buttons and wires.

Are there ways to defend a WLAN network against this attack? Sure there are! Although I think home Wi-Fi networks are unlikely to become targets of such attacks besides pranks, if you ever decide to upgrade your home networking to make it deauth-proof, you can also use your freshly-built ESP8266 hacking rig to test if your new defense works. It's another great way to make use of the gadget you are building/will build and to see first hand how security vulnerabilities get discovered, tested, and mitigated (fixed).

# Fixing authentication and association

If authentication and association messages are so easy to fake, why not encrypt them with a password that only the AP and client know? Isn't encrypting traffic the whole point of Wi-Fi passwords?

It turns out the deauth attack is effective even on password-protected networks. This is because by design, (de)auth and (dis)assoc frames *cannot* be encrypted. These frames are also known as **management frames**, and they need to remain open so all clients of a WLAN - those who already joined, are joining, or would like to join - can understand them. A password protected Wi-Fi network instead encrypts **data frames**, which are messages that carry network traffic, such as your calls, streams, and gaming sessions. This is why on a secure Wi-Fi network, even though an attacker might not know what you're doing on the internet, they can still launch a deauth attack to kick you off the network.

The vulnerability of 802.11 management frames is by design, but the design can be fixed. In the IEEE 802.11w standard, IEEE made amendments to 802.11 and allowed WLAN stations to transmit **protected management frames**. After a client has connected to a WLAN, this feature allows the client and AP to protect management frames (broadcast ones included too!) with cryptographic keys. With protected management frames, an attacker is going to have a much harder time mounting deauth attacks if they don't know the Wi-Fi password.

## It takes time

Fixing security issues in Wi-Fi and its 802.11 standards is often not as easy as changing lines of code in an app. If possible, Wi-Fi equipment companies can release updates for their routers and access points to fix security issues. The catch is that a typical home router is yet another embedded system. From the last chapter, you know that embedded systems can be hard to update once they've left the engineer's desk and the factory into the hands of customers! Not every Wi-Fi router knows how to update themselves, and not everyone who buys a router feels the need to install updates ("Why update if it works?"). For the deauthentication attack though, the most realistic fix for average consumers is to buy a new router that supports Wi-Fi 6, standardized by IEEE as 802.11ax, that makes protected management frames a mandatory feature. The difficulty - and sometimes impossibility - of easily patching security holes in Wi-Fi equipment is why many Wi-Fi networks today still use outdated, insecure protocols, making them open to attacks of varying severity.