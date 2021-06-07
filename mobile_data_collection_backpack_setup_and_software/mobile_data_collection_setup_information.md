# Mobile Data Collection Backpack Setup Information _(Under Construction)_

## Parts List for One Mobile Data Collection Backpack
* [Jetson TX2 Development Kit](https://www.amazon.com/NVIDIA-Jetson-TX2-Development-Kit/dp/B06XPFH939)
* [SD Card](https://www.amazon.com/SanDisk-256GB-Extreme-UHS-I-SDSDXXY-256G-GN4IN/dp/B07H9VX76D/)
* [RealSense D435i Depth Camera](https://www.intelrealsense.com/depth-camera-d435i/) 
* [Logitech USB Webcam](https://www.amazon.com/Logitech-C930e-1080P-Video-Webcam/dp/B00CRJWW2G)
* [Backpack](https://www.amazon.com/Backpack-Breathable-Ventilation-Features-Traveling/dp/B07RQQYRQM/)
* [Battery Pack](https://www.amazon.com/dp/B01337QXMA)
* [USB Hub](https://www.amazon.com/RSHTECH-Splitter-Portable-Aluminum-Individual/dp/B07HMZSRS7)
* [5V USB-to-Barrel-Jack Cable](https://www.amazon.com/CCYC-Barrel-Wireless-Router-Speakers/dp/B079K2DS3H)
* [Chest and Head Mounts](https://www.amazon.com/VVHOOY-Universal-Compatible-Crosstour-Accessories/dp/B07L4T25JY)
* [Camera Screw Adapters](https://www.amazon.com/Sametop-Universal-Conversion-Adapter-Compatible/dp/B06ZYKXYQK)
* Thalmic Labs Myo Band (Discontinued as of 2018)
* Zip Ties 

## Mobile Data Collection Backpack Setup Instructions
* Follow [Jetson TX2 Setup Instructions](#jetson-tx2-setup-instructions). Once all of the software is installed, shut down the system, remove everything but the USB hub, and mount the TX2 against the back of the backpack with zip ties.
* Place the battery pack in the side pocket of the backpack and connect the 12V output to the TX2 and the 5V output to the USB hub.
* Secure the USB hub and any loose cables with zip ties. 

## Jetson TX2 Setup Instructions
* Connect the WiFi antennas and the USB hub to the TX2.
* Connect a monitor to the HDMI port on the TX2. Connect a keyboard and mouse to the TX2.
* Attach the power adapter to the barrel jack on the TX2 and press the power button (first button from the center) to boot up the TX2.
* Follow the on-screen instructions for running the provided OS installer, then reboot.
* Connect the TX2 to a host computer via the micro USB cable.
* Open the Nvidia SDK Manager on the host computer and install Jetpack 4.2.2.
    - Step 1:
        - Set the Hardware configuration to Host Machine and Jetson TX2.
        - Set the target operating system to Linux Jetpack 4.2.2.

    - Step 2:
        - Select Jetson OS and Jetson SDK Components (Additional SDKs are optional but not necessary)

    - Step 3:
        - Follow the instructions for the manual flash process.
            - You will have to put the TX2 in Force Recovery mode [power off, then hit the power button, hold the recovery button (2nd button from center), hit the reset button, and then release the recovery button].
            - The screen will prompt you to fill out some setup options on the TX2 (using the monitor, mouse, and keyboard connected to the TX2).
                - Choose a username, computer name, and password.
                - Choose automatic login.

            - Return to the host PC to complete the SDK install.

* Set up the TX2 to boot from the external SD card.
    - Insert SD card into slot.
    - Follow [these instructions](https://www.jetsonhacks.com/2017/01/26/run-jetson-tx1-sd-card/).

* Other setup
    - Add the username you specified earlier to the dialout group.
        - sudo usermod -a -G dialout [username]

    - Create logs directory (bagfiles will automatically be created there).
        - mkdir ~/logs

_Instructions about software installation to come - please see "Mobile Data Collection Backpack Setup Information" for full instructions._
