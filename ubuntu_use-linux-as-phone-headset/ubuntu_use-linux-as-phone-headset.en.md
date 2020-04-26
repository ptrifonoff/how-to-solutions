# Ubuntu: Linux als Headset für Smartphone verwenden

## Goal
I would like to use the PC with a bluetooth paired smartphone for audio transmission of playback
audio files or also for phone calls via the PC microphone and the PC speaker.

## Test-System
* Notebook HP EliteBook 2570p
* USB Bluetooth Dongle (optional - in my case the built-in Bluetooth-Module was not working)
* Elementary OS 5.1.3 (based on Ubuntu)

## 1. Setup for first run
* bluez is the bluetooth implementation for Linux (already installed)
* install Ofono:
    * <code>sudo apt-get update</code>
    * <code>sudo apt-get install ofono</code>
    * With Ofono it is possible to connect the PC under Linux for phone calls (otherwise it only 
      connects to "media", e.g. to playback music)
* install PulseAudio

## 2. Playback recording
* Pair smartphone via Bluetooth with your PC 
* Open Pulseaudio
* Make a call on paired smartphone
* Playback audio on your PC, e.g. within a Browser or a local file with Audacity
* Use Pulseaudio to select the paired smartphone as output device under "Playback"
* The person you called should now hear the audio file
