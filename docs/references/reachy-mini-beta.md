---
id: reachy-mini-beta
title: Reachy Mini Beta
slug: /references/reachy-mini-beta
unlisted: true
---

# Reachy Mini Beta - Random Notes 

Working through more complicated and nuanced challenges while thinking out loud. Most of this came from others I'm just pulling it all together. Sharing in case helpful for others in the Reachy Mini beta community.

---

## Sound Issues with the reSpeaker devices

There seem to be a lot of challenges around this, but for what it's worth I think the pain is worthwhile. Firstly, I think it's all solvable, and secondly, because I think there's a lot of very cool things that can be done with this setup. I like the design choice and it's a cool array. These seem like expected growing pains, not dealbreakers.

You can generally use the Mic/Speaker as an input/output device. 

Reachy will self-discover and set it automatically (but no reason this couldn't be swapped for any other input/output). If it's not self-discovering and you're seeing a message like "could not find reSpeaker device, using default" then make sure you're using the latest code since related fixes have been merged.

In Windows it shows up as one device. In Linux this shows up as two devices, each accessable through both analog and digital interfaces.  I couldn't find any meaningful difference in sound quality or features. 

#### Resolving Sound Issues

The issues mostly seem to be around configuration and how the system recognizes the devices, not the hardware itself. As a result, challenges differ between Windows/Linux. It seems reliable and stable once you get there (the most important part) despite some rakes along the way.

I did encounter an [EHCI controller-specific high-speed USB 2.0 concern which can affect older hardware](https://github.com/pollen-robotics/reachy_mini/issues/389), but I think that's going to be a pretty uncommon issue unless you're running on equipment before ~2013.

### Sound Resolution - Basic / Initial Troubleshooting 

	verify that the hardware is working properly:
    ```
	# List the available audio devices to find the ReSpeaker card ID:
	$ aplay -l
	# Record a 2-second WAV file to test the microphone:
	$ arecord -Dplughw:<card_id_number> -d 2 -f cd -t wav -r 16000 -c 1 test.wav
	# Make sure that the volume of PCM,0 and PCM,1 are at maximum
	$ alsamixer
	# Play the recorded WAV file to verify the audio output:
	$ aplay -Dplughw:<card_id_number> test.wav
	# Alternatively, test the speaker directly:
	$ timeout 2s speaker-test -Dplughw:<card_id_number>
	```
	
	??? how does this compare to the test script?

	
### Sound Resolution - Turn Up System Volume 
	Fixing the Audio Volume (ReSpeaker Volume Fix)
	This is the sequence that fixes my quiet volume.
	Run this on the HOST terminal.

	---

	Step 1 — Detect the ReSpeaker CARD ID

	`CARD=$(aplay -l | grep -i "reSpeaker" | head -n1 | sed -n 's/^card ([0-9]):./\1/p')
	`echo $CARD`

	---

	Step 2 — Set volume to MAX

	`amixer -c "$CARD" set PCM,1 100%`
	If you want to force all PCM channels to max use:
	`amixer -c "$CARD" sset 'PCM' 100%`

	---

	Step 3 — Persist volume across reboots

	`sudo alsactl store "$CARD"`
	enter your password if asked for
	
	Optional: Check current levels
	`amixer -c "$CARD"`

### Sound Resolution - Reboot on Startup
Please run the following command every time the robot is plugged in or the host machine reboots to ensure proper functionality: xvf_host REBOOT 1 (xvf_host available here)


### Sound Resolution - Firmware 
	dfu-util 
	If the ReSpeaker firmware version is ≤ 2.1.0, verify it using dfu-util -l,
	
### Sound Resolution - Unable to find reSpeaker, using default device 
Playback may not work on Windows 
I found a Windows-specific issue where 
(https://github.com/pollen-robotics/reachy_mini/pull/362)






### MISC / Untested / Unsure if valuable
One issue was resolved by settting default system output to something else other than your Reachy and restart the app/daemo.

There was a concern related to a different dependency... pyaudio-19-dev (that's not it, but something similar) that seemed to be helpful (will update when I come across it again).

I have an issue where when I go from one app to another, something breaks (e.g., Default conversation demo app, to another similar app, and back again). It's probably an edge case, but I suspect figuring out what's causing it may shed some light on general concerns.

###TBD: what was windows specific resolution?
	

###TBD: various settings (& explore) 
	there were ~a dozen modes?


	
## Startup Issues 

### MISSING MOTOR ID's 
	I get this intermittently and the daemon won't start. Sometimes I have to restart the daemon a few times and it's find. It's intermitten enough I'm not worried about it, a failure retry/recovery would probaly help here, but it's intermitten enough that restarting a few times seems to do the trick.
	
	Others have reported persistent issues with loose or not fully connected motor cables. 



## Optimizations 

The default Dynamixel PID settings are pretty agressive. It works as-is, but I've come to like the updated settings I've been exploring. 