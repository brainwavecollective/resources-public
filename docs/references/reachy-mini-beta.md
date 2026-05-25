---
id: reachy-mini-beta
title: Reachy Mini Beta
slug: /references/reachy-mini-beta
unlisted: true
---

# Reachy Mini Beta - Random Notes 

Working through more complicated and nuanced challenges while thinking out loud. Most of this came from others in the Discord, I'm just pulling it all together for ease of refernece. Sharing in case helpful for others in the Reachy Mini beta community.


What's working now: 

### FIRST WINDOW (Daemon)
```
cd ~~brainwavecollective/reachy_mini
git checkout windows
uv run reachy-mini-daemon
```
### SECOND WINDOW (App)
```
cd ~~pollen-robotics/reachy_mini_conversation_app
git checkout local-wip
#if any updates are necessary... need to run this to ensure latest from above (could be smaller, but it's just a little slower)
# uv pip install --force-reinstall -e .
uv run reachy-mini-conversation-app
```

git remote add show0000 git@github.com:show0000/reachy_mini.git

---

## Sound Issues with the reSpeaker devices

There seem to be a lot of challenges around this, but for what it's worth I think the pain is worthwhile. I think it's all solvable, and there are a lot of very cool things that can be done with this setup. I like the design choice and it's a cool array. TL;DR: These seem like expected growing pains, not dealbreakers.

You'll find that the Mic/Speaker is recognized for general input/output.

Reachy will self-discover and set it automatically (but no reason this couldn't be swapped for any other input/output). If it's not self-discovering and you're seeing a message like "could not find reSpeaker device, using default" then make sure you're using the latest code since related fixes have been merged.

In Windows it shows up as one device. In Linux this shows up as two devices, each accessable through both analog and digital interfaces.  I couldn't find any meaningful difference in sound quality or features. 

#### Resolving Sound Issues

The issues mostly seem to be around configuration and how the system recognizes the devices, not the hardware itself. As a result, challenges differ between Windows/Linux. It seems reliable and stable once you get there (the most important part) despite some rakes along the way.

I did encounter an [EHCI controller-specific high-speed USB 2.0 concern which can affect older hardware](https://github.com/pollen-robotics/reachy_mini/issues/389), but I think that's going to be a pretty uncommon issue unless you're running on equipment before ~2013.

### Sound Resolution - A note about firmware 

Using arbitrary `${AUDIO_NAME}` to arbitrarily identify the name of the device that was changed at firmware version 2.1.3. This is just to make it easier for me to manage docs.

** Firmware 2.1.3 and ABOVE **
export AUDIO_NAME="Reachy Mini Audio"

** Firmware 2.1.2 and BELOW ** 
export AUDIO_NAME="reSpeaker"


### Sound Resolution - Basic / Initial Troubleshooting 

verify that the hardware is working properly:
```
# List the available audio devices to find the ReSpeaker card ID:
$ aplay -l
# Alternatively... 
`CARD=$(aplay -l | grep -i "${AUDIO_NAME}" | head -n1 | sed -n 's/^card \([0-9]\+\):.*/\1/p')`
`echo $CARD` (expect a number like '4')
# Record a 2-second WAV file to test the microphone:
$ arecord -Dplughw:${CARD} -d 2 -f cd -t wav -r 16000 -c 1 test.wav


# Make sure that the volume of PCM,0 and PCM,1 are at maximum
$ alsamixer




# Play the recorded WAV file to verify the audio output:
$ aplay -Dplughw:<card_id_number> test.wav
# Alternatively, test the speaker directly:
$ timeout 2s speaker-test -Dplughw:<card_id_number>
```  

TBD: how does this approach compare to the test script?


### Sound Resolution - Turn Up System Volume 
Fixing the Audio Volume (ReSpeaker Volume Fix)
This is the sequence that fixes quiet volume.
Run this on the HOST terminal.

---

#Step 1 — Detect the ReSpeaker CARD ID

`CARD=$(aplay -l | grep -i "${AUDIO_NAME}" | head -n1 | sed -n 's/^card \([0-9]\+\):.*/\1/p')`
`echo $CARD`

---76

#Step 2 — Set volume to MAX

`amixer -c "$CARD" set 'PCM',1 100%`
If you want to force all PCM channels to max use:
`amixer -c "$CARD" set 'PCM' 100%`

---

#Step 3 — Persist volume across reboots

`sudo alsactl store "$CARD"`
enter your password if asked for

Optional: Check current levels
`amixer -c "$CARD"`

### Sound Resolution - Reboot on Startup

#### Firmware 2.1.2 and below 

	Run the following command every time the robot is plugged in or the host machine reboots: 
	`sudo ./xvf_host REBOOT 1`

	xvf_host is available here: 
	https://github.com/respeaker/reSpeaker_XVF3800_USB_4MIC_ARRAY/tree/master/host_control

	TBD IF NECESSARY 
		Note that on Windows you also need to install the libusb-win32 Driver with Zadig. These instructions are for firmware 2.1.3 but will likely be different with firmware 2.1.2 since there was a rename.

		Install libusb-win32 on:
		 - Reachy Mini Audio DFU Factory (Interface 4) - This is the DFU interface
		 - Reachy Mini Audio Control (Interface 3) - This is the Control interface

		On my machine I was replacing WINUSB (v10.0.19041.1) with libusb-win32 (v1.4.0.0). After installing it shows libusb0 (v1.4.0.0).

		FYI Windows is broken, either becuase ??? does not fully resolve the way that 316/??? did

		Either way I'm dead in the water for Windows because I can't run xvf_host to rule that out.


#### Firmware 2.1.3 and above 
	(see notes from below, simplify both)


### Sound Resolution - Everyone should upgrade firmware 

There have been a number of related updates. Whats new :
	- No more initialization issues with the microphones 🛠️ 
	- More audio customization options accessible from `src/reachy_mini/media/audio_control_utils.py` 🧪 
	- Improved audio echo cancellation performances and cleaner implementation of the audio localisation (DoA) feature 🦾 

	To update the firmware...

	UNIX/LINUX 
	```
	cd src/reachy_mini/assets/firmware
	./update.sh reachymini_ua_io16_lin_v2.1.3.bin
	```
	
	WINDOWS ([PwnosaurusRex](https://discord.com/channels/519098054377340948/1430980007697711296/1441902022403162133))
	
		https://wiki.seeedstudio.com/respeaker_xvf3800_introduction/#install-dfu-util

		- Download [this file](https://dfu-util.sourceforge.net/releases/dfu-util-0.11-binaries.tar.xz) and extract the `win64` folder contents to a new directory.
			- Use 7-zip or NanaZip to open it. For the rest of this example we'll assume a folder like `C:\reachy-firmware`.
				- To install NanaZip, you can run `winget install -e --id M2Team.NanaZip`
				- Within that folder you should now have the following:
			```
			C:\reachy-firmware
				dfu-prefix.exe
				dfu-suffix.exe
				dfu-util-static.exe
				dfu-util.exe
				libusb-1.0.a
				libusb-1.0.dll
				libusb-1.0.dll.a
				libusb-1.0.la
				lsusb-static.exe
				lsusb.exe
			```
			- Open a terminal and change directory (`cd`) to it by entering `cd C:\reachy-firmware`
			- Test connection to the utility with `dfu-util -V`. You should see:
			```
			dfu-util 0.11

			Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
			Copyright 2010-2021 Tormod Volden and Stefan Schmidt
			This program is Free Software and has ABSOLUTELY NO WARRANTY
			Please report bugs to http://sourceforge.net/p/dfu-util/tickets/
			```
			- Enter `dfu-util -l` to test connection to the actual device; you should see some output that looks like the following
			```
			dfu-util 0.11

			Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
			Copyright 2010-2021 Tormod Volden and Stefan Schmidt
			This program is Free Software and has ABSOLUTELY NO WARRANTY
			Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

			Found DFU: [38fb:1001] ver=0213, devnum=43, cfg=1, intf=4, path="1-10.2.1", alt=1, name="Reachy Mini Audio DFU Upgrade", serial="XXX..."
			Found DFU: [38fb:1001] ver=0213, devnum=43, cfg=1, intf=4, path="1-10.2.1", alt=0, name="Reachy Mini Audio DFU Factory", serial="XXX..."
			```
			- Download the [latest firmware](https://github.com/pollen-robotics/reachy_mini/tree/develop/src/reachy_mini/assets/firmware) and copy it to the same directory as the other files.
			- The directory should now have that additional file in it:
			```
			C:\reachy-firmware
				dfu-prefix.exe
				dfu-suffix.exe
				dfu-util-static.exe
				dfu-util.exe
				libusb-1.0.a
				libusb-1.0.dll
				libusb-1.0.dll.a
				libusb-1.0.la
				lsusb-static.exe
				lsusb.exe
				reachymini_ua_io16_lin_v2.1.3.bin
			```
			- Plug in the robot.
			- Now flash it with `dfu-util -R -e -a 1 -D .\reachymini_ua_io16_lin_v2.1.3.bin`.
				- Replace the firmware filename with the actual one you downloaded.
			- After successful flashing, disconnect and reconnect the robot.
			- Start the daemon with `uvx --from reachy-mini reachy-mini-daemon` (assuming you have [uv](https://docs.astral.sh/uv/getting-started/installation/) installed). You should see the robot come to life and play the launch sound through its internal speaker.

			For more information (including troubleshooting steps) [see the docs](https://wiki.seeedstudio.com/respeaker_xvf3800_introduction/#update-firmware).
		
	ADDITIONAL MISC NOTES
```	
		$ ./dfu-util.exe -l
			dfu-util 0.11

			Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
			Copyright 2010-2021 Tormod Volden and Stefan Schmidt
			This program is Free Software and has ABSOLUTELY NO WARRANTY
			Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

			Found DFU: [38fb:1001] ver=0213, devnum=26, cfg=1, intf=4, path="2-2.1", alt=1, name="Reachy Mini Audio DFU Upgrade", serial="202000386253800130"
			Found DFU: [38fb:1001] ver=0213, devnum=26, cfg=1, intf=4, path="2-2.1", alt=0, name="Reachy Mini Audio DFU Factory", serial="202000386253800130"
			(base)

		$ ./dfu-util -R -e -a 1 -D reachymini_ua_io16_lin_v2.1.3.bin -v
			dfu-util 0.11

			Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
			Copyright 2010-2021 Tormod Volden and Stefan Schmidt
			This program is Free Software and has ABSOLUTELY NO WARRANTY
			Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

			libusb version 1.0.24 (11650)
			Opening DFU capable USB device...
			Device ID 38fb:1001
			Run-Time device DFU version 0101
			DFU attributes: (0x0f) bitCanDnload bitCanUpload bitManifestationTolerant bitWillDetach
			Detach timeout 1000 ms
			Claiming USB DFU Interface...
			Setting Alternate Interface #1 ...
			Determining device status...
			DFU state(2) = dfuIDLE, status(0) = No error condition is present
			DFU mode device DFU version 0101
			Device returned transfer size 256
			Copying data from PC to DFU device
			Download        [=========================] 100%       933888 bytes
			Download done.
			Sent a total of 933888 bytes
			DFU state(7) = dfuMANIFEST, status(0) = No error condition is present
			DFU state(2) = dfuIDLE, status(0) = No error condition is present
			Done!
			Resetting USB to switch back to Run-Time mode
			Warning: Invalid DFU suffix signature
			A valid DFU suffix will be required in a future dfu-util release
			(base)

		After...
			$ ./dfu-util.exe -l
			dfu-util 0.11

			Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
			Copyright 2010-2021 Tormod Volden and Stefan Schmidt
			This program is Free Software and has ABSOLUTELY NO WARRANTY
			Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

			Found DFU: [38fb:1001] ver=0213, devnum=27, cfg=1, intf=4, path="2-2.1", alt=1, name="Reachy Mini Audio DFU Upgrade", serial="202000386253800130"
			Found DFU: [38fb:1001] ver=0213, devnum=27, cfg=1, intf=4, path="2-2.1", alt=0, name="Reachy Mini Audio DFU Factory", serial="202000386253800130"


power cycled. unplugged both USB and power for ~30 seconds.

$ ./dfu-util.exe -l

dfu-util 0.11

Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.

Copyright 2010-2021 Tormod Volden and Stefan Schmidt

This program is Free Software and has ABSOLUTELY NO WARRANTY

Please report bugs to http://sourceforge.net/p/dfu-util/tickets/

Found DFU: [38fb:1001] ver=0213, devnum=31, cfg=1, intf=4, path="2-2.1", alt=1, name="Reachy Mini Audio DFU Upgrade", serial="202000386253800130"

Found DFU: [38fb:1001] ver=0213, devnum=31, cfg=1, intf=4, path="2-2.1", alt=0, name="Reachy Mini Audio DFU Factory", serial="202000386253800130"



$ python src/reachy_mini/media/audio_control_utils.py VERSION

Error executing command VERSION: Operation not supported or unimplemented on this platform



(base) PS C:\Users\Daniel> Get-PnpDevice -PresentOnly | Where-Object {$_.InstanceId -match "VID_38FB"} | Select-Object FriendlyName, InstanceId, Status, Class | Format-List


FriendlyName : Reachy Mini Audio DFU Factory
InstanceId   : USB\VID_38FB&PID_1001&MI_04\7&109CF04B&0&0004
Status       : OK
Class        : USBDevice

FriendlyName : Reachy Mini Audio
InstanceId   : USB\VID_38FB&PID_1001&MI_00\7&109CF04B&0&0000
Status       : OK
Class        : MEDIA

FriendlyName : USB Input Device
InstanceId   : USB\VID_38FB&PID_1001&MI_05\7&109CF04B&0&0005
Status       : OK
Class        : HIDClass

FriendlyName : USB Composite Device
InstanceId   : USB\VID_38FB&PID_1001\202000386253800130
Status       : OK
Class        : USB

FriendlyName : Reachy Mini Audio Control
InstanceId   : USB\VID_38FB&PID_1001&MI_03\7&109CF04B&0&0003
Status       : OK
Class        : USBDevice

FriendlyName : HID-compliant vendor-defined device
InstanceId   : HID\VID_38FB&PID_1001&MI_05&COL04\8&906ABE6&0&0003
Status       : OK
Class        : HIDClass

FriendlyName : HID-compliant vendor-defined device
InstanceId   : HID\VID_38FB&PID_1001&MI_05&COL03\8&906ABE6&0&0002
Status       : OK
Class        : HIDClass

FriendlyName : HID-compliant consumer control device
InstanceId   : HID\VID_38FB&PID_1001&MI_05&COL02\8&906ABE6&0&0001
Status       : OK
Class        : HIDClass

FriendlyName : HID-compliant phone
InstanceId   : HID\VID_38FB&PID_1001&MI_05&COL01\8&906ABE6&0&0000
Status       : OK
Class        : HIDClass

$ python check_driver.py
Backend: <usb.backend.libusb1._LibUSB object at 0x00000221144CAD80>
Device found: DEVICE ID 38fb:1001 on Bus 002 Address 035 =================
 bLength                :   0x12 (18 bytes)
 bDescriptorType        :    0x1 Device
 bcdUSB                 :  0x201 USB 2.01
 bDeviceClass           :   0xef Miscellaneous
 bDeviceSubClass        :    0x2
 bDeviceProtocol        :    0x1
 bMaxPacketSize0        :   0x40 (64 bytes)
 idVendor               : 0x38fb
 idProduct              : 0x1001
 bcdDevice              :  0x213 Device 2.13
 iManufacturer          :    0x1 Pollen Robotics
 iProduct               :    0x2 Reachy Mini Audio
 iSerialNumber          :    0x3 202000386253800130
 bNumConfigurations     :    0x1
  CONFIGURATION 1: 400 mA ==================================
   bLength              :    0x9 (9 bytes)
   bDescriptorType      :    0x2 Configuration
   wTotalLength         :  0x134 (308 bytes)
   bNumInterfaces       :    0x6
   bConfigurationValue  :    0x1
   iConfiguration       :    0x0
   bmAttributes         :   0xa0 Bus Powered, Remote Wakeup
   bMaxPower            :   0xc8 (400 mA)
    INTERFACE 0: Audio =====================================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x0
     bAlternateSetting  :    0x0
     bNumEndpoints      :    0x0
     bInterfaceClass    :    0x1 Audio
     bInterfaceSubClass :    0x1
     bInterfaceProtocol :   0x20
     iInterface         :    0x4 Reachy Mini Audio
    INTERFACE 1: Audio =====================================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x1
     bAlternateSetting  :    0x0
     bNumEndpoints      :    0x0
     bInterfaceClass    :    0x1 Audio
     bInterfaceSubClass :    0x2
     bInterfaceProtocol :   0x20
     iInterface         :    0x0
    INTERFACE 1, 1: Audio ==================================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x1
     bAlternateSetting  :    0x1
     bNumEndpoints      :    0x1
     bInterfaceClass    :    0x1 Audio
     bInterfaceSubClass :    0x2
     bInterfaceProtocol :   0x20
     iInterface         :    0x0
      ENDPOINT 0x1: Isochronous OUT ========================
       bLength          :    0x7 (7 bytes)
       bDescriptorType  :    0x5 Endpoint
       bEndpointAddress :    0x1 OUT
       bmAttributes     :    0xd Isochronous
       wMaxPacketSize   :   0x20 (32 bytes)
       bInterval        :    0x3
    INTERFACE 2: Audio =====================================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x2
     bAlternateSetting  :    0x0
     bNumEndpoints      :    0x0
     bInterfaceClass    :    0x1 Audio
     bInterfaceSubClass :    0x2
     bInterfaceProtocol :   0x20
     iInterface         :    0x0
    INTERFACE 2, 1: Audio ==================================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x2
     bAlternateSetting  :    0x1
     bNumEndpoints      :    0x1
     bInterfaceClass    :    0x1 Audio
     bInterfaceSubClass :    0x2
     bInterfaceProtocol :   0x20
     iInterface         :    0x0
      ENDPOINT 0x81: Isochronous IN ========================
       bLength          :    0x7 (7 bytes)
       bDescriptorType  :    0x5 Endpoint
       bEndpointAddress :   0x81 IN
       bmAttributes     :    0xd Isochronous
       wMaxPacketSize   :   0x20 (32 bytes)
       bInterval        :    0x3
    INTERFACE 3: Vendor Specific ===========================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x3
     bAlternateSetting  :    0x0
     bNumEndpoints      :    0x0
     bInterfaceClass    :   0xff Vendor Specific
     bInterfaceSubClass :    0x0
     bInterfaceProtocol :    0x0
     iInterface         :    0x5 Reachy Mini Audio Control
    INTERFACE 4: Application Specific ======================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x4
     bAlternateSetting  :    0x0
     bNumEndpoints      :    0x0
     bInterfaceClass    :   0xfe Application Specific
     bInterfaceSubClass :    0x1
     bInterfaceProtocol :    0x2
     iInterface         :    0x6 Reachy Mini Audio DFU Factory
    INTERFACE 4, 1: Application Specific ===================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x4
     bAlternateSetting  :    0x1
     bNumEndpoints      :    0x0
     bInterfaceClass    :   0xfe Application Specific
     bInterfaceSubClass :    0x1
     bInterfaceProtocol :    0x2
     iInterface         :    0x7 Reachy Mini Audio DFU Upgrade
    INTERFACE 5: Human Interface Device ====================
     bLength            :    0x9 (9 bytes)
     bDescriptorType    :    0x4 Interface
     bInterfaceNumber   :    0x5
     bAlternateSetting  :    0x0
     bNumEndpoints      :    0x1
     bInterfaceClass    :    0x3 Human Interface Device
     bInterfaceSubClass :    0x0
     bInterfaceProtocol :    0x0
     iInterface         :    0x8 Reachy Mini Audio HID
      ENDPOINT 0x82: Interrupt IN ==========================
       bLength          :    0x7 (7 bytes)
       bDescriptorType  :    0x5 Endpoint
       bEndpointAddress :   0x82 IN
       bmAttributes     :    0x3 Interrupt
       wMaxPacketSize   :   0x40 (64 bytes)
       bInterval        :    0x7

...is_kernel_driver_active() is a Linux-specific call that doesn't work on Windows. 


Linux Only `/etc/udev/rules.d/99-reachy.rules`
SUBSYSTEM=="usb", ATTRS{idVendor}=="38fb", ATTRS{idProduct}=="1001", MODE="0666"
NOTE: ATTRS instead of ATTR for vendor/product ID rules matches the parent USB device instead of the interface.
	TBD - probaby more 
		
FYI if you're unsure you can determine the ReSpeaker firmware version with `dfu-util -l`
```


#### TBD 2.1.3
$ source .venv/Scripts/activate
$ python src/reachy_mini/media/audio_control_utils.py REBOOT --values 1


### Sound Resolution - Need to referenced correct version of local install 
REF: "reachy-mini @ file:///~~GitHub/brainwavecollective/reachy_mini"
and then uv pip install . to lock down that local version (with ~~brainwavecollective/reachy_mini (v1.0.0rc4_plus-audio-fix-windows) checked out)


	
	
### Sound Resolution - Unable to find reSpeaker, using default device 
Playback may not work on Windows, issue here:
https://github.com/pollen-robotics/reachy_mini/pull/362



#### FYI  UDEV RULES (TBD where to place, and which ones are before vs. after)
```
SUBSYSTEM=="usb", ATTR{idVendor}=="38fb", ATTR{idProduct}=="1001", MODE="0666", GROUP="plugdev"
SUBSYSTEM=="tty", ATTRS{idVendor}=="38fb", ATTRS{idProduct}=="1001", MODE="0666", GROUP="dialout" #Reachy Mini soundcard' \

SUBSYSTEM=="usb", ATTR{idVendor}=="2886", ATTR{idProduct}=="001a", MODE="0666", GROUP="plugdev"
SUBSYSTEM=="tty", ATTRS{idVendor}=="2886", ATTRS{idProduct}=="001a", MODE="0666", GROUP="dialout" #Reachy Mini soundcard' \
```




### MISC / Untested / Unsure if valuable
One issue was resolved by settting default system output to something else other than your Reachy and restart the app/daemo.

There was a concern related to a different dependency... portaudio-19-dev (that may not be exactly it, but something similar) that seemed to be helpful. will update when I confirm.

I have an issue where when I go from one app to another, something breaks (e.g., Default conversation demo app, to another similar app, and back again). It's probably an edge case, but I suspect figuring out what's causing it may shed some light on general concerns.

### TBD: what was windows specific resolution?
	

### TBD: Exploring various settings
	there were ~a dozen modes


	
## Startup Issues 

### MISSING MOTOR ID's 

I get this intermittently and the daemon won't start. Sometimes I have to restart the daemon a few times and it's find. It's intermitten enough I'm not worried about it, a failure retry/recovery would probaly help here, but it's intermitten enough that restarting a few times seems to do the trick.  
	
Others have reported persistent issues with loose or not fully connected motor cables. 


### Unable to detect port on Windows 
Believed to be caused by "an issue in the low level rust controller, which expects the serial port to be an actual path - Only true on Unix systems."

Resolved with this PR - https://github.com/pollen-robotics/reachy-mini-motor-controller/pull/23

For now, you'll need to build the rust controller with `cargo build` and then the python bindings with `pip install .` To install cargo on windows : https://doc.rust-lang.org/cargo/getting-started/installation.html


### Troubleshooting Motors - direct control with Xl330PyController
You may have to check the motors wiring, and check for blinking red LEDs on the motors - Blinking means the motor is in an error state :/
In case of blinking LEDs, you can try to run this for quick and dirty troubleshooting :
from rustypot import Xl330PyController

c = Xl330PyController(serial_port='COMXXX', baudrate=1_000_000, timeout=0.1)
IDs = [10,11,12,13,14,15,16,17,18]

for id in IDs:
  c.reboot(id)
Caroline — Yesterday at 3:03 AM
You will need to install rustypot with pip install rustypot prior to that !


## Optimizations 

The default Dynamixel PID settings are overly agressive. It works as-is, but I've come to prefer updated settings. I'm still exploring, but have been fairly content running `script to set PID values` with `-p 180 and -d 20` for all servos (`--motor-id 10` through `--motor-id 18`).

 .venv\Lib\site-packages\reachy_mini_motor_controller\assets\config\hardware_config.yaml


## Miscellaneous
Exhausted. Half a night on VAD and Whispers failing to filter non-speech.
(I also improved tool calling and actions - this will “spill” back to the MCP)

SileroVAD for VAD ([thanks @nachos](https://discord.com/channels/519098054377340948/1428484261802934312/1444133897070706779)

https://www.youtube.com/watch?v=b0iJZS9HgJA



## Projects 
https://github.com/LAURA-agent/reachy_mini_auto_dancer @Townie 


