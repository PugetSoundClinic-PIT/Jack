
Hi there, and welcome to *Weapon of Mass Disconnection*! My name's Jack. I'm quite glad this guide has piqued your interest and I appreciate you taking time to check it out. You may have read the cover page and learned that *Weapon of Mass Disconnection*, or WMD, is a guide about building electronics gadgets and hacking Wi-Fi networks. We're going to explore the world of fingernail-sized computers called microcontrollers, peek under the hood of Wi-Fi to see why its security might (not) work, then put all learnings together to build a custom Wi-Fi scanner that can not just detect devices... but also block them. If any of these things sound good to you, you're in the right place and I hope you have a good time.

Depending on your experience with electronics engineering or cybersecurity, topics in this guide either make no sense or are literally what you do every day. They can also be anywhere in between. Don't panic though! Hardware and hacking are both complex topics and people have written loads of books on them. I try to make WMD as approachable as possible and talk through things in great detail. If you already have knowledge on the subject matters, feel free to skip ahead when you feel like it. Throughout the guide, you will notice shortcuts and "escape hatches" that let you follow the guide at your own pace.

The first chapter is just around the corner. It focuses on the ~~elephant in the room~~ the chip on the circuit board: embedded software, hardware, and the all-mighty ESP8266.

# It's hacking o'clock

English is wonderful; the word "hacking" means a lot of things. It commonly means digital trespassing: gaining unauthorized access to a computer, taking secret information, breaking or slowing down networks and such. It can also mean physically cutting something, like in "hacking at a tree with an ax." Some use the word to describe creative or non-standard ways to do something, like taking shortcuts when writing computer code or in "life hacking." There's also [that one meme](https://knowyourmeme.com/memes/hackerman). The kind of hacking we're talking about here is:

- Digital: we'll be analyzing and breaking Wi-Fi networks, not tree branches
	- *Until the [biohackers](https://en.wikipedia.org/wiki/Biohacking) catch on...*
- Ethical: we'll only test disruptive techniques on networks and devices we own
- Educational: while the results are cool, we also want to learn along the way

> The mechanic, who wishes to do his work well, must first sharpen his tools. - Confucius, in the *Analects, Book 15*

Let's get ourselves a sharp "digital ax" for our digital hacking. There are a lot of Wi-Fi security tools, hardware and software alike, that run on PCs and laptops. But computers are bulky and expensive, for they have a lot of power and can do a lot of things. Smartphones are small and quite powerful, but they're still expensive and doesn't offer the same freedom as does a PC for our intrusive creativity. Isn't there a mini computer that doesn't cost an arm and a leg, is small enough to fit on a palm and works great for Wi-Fi security research? There is! It's a *microcontroller* called *ESP8266*. What is it?

# The ESP8266 is a microcontroller

Look around and you may notice a lot of electronics and appliances. Some of them are fairly simple, like lamps, light switches, and fans. Some of them are "smarter":

- Home assistants like Alexa or Google Home listen to your voice commands, give you answers from the internet and control smart appliances in your home.
- An RGB light strip[^0] receives commands from a remote or an app to change the color, intensity and blink pattern of its lights.
- An air purifier knows to automatically change its indicator light color and adjust the fan speed, based on air quality.

Surprisingly, even the dumbest-looking electronics can have some sort of logic in them. For example:

- Many electric toothbrushes buzz the user every thirty seconds of usage and automatically shuts off after two minutes.
- A credit card with a microchip needs to be able to encrypt, decrypt data and communicate wirelessly with a payment terminal to make secure transactions happen.
- [Certain COVID-19 test kits](https://hackaday.com/2021/10/17/electronic-covid-test-tear-down-shows-frustrating-example-of-1-time-use-waste/) can even use optical electronics to interpret a test result and Bluetooth it to your phone!
	- These tests may be easier to use and more reliable, but one may also argue they are an excessive waste of severely limited microchip resources during the COVID-19 pandemic.

Well, copper wires and batteries can’t think by themselves, so what’s there to implement this “smartness”? The answer is usually a type of microchip called **microcontroller unit**, or **MCU**. Like in examples above, if a device operates using logical rules or need to transfer data, chances are it contains a MCU that runs a program to make the device do its job. Note that not all electronics need a MCU at its heart, though it holds true for a lot of them around us.

A microcontroller is a tiny computer. Many microcontrollers come with memory space, known as flash memory or "**flash**[^1]," which makes it possible to *store program and data*. Flash to a microcontroller is like the storage to a laptop or smartphone. To *run the program* stored in its flash, a MCU has a processor core, or **CPU**. This is like the Intel or AMD processor in your PC, or the Apple Silicon in a MacBook. A lot of MCUs also come with some temporary **RAM** storage, *needed by the code to run*. And finally, a MCU has parts that let it *control and communicate with* other electronic components outside itself. These parts are generally called **peripherals**. As in their name, microcontrollers are great for digitally controlling things like toys, home appliances, factory equipment, and all the way to fighter jets and rockets.

The **ESP8266** is precisely a microcontroller. Let's take a look at the checklist:

- [x] It has a **CPU** that runs up to 160 MHz (megahertz) fast
- [x] Up to 50 KB (kilobytes) of **RAM**
- [ ] No internal **flash**
	- Not a problem though. This MCU lets you connect an *external flash* so you can have as much flash as you need.
- [x] Plenty of **peripherals** like GPIO, SPI, UART, ADC...
	- These are ways to control or communicate with other electronics and sensors, common in the world of electronics engineering and computer science.

Though there's one more peripheral I didn't mention. The ESP8266 has a killer feature that not a lot of other MCUs have - it can send and receive **Wi-Fi** data. This Wi-Fi peripheral is exactly why many smart home gadgets like smart plugs and light bulbs use the ESP8266: you can have them join your home Wi-Fi network and easily control them through a phone app! We can program an ESP8266 to send and receive pretty much any Wi-Fi data we want, and this is what makes it perfect for testing wireless network security.

# The ESP8266 is an embedded device

Now we know that microcontrollers run code. After all, a MCU is a tiny computer, and the best way to get a computer to do something is to run code on it. However, writing code for a microcontroller is usually quite different from making software for, say, a laptop or server. For example:

- Microcontroller programmers often stick to programming languages like C and C++.
    - Of all popular programming languages these days, C and C++ are among the ones trickier to use, but well-written C/C++ programs run quickly and save valuable flash and RAM.
    - Microcontroller code doesn't have all the help from an entire operating system running on a PC. Therefore, it needs deeper control of the microcontroller hardware that it runs on to complete tasks. C/C++ makes this control easy to happen.
- Updating the program on a microcontroller can be a lot more difficult than simply copying or downloading a file.
    - On some microcontrollers[^2] it can actually be *that* simple! Not on the ESP8266 though. To program a microcontroller like ESP8266 from your computer, you often need adapters and wires to hook them up. Then you will use a software to copy your program to the chip.
- Computer software have so many ways to take in or put out information, e.g. through input/output (I/O) devices like keyboard, network and screen. Microcontroller programs are more limited in their interaction with the world outside the chip.
    - You can usually find alternatives to computer I/O devices for microcontrollers. Replace a keyboard with a couple of buttons and knobs. Replace the mouse and monitor with a tiny LCD display with touchscreen input! And the ESP8266 can connect to the internet through its Wi-Fi radio already.

If MCUs come with so many restrictions, why not replace them all with general-purpose processors like the ones in iPhones and PCs? While everyday processors are more powerful and versatile, they are also much bigger, power hungry, and expensive. Besides, it's often a lot harder to design a high-performance computer or mobile motherboard than making a circuit board with a microcontroller. Although the ESP8266 is never going to be as good at running games or playing TV shows as your smartphone, it is brilliant as an **embedded device** in places where you, as an embedded developer, want it to do just a couple of things but do them really well. Suppose you are inventing a smart wardrobe mirror that turns on when you walk by and tells you the time and weather. Here's how you can use a microcontroller like the ESP8266 to make all these features possible:

- Design a circuit board that connects an ESP8266 to a touch button, a motion sensor and a LCD display, and provide power to components above.
- Write code for the microcontroller so that it detects button presses and motion sensor activity to turn the display on or off.
- Write more code to let the MCU receive current date, time, and weather over the internet then send these information to the LCD for display.

The actual development process for an embedded system like this smart mirror will be longer, with perhaps more engineers and time spent coding and testing. What doesn't change is the fact that embedded systems all perform specific functions at high reliability and performance. These goals can come to an extreme when an embedded system becomes safety critical, like when controlling the brakes on a car or sensors in a X-ray machine. Luckily, our Wi-Fi hacking project that is soon to formally begin doesn't need to be as reliable as engines on a rocket!

# Footnotes

[^0]: This is a flexible strip of lights that can shine in any color. Commonly sold online and used for decorating bedrooms or parties.

[^1]: It's called "flash" because [its inventors think](https://www.eweek.com/storage/1987-toshiba-launches-nand-flash/) the process of erasing the memory is like the flash of a camera.

[^2]: Some microcontrollers like the [RP2040](https://www.raspberrypi.com/products/raspberry-pi-pico/) allow you to directly plug it into a computer and copy code using the operating system's standard file manager. No extra tools required. (Beside, maybe, a notepad or code editor to actually write the code.)