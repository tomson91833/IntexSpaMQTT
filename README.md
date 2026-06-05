# IntexSpaMQTT – Intex PureSpa lokal in IP-Symcon

![Version](https://img.shields.io/badge/Version-2.7.1-blue) ![IP-Symcon](https://img.shields.io/badge/IP--Symcon-7.0%2B-orange) ![License](https://img.shields.io/badge/License-MIT-green)

Lokale Steuerung eines **Intex PureSpa** (ab Baujahr 2024, Tuya, App „INTEX Link")
in **IP-Symcon** – **ohne Cloud-Zwang**. Eine kleine MQTT-Brücke (Python/tinytuya)
spricht den Spa lokal an, IP-Symcon übernimmt Steuerung, Visualisierung, Zeitplan
und die Anbindung an den **Energie Manager** (Heizen bei PV-Überschuss).

> 🤖 **Hinweis:** Code-Begleitung und die ausführlichen Anleitungen wurden
> **mit Hilfe von KI (Claude)** erstellt und sorgfältig getestet.

---

## ✨ Funktionen

- 🔌 **Lokale Steuerung** ohne Cloud: Power, Heizung, Filter, Sprudler, Jets, Hygienisierung
- 🌡️ **Ist-/Soll-Temperatur** lesen und setzen (intern °F ↔ °C)
- 🟦 **Kachel** mit Steuerung **und** Zeitplan (Wochenplan)
- ☀️ **Energie-Manager-Anbindung:** eine Schaltvariable „PV-Heizung", die der
  offizielle IP-Symcon-Energie-Manager schaltet – das Modul erledigt die richtige
  **Reihenfolge** (erst Gerät an, dann Heizung; beim Aus umgekehrt)
- ✋ **Manueller Vorrang:** Heizt du von Hand, darf die Automatik **nicht** abschalten
- 📲 **Eine** Push-Erinnerung, wenn du manuell heizt und die Automatik abschalten würde
- 🔁 **Robust gegen Tuya-Eigenheiten:** echte Geräte-Pushes werden sofort übernommen,
  die nachhinkende Vollabfrage hält kurz den geschalteten Wert (keine springende Kachel)

---

## 🧩 So funktioniert es

```
Intex PureSpa  ──(Tuya, lokal, v3.5)──►  bridge.py  ──(MQTT)──►  IP-Symcon (MQTT-Server)
                                          tinytuya                 + Modul IntexSpaMQTT
```

Die Brücke läuft als kleiner Docker-Container; IP-Symcon ist selbst der MQTT-Broker.

---

## ✅ Voraussetzungen

- Intex PureSpa **ab 2024** (Typcode endet auf **„TY"**), im WLAN mit fester IP
- **Local Key** & **Device-ID** des Spa (einmalig via Tuya-IoT + `tinytuya` auslesen)
- **IP-Symcon 7.0+** mit Modul **„MQTT Server"**
- Ein Ort für die Brücke (z. B. **Docker** auf einem NAS)

---

## 🚀 Installation (Kurzfassung)

1. In IP-Symcon: **Kern Instanzen → Modules → „+"** und diese Repo-URL eintragen:
   ```
   https://github.com/tomson91833/IPSymcon-IntexSpaMQTT
   ```
2. **MQTT Server**-Instanz anlegen (Server-Socket, Port 1883).
3. **Brücke** (`bridge.py`) mit deinen Spa-Daten füllen und als Container starten.
4. Instanz **„IntexSpaMQTT"** anlegen, mit dem MQTT-Server verbinden, Basis-Thema `intexspa`.

> 📘 **Die komplette, bebilderte Schritt-für-Schritt-Anleitung** (von der App über das
> Auslesen des Local Keys bis zum Energie-Manager und Push) liegt im Ordner
> [`anleitungen/`](anleitungen) bzw. als PDF im Release.

---

## ⚡ Energie-Manager & Push (Kurz)

- Im Energie Manager den **Verbraucher** auf die Variable **„PV-Heizung (Energie
  Manager)"** setzen, **Maximaler Verbrauch 2200 W**, Mindest-/Nachlaufzeit nach
  Wunsch, **keine Bedingung**.
- Push läuft über die **Kachel-Visualisierung** mit `VISU_PostNotification`
  (Skript `Spa_Push.php` beiliegend).

---

## 🩺 Fehlerbehebung

Siehe **`anleitungen/11_Fehlerbehebung_und_Wartung.md`**. Häufigste Punkte:
`MQTT_HOST` = Container-Name von IP-Symcon, gleiches Docker-Netz, `library.json`
im Repo-Wurzelverzeichnis, aktuelles Modul per **Modules → Aktualisieren** laden.

---

## ⚠️ Haftungsausschluss

Dieses Projekt ist ein **inoffizielles, privates Hobby-Projekt** und steht in
**keiner Verbindung** zu Intex oder Tuya. Namen/Marken gehören den jeweiligen
Eigentümern. Nutzung **auf eigene Gefahr** – prüfe sicherheitsrelevante Schritte
selbst. Die Anleitungen wurden **mit Hilfe von KI** erstellt.

## 📄 Lizenz

MIT – siehe [LICENSE](LICENSE).
