# Ubuntu: Connect 2 conference systems via audio

## Ziel
I would like to connect two conference systems via audio - in this case it is an audio-only conference 
system via browser and the video conference system "Zoom".

The solution should cross out the two systems with "virtual cables":
* Zoom Audio-Output -> Browser-Microphone
* Browser Audio-Output -> Zoom-Microphone

General Conditions:
* Individual participants of the browser system do not have to be visible in Zoom.
* The user of the computer on which the two systems are connected does not actively participate in the 
  meeting via notebook.
* A few seconds of latency in the transmission are acceptable.

## Test-System
* Notebook HP EliteBook 2570p
* Elementary OS 5.1.3 (based on Ubuntu)

## 1. Setup for first run
* Install PulseAudio to create virtual audio devices
* Edit default settings in File <code>/etc/pulse/default.pa</code> 
  (e.g. with command <code>sudo nano /etc/pulse/default.pa</code>) and append these lines at the end of the file:
    * <code>load-module module-null-sink sink_name="out_zoom_meeting" sink_properties=device.description="Zoom_Meeting"</code>
    * <code>load-module module-remap-source source_name="in_browser_conf" master="out_zoom_meeting.monitor" source_properties=device.description="Zoom-to-Browser"</code>
    * <code>load-module module-null-sink sink_name="out_browser_audio" sink_properties=device.description="Browser_Audio"</code>
    * <code>load-module module-remap-source source_name="in_zoom_meeting" master="out_browser_audio.monitor" source_properties=device.description="Browser-to-Zoom"</code>
* The command with <code>module-remap-source</code> is necessary, because Zoom does not fully support PulseAudio - so you cannot select
  the virtual device <code>out_browser_audio.monitor</code>. Because of this remap-command it is possible to select browser-output as audio-source in Zoom.
    * *Note:* the remap command with name "in_browser_conf" is not required if the browser supports PulseAudio and "out_zoom_meeting.monitor" 
      can be selected in the browser.
* If the settings above are not meant to be set by default but only temporarily, the commands can also be entered temporarily each time Pulse Audio has been started by entering 
  them in the terminal with the leading command <code>pactl</code> (<code>pactl load-module ...</code>).

## 2. Verbindung herstellen
* Open Zoom
    * The meeting can be joined as a guest, in case that the meeting security settings allow this (e.g. join with the name "Browser Conference System")
    * Select the virtual output device "Zoom_Meeting" in Zoom.
    * Select the virtual microphone "Browser-to-Zoom" in Zoom.
* Open browser - in this test setup browser "Chromium" has been used
    * Go to the web application to access the other conference system
    * Connect to the other conference system within the browser
* Open the application Pulseaudio
    * Switch to Playback and change the virtual device of the application "Chromium" to "Browser_Audio"
    * Switch to Recording and change the virtual microphone "Zoom_Meeting Monitor" to "Chromium"
* If mute is switched off in Zoom, the Zoom participants can hear the participants of the other conference system and the other way round
    * The person who connects the systems can also act as a mediator and only unmute the device if necessary. In Zoom this person
      could use the "raise hand" function at the guest access to indicate that someone from the other conference system wants to participate.
      
## Conclusion
* With these steps, the systems with virtual devices were successfully connected to each other and participants of both 
  systems can communicate with each other.
* With the systems used, there was an audio latency of approximately 3 seconds, which was acceptable in this case.
