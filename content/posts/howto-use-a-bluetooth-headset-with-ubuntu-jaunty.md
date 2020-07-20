---
title: 'HowTo: Use a Bluetooth headset with Ubuntu Jaunty'
tags:
  - Linux
categories:
  - Linux
date: 2009-07-26 18:21:00
---

[HowTo: Use a Bluetooth headset with Ubuntu Jaunty - Overclockers Australia Forums](http://forums.overclockers.com.au/showthread.php?t=780054)

At the risk of self-appointing myself tech support for this, I present an updated version of my Bluetooth headset with Ubuntu Hardy guide, this time for Ubuntu Jaunty 9.04.

This HowTo illustrates how to pair and configure a Bluetooth headset with Ubuntu Jaunty 9.04 and redirect your music and media players to it. This guide is based on the guides at Ubuntu Forums with some modifications to be more step-explicit and thus newbie-friendly and reflects the changes to the Bluetooth stack since my original Hardy instruction.


## Purpose:

To play your audio such as MP3's and movie output through a Bluetooth-connected audio headset using Ubuntu Jaunty 9.04.

## Scenario:

You are too lazy to plug in a cable to your PC. You want to walk around your house without a long cable after you. You want to look cool to your mates. You want to psyche out your non-tech parents by listening to music without any audio device or cables attached to you. Problem is, very few Linux apps have direct support for directing sound to any other device.

## Solution:

Redirect all audio using the PulseAudio Server on Ubuntu Jaunty.

## Prerequisites:

* Bluetooth 1.2 compliant (or better) adapter to your PC. This can be in-built, such as on a modern notebook PC, or a USB dongle such as the one I use. You will NOT have success with Bluetooth adapters that are only compliant with the 1.0 or 1.1 specification.
* Ubuntu Jaunty 9.04 (I'm using the 64-bit version in this example).
* Internet access or other such access to the Ubuntu repositories to install extra software from.
* Suitable Bluetooth headset. The unit I bought is a TDK BT100 pair of buds. Supports Bluetooth 1.2 and 2.0, does headset, handsfree A2DP and AVRCP functions (typical function set of any headset/handsfree unit these days). Though the sound quality of these things isn't that great, it was only $49 at my local PC shop.

## Pros:

* You are cable free! You can walk around the house (as far as the range of your Bluetooth adapter goes) and listen to music/movies.
* If your headset also has a mic, you can use that too for VOIP apps and the like, though this is not a working feature (at least for me) in Jaunty.

## Cons:

* A Bluetooth headset is generally more expensive than a cabled one.
* Most headset batteries only last four hours in continuous use before needing a recharge (though the TDK unit I have plugs into mini-USB to recharge and can still be used whilst recharging, which is convenient).
* Audio quality is generally not as good as a dedicated cabled headset.
* For those headsets that have battery-saving functions when idle, some are known to clip the start of the audio playback when turning back on (my set causes Ubuntu to wait until it's up and running before streaming the audio, thus no clipping).
* Due to large changes in how Bluetooth works in Ubuntu between Hardy and Jaunty, you may find your Bluetooth headset might not be visible at all by the system or is seen, but refuses to pair.

## Steps:

These instructions should be adaptable to other distributions.

1. Fire up/install Ubuntu as normal.
2. Plug in or enable your Bluetooth adapter. Your Bluetooth adapter will be automatically detected and drivers loaded - there is nothing for you to do manually here.
3. Turn on your Bluetooth headset.
4. Switch your headset into pairing mode (refer to your headset's manual).
5. While the headset is in pairing mode, left click the Bluetooth icon in your system tray and choose "Setup new device" from the menu. Follow the wizard prompts to seek out and pair your headset.
6. Once paired, open a terminal, and type in the following:
    ```bash
    $ hcitool scan
    ```
    Your PC will now scan for local Bluetooth devices and your headset should appear in the resulting list after a few seconds (along with anyone's Bluetooth-enabled mobile phones that are in range). The output will look something like:
    ```bash
    $ hcitool scan 
      Scanning ... 
      00:11:22:AA:BB:CC Nokia N95 
      00:33:44:DD:EE:FF BT81
    $
    ```
    In this example, my PC has found my Nokia mobile phone and my Bluetooth headset and shown me the MAC addresses for both of them.
7. Highlight and copy the MAC address of the headset to the clipboard using your mouse and CTRL + SHIFT + C. In this example my headset's MAC address is "00 : 33 : 44 : DD : EE : FF". Yours will be different - copy YOUR address, not this example's address.
8. Now type in:

    ```bash
    sudo gedit ~/.asoundrc
    ```

    Note the period before "asoundrc". This will create a new hidden text file called .asoundrc in the root of your Home directory and open GEdit so you can add to it. The file is hidden because of the leading period.

9. In the text editor, type in the following, replacing the MAC address in the example with the one you copied earlier from YOUR output (paste with CTRL + V):
    ```bash
    pcm.btheadset { 
        type bluetooth
        device 00:33:44:DD:EE:FF
        profile "auto"
    }
    ```

10. Save and exit.

11. Now type in:
    ```bash
    $ sudo hciconfig hci0 voice 0x0060
    ```

    This will enable your Bluetooth adapter to carry Bluetooth audio.

12. Now we need to tell PulseAudio that your Bluetooth headset exists:
    ```bash
    $ pactl load-module module-alsa-sink device=btheadset
    $ pactl load-module module-alsa-source device=btheadset
    ```
    Note that this enables your Bluetooth headset for PulseAudio only temporarily. When you reboot, the PulseAudio configuration for Bluetooth will be lost. For future convenience, create a bash script with the above commands in it and create a launcher on your desktop to run the commands when you double-click on the launcher icon. Due to needing to have the headset paired BEFORE you run these commands, you cannot have these commands run automatically during system startup. It will cause PulseAudio to fail.

13. Once pairing has completed, we can now test to see if we can send audio to the headset. In your terminal, type in the following:
    ```bash
    $ aplay -D btheadset -f s16_le /usr/share/sounds/ubuntu/stereo/dialog-question.wav
    ```

    This will attempt direct communication with your headset, and within a second or so, you should suddenly hear the familiar Ubuntu "login ready" drum sound play through your headset! If you didn't head it first time, try the command a second time as there may be a delay between "activating" your headset and playing sound.

    Unfortunately only aplay will play anything through your headset. All other sounds are still coming through your speakers. Unless the application in question can redirect audio to another detected device, it will always play through the standard-out, so applications such as Totem and Rhythmbox will still output via your speakers and not give a hoot about your Bluetooth headset. To fix this, we need to make use of the PulseAudio Server which can redirect output to another device.

14. The PulseAudio Server is already installed by default in Ubuntu Jaunty, so we just need to install some tools to manipulate it. Go back to your terminal and type in the following:
    ```bash
    $ sudo apt-get install paprefs paman padevchooser
    ```

    This will install the PulseAudio Preferences app, the PulseAudio Manager app and the PulseAudio Device Chooser app.

15. Once installed, go to Applications-&gt;Sound &amp; Video-&gt;PulseAudio Device Chooser. This will add a black microphone jack icon to your system tray.
16. Do a left-click on the jack icon and a menu appears. In this menu, choose "Manager". A new window appears.
17. If it's not already connected, click on the "Connect" button to connect to your local PulseAudio server. When connected, you will see details about it listed.
18. Click on the Devices tab. Under "Sinks" you should see an entry for "alsa_output.btheadset". This is picked up directly from your .asoundrc file.
19. Now go to the Sample Cache tab. You are shown a list of sounds. Choose a WAV file from this list (it won't play any other format). At the bottom is a "Playback on" drop-down. Choose "alsa_output.btheadset" from this list and click on the Play button. You should hear the Ubuntu "login ready" sound through your speakers. This proves to us that PulseAudio can play through your Bluetooth headset (but this is NOT the redirection - this is just a test).
20. Close the PulseAudio Manager.
21. Do another left-click on the mic jack icon in your system tray.
22. Go to "Default Sink" and then choose "Other" from the sub-menu. A window appears.
23. In this window, type in "alsa_output.btheadset" and click OK.
24. Play a sound from somewhere, eg: MP3 or movie in Totem. You should now hear your audio coming through your Bluetooth headset! NOTE: Existing audio streams at the time of changing the sink will continue to play through whatever they were playing through until stopped and started again.
25. To switch back to your speakers, simply click on the mic jack icon again, choose "Default Sink" and choose "Default" from the sub-menu. The next audio stream played will go back through your speakers.
26. To make the PulseAudio Device Chooser start automatically on startup, click on the mic jack icon again, choose Preferences from the menu and then click on "Start applet on Session Login" in the window.
27. Enjoy!

## KNOWN ISSUES:

* This does not work with Skype. Despite the "btheadset" device being listed as a Sound Device option within Skype, you will get errors when it tries to playback or record audio via the headset and it will in fact kill the PulseAudio server forcing you to restart PulseAudio or your PC to get it running again. I have not figured this one out yet.
* You cannot have your headset auto-pair and be auto-configured with PulseAudio upon startup (yet?). You will need to pair first, then run the two "pactl" commands in step 10 manually or via a script launcher. Before you say "can't I put those commands in startup?", you cannot have these commands auto-run on startup or PulseAudio will hang or crash (because the pairing with your headset has not been established yet).
* The Sound Recorder is unable to lock onto the headset for recording audio (in fact, it goes nuts when trying to record).
* The second "pactl" command in Step 12 may cause unusual undesired system behaviour. Since the second command only exists to setup the microphone on your headset, if you do not have one or don't intend to use the microphone, you may omit this line.

## CONTRIBUTIONS:

* mbarclay has contributed a nifty script that can be used to prepare PulseAudio to talk to your headset (note that you still need to pair beforehand, and then redirect all audio with PulseAudio Dev Chooser after the script is run). You can use this script with a simple launcher icon from the desktop or a panel.
