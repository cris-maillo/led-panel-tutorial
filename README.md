# LED Panel Tutorial
Tutorial & resources on how to get a RGB LED Screen working using a ESP32-WROOM-32 microcontroller and how to display gifs on it!

> Disclaimer: I might have forgotten some steps as I tried a trillion different things to get this working so please let me know and I will fix any issues with the instructions :)  


## You will need:

- **A microcontroller**: I used a [ESP32-WROOM-32 Type C](https://www.amazon.co.uk/dp/B0BP1QWVMB?th=1)
- **An LED Screen**: I used a [Seengreat RGB LED 64x64 Panel](https://www.amazon.co.uk/SEENGREAT-RGB-Full-Colour-Raspberry-Interfaces/dp/B0BWJD57BS/ref=cm_cr_arp_d_product_top?ie=UTF8). It is a P3.0-64x64x1 with 1/32 scan with a HUB75 Header (I don't exactly know what these terms mean but they are important so that you know what board you have and what libraries or hardware it is compatible with, sometimes the chips make & model can be useful, I believe mine is FM6124C).
The screen I linked comes with Dupont cables to connect to the pins in the microcontroller and a power supply terminal adapter.
- **A power supply for the screen:** I have [this](https://www.amazon.co.uk/Power-Supply-Adapter-Converter-Universal/dp/B099F196YQ/ref=sr_1_6?crid=3HYYWLZFVYHXP&dib=eyJ2IjoiMSJ9.dzjixvuaQP4YoSIpc3QECfcMw8TKAtGt5dwX-RvPCadek3icYV731dmMtLsZ2HbHXdJL88K78GECJmBgLxFqPlaAdEGv-jI63BDFWNTYb6v2MOT9b8E2J5JP5D7nE4-hbJvIs4jmfECEVNUYF1HRpYri_cMtmdjtXVOdGHGda-1TZoa2mUGfd6byvREAzTptrDLfIoV6GP93Wms8uGq5biEPscrBNtgo9FrhlN7VuzfhDgvA-ya3aNDx1_Og_0dCByUnr6NeqIMCZ4d_GONfT376DNzxV3_-n-E9J1p0CJw.mIBt-KriAoijbitFAK-S4Taa9rM5_USN0blD8L8Pjok&dib_tag=se&keywords=5V%2F4A&qid=1728231356&s=computers&sprefix=5v%2F4a%2Ccomputers%2C134&sr=1-6) as my board requires 5V/4A.
- **A way to connect the microcontroller to your laptop:** This was a headache as even though the ESP32 has an USB Type C connector, that cable must be able to do data transfer. I tried multiple USB-C to USB-C Apple cables and this did not work. The only way I got it to recognise the ESP32 was to connect to it via a USB-C to USB-A cable (mine has a blue detail which I think it means it should do data transfer) with an adapter (I am using [this hub](https://www.amazon.co.uk/UGREEN-Revodok-Multiport-Adapter-Aluminum-White/dp/B0CR6J7ZQK/ref=sr_1_4?dib=eyJ2IjoiMSJ9.tkum2xleNE1bfGiT3CFqj9bnH6_hxhau9w83RXtvwhO3_9wBoriE_-uXfugZ-mxFiBJ0fDx4uoDE7WQM9eziPz2K7MmjpoKwhQ4gWJiNn2iy-r6Rta2t5n8ZoCI2mI3o3NCS5sZfnqNHVwx2rMq1QTMrxb7DDWA71riaT8JpE1QK5xGm8-LWg1wdm-iFqbm_k2wVB_IJcEhDUoTgXQ_dtn2XOrbWTbqEYVLpB0WsV8c.iKBv5waDjLIVezTyx1z4Yj3tjElmYPpgOK3Hzb_Z2b4&dib_tag=se&keywords=USB+to+USB+dock+ugreen+white+hdmi&qid=1728231757&s=computers&sr=1-4)). 
    - So my connection looks like: Laptop USB-C port (Macbook Air) connected to UGREEN hub, then one of its USB-A ports connected to an USB-A to USB-C data transfer cable, and that USB-C port connected to the ESP32.



## Instructions:

### Connecting ESP32 to your laptop/Mac
> Instructions adapted from [Instructables](https://www.instructables.com/Getting-Started-With-ESP32-on-a-Mac/)

**1. Download a driver:** You need to install a driver to enable the microcontroller to chat with your OS. Install [this driver](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads).
    Allow the driver if macOS prompts you for permission and you might need to restart your Mac.
    Plug in your ESP32, then open your Terminal and type:
```
ls /dev/tty.*
```
You should see a device like /dev/tty.SLAB_USBtoUART. This means your ESP32 is connected and working!

**2. Download Arduino IDE:** [Download link](https://www.arduino.cc/en/software)

**3. Installing ESP32 Add-on:** In Arduino IDE > Preferences > Additional Board Manager URLs, copy and paste the following URL: https://dl.espressif.com/dl/package_esp32_index.json.

- Go to Tools > Board > Boards Manage.
- Search for ESP32 and press install button for the “ESP32 by Espressif Systems“.

**4. Test the ESP32:** At this stage, I'd recommend testing the set up by running a small program, [blink.ino](https://docs.arduino.cc/built-in-examples/basics/Blink/), which blinks the ESP32's on-board LED. These are the steps:

 - Select your Board in Tools > Board menu (in my case it’s the ESP32-WROOM-DA Module)
 - Select the Port /dev/cu.SLAB_USBtoUART.

![board-config](https://github.com/cris-maillo/led-panel-tutorial/blob/main/assets/ide-board-config.png?raw=true)

 - Write down the following code:
    ```
    #define LED 2

    void setup() {  
        // Set pin mode  
        pinMode(LED,OUTPUT);  
    }  

    void loop() {  
        delay(500); // 500ms  
        digitalWrite(LED,HIGH); // Turn on LED  
        delay(500); // 500ms   
        digitalWrite(LED,LOW); // Turn off LED  
    }
    ```
- Press the Upload button in the Arduino IDE. Wait a few seconds while the code compiles and uploads to your board. You will (hopefully!) see a Blue LED blinking on the ESP32.

### Connecting the ESP32 to the LED screen

**1. Connect the LED screen to power:** Plug in your power supply to a socket and the DC port from the power supply into the adapter. Then, into the adapter, insert the power cable that came with the LED screen: the black power cable into the negative input and then the red cable to the positive input. Finally plug in one of the terminals to the power input terminal labelled +5V on the screen, the other terminal will be left unplugged.
This picture shows the set up:

<p align="center">
    <img src="https://github.com/cris-maillo/led-panel-tutorial/blob/main/assets/power-config.png?raw=true" width="300" />
</p>

**2. Connect the Dupont cables:** Connect the joint head of the Dupont cables to the input terminal labelled IN on the screen, like so:

<p align="center">
    <img src="https://github.com/cris-maillo/led-panel-tutorial/blob/main/assets/dupont-config.png?raw=true" width="300" />
</p>

**3. Connect the pins into the ESP32:** Finally, we need to connect the ends of the Dupont cables to the pins in the ESP32. I used the configuration detailed [here - under the ESP32 section](https://www.waveshare.com/wiki/RGB-Matrix-P2-64x64#Hardware_Connection_4).

<p align="center">
    <img src="https://github.com/cris-maillo/led-panel-tutorial/blob/main/assets/RGB-Matrix-P2-64x64_ESP3209.jpg?raw=true"/>
</p>

Things to keep in mind (this might be obvious to some people but I was clueless before starting):
- There are multiple GND (ground) pins, but you only need one, therefore there is a cable that will remain unplugged (that is normal).
- I would follow the diagram exactly. If you get the same screen that I got, the Dupont cable colours match the colours in the diagram, which makes life so much easier.
- The E pin is only required for 64x64 screens (which mine is), and so the exact pin you attach it to does not matter (I put it on the GPIO32 following the wiki). Whatever pin you attach it to, there is a chance that you will have to explicitly state it in the code you upload to the ESP32 (the examples I link to afterwards will include this info).

It should look like this:
<p align="center">
    <img src="https://github.com/cris-maillo/led-panel-tutorial/blob/main/assets/pins-esp32.jpeg?raw=true" width="300" />
</p>

### Interacting with the screen

**1. Install the required library:** We will be using [ESP32-HUB75-MatrixPanel-DMA](https://github.com/mrcodetastic/ESP32-HUB75-MatrixPanel-DMA) by mrcodetastic as this is suitable for HUB75 screens like ours and ESP32s. If you have any issues with your project, my tip is to look at the closed issues in their repository as someone else might have had the same issue and can provide a solution!

- To install this library, you can search "ESP32 HUB75 LED" in Arduino's Library Manager and install it. 
- You will also have to install the "Adafruit_GFX" library.

<p align="center">
    <img src="https://github.com/cris-maillo/led-panel-tutorial/blob/main/assets/Screenshot%202024-10-06%20at%2019.00.57.png?raw=true" width="300" />
</p>


**2. Run a test sketch:**
Run these two sketches, the first one will help you identify issues with your set up and the second one looks cool.
- [Simple Test Shapes](https://github.com/mrcodetastic/ESP32-HUB75-MatrixPanel-DMA/blob/master/examples/1_SimpleTestShapes/1_SimpleTestShapes.ino): if you have a 64x64 board like mine, remember to configure a valid E_PIN (as we discussed, I opted for GPIO 32). To do so, you can uncomment line 97 ```mxconfig.gpio.e = 18;``` and change the 18 to 32. You will also have to change line 10 ```#define PANEL_RES_Y 32``` to ```#define PANEL_RES_Y 64```
- [Pattern Plasma](https://github.com/mrcodetastic/ESP32-HUB75-MatrixPanel-DMA/blob/master/examples/2_PatternPlasma/2_PatternPlasma.ino): this sketch already is set up for 64x64 screens.



## Uploading gifs to the screen
Instructions adapted from the [ESP32-HUB75-MatrixPanel-DMA](https://github.com/mrcodetastic/ESP32-HUB75-MatrixPanel-DMA/tree/master/examples/AnimatedGIFPanel_LittleFS) library.

**1. Install the GIF library:** We will be using bitbank2's [AnimatedGIF library](https://github.com/bitbank2/AnimatedGIF) as a GIF decoder, install it by searching for AnimatedGIF in Arduino's library manager. 

**2. Install the LittleFS Plugin:** This is a filesystem that will let you access gifs stored inside the flash memory of the ESP32. Use this [tutorial](https://randomnerdtutorials.com/arduino-ide-2-install-esp32-littlefs/) to set it up. 

**3. Choose a gif:** Choose a gif that matches the resolution of the screen (in our case 64x64). To resize a gif, you can use [ezgif.com](https://ezgif.com/). I used a gif of a 3D model of my monsterra that I created on Spline 3D, I've made the gif available under the assets/ folder in case you'd like to use it! As a side note, I had issues with transparent pixels so I would abstain from using transparent gifs.

**4. Upload the gif (or multiple gifs!) to the LittleFS:** To do so, in the data/ folder you created in step 2, upload the gif you want to use, then in Arduino, press [⌘] + [Shift] + [P] on MacOS to open the command palette. Search for "Upload LittleFS to ESP32" and click on it. After a few seconds, you should see the "Completed upload" message. 

**5. Upload the following code to your sketch:** Copy and paste the [code available on the library for animated gifs](https://github.com/mrcodetastic/ESP32-HUB75-MatrixPanel-DMA/blob/master/examples/AnimatedGIFPanel_LittleFS/AnimatedGIFPanel_LittleFS.ino), you'll have to change a few things to enable it for 64x64 screens:
- Change line 30 from ```#define PANEL_RES_Y 32``` to ```#define PANEL_RES_Y 64```
- Uncomment line 222 ```mxconfig.gpio.e = 18;``` and change to ```mxconfig.gpio.e = 32;``` (or whatever E_PIN you used)

I also had to change line 238 ```String gifDir = "/gifs";``` to ```String gifDir = "/";``` just because the structure of my project is simply:
```
project
│   sketch.ino
└───data
│   │   gif_name.gif
```



-------

And that should be it! If you have any problems, use chatgpt. Kiiiiidding, open an issue and I might be able to help out! Good luck and enjoy. Also if you do any cute projects please take pictures and post those too!
Thanks a million to my brother [@tomasmaillo](https://github.com/tomasmaillo) for getting me this kit for my bday and to all of the people creating open source libraries and tutorials for others to use <3.







