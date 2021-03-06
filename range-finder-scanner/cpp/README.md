﻿# Range finder scanner in C++

This range finder scanner application is part of a series of how-to Intel® Internet of Things (IoT) code sample exercises using the Intel® IoT Developer Kit, Intel® Edison board, cloud platforms, APIs, and other technologies.

From this exercise, developers will learn how to:

- Connect the Intel® Edison board, a computing platform designed for prototyping and producing IoT and wearable computing products.
- Interface with the Intel® Edison board IO and sensor repository using MRAA and UPM from the Intel® IoT Developer Kit, a complete hardware and software solution to help developers explore the IoT and implement innovative projects.
- Run these code samples in the Intel® System Studio IoT Edition (Eclipse\* IDE for C/C++ and Java\* development) for creating applications that interact with sensors and actuators, enabling a quick start for developing software for the Intel® Edison board or the Intel® Galileo board.
- Set up a web application server to view range finder data using a web page served up directly from the Intel® Edison board.

## What It Is

Using an Intel® Edison board, this project lets you create a range finding scanner that:

- continuously checks the Grove* IR Distance Interrupter.
- moves the stepper motor in a 360-degree circle.
- can be accessed via a built-in web interface to view range finder data.

## How It Works

As the stepper motor turns, it pauses to get readings from the Grove* IR Distance Interrupter.

These readings can be viewed on a web page served up directly from the Intel® Edison board.

## Hardware requirements
Grove* Robotics Kit containing:

1. Intel® Edison board with an Arduino* breakout board
2. Grove Base Shield V2
3. [Grove IR Distance Interrupter](http://iotdk.intel.com/docs/master/upm/node/classes/rfr359f.html)
4. [Stepper Motor Controller & Stepper Motor](http://iotdk.intel.com/docs/master/upm/node/classes/uln200xa.html)

## Software requirements

1. [Intel® System Studio IoT Edition (Eclipse* IDE for C/C++ and Java* development)](https://software.intel.com/en-us/eclipse-getting-started-guide)

## How to set up

To begin, clone the **How-To Intel IoT Code Samples** repository onto your computer with Git* as follows:

    $ git clone https://github.com/intel-iot-devkit/how-to-code-samples.git

To download a .zip file, in your web browser go to [https://github.com/intel-iot-devkit/how-to-code-samples](https://github.com/intel-iot-devkit/how-to-code-samples) and click the **Download ZIP** button on the right-hand side. Once the .zip file is downloaded, uncompress it and use the files in the directory for this example.

### Adding the program to Eclipse

In Eclipse, select **Import Wizard** to import an existing project into the workspace as follows:

1. From the main menu, select **File > Import**.<br>
![](./../../images/cpp/cpp-eclipse-menu.png)
2. The **Import Wizard** dialog box opens. Select **General > Existing Project into Workspace** and click **Next**.<br>
![](./../../images/cpp/cpp-eclipse-menu-select-epiw.png)
3. Click **Select root directory** and then the associated **Browse** button to locate the directory that contains the project files.<br>
![](./../../images/cpp/cpp-eclipse-menu-select-rootdir.png)
4. Under **Projects**, select the directory with the project files you'd like to import and click **OK** and then **Finish** to import the files into Eclipse*.<br>
![](./../../images/cpp/cpp-eclipse-menue-epiw-rootdir.png)
5. Your main .cpp program is now displayed in your workspace under the **src** folder.<br>
![](./../../images/cpp/cpp-eclipse-menu-src-loc.png)

### Connecting the Grove* sensors

You need to have a Grove* Base Shield V2 connected to an Arduino\*-compatible breakout board to plug all the Grove devices into the Grove Base Shield V2. Make sure you have the tiny VCC switch on the Grove Base Shield V2 set to **5V**.

You need to power the Intel® Edison board with the external power adapter that comes with your starter kit, or substitute an external **12V 1.5A** power supply. You can also use an external battery, such as a **5V** USB battery.

In addition, you need a breadboard and an extra **5V** power supply to provide power to the stepper motor. 

Note: You need a separate battery or power supply for the motor. You cannot use the same power supply for both the Intel® Edison board and the motor, so you need either 2 batteries or 2 power supplies in total.

1. Plug the stepper motor controller into four pins (9, 10, 11, and 12) on the Arduino* breakout board. It also must be connected to ground (GND), to the **5V** power coming from the Arduino* breakout board (VCC), and to the separate **5V** power for the motor (VM).<br>
![](./../../images/js/range-finder.jpg)
2. Plug one end of a Grove* cable into the Grove* IR Distance Interrupter, and connect the other end to the D2 port on the Grove* Base Shield V2.

### Intel® Edison board setup

This example uses the Crow web micro-framework to provide a simple-to-use, yet powerful web server. The Crow library requires the **libboost** package be installed on the Intel® Edison board, as well as adding the needed include and lib files to the Eclipse Cross G++ Compiler and Cross G++ Linker.

1. Update opkg base feeds so you can install the needed dependencies. Establish an SSH connection to the Intel® Edison board and run the following command:<br>

        vi /etc/opkg/base-feeds.conf

2. Edit the file so that it contains the following:<br>

        src/gz all http://repo.opkg.net/edison/repo/all
        src/gz edison http://repo.opkg.net/edison/repo/edison
        src/gz core2-32 http://repo.opkg.net/edison/repo/core2-32

3. Save the file by pressing **Esc**, then **:**, then **q**, and **Enter**.

This only needs to be done once per Intel® Edison board, so if you've already done it, you can skip to the next step.

Install the **boost** libraries onto the Intel® Edison board by running the following command:

    opkg update
    opkg install boost-dev

### Copy the libraries

You need to copy the libraries and include files from the board to your computer where you're running Eclipse* so the Cross G++ Compiler and Cross G++ Linker can find them. The easiest way to do this is by running the `scp` command from your computer (NOT the Intel® Edison board), as follows:

    scp -r USERNAME@xxx.xxx.x.xxx:/usr/include/boost ~/Downloads/iotdk-ide-linux/devkit-x86/sysroots/i586-poky-linux/usr/include
    scp USERNAME@xxx.xxx.x.xxx:/usr/lib/libboost* ~/Downloads/iotdk-ide-linux/devkit-x86/sysroots/i586-poky-linux/usr/lib

Change `USERNAME@xxx.xxx.x.xxx` to match whatever username and IP address you set your board to.

Change `~/Downloads/iotdk-ide-linux` to match the location on your computer where you installed the Intel® IoT Developer Kit.

### Copy the libraries on Windows*

For help installing and using WinSCP, go to this link:

[using-winscp.md](./../../docs/cpp/using-winscp.md)

Note: You need to turn SSH on by running the `configure_edison --password` command on the board. Once you set the password, make sure you write it down. You only need to do this once and it is set when you reboot the Intel® Edison board.

### Connecting your Intel® Edison board to Eclipse*

1. In the bottom left corner, right-click anywhere on the **Target SSH Connections** tab and select **New > Connection**.<br>
![](./../../images/cpp/cpp-connection-eclipse-ide-win.png)
2. The **Intel(R) IoT Target Connection** window appears. In the **Filter** field, type the name of your board.<br>
![](./../../images/cpp/cpp-connection-eclipse-ide-win2.png)
3. In the **Select one of the found connections** list, select your device name and click **OK**.<br>
![](./../../images/cpp/cpp-connection-eclipse-ide-win3.png)
4. On the **Target SSH Connections** tab, right-click your device and select **Connect**.<br>
![](./../../images/cpp/cpp-connection-eclipse-ide-win4.png)

If prompted for the username and password, the username is **root** and the password is whatever you specified when configuring the Intel® Edison board.

### Running the code on the Intel® Edison board

When you're ready to run the example, click **Run** at the top menu bar in Eclipse.<br>
![](./../../images/cpp/cpp-run-eclipse.png)

This compiles the program using the Cross G++ Compiler, links it using the Cross G++ Linker, transfers the binary to the Intel® Edison board, and then executes it on the board itself.

After running the program, you should see output similar to the one in the image below.<br>
![](./../../images/cpp/cpp-run-eclipse-successful-build.png)

## Regenerating HTML and CSS

If you make any changes to either the **index.html** or **styles.css** file, you need to regenerate the .hex file used to serve up the assets via the built-in Crow* web server.
For help using the shell script, go to this link:

[how-to-run-the-shellscript.md](./../../docs/cpp/how-to-run-the-shellscript.md)

### Viewing the range finder data

Range data is viewed using a single-page web interface served directly from the Intel® Edison board while the sample program is running.<br>
![](./../../images/js/range-finder-web.png)

The web server runs on port `3000`, so if the Intel® Edison board is connected to WiFi* on `192.168.1.13`, the address to browse to if you are on the same network is `http://192.168.1.13:3000`.

IMPORTANT NOTICE: This software is sample software. It is not designed or intended for use in any medical, life-saving or life-sustaining systems, transportation systems, nuclear systems, or for any other mission-critical application in which the failure of the system could lead to critical injury or death. The software may not be fully tested and may contain bugs or errors; it may not be intended or suitable for commercial release. No regulatory approvals for the software have been obtained, and therefore software may not be certified for use in certain countries or environments.
