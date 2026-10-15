# SR-Display für SecondRide SR23/SR24 und MID50

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
| `SR-Display_<Version>_AMOLED143.bin` / `…_AMOLED175.bin` | **Update** über das Webinterface – Einstellungen, Kopplung, Kilometerstand und Fahrten bleiben erhalten |
| `SR-Display_<Version>_AMOLED143_komplett.bin` / `…_AMOLED175_komplett.bin` | **Erstinstallation per USB** auf ein neues Display – löscht alle Einstellungen |

## Update (Display läuft schon mit SR-Display-Firmware)

1. Passende Datei `SR-Display_<Version>_AMOLED1xx.bin` herunterladen.
2. Webinterface öffnen (`http://srdisplay/` im Heim-WLAN oder `http://192.168.4.1` über den Hotspot).
3. **System → Firmware-Update**, Datei auswählen, **Hochladen & installieren**. Nach etwa einer halben Minute startet das Display neu.

Eine Datei für die andere Platine wird abgelehnt – das Display läuft dann unverändert weiter.

## Erstinstallation per USB (neues Display)

Dafür braucht es einen PC mit Python und das Werkzeug **esptool**:

```
pip install esptool
```

1. Display per USB-C an den PC anschließen.
2. Den COM-Port herausfinden (Windows: Geräte-Manager → Anschlüsse, z. B. `COM5`).
3. Passende Datei `…_komplett.bin` aufspielen:
   ```
   esptool --chip esp32s3 --port COM5 write-flash 0x0 SR-Display_1.0.0_AMOLED143_komplett.bin
   ```
   Startet der Vorgang nicht: **BOOT**-Taste gedrückt halten, kurz **RESET** drücken, BOOT loslassen und erneut versuchen.
4. Danach startet das Display mit dem Hotspot **„SR HotSpot 192.168.4.1“** – weiter im [Benutzerhandbuch, Erste Inbetriebnahme](Benutzerhandbuch.md#erste-inbetriebnahme).

## Hinweise

- Nutzung auf eigene Gefahr. Das Display liest die Daten des Antriebs nur aus und greift nicht in die Steuerung ein.
- Fehler oder Wünsche bitte als [Issue](../../issues) melden. Hilfreich sind dabei der Text unter **System → Protokoll** und **System → Neustarts und Abstürze**.
- Der Quellcode ist nicht öffentlich.
