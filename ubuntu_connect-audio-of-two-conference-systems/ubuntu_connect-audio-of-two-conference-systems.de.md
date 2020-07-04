# Ubuntu: 2 Konferenzsysteme per Audio miteinander verbinden

## Ziel
Ich möchte zwei Konferenzsysteme per Audio miteinander verbinden - in dem Fall geht es um ein Audio-only
Konferenzsystem via Browser und dem Videokonferenzsystem "Zoom".

Das Ergebnis sollte die beiden Systeme mit "virtuellen Kabeln auskreuzen":
* Zoom Audio-Ausgabe -> Browser-Mikrofon
* Browser Audio-Ausgabe -> Zoom-Mikrofon

Rahmenbedingungen:
* In Zoom müssen nicht einzelne Teilnehmer sichtbar sein, sondern als eine Zuschaltung.
* Der Benutzer des Rechners, auf dem die beiden Systeme verbunden werden, nimmt nicht aktiv an der Besprechung teil.
* Wenige Sekunden Latenz bei der Übertragung sind akzeptabel.

## Test-System
* Notebook HP EliteBook 2570p
* Elementary OS 5.1.3 (basierend auf Ubuntu)

## 1. Erstmalige Einrichtung
* PulseAudio installieren um virtuelle Audio-Geräte zu erstellen
* Default Settings in Datei <code>/etc/pulse/default.pa</code> bearbeiten 
  (z.B. mit <code>sudo nano /etc/pulse/default.pa</code>) und diese Zeilen am Ende der Datei einfügen:
    * <code>load-module module-null-sink sink_name="out_zoom_meeting" sink_properties=device.description="Zoom_Meeting"</code>
    * <code>load-module module-remap-source source_name="in_browser_conf" master="out_zoom_meeting.monitor" source_properties=device.description="Zoom-to-Browser"</code>
    * <code>load-module module-null-sink sink_name="out_browser_audio" sink_properties=device.description="Browser_Audio"</code>
    * <code>load-module module-remap-source source_name="in_zoom_meeting" master="out_browser_audio.monitor" source_properties=device.description="Browser-to-Zoom"</code>
* Der Befehl mit <code>module-remap-source</code> ist erforderlich, weil Zoom PulseAudio nicht voll unterstützt und dadurch
  das virtuelle Gerät <code>out_browser_audio.monitor</code> nicht ausgewählt werden kann, wie es möglich wäre, wenn PulseAudio
  unterstützt werden würde. Mit diesem remap-Befehl ist es trotzdem möglich, die Browser-Ausgabe als Audio-Quelle auszuwählen.
    * *Hinweis:* der remap-Befehl mit Namen in_browser_conf ist nicht erforderlich, wenn der Browser PulseAudio unterstützt
      und im Browser "out_zoom_meeting.monitor" ausgewählt werden kann.
* Wenn die obigen Einstellungen nicht standardmäßig gesetzt werden sollen, sondern nur temporär, können die genannten Befehle
  auch temporär bei jedem Start von Pulse Audio erneut hergestellt werden, indem die Befehle mit vorangestelltem
  <code>pactl</code> im Terminal eingegeben werden (<code>pactl load-module ...</code>).


## 2. Verbindung herstellen
* Zoom öffnen
    * Zugang zum Meeting kann als Gast hergestellt werden, sofern es die Meeting-Einstellungen zulassen (z.B. mit Namen "Browser Conference System")
    * Unter Ausgabe als virtueller Lautsprecher auf "Zoom_Meeting"
    * Unter Eingabe als virtuelles Mikrofon auf "Browser-to-Zoom" stellen
* Browser öffnen - hier wird der Browser "Chromium" verwendet
    * Verbindung zu anderem Konferenzsystem über die Webapplikation herstellen
* Anwendung Pulseaudio öffnen
    * Unter Wiedergabe das Programm "Chromium" auf das virtuelle Gerät "Browser_Audio" umstellen
    * Unter Aufnahme das Programm "Chromium" auf "Zoom_Meeting Monitor" oder auf "Zoom-to-Browser" umstellen
* Wird die Stummschaltung in Zoom nun aufgehoben, können Zoom Teilnehmer die Teilnehmer aus dem Browser hören bzw. umgekehrt
    * Derjenige, der die Systeme verbindet, kann auch als Mittler fungieren und nur bei Bedarf die Stummschaltung aufheben
      bzw. in Zoom z.B. durch die "Hand-heben"-Funktion am Gast-Zugang ankündigen, dass sich jemand aus dem anderen Konferenzsystem
      beteiligen möchte.
      
## Fazit
* Mit diesen Schritten wurden die Systeme mit virtuellen Geräten miteinander erfolgreich verbunden und Teilnehmer beider Systeme 
  können miteinander kommunizieren.
* Mit den verwendeten Systemen gab es eine Audio-Latenz von ca. 3 Sekunden, die in diesem Fall allerdings akzeptabel war.
