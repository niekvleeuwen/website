---
title: "Building a handheld gaming console based on a Raspberry Pi and RetroPie"
date: 2020-08-21T21:15:04+02:00
categories: ["Hardware"]
tags: ["Raspberry Pi"]
aliases: ["/2020/08/pids/", "/blog/hardware/creating-a-portable-gaming-console-with-a-raspberry-pi"]
---

During the second year of my studies, I followed the FabLab course at [Stadslab Rotterdam](https://stadslabrotterdam.nl/). At the beginning of this course my first idea was to make a housing for my Raspberry Pi. This casing would have to resemble the Nintendo Entertainment System. Soon I found out that this is a fun but simple idea. To create a challenge, I thought it would be a fun idea to make a handheld game console. I would be able to take it with me and play games on the go! After this I started to develop this idea further. I used to spend many hours playing games on my Nintendo DS. What I found ideal about this game console was its compactness, you could take it anywhere with you because of its foldable design. I want to make this with a Raspberry Pi, so I can play all the games I want on the road. Hence the name of my project: PiDS. So to sum it up, I intend to incorporate a Raspberry Pi, a screen and a controller into a product for the purpose of playing games.

### PiDS Supplies

Below you will find the parts needed to make the PiDS.

*   **Raspberry Pi** - [Retropie](https://retropie.org.uk/about/building/) recommends a RPI 3 or better
*   **Screen** - I used [this](https://www.banggood.com/Geekcreit-3_5-inch-TFT-LCD-Touch-Screen-Protective-Case-Heatsink-Touch-Pen-Kit-For-Raspberry-Pi-32Model-p-1269846.html?rmmds=myorder&cur_warehouse=UK) screen.
*   **Controller** - I used [this](https://nl.aliexpress.com/item/32863906297.html) controller.
*   **Powerbank** - Make sure the powerbank is strong enough to power the pi and the screen.

### Start up process

As a first step, I researched a screen for my Raspberry Pi. Since I am a poor student, I quickly went to Banggood. Here I ordered a 3.5 inch touchscreen with a resolution of 320 × 480 pixels for 14 euros. With not too good expectations I had a screen with Priority Direct Mail (1,99€) sent, because I wanted it before my course was already finished. Within 1.5 week the screen was delivered. Installing it is easy, just click it on the Pi. Full of enthusiasm I had already installed RetroPie, but if I had read the description I would have known that I still needed drivers. I downloaded it from the product page and it turned out to be a 2 year old version of raspbian. Luckily RetroPie also has an installer for Raspbian, only it didn't work on the heavily outdated version of Raspbian. Updating to the latest version of Raspbian (a process of 5/6 hours) caused the drivers to stop working and the screen remained white. After some research II found out that the drivers were on Github. After downloading the latest version of Raspbian and installing the drivers on it, I got a picture. Finally I could install RetroPie, at least that's what I thought. Starting the emulationstation just didn't work out. After some research I came up with this guide. It describes how it is possible to get screens working with RetroPie. After finding this guide I reinstalled RetroPie. The steps from the guide were not quite right anymore because the git repository was updated, see my own guide for the updated steps. After these steps the screen was fully functional!

### Step 1 - Prototyping

Make a prototype. I recommend making a prototype first, so you know what you're getting into. My designs can of course be adapted to your own wishes and a prototype can help you determine what you want your own piDS to look like.

### Step 2 - Install the Raspberry Pi and install RetroPie
*   Download [RetroPie](https://retropie.org.uk/) Write RetroPie to an SD card. For detailed instructions, please refer to the [documentation](https://www.raspberrypi.org/help/quick-start-guide/2/). Create a file without an extension called SSH. Put it on the SD card. This will automatically turn on SSH on the first boot.
*   Find the IP address of your Pi by logging into your router, a [IP Scanner](https://www.advanced-ip-scanner.com/nl/) or something similar. You can also connect the Pi to a monitor and keyboard.
* Log in with the username pi and the password raspberry
* Change your password with the command `passwd` 

If you have a different touchscreen than I do, skip the following commands and look in the manual of your screen how to install it.

```bash
git clone https://github.com/swkim01/waveshare-dtoverlays.git

sudo cp waveshare-dtoverlays/waveshare35a.dtbo /boot/overlays/waveshare35a-overlay.dtb

sudo nano /boot/config.txt
```

Add the following line to the end of this file

`dtoverlay=waveshare35a`

If all went well, you will see this after the next command: 

`dev/fb0 /dev/fb1`

```bash
ls /dev/fb*
```

Now continue with the following commands

```bash
sudo apt-get install cmake

git clone https://github.com/tasanakorn/rpi-fbcp

cd rpi-fbcp/

mkdir build

cd build/

cmake ..

sudo install fbcp /usr/local/bin/fbcp

sudo nano /etc/rc.local
```

Add this line **before** exit 0

`/usr/local/bin/fbcp &`

```bash
sudo reboot
```

If all went well, the screen will now work.

### Step 3 - 3D Printing

In this step we are going to print the first parts of the piDS.

{{< figure src="/img/posts/creating-a-portable-gaming-console-with-a-raspberry-pi/3d1.jpg" caption="Model of the PiDS made with Tinkercad" >}}

For the 3D printing I used the Prusa I3. First load this .STL file in the Prusa software. I have included each part separately, so there's room to adjust the placement and you can print the parts separately. I also recommend to print the bar and the lid so that as little padding as possible is required. The lid must be 'upside down' and the bar with the passage of the cables must be vertical. Export the .gcode file to an SD card and put it in the 3D printer. If you don't have a 3D printer available, there is the possibility to have it printed via a service or for example in a Fablab, as in Rotterdam. The result:

{{< figure src="/img/posts/creating-a-portable-gaming-console-with-a-raspberry-pi/3d3.jpg" caption="3D printed housing of the PiDS" >}}

### Step 4 - Laser Cutting

In this step we are going to cut the second part of the house from a piece of wood. I used the [Rayjet](https://www.rayjetlaser.com/) during laser cutting. For this I used 3mm plywood. Below I describe how I used the machine and what settings I used on the machine.

*   Download the file [piDs_housing.ai](niekvanleeuwen.nl/https://niekvanleeuwen.nl/projects/2019/pids/files/piDs_housing.ai) in Adobe Illustrator.
*   (Optional) Adjust the text/images. Make sure the images are in black and white! The lines that are cut are red (255,0,0). Click on Print (CTRL+P). Select the right printer. Go to setting. Engrave is black and Cutting is red.
*   Engrave both settings (Power and speed) to 100 percent and Cutting on speed to 1 percent speed and power to 100 percent.
* Also make sure Air Assist is on for both.
*   Go to summary and check all your settings.
*   Click on the play button and print the print in illustrator.
*   Go to the rayjet software.
*   Select the correct file and select start.

### Step 5 - Assembling

Now you have six loose pieces of wood. You can slide them together and glue them together. Next you slide the bar through the two holes in the 3D printed part. Make sure you have the holes in the bar and housing in the same place!

Use contact glue to glue the piece of wood in the bar. After this you slide the piece of wood through the slot in the wooden part. Glue this with wood glue. If everything went well it looks like the following:

{{< figure src="/img/posts/creating-a-portable-gaming-console-with-a-raspberry-pi/hout4.jpg" caption="3D printed housing combined with the laser cut part" >}}

Now we will place the buttons. First, we take our controller apart. We now see a small PCB. Second, I put the piece of wood upside down. Then I took the buttons out of the old housing and carefully put them in the holes. After this we put the soft part very precisely over the buttons. Then we can put the PCB (also very precisely) over the buttons. Now it is time to tape the PCB to the wood. This was the only way that worked well because in other ways the buttons got stuck due to a lack of space. After all these steps, I strongly recommend testing the controller. At the moment you can easily move the pcb or the buttons a little bit. With a tool like [HTML5 Gamepad Tester](https://www.html5gamepad.com/controllers) it is possible to test each button. If you find out that a button does not work, I recommend repeating all the steps in step 5 until all buttons work correctly.

{{< figure src="/img/posts/creating-a-portable-gaming-console-with-a-raspberry-pi/knoppen8.jpg" caption="" >}}

Assuming that all buttons work now, we're going to start closing our new controller. Pull the cable through the hole at the back. Then you put the powerbank under the controller. Pull this cable through the hole as well. I advise you to use as short a cable as possible because of lack of space. I used the cable below. Finally, you can now close the housing. That looks like this:

{{< figure src="/img/posts/creating-a-portable-gaming-console-with-a-raspberry-pi/knoppen9.jpg" caption="" >}}

### Step 6 - Add the Pi 
In step six we are going to connect the last things. Put the Pi in the 3D printed enclosure. Now connect the USB connector of the cable to the USB port on the Raspberry Pi. Second connect the micro USB cable to the power port. Last, place the screen on the GPIO pins of the Pi. Now the lid can be placed and we are done!

### Done!

We've had the last step, congratulations on the end result if you have managed to follow all the steps! Showcase of my piDS: [Video](https://www.youtube.com/watch?v=VTlReHkWt9E)