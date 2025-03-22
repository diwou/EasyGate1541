# EasyGate1541

![](https://codeberg.org/diwou/EasyGate1541/raw/branch/main/Images/EasyGate1541.jpg)

**EasyGate1541** ist ein Ersatz für den Gate-Array (MOS 325572-01) des Commodore-Diskettenlaufwerks VC1541. 
Er ersetzt den Chip vollständig im Sockel **UC1**.

Ich habe die Funktion des Gate-Arrays in ein **CPLD von Xilinx (XC9572XL)** übertragen. Zusätzlich habe ich **LEDs** hinzugefügt – diese sind eher als kleines Gimmick zu verstehen, da noch freie Pins zur Verfügung standen.

---

## Aufbau

Die Platine ist im Aufbau recht einfach. Sie besteht im Wesentlichen nur aus:
- dem CPLD,
- einem Spannungswandler von 5V auf 3,3V,
- LEDs mit Vorwiderständen,
- sowie Abblockkondensatoren.

Das Ganze hat die Abmessungen eines **40-poligen DIP-ICs**. 
Da alle Bauteile auf der Unterseite verlötet wurden, bleibt die Oberseite "clean".

![](https://codeberg.org/diwou/EasyGate1541/raw/branch/main/Images/EasyGate1541_Top.jpg)

Die LEDs wurden auf der Unterseite montiert. Es gibt entsprechende Durchkontaktierungen, sodass die LEDs von oben sichtbar sind.

Die **Power-LED** ist direkt mit dem Spannungswandler verbunden. Sie zeigt an, ob der Spannungswandler funktioniert.

Die **Clock-LED** ist mit dem Takteingang verbunden und wird entsprechend verlangsamt, sodass sie blinkt. Falls diese LED nicht blinkt, fehlt wahrscheinlich der Takt vom IC UF6.

Zusätzlich gibt es eine **Motor-LED**, die anzeigt, wenn der Motor läuft oder laufen sollte (Signal kommt vom VIA 6522).

Und schließlich gibt es eine **Write-LED**, die leuchtet, wenn ein Schreibvorgang stattfindet.

Wer diese LEDs nicht möchte, kann sie einfach weglassen. Sie sind für den Betrieb nicht erforderlich.

![](https://codeberg.org/diwou/EasyGate1541/raw/branch/main/Images/EasyGate1541_Bottom.jpg)

Welche LED-Farbe wo verwendet wird, bleibt dem persönlichen Geschmack überlassen. Als "Serviervorschlag" gibt es folgende Grafik:

![](https://codeberg.org/diwou/EasyGate1541/raw/branch/main/Images/EasyGate1541_LEDs.jpg)

---

## Firmware flashen

Die Firmware ist als **SVF-, XSF- und JED-Datei** verfügbar und kann mit einem JTAG-Programmer eingespielt werden. Ein einfacher SVF-Player reicht dafür aus. 
Das JTAG-Interface wurde auf freie, ungenutzte Pins gelegt, die auf der 1541-Platine nicht verbunden sind.

Am einfachsten ist es, das Ganze auf einem Breadboard aufzubauen und wie folgt zu verdrahten:

| Pin | Funktion   |
|-----|------------|
| 1   | +5V        |
| 7   | TDI        |
| 9   | TMS        |
| 11  | TCK        |
| 13  | VREF (3.3V)|
| 20  | GND        |
| 33  | TDO        |

Die 5V werden über ein Netzteil eingespeist. Dann das JTAG-Interface gemäß der obigen Zuordnung verbinden und die SVF-Datei flashen – fertig.

Danach kann der Ersatz-Chip in das Laufwerk gesteckt werden.

Zur besseren Übersicht hier eine Abbildung zur Verdrahtung auf dem Breadboard:

![](https://codeberg.org/diwou/EasyGate1541/raw/branch/main/Images/EasyGate1541_JTAG.jpg)

Die einfachste und günstigste Methode zum Flashen der Firmware per JTAG ist ein **FT232H-Modul**. Dieses Modul gibt es z. B. bei AliExpress für ca. 7 $. 
Mit diesem Modul und der Software **xc3sprog** lässt sich die JED-Datei sehr einfach flashen.

Ich empfehle folgende Anleitung: [https://github.com/1c3d1v3r/neatPLA/tree/master/programming](https://github.com/1c3d1v3r/neatPLA/tree/master/programming)

Dort finden sich auch die Links zur xc3sprog-Software und zur Flash-Prozedur. Die Anleitung ist auch für das EasyGate1541-Projekt geeignet. Lediglich die Pinbelegung des FT232H ist anders, und natürlich muss die JED-Datei aus diesem Repository verwendet werden.

---

## Stückliste (BOM)

Bis auf das CPLD können auch Vergleichstypen verwendet werden.

| Nr. | Bezeichner | Bauteil                         | Beschreibung                                 |
|-----|------------|----------------------------------|----------------------------------------------|
| 1   | U1         | MaxLinear SPX5205M5-L-3-3/TR    | Spannungsregler, SOT-23-5                    |
| 2   | U2         | Xilinx XC9572XL-10VQG44C        | CPLD, VQFP-44                                 |
| 3   | D1         | Everlight 25-21/GHC-YSU/2A      | LED grün, 1206                               |
| 4   | D2         | Everlight 25-21/BHC-XR2S1/2A    | LED blau, 1206                               |
| 5   | D3         | Everlight 25-21/T1D-ANQHY/2A    | LED weiß, 1206                               |
| 6   | D4         | Everlight 25-21SURC/S530-A4/TR8 | LED rot, 1206                                |
| 7   | C1–C3      | Kemet C0603C104J4RACTU           | Keramikkondensator, 100nF, 16V, X7R, 0603     |
| 8   | C4, C5     | Kemet C0805C106J8RACAUTO        | Keramikkondensator, 10µF, 10V, X7R, 0805     |
| 9   | R1–R4      | Yageo SR0603FR-7T1KL             | Widerstand, 1kΩ, 1%, 1/4W, 0603              |
| 10  | CN1, CN2   | Mill-Max 800-10-020-10-002000   | Stiftleiste, 20-pol., Präzision, Raster 2.54mm |

---

## Lizenz

Der Inhalt dieses Repositories steht unter folgender Lizenz:

**Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)**  
Vollständiger Lizenztext: [LICENSE.CC-NC-BY-SA-4.0](https://codeberg.org/diwou/EasyGate1541/raw/branch/main/LICENSE.CC-NC-BY-SA)  
Online unter: [https://creativecommons.org/licenses/by-nc-sa/4.0/](https://creativecommons.org/licenses/by-nc-sa/4.0/)
