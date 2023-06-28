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

# Ingredients

By now you may have guessed that WMD is not a purely software project. You will need to buy (or borrow from the friend/teacher/lab/maker space that you know) some electronics to make this all work, including a ESP8266 circuit board. So here are all the flour and sugar you will need to make this cake:

## Hardware

*For a concise Bill-of-Materials (BOM) table of all required and recommended hardware, refer to the [table](https://github.com/PugetSoundClinic-PIT/WMDisconnection/blob/main/hardware/WMDHardwareBillOfMaterials.csv) in WMD's GitHub repository.*

### A note on buying parts

You will notice that a lot of parts below link to [Adafruit](https://www.adafruit.com/). Adafruit is a company that produces hardware and content to help people learn electronics. This guide is not affiliated with Adafruit but we recommend it for the quality of its products and great support. However, Adafruit's products can seem a little pricey when you compare them to equivalent offers on other websites like Amazon. Feel free to buy parts you need from other places, though you may need to do more research to get your parts to work.

### Parts for WMD

#### Microcontroller: [Adafruit Feather HUZZAH with ESP8266 - Loose Headers](https://www.adafruit.com/product/2821)

Although the ESP8266 is at the core of this project, we won't be buying and using the chip directly, as it is tiny (it fits on your pinky fingernail) and tricky to directly work with. Instead, we will buy a circuit board that packages the ESP8266 chip in an easy-to-use format. This HUZZAH board is one that does that! You can directly plug it into a computer to program or talk to it, no special equipment is required. The board also comes with a battery connector that makes it easy to power and use it on the go.

There are three options for how you want the *headers* on this board. Headers are electrical pins that let you connect the ESP8266 to off-board devices like buttons and screens. I recommend purchasing the **Loose Headers - Soldering Required** version so that you can save a few dollars and have some easy soldering fun. If you'd like to minimize soldering, pick **Assembled Headers -  Solder-free**. For maximum flexibility, pick the [Stacking Headers - Solder-free](https://www.adafruit.com/product/3213) version which lets you connect to the board from both top and bottom, or to stack two of the same boards together!

**Recommended quantity:** 2 - the extra for setting up a test Wi-Fi network or as a spare

#### Battery: [Lithium Ion Polymer Battery - 3.7v 500mAh](https://www.adafruit.com/product/1578)

This battery conveniently plugs into the Adafruit Feather HUZZAH board. If you connect both the battery and USB to the board, it can charge the battery for you so you don't need a separate charger. According to the product page, batteries like this one with a *2-pin JST-PH* connector and 3.7 V voltage are all compatible with the HUZZAH board. The battery has a capacity of 500 mAh. Adafruit's website has compatible batteries with a bigger or smaller capacity if you'd like, such as [this one](https://www.adafruit.com/product/258).

**If you'd like to use a battery you already have or buy one from another website, be careful!** Batteries have polarity, that is, the order of the red wire (positive) and black wire (negative) matters. Polarity is why your TV remote won't work when you insert its batteries backwards. Your HUZZAH will be much more delicate than a TV remote though, and plugging in a battery with a reversed polarity or incompatible voltage can easily destroy it. If you still intend to hook up a non-Adafruit battery, I trust that you know how to verify a proper connection with a multimeter.

If you would rather not work with bare batteries but still want to use the HUZZAH board on the go, you can bring a portable charger. Simply use a Micro-USB cable to plug the board into the charger.

**Recommended quantity: 1** - optional

#### Breadboard: [Half-Size Breadboard with Mounting Holes](https://www.adafruit.com/product/4539)

This is a standard "breadboard," a handy tool for developing and testing electronic gadgets. You can insert headers and jumper wires on it to make power and data connections. We will put buttons and lights on this board and connect them with the HUZZAH board to give it input and output.

A breadboard lets you swap components and connections easily. If you're done with your design and want to preserve your work permanently, try the [Adafruit Perma-Proto](https://www.adafruit.com/product/1609) which works just like this breadboard but uses soldered connections.

**Recommended quantity:** 2 - two gives you more space to work with

#### Display: [Monochrome 1.3" 128x64 OLED graphic display - STEMMA QT / Qwiic](https://www.adafruit.com/product/938)

We're all used to interacting with a computer through a screen, so why not have one for our project? Once the ESP8266 Wi-Fi hacker is ready to go, the screen will work as a visual menu allowing you to go through the options, trigger attacks and see results.

The screen contains a driver chip, SSD1306, mentioned on its product page. The driver chip sits between the microcontroller (ESP8266) and the actual screen. ESP8266 will tell the driver chip what to show on the screen, and the driver chip will proceed to turn on or off the actual pixels to change what the screen says.

If a microcontroller wants to control a screen, it needs to know the size of the screen (screen resolution) and how to talk to the screen (driver chip). Therefore, if you'd like to look for a cheaper screen on Amazon, be sure to stick to the same resolution (128x64) and driver chip (SSD1306) so that it can work with minimal effort just like this Adafruit screen. The wiring between your screen and the HUZZAH board may be different though.

**Recommended quantity:** 2 - the extra for the HUZZAH board setting up test Wi-Fi or as a spare

### Button: [Colorful Round Tactile Button Switch Assortment - 15 pack](https://www.adafruit.com/product/1009)

Ah yes, buttons... the mouse and keyboard in the microcontroller world. Buttons are perhaps the easiest way to give input to a MCU, which is exactly what we're going to do. If you'd like to hunt for cooler looking buttons, feel free! Just be sure that they are breadboard-friendly so you can easily work with them.

**Recommended quantity:** 1 - a pack of 15 should be more than enough

#### Light: [Breadboard-friendly RGB Smart NeoPixel - Pack of 4](https://www.adafruit.com/product/1312)

These are bright and colorful lights that you can control from the ESP8266. They are entirely optional - you can get some and hook them up with your project to give it a bit more personality. Each of these lights have three input pins: two for power, and the third for color and brightness commands. What's nice is you can also chain many of these lights together with the same three pins. These "NeoPixels" often go out of stock, though.

**Recommended quantity:** 1 - optional

#### Wire: [Breadboard Jumper Wire Kit](https://www.amazon.com/AUSTOR-Lengths-Assorted-Preformed-Breadboard/dp/B07CJYSL2T)

These are simply wires that let you plug one thing into another on a breadboard. No magic here. Adafruit has a smaller kit [here](https://www.adafruit.com/product/3314) that comes with two extra pieces of breadboard. These are *solid* wires that keep shape after you bend them. If you'd like more flexible *stranded* jumper wires, Adafruit has a lot of them [here](https://www.adafruit.com/category/306) and you can also find them on Amazon.

### Tools and supplies

You need a screwdriver and maybe a drill to assemble furniture. When you build and test electronics, you will need access to a soldering iron and a multimeter at a minimum. A soldering iron lets you melt solder metal to make sturdy electrical connections. A multimeter lets you accurately measure voltage, resistance, continuity, and other electrical parameters. If you already have these tools, that's great! If not, your school's library or local maker space usually have them. If you would like to buy your own, search online for tool recommendations or refer to the [Adafruit Guide to Excellent Soldering](https://learn.adafruit.com/adafruit-guide-excellent-soldering/tools).

## Software

You need to download some software to your computer to develop with and program your ESP8266 board. Gone are the days of software shipping on DVD in carton boxes, so if you have made your picks on hardware to buy, it's a good time to set up and get yourself familiarized with these software while you wait for the mail.

### [ESP8266 Deauther](https://github.com/SpacehuhnTech/esp8266_deauther/releases)

This is the program you will load onto your ESP8266 to run attacks on Wi-Fi networks! We will have a proper introduction for this wonderful firmware project later. For now, you should download its source code for use later. Follow the header link to the ESP8266 Deauther project's releases page. You will see the newest release at the top of the page with a green "Latest" badge. The release has an "Assets" section. Near the bottom of the section, click on **Source code (zip)** to download a compressed copy of the firmware source code.

The *source code* of a program is what you write, but you will need to *compile* the source code to a format that a CPU can understand and run. The tool that compiles source code is called a *compiler*. In our case, we will use...

### [Arduino IDE](https://www.arduino.cc/en/software)

The Arduino IDE is the tool you will use to read, edit and compile ESP8266 Deauther. Once the source code is compiled, it also lets you *upload* the result to your ESP8266 to run. The Arduino IDE lets you do all the things you need to do when developing a firmware project! That's why it is called an **IDE**: Integrated Development Environment.

Clicking on the header link brings you to the download page of Arduino IDE. Depending on your PC configuration, you need to pick a download option:

- **If you have a Windows laptop, PC, or tablet:** select *Win 10 and newer, 64 bits*
- **If you have a Mac with Apple Silicon:** select *Apple Silicon, 11: “Big Sur” or newer, 64 bits*
- **If you have a Mac with Intel processor:** select *Intel, 10.14: “Mojave” or newer, 64 bits*
- **If you have a Linux computer:** you know what to do. Select either *AppImage 64 bits (X86-64)* or, more commonly, *ZIP file 64 bits (X86-64)* as you like.

That's it for now! Once you have placed your hardware orders and downloaded all software you will need, it's time to sit back and relax. As you wait for your packages to arrive, here are some ways to kill time:

- Get to know your Feather HUZZAH ESP8266 development board better by reading [Adafruit's tutorial](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266).
- Following the [Adafruit Guide To Excellent Soldering](https://learn.adafruit.com/adafruit-guide-excellent-soldering), practice soldering on wires or unused circuit boards and see if you like this kind of handcrafting.
- Read ahead in the [ESP8266 Deauther DIY Tutorial](https://deauther.com/docs/category/diy-tutorial) for a glimpse of what's about to come.
- If you're feeling extra adventurous: dive into the [ESP8266 Datasheet](https://www.espressif.com/sites/default/files/documentation/0a-esp8266ex_datasheet_en.pdf) like an electronics engineer, explore the chip's capabilities, and get confused by loads of acronyms.
- **Read on to the next chapter, where we talk about the whats and hows of hacking Wi-Fi!**

# Footnotes

[^0]: This is a flexible strip of lights that can shine in any color. Commonly sold online and used for decorating bedrooms or parties.

[^1]: It's called "flash" because [its inventors think](https://www.eweek.com/storage/1987-toshiba-launches-nand-flash/) the process of erasing the memory is like the flash of a camera.

[^2]: Some microcontrollers like the [RP2040](https://www.raspberrypi.com/products/raspberry-pi-pico/) allow you to directly plug it into a computer and copy code using the operating system's standard file manager. No extra tools required. (Beside, maybe, a notepad or code editor to actually write the code.)