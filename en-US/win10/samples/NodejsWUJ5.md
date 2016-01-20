---
layout: default
title: NodejsWUJ5
permalink: /en-US/win10/samples/NodejsWUJ5.htm
lang: en-US
---

##Johnny-Five Node.js (Universal Windows) Sample

{% include VerifiedVersion.md %}

In this sample, you will use [Johnny-Five](https://www.npmjs.com/package/johnny-five) running on a Raspberry Pi 2 to control a servo connected to an Arduino (with [Firmata](https://www.npmjs.com/package/firmata) installed).


###Hardware required
* Raspberry Pi 2.
* [Arduino Board](https://www.arduino.cc/en/main/products) (Uno is used in this sample).
* USB to USB B cable.
* Servo.


###Set up your PC
* Install Windows 10 [with November update](http://windows.microsoft.com/en-us/windows-10/windows-update-faq).
* Install Visual Studio 2015 Update 1.
* Install the latest Node.js Tools for Windows IoT from [here](https://github.com/ms-iot/ntvsiot/releases).
* Install [Python 2.7](https://www.python.org/downloads/){:target="_blank"}.
* Install Arduino software from [here](https://www.arduino.cc/en/Main/Software).
* Install [Git for Windows](http://git-scm.com/download/win). Ensure that Git is included in your ‘PATH’ environment variable.


###Upload Firmata to your Arduino
* Connect the Arduino board with your PC using the USB cable.
* Open Arduino software.
* Go to Tools->Port and select your device.
* Go to Tools->Board and click on the type of Arduino you have.
* Go to File->Examples->Firmata and select StandardFirmata. This will open up a new window with the Firmata sketch.
* Click the upload button to upload the sketch to the Arduino board. You should see a "Done uploading" message when the upload is complete.


###Create a new Johnny-Five (Universal Windows) project
Start Visual Studio 2015 and create a new project (File \| New Project...). In the `New Project` dialog, navigate to `Node.js` as shown below (in the left pane in the dialog: Templates \| JavaScript \| Node.js).

Select the template `Basic Node.js Johnny-Five Application (Universal Windows)`

![Node.js Johnny-Five Project Dialog]({{site.baseurl}}/images/Nodejs/nodejswuj5-newprojectdialog.png)

You may get a prompt (shown below) to run npm dedupe. If you do, make sure to run it.

![npm dedupe dialog]({{site.baseurl}}/images/Nodejs/npm-dedupe-dialog.PNG)

If you don't get the prompt, you still need to run npm dedupe to avoid node module paths that are too long for deployment on the Raspberry Pi 2.
To do this, right click on the node_modules folder in the Solution Explorer window. Then click on "Open Command Prompt Here...". 
When the command window opens, run `npm dedupe`.


###Get Serialport
**Note:** Even though serialport is installed when a new Johnny-Five project is created, you still need to get a version that:

* Corresponds with the processor architecture of the device you are targeting (in this case ARM for Raspberry Pi 2).
* Is UWP (Universal Windows Platform) compatible (built from [this](https://github.com/ms-iot/node-serialport/tree/uwp) fork of serialport).

Steps to get serialport:

* Copy and unzip the file [here](https://github.com/ms-iot/ntvsiot/releases/download/2.0.4/serialport_WinIoT.zip) to your PC.
* Copy &lt;Unzipped folder&gt;\uwp\arm\serialport.node to [Johnny-Five project root]\node_modules\serialport\build\Release\node-v47-win32-arm\serialport.node  
  **Note:** node-v47-win32-arm is a new folder you will create. [Johnny-Five project root] is the the folder created by your new project in the previous section.
* Copy &lt;Unzipped folder&gt;\uwp\serialport.js to [Johnny-Five project root]\node_modules\serialport\serialport.js.


###Set up the connection between your Arduino and Raspberry Pi 2
Connect your Arduino and Raspberry Pi 2 with the USB cable. If your Raspberry Pi 2 is connected to a monitor, 
you should see the device getting recognized as shown in the image below (the name of the device may be "Arduino Uno" instead of "USB Serial Device"):

![Arduino Uno Start Screen]({{site.baseurl}}/images/Nodejs/arduino-uno-startscreen.png)


* Replace the code in app.js with the code shown below.
  
<UL>
{% highlight JavaScript %}
var five = require("johnny-five");
var board = new five.Board();

board.on("ready", function () {
    
    var servo = new five.Servo(3);
    
    // Sweep from 0-180 and repeat.
    servo.sweep();
});
{% endhighlight %}
</UL>

* Attach the servo to the the arduino board using pin 3 (you can also change the pin number in app.js). In the setup shown below, the signal wire is connected to pin 3 and the power source is the Raspberry Pi 2.

![Arduino Servo RPi2]({{site.baseurl}}/images/Nodejs/arduino-servo-rpi2.png)


###Deploy the app to your Raspberry Pi 2
* Go to the Project menu and select '&lt;Your project name&gt; Properties' (You could also right-click on the project node in solution explorer to access Properties). Enter the IP Address in the Remote Machine text box. Since you're building for Raspberry Pi 2, select `ARM` in the dropdown menu.

* Now we're ready to deploy the app to the Raspberry Pi 2. Simply press F5 (or select Debug \| Start Debugging) to start debugging the app. This step will also start rotating the motor on the servo.


### GitHub
* NTVS IoT Extension source code: [https://github.com/ms-iot/ntvsiot](https://github.com/ms-iot/ntvsiot)