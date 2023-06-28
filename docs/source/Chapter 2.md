
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