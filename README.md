# LED Panel Tutorial
Tutorial & resources on how to get a RGB LED eLED Screen working using a ESP32-WROOM-32 microcontroller.

> Disclaimer: I might have forgotten some steps as I tried a trillion different things to get this working so please let me know and I will fix any issues with the instructions :)  

## You will need:

- **A microcontroller**: I used a [ESP32-WROOM-32 Type C](https://www.amazon.co.uk/dp/B0BP1QWVMB?th=1)
- **An LED Screen**: I used a [Seengreat RGB LED 64x64 Panel](https://www.amazon.co.uk/SEENGREAT-RGB-Full-Colour-Raspberry-Interfaces/dp/B0BWJD57BS/ref=cm_cr_arp_d_product_top?ie=UTF8). It is a P3.0-64x64x1 with 1/32 scan with a HUB75 Header (I don't exactly know what these terms mean but they are important so that you know what board you have and what libraries or hardware it is compatible with, sometimes the chips make & model can be useful, I believe mine is FM6124C).
The one I linked comes with Dupont cables to connect it to the pins in the microcontroller and a power supply terminal adapter.
- **A power supply for the screen:** I have [this](https://www.amazon.co.uk/Power-Supply-Adapter-Converter-Universal/dp/B099F196YQ/ref=sr_1_6?crid=3HYYWLZFVYHXP&dib=eyJ2IjoiMSJ9.dzjixvuaQP4YoSIpc3QECfcMw8TKAtGt5dwX-RvPCadek3icYV731dmMtLsZ2HbHXdJL88K78GECJmBgLxFqPlaAdEGv-jI63BDFWNTYb6v2MOT9b8E2J5JP5D7nE4-hbJvIs4jmfECEVNUYF1HRpYri_cMtmdjtXVOdGHGda-1TZoa2mUGfd6byvREAzTptrDLfIoV6GP93Wms8uGq5biEPscrBNtgo9FrhlN7VuzfhDgvA-ya3aNDx1_Og_0dCByUnr6NeqIMCZ4d_GONfT376DNzxV3_-n-E9J1p0CJw.mIBt-KriAoijbitFAK-S4Taa9rM5_USN0blD8L8Pjok&dib_tag=se&keywords=5V%2F4A&qid=1728231356&s=computers&sprefix=5v%2F4a%2Ccomputers%2C134&sr=1-6) as my board requires 5V/4A.
- **A way to connect the microcontroller to your laptop:** This was a headache as even though the ESP32 has an USB Type C connector, that cable must be able to do data transfer. I tried multiple USB-C to USB-C Apple cables and this did not work. The only way I got it to recognise the ESP32 was to connect to it via a USB-C to USB-A cable (mine has a blue detail which I think it means it should do data transfer) with an adapter (I am using [this hub](https://www.amazon.co.uk/UGREEN-Revodok-Multiport-Adapter-Aluminum-White/dp/B0CR6J7ZQK/ref=sr_1_4?dib=eyJ2IjoiMSJ9.tkum2xleNE1bfGiT3CFqj9bnH6_hxhau9w83RXtvwhO3_9wBoriE_-uXfugZ-mxFiBJ0fDx4uoDE7WQM9eziPz2K7MmjpoKwhQ4gWJiNn2iy-r6Rta2t5n8ZoCI2mI3o3NCS5sZfnqNHVwx2rMq1QTMrxb7DDWA71riaT8JpE1QK5xGm8-LWg1wdm-iFqbm_k2wVB_IJcEhDUoTgXQ_dtn2XOrbWTbqEYVLpB0WsV8c.iKBv5waDjLIVezTyx1z4Yj3tjElmYPpgOK3Hzb_Z2b4&dib_tag=se&keywords=USB+to+USB+dock+ugreen+white+hdmi&qid=1728231757&s=computers&sr=1-4). 
    - So my connection looks like: Laptop USB-C port (Macbook Air) connected to UGREEN hub, then one of its USB-A ports connected to an USB-A to USB-C data transfer cable, and that USB-C port connected to the ESP32.


## Instructions:

### Connecting ESP32 to your laptop/Mac
Instructions adapted from [Instructables](https://www.instructables.com/Getting-Started-With-ESP32-on-a-Mac/)

**1. Download a driver:** You need to install a driver to enable the microcontroller to chat with your OS [Driver Link](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads)
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
 - Select your Board in Tools > Board menu (in my case it’s the ESP32-WROOM-DA Module) PICTURE!
 - Select the Port /dev/cu.SLAB_USBtoUART.
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
- Press the Upload button in the Arduino IDE. Wait a few seconds while the code compiles and uploads to your board. You will see a Blue LED blinking on the ESP32.

### Connecting the ESP32 to the LED screen

