# Ubuntu: Linux als Headset für Smartphone verwenden

## Ziel
Ich möchte den PC mit gekoppeltem Smartphone zur Live-Übertragung von abgespielten Audiodateien
oder auch zum Telefonieren über das PC-Mikrofon und den PC-Lausprecher verwenden.

## Test-System
* Notebook HP EliteBook 2570p
* USB Bluetooth Dongle (nicht unbedingt zwingend - in meinem Fall hat das eingebaute Bluetooth-
  Modul nicht funktioniert)
* Elementary OS 5.1.3 (basierend auf Ubuntu)

## 1. Erstmalige Einrichtung
* bluez ist die Bluetooth Implementierung für Linux (bereits vorinstalliert)
* ofono installieren:
    * <code>sudo apt-get update</code>
    * <code>sudo apt-get install ofono</code>
    * Ofono ermöglicht es den PC unter Linux für Anrufe zu verbinden (andernfalls verbindet es nur
      mit "Medien", z.B. um Musik abzuspielen)
* PulseAudio installieren

## 2. Aufnahme abspielen
* Smartphone per Bluetooth mit Notebook koppeln 
* Pulseaudio öffnen
* Anruf am gekoppelten Smartphone herstellen
* Audio z.B. im Browser oder eine lokale Datei in Audacity am Notebook abspielen
* In Pulseaudio das gekoppelte Smartphone als Ausgabegerät unter "Wiedergabe" auswählen
