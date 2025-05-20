# ProtoProjet-II.1

Ein Node-RED-basiertes Verwaltungssystem zur Steuerung und Überwachung eines Minecraft-Servers mit Discord-Integration und physischer Hardware-Steuerung.

## Überblick

Crafty_Bot ist eine umfassende Lösung zur Verwaltung eines Minecraft-Servers mit folgenden Funktionen:

- Echtzeit-Statusüberwachung des Servers in Discord
- Interaktive Discord-Schaltflächen zur Serversteuerung (Start/Stopp/Neustart)
- LCD-Display zur Anzeige von Serverinformationen
- LED-Anzeigen für die Spieleranzahl
- Physische Tasten zur Serversteuerung

Das System basiert auf Node-RED und läuft auf einem Raspberry Pi, der mit einem Minecraft-Server kommuniziert, der über Crafty Controller verwaltet wird.

## Komponenten

### 1. Discord-Schnittstelle (Crafty_Bot 01)

- Veröffentlicht eine Server-Status-Einbettungsnachricht in einem Discord-Kanal
- Aktualisiert den Status alle 5 Sekunden mit:
  - Servername, IP und Port
  - Betriebszustand
  - CPU- und Speichernutzung
  - Spieleranzahl und Spielerliste
  - Weltname und Version
- Interaktive Schaltflächen für:
  - Starten des Servers, wenn er offline ist
  - Stoppen des Servers, wenn er online ist
  - Neustarten des Servers, wenn er online ist

### 2. Tastenhandler (Crafty_Bot 02)

- Verarbeitet Tasteninteraktionen von Discord
- Sendet API-Anfragen an Crafty Controller zur Steuerung des Servers
- Bietet Feedback-Nachrichten über den Betriebsstatus
- Verfügt über Integration physischer Tasten über GPIO-Pins

### 3. Hardware-Komponenten

- 16x2 LCD-Display mit Anzeige von:
  - Servername (Zeile 1)
  - Serverstatus und Spieleranzahl (Zeile 2)
- LED-Spieleranzahlanzeige (8 LEDs für 0-8 Spieler)
- Physische Steuertasten für:
  - Starten des Servers (GPIO-Pin 19)
  - Stoppen des Servers (GPIO-Pin 16)
- LCD-Hintergrundbeleuchtungs-Umschalttaste (GPIO-Pin 21)

## API-Integration

Das System verbindet sich mit einer Minecraft-Server-Management-API unter `minecraft.lennygodart.org` und nutzt folgende Endpunkte:

- `/api/v2/servers/{server-id}/stats` - Server-Statistiken abrufen
- `/api/v2/servers/{server-id}/action/start_server` - Server starten
- `/api/v2/servers/{server-id}/action/stop_server` - Server stoppen
- `/api/v2/servers/{server-id}/action/restart_server` - Server neustarten

## Einrichtungsanforderungen

- Raspberry Pi mit installiertem Node-RED
- Discord-Bot-Token mit entsprechenden Berechtigungen
- Crafty Controller API-Token
- 16x2 LCD-Display (I2C-Schnittstelle)
- 8 LEDs, angeschlossen an GPIO-Pins
- 3 Drucktasten, angeschlossen an GPIO-Pins

## GPIO-Pin-Belegung

### Eingänge
- GPIO 16: Server-Stopp-Taste
- GPIO 19: Server-Start-Taste
- GPIO 21: LCD-Hintergrundbeleuchtungs-Umschalttaste

### Ausgänge (LED-Spieleranzahl)
- GPIO 12: LED 8
- GPIO 13: LED 7
- GPIO 14: LED 6
- GPIO 15: LED 5
- GPIO 22: LED 4
- GPIO 17: LED 3
- GPIO 18: LED 2
- GPIO 27: LED 1

## Python-Skripte

Das System ruft folgende Python-Skripte zur LCD-Steuerung auf:
- `/home/lenny/digilab/lcd/init.py` - LCD initialisieren und löschen
- `/home/lenny/digilab/lcd/backlight.py` - LCD-Hintergrundbeleuchtung steuern
- `/home/lenny/digilab/lcd/write.py` - Text auf LCD-Bildschirm schreiben

## Installation

1. Importieren Sie die Node-RED-Flows in Ihre Node-RED-Instanz
2. Konfigurieren Sie Discord-Anmeldedaten in den Discord-Knoten
3. Aktualisieren Sie das API-Token in den HTTP-Anforderungsknoten
4. Verbinden Sie die Hardware-Komponenten mit den entsprechenden GPIO-Pins
5. Stellen Sie sicher, dass die Python-LCD-Skripte vorhanden sind
6. Stellen Sie die Flows bereit

## Flows

### Crafty_Bot 01
Verwaltet die Server-Statusüberwachung, Discord-Statusaktualisierungen, LCD-Anzeige und LED-Anzeigen.

### Crafty_Bot 02
Verwaltet Tasteninteraktionen von Discord und physischen Tasten.

## Sicherheitshinweis

Die API-Tokens in den Flow-Dateien sollten vor der Bereitstellung durch Ihre eigenen Tokens ersetzt werden.
