# MID50-Display für SecondRide SR23/SR24 und MID50

Rundes Akku-, Tacho- und Diagnose-Display für Mopeds mit SecondRide-Elektroumbau.
Eine Firmware für beide Antriebe: **SR23/SR24** (per Kabel oder Bluetooth über die SR-Bridge) und **MID50** (Bluetooth).

<img src="bilder/display/ladeanzeige.png" alt="Akkuanzeige" width="220"> <img src="bilder/display/tacho.png" alt="Tacho" width="220"> <img src="bilder/display/antrieb.png" alt="Antrieb" width="220">

Bedienung, Einrichtung und Fehlersuche: **[Benutzerhandbuch](Benutzerhandbuch.md)**

## Passende Hardware

| Display | Firmware-Datei |
|---|---|
| Waveshare ESP32-S3-Touch-AMOLED-**1.43** | `…_AMOLED143.bin` |
| Waveshare ESP32-S3-Touch-AMOLED-**1.75** | `…_AMOLED175.bin` |

Welche Platine verbaut ist, zeigt das Webinterface unter **System → Gerät → Platine** (1,43 bzw. 1,75 Zoll Bilddiagonale).

## Download

Die aktuelle Firmware gibt es unter **[Releases](../../releases/latest)**. Je Version und Platine liegen dort zwei Dateien:

| Datei | Wofür |
|---|---|
| `MID50-Display_<Version>_AMOLED143.bin` / `…_AMOLED175.bin` | **Update** über das Webinterface – Einstellungen, Kopplung, Kilometerstand und Fahrten bleiben erhalten. Auch für den **Umstieg** von der alten Display-Software |
| `MID50-Display_<Version>_AMOLED143_komplett.bin` / `…_AMOLED175_komplett.bin` | **Erstinstallation per USB** auf ein neues Display – löscht alle Einstellungen |

## Update (Display läuft schon mit MID50-Display-Firmware)

1. Passende Datei `MID50-Display_<Version>_AMOLED1xx.bin` herunterladen.
2. Webinterface öffnen (`http://srdisplay/` im Heim-WLAN oder `http://192.168.4.1` über den Hotspot).
3. **System → Firmware-Update**, Datei auswählen, **Hochladen & installieren**. Nach etwa einer halben Minute startet das Display neu.

Eine Datei für die andere Platine wird abgelehnt – das Display läuft dann unverändert weiter.

## Umstieg von der alten Display-Software

Läuft auf dem Display noch die alte Software (Arduino-Version), genügt ein Update über deren Webinterface – ganz ohne Kabel:

1. Passende Datei `MID50-Display_<Version>_AMOLED1xx.bin` herunterladen (**nicht** die `_komplett.bin`).
2. Im Webinterface der alten Software das Firmware-Update öffnen und die Datei hochladen.
3. Das Display startet mit der neuen Software. Die alten Einstellungen werden nicht übernommen: Ist kein WLAN eingetragen, öffnet es den Hotspot **„SR HotSpot 192.168.4.1“** – weiter im [Benutzerhandbuch, Erste Inbetriebnahme](Benutzerhandbuch.md#erste-inbetriebnahme).

Spätere Updates laufen dann wie oben beschrieben.

## Erstinstallation per USB (neues Display)

Für ein neues Display oder wenn das Webinterface nicht erreichbar ist. Dabei werden **alle Einstellungen gelöscht**.

### Mit dem Flash Download Tool (Windows, empfohlen)

Das kostenlose [Flash Download Tool](https://www.espressif.com/en/support/download/other-tools) von Espressif herunterladen, entpacken und starten – eine Installation ist nicht nötig.

1. Display per USB-C an den PC anschließen.
2. Im Startfenster wählen: **ChipType: ESP32-S3**, **WorkMode: Develop**, **LoadMode: USB** → OK.
3. In der ersten Zeile der Dateiliste: Haken setzen, die passende `…_komplett.bin` auswählen, Adresse **0x0** eintragen. Alle anderen Zeilen leer lassen.
4. Darunter einstellen:
   - **SPI SPEED:** 80MHz
   - **SPI MODE:** QIO
   - **FLASH SIZE:** 128Mbit
   - **DoNotChgBin:** ankreuzen (wichtig!)
5. **COM:** den Anschluss des Displays wählen (Windows: Geräte-Manager → Anschlüsse), **BAUD:** 921600, dann **START**.
6. Nach „FINISH“ das USB-Kabel kurz abziehen oder **RESET** drücken.

Findet das Tool das Display nicht: **BOOT**-Taste gedrückt halten, kurz **RESET** drücken, BOOT loslassen und erneut **START**.

### Mit esptool (Kommandozeile, alle Betriebssysteme)

Braucht Python und das Werkzeug **esptool** (`pip install esptool`). Den Anschluss (z. B. `COM5`, unter Linux/macOS `/dev/ttyACM0`) anpassen:

```
esptool --chip esp32s3 --port COM5 write-flash 0x0 MID50-Display_1.0.0_AMOLED143_komplett.bin
```

### Danach

Das Display startet mit dem Hotspot **„SR HotSpot 192.168.4.1“** – weiter im [Benutzerhandbuch, Erste Inbetriebnahme](Benutzerhandbuch.md#erste-inbetriebnahme).

## Hinweise

- Nutzung auf eigene Gefahr. Das Display liest die Daten des Antriebs nur aus und greift nicht in die Steuerung ein.
- Fehler oder Wünsche bitte als [Issue](../../issues) melden. Hilfreich sind dabei der Text unter **System → Protokoll** und **System → Neustarts und Abstürze**.
- Der Quellcode ist nicht öffentlich.
