
By now you may have guessed that WMD is not a purely software project. You will need to buy (or borrow from the friend/teacher/lab/maker space that you know) some electronics to make this all work, including a ESP8266 circuit board. So here are all the flour and sugar you will need to make this cake:

# Hardware

*For a concise Bill-of-Materials (BOM) table of all required and recommended hardware, refer to the [table](https://github.com/PugetSoundClinic-PIT/WMDisconnection/blob/main/hardware/WMDHardwareBillOfMaterials.csv) in WMD's GitHub repository.*

## A note on buying parts

You will notice that a lot of parts below link to [Adafruit](https://www.adafruit.com/). Adafruit is a company that produces hardware and content to help people learn electronics. This guide is not affiliated with Adafruit but we recommend it for the quality of its products and great support. However, Adafruit's products can seem a little pricey when you compare them to equivalent offers on other websites like Amazon. Feel free to buy parts you need from other places, though you may need to do more research to get your parts to work.

There are no affiliate links in this guide. We don't make money from your purchases.

## Parts for WMD

### Microcontroller: [Adafruit Feather HUZZAH with ESP8266 - Loose Headers](https://www.adafruit.com/product/2821)

**ESP8266 development board.** Although the ESP8266 chip is at the core of this project, we won't be buying and using the chip directly, as its size (it's smaller than your pinky fingernail) makes it tricky to directly work with. Instead, we will buy a circuit board that packages the ESP8266 chip in an easy-to-use format. The HUZZAH is one that does that! You can directly plug it into a computer to program or talk to it, no special equipment is required. The board also comes with a battery connector that makes it easy to power and use it on the go.

If you look to the right of the Adafruit page you will see three options for how you want the *headers* on this board. Headers are electrical pins that let you connect the ESP8266 to off-board devices like buttons and screens. There are three options here, from top to bottom:

- [Loose Headers - Soldering Required.](https://www.adafruit.com/product/2821) If you are familiar with or want to try soldering, an essential skill for electronics tinkering, pick this. This "some assembly required" version also saves you a few dollars.
- [Assembled Headers - Solder-free.](https://www.adafruit.com/product/3046) This is exactly what your board will look like if you pick the first option and solder the headers yourself. Adafruit solders on the headers for you.
- [Stacking Headers - Solder-free.](https://www.adafruit.com/product/3046) **Strongly recommended!** Standard headers only let you connect things to the board from underneath. Stacking headers provide sockets above the board for you to plug more wires into, should you want to attach more gadgets to your HUZZAH.  The sockets also provide a bit of physical protection for delicate electrical parts on the board.
	- If you want to solder stacking headers yourself: order the Loose Headers version and [stacking headers](https://www.adafruit.com/product/2830) separately. Keep the standard headers for later use.

**Pro tip:** there are many more ESP8266 development boards designed by Adafruit and other companies/people! For example, you can also use a **NodeMCU** development board commonly found on Amazon. Generally speaking, any ESP8266-based board should work, though some may pose a challenge when you try to get the project to run. I recommend that you stick to the HUZZAH - the board that this guide is based on - unless you have previous experience with microcontrollers.

### Battery: [Lithium Ion Polymer Battery - 3.7v 500mAh](https://www.adafruit.com/product/1578)

**Battery for on-the-go, optional.** This battery conveniently plugs into the HUZZAH. If you connect both the battery and USB to the board, the board can even charge the battery for you. Adafruit's website has compatible batteries with a bigger or smaller capacity if you'd like, such as [this one](https://www.adafruit.com/product/258).

**If you'd like to use a battery you already have or buy one from another website, be careful!** Batteries have polarity, that is, the order of the red wire (positive) and black wire (negative) matters. Polarity is why your TV remote won't work when you insert its batteries backwards. The HUZZAH is much more sensitive than a TV remote though, and plugging in a battery with wrong polarity or incompatible voltage can easily destroy it. If you still intend to hook up a non-Adafruit battery, you should know how to verify a proper connection with a multimeter.

If you would rather not work with bare batteries but still want to use the HUZZAH on the go, you can bring a portable charger. Simply use a Micro-USB cable to plug the board into the charger.

**Pro tip:** the HUZZAH has a [power management](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/power-management) page that explains everything about powering the board.

### Breadboard: [Solderless Plug-in Breadboard, 830 Tie Points](https://www.amazon.com/dp/B0040Z4QN8)

**Where your project lives.** This is a standard "breadboard," a handy tool for developing and testing electronic gadgets. You can plug in headers and jumper wires to make electrical connections between components. We will put a screen, some buttons, and maybe some lights on this board and connect them with the HUZZAH to give it input and output.

Breadboards are so commonplace that there are countless companies making and selling them. Adafruit has a similar one [here](https://www.adafruit.com/product/239) but it's out of stock as I write down these words. I have had good experience with BusBoard branded breadboards, which the link above takes you to. Feel free to shop around!

A breadboard lets you swap components and connections easily. If you're done with your design and want to preserve your work permanently, try the [Adafruit Perma-Proto](https://www.adafruit.com/product/1606) which works just like this breadboard but uses soldered connections.

**Pro tip:** breadboards come in many sizes and shapes. You can get a glimpse of them on Adafruit's solderless [Breadboards & Protoboards](https://www.adafruit.com/category/462) category.

### Display: [Monochrome 1.3" 128x64 OLED graphic display - STEMMA QT / Qwiic](https://www.adafruit.com/product/938)

**Graphical display.** We're all used to interacting with a computer through a screen, so why not have one for our project? Once the ESP8266 Wi-Fi hacker is ready to go, the screen will work as a visual menu allowing you to scan nearby wireless devices, trigger attacks and see results.

The screen contains a driver chip, SSD1306, mentioned on its product page. The driver chip sits between the microcontroller (ESP8266) and the actual screen. ESP8266 will tell the driver chip what to show on the screen, and the driver chip will carry out that order by turning on or off the actual pixels.

If you'd like to look for an alternative screen on Amazon, be sure to stick to the same resolution (128x64) and driver chip (SSD1306) so that it can work with minimal effort just like this Adafruit screen. The wiring between your screen and the HUZZAH may be different though. I recently tested [these screens](https://www.amazon.com/dp/B0BFD4X6YV) to work and they even come with pre-soldered headers but again - shop around! The keyword for Amazon is simply "ssd1306 128x64".

**Pro tip:** you're in luck if you chose stacking headers with your HUZZAH, for you get to enjoy the [Adafruit FeatherWing OLED](https://www.adafruit.com/product/4650)! This is a circuit board with a compatible screen *and three buttons* that sits on top of your HUZZAH to give it a clean, cool look. Well, if you don't have stacking headers, you can also connect this FeatherWing separately through jumper wires. Either way, be advised that you will need to solder on loose headers.

### Button: [Colorful Round Tactile Button Switch Assortment - 15 pack](https://www.adafruit.com/product/1009)

**Ah yes, buttons...** the mouse and keyboard of the microcontroller world. Buttons are perhaps the easiest way to give manual commands to a MCU. If you'd like to hunt for cooler looking buttons, feel free! Just be sure that they are breadboard-friendly so you can easily work with them.

**Pro tip:** I don't really have a lot to say about buttons. Just a reminder that you will need three buttons. If you choose to go with the Adafruit FeatherWing OLED above, you don't need any extra buttons!

### Light: [Breadboard-friendly RGB Smart NeoPixel - Pack of 4](https://www.adafruit.com/product/1312)
https://www.adafruit.com/product/1312
**Colorful lights, optional.** These are bright and colorful [LEDs](https://learn.adafruit.com/all-about-leds/overview) that you can control from the ESP8266. They are entirely optional - you can get some and hook them up with your project to give it a bit more personality. Each of these lights have three input pins: two for power, and the third for color and brightness commands. What's nice is you can also chain many of these lights together with the same three pins. These "NeoPixels" often go out of stock, though.

**Pro tip:** these lights are formally known as WS2812. Seems like only Adafruit is selling them in singular, breadboard-friendly format; there are plenty of WS2812-based LED strips and panels on Amazon, but connecting an *entire reel* of LEDs to a project like this might seem a little excessive... both in visuals and power usage.

### Wire: [Breadboard Jumper Wire Kit](https://www.amazon.com/AUSTOR-Lengths-Assorted-Preformed-Breadboard/dp/B07CJYSL2T)

These are simply wires that let you plug one thing into another on a breadboard. No magic here. Adafruit has a smaller kit [here](https://www.adafruit.com/product/3314) that comes with two extra pieces of breadboard. These are *solid* wires that keep shape after you bend them. If you'd like more flexible *stranded* jumper wires, Adafruit has a lot of them [here](https://www.adafruit.com/category/306) and you can also find them on Amazon.

**Pro tip:** you'll notice the colorful, stranded jumper wires all have black, square plastic ends. An end with a protruding metal pin is called a **male** end, good for plugging into a socket or breadboard. An end with a hole is called a **female** end, good for plugging into a pin. If you like these wires, be sure to stock up on male-to-male, male-to-female and female-to-female jumpers so you're ready to connect anything.

### Tools and supplies

When you build and test electronics, you will most likely need access to a soldering iron and a multimeter. A soldering iron lets you melt solder metal to make sturdy electrical connections. A multimeter lets you accurately measure voltage, resistance, continuity, and other important electrical parameters of your project. If you already have these tools, that's great! If not, your school's library or local maker space usually have them. If you would like to buy your own, search online for tool recommendations or refer to the [Adafruit Guide to Excellent Soldering](https://learn.adafruit.com/adafruit-guide-excellent-soldering/tools).

# Software

You need to download some software to your computer to develop with and program your ESP8266 board. Gone are the days of software shipping on DVD in carton boxes, so if you have made your picks on hardware to buy, it's a good time to set up and get yourself familiarized with these software while you wait for the mail.

## [ESP8266 Deauther](https://github.com/SpacehuhnTech/esp8266_deauther/releases)

This is the firmware you will load onto your ESP8266 to attack Wi-Fi networks! We will soon have a proper introduction for this wonderful project. For now, you should download its source code for use later. Follow the header link to the ESP8266 Deauther project's releases page. You will see the newest release at the top of the page with a green "Latest" badge. The release has an "Assets" section. Near the bottom of the section, click on **Source code (zip)** to download a compressed copy of the firmware source code.

The *source code* of a program is computer instructions that you as a programmer write, but you will need to *compile* the source code to a format that a machine can understand and run. The tool that compiles source code is called a *compiler*. In our case, we will use...

## [Arduino IDE](https://www.arduino.cc/en/software)

The Arduino IDE is the tool you will use to read, edit and compile the source code of ESP8266 Deauther. Once the source code is compiled, it also lets you *upload* the result to your ESP8266 to run. The Arduino IDE actually does much more than just compiling your code: it has all the tools you need to make firmware! That's why it is called an **IDE**: Integrated Development Environment.

Clicking on the header link brings you to the download page of Arduino IDE. Depending on your computer, you need to pick a download option:

- **If you have a Windows laptop, PC, or tablet:** select *Win 10 and newer, 64 bits*
- **If you have a Mac with Apple Silicon:** select *Apple Silicon, 11: “Big Sur” or newer, 64 bits*
- **If you have a Mac with Intel processor:** select *Intel, 10.14: “Mojave” or newer, 64 bits*
- **If you have a Linux computer:** you know what to do. Select either *AppImage 64 bits (X86-64)* or, more commonly, *ZIP file 64 bits (X86-64)* as you like.

That's it for now! Once you have placed your hardware orders and downloaded all software you will need, it's time to sit back and relax. As you wait for your packages to arrive, here are some ways to kill time:

- Get to know your Feather HUZZAH ESP8266 development board better by reading [Adafruit's tutorial](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266).
- Following the [Adafruit Guide To Excellent Soldering](https://learn.adafruit.com/adafruit-guide-excellent-soldering), practice soldering on wires or scrap circuit boards and see if you like this kind of handcrafting.
- Read ahead in the [ESP8266 Deauther DIY Tutorial](https://deauther.com/docs/category/diy-tutorial) for a rough idea of what's about to come.
- If you're curious: dive into the [ESP8266 Datasheet](https://www.espressif.com/sites/default/files/documentation/0a-esp8266ex_datasheet_en.pdf) like an electronics engineer, explore the chip's capabilities, and get confused by loads of acronyms.
- **Read on to the next chapter, where we talk about the whats and hows of hacking Wi-Fi!**