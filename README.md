# Fax-Dekodierung aus G.711 RTP Streams (Konzept)

Dieses Bash-Skript extrahiert aus einem `.pcap`-File mit einem G.711 kodierten Fax-RTP-Stream automatisch den Faxinhalt und wandelt ihn in ein TIFF (und ein PNG-Bild) um

---

## Funktionsweise

1. **Analyse des PCAP-Files:**  
   Das Skript erkennt UDP-Streams im `.pcap` und überprüft, ob genau ein oder zwei mögliche RTP-Streams vorhanden sind. Bei mehr als zwei Streams bricht es ab, um Fehlinterpretationen zu vermeiden. Bei zwei Streams (Hin- und Rückrichtung) wird der Stream mit den meisten Paketen verwendet

2. **Codec-Erkennung:**
   Automatische Erkennung, ob der G.711-Stream µ-law (Payload Type 0) oder A-law (Payload Type 8) ist

3. **Extraktion & Konvertierung:**
   - RTP-G.711 Payload wird extrahiert und in eine rohe Audiodatei geschrieben
   - Diese wird mit `sox` in eine WAV-Datei umgewandelt
   - `rxfax` dekodiert aus der WAV-Datei das Fax in eine TIFF-Datei
   - Optional wird das TIFF mit ImageMagick (`convert`) in ein PNG-Bild konvertiert

---

## Voraussetzungen

Das Skript benötigt folgende Programme, die installiert sein müssen:

| Programm  | Paketname (Debian/Raspbian) | Zweck                               |
|-----------|-----------------------------|-------------------------------------|
| `tshark`  | `tshark`                    | RTP-Stream Analyse                  |
| `sox`     | `sox`                       | Audio-Konvertierung                 |
| `rxfax`   | `spandsp-utils`             | Fax-Dekodierung                     |
| `convert` | `imagemagick`               | TIFF → PNG Konvertierung [optional] |
| `xxd`     | `vim-common`                | Hexdump in Binärdatei umwandeln     |

---

## Installation der Abhängigkeiten

```bash
sudo apt update
sudo apt install tshark sox spandsp-utils imagemagick vim-common
