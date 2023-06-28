# Story: In case of boredom, hack Wi-Fi

\[TODO\]

# Six easy steps to becoming a digital pirate

If you'd rather read a synopsis, or a TL;DR format of this guide:

- Buy the ESP8266 and associated embedded hardware
- Solder them and assemble the system on a breadboard
- Flash the *ESP8266 Deauther* firmware to the ESP8266
    - Optional: set up a test Wi-Fi network
- Hack the planet
    - Optional: modify or improve the firmware code

Depending on your experience with topics in this guide, these bullet points either make no sense (what is an ESP8266???) or are literally what you do every day. They can also be anywhere in between. **It is okay to be confused.** Hardware and hacking are both complex topics and people have written loads of books on them. If you have absolute no idea what is going on in those steps, this guide is actually perfect for you. If you already have knowledge on the subject matters, feel free to skip ahead when you feel like it. Throughout the guide, you will notice shortcuts and "escape hatches" that let you follow the guide at your own pace.

This first chapter focuses on the ~~elephant in the room~~ the chip on the circuit board: embedded software, hardware, and the all-mighty ESP8266.

# The ESP8266 is a microcontroller

Look around and you may notice a lot of electronics and appliances. Some of them are fairly simple, like lamps, light switches, and fans. Some of them are "smarter":

- Home assistants like Alexa or Google Home listen to your voice commands, give you answers from the internet and control smart appliances in your home.
- An RGB light strip[^0] receives commands from a remote or an app to change the color, intensity and blink pattern of its lights.
- An air purifier knows to automatically change its indicator light color and adjust the fan speed, based on air quality.

Surprisingly, even the dumbest-looking electronics can have some sort of logic in them. For example:

- Many electric toothbrushes buzz the user every thirty seconds of usage and automatically shuts off after two minutes.
- A credit card with a microchip needs to be able to encrypt, decrypt data and communicate wirelessly with a payment terminal to make secure transactions happen.
- [Certain COVID-19 test kits](https://hackaday.com/2021/10/17/electronic-covid-test-tear-down-shows-frustrating-example-of-1-time-use-waste/) can even use optical electronics to interpret a test result and Bluetooth it to your phone!
    - These tests may be more reliable and easy to use, but one may also argue they are an excessive waste of severely limited microchip resources during the COVID-19 pandemic.

Well, copper wires and batteries can’t think by themselves, so what’s there to implement this “smartness”? The answer is usually a type of microchip called **microcontroller unit**, or **MCU**. Like in examples above, if a device operates using logical rules or need to transfer data, chances are it contains a MCU that runs a program to make the device do its job. Note that not all electronics need a MCU at its heart, though it holds true for a lot of them around us.

A microcontroller is a tiny computer. Many microcontrollers come with memory space, known as flash memory or "**flash**[^1]," which makes it possible to *store program and data*. Flash to a microcontroller is like the storage to a laptop or smartphone. To *run the program* stored in its flash, a MCU has a processor core, or **CPU**. This is like the Intel or AMD processor in your PC, or the Apple Silicon in a MacBook. A lot of MCUs also come with some temporary **RAM** storage, *needed by the code to run*. And finally, a MCU has parts that let it *control and communicate with* other electronic components outside itself. These parts are generally called **peripherals**. Microcontrollers are great for digitally control things like toys, home appliances, factory equipment, and all the way to fighter jets and rockets.

The **ESP8266** is precisely a microcontroller. Let's take a look at the checklist:

- [x] It has a **CPU** that runs up to 160 MHz (megahertz) fast
- [x] Up to 50 KB (kilobytes) of **RAM**
- [ ] No internal **flash**
    - Not a problem though. This MCU lets you connect an *external flash* so you can have as much flash as you need.
- [x] Plenty of **peripherals** like GPIO, SPI, I2C, I2S, UART, ADC...
    - These are ways to control or communicate with other electronics, common in the world of electronics engineering and computer science.

Though there's one more peripheral I didn't mention. The ESP8266 has a killer feature that not a lot of other MCUs have - it can create **Wi-Fi** networks and communicate on them. This Wi-Fi peripheral is exactly why many smart home gadgets like smart plugs and light bulbs use the ESP8266: you can have them join your home Wi-Fi network and easily control them through a phone app! We can program an ESP8266 to send or receive pretty much any Wi-Fi data, and this is what makes it perfect for testing wireless network security.

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