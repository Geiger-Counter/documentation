---
title: "GeigerCounter: Zusammenfassung & Nachbauen"
date: 2021-03-23T20:20:30+01:00
draft: false
---

## GeigerCounter: Zusammenfassung & Nachbauen

### Fazit

Das Projekt hat mir sehr viel Spaß gemacht. Aber auch sehr, sehr viele Nerven gekostet. Ich habe unglaublich viel dazugelern. Von Themen, welche ich sonst eher nicht angegangen wäre. Sei es die vielfältigen Möglichkeiten, welche die Mikrocontroller bieten, der Einblick in "hardware-nähere" Programmierung mit C++, die Hochs- und Tiefs mit BLE oder die Entdeckung einer neuen Programmiersprache mit Swift. 

Sollte es mir meine Freizeit erlauben, möchte ich an diesem Projekt, aber auch an anderen gerne weiterer arbeiten. Dann aber ohne den Druck einer kommenden Abgabe ;)

Das Fach "Sketching with Hardware" kann ich also nur empfehlen. Ganz vielen neue Einblicke. Viel gelernt. Tolle Glücksmomente, wenn etwas nach tagelanger Arbeit so funktioniert wie gewünscht. Aber auch harte Frustration über die Problem und Bugs, welche so mit sich kamen.

### HowTo: GeigerCounter

Du willst auch unbedingt mal deinen eigenen GeigerCounter basteln um damit Strahlung sichtbar zu machen? Kein Problem: dieses kurze HowTo zeigt dir in zwei Schrittend alles was du wissen musst.

#### Schritt 1: Materialien

Du brauchst ein paar Grundmaterialien um das Projekt (zumindest so wie ich) nachzubauen:

- Geiger-Zähler-Kit (gibt es für rund 25$ bei Ebay): https://www.ebay.de/itm/Assembled-DIY-Geiger-Counter-Kit-Module-Nuclear-Radiation-Detector-M3X5/274586907280?hash=item3feea7b290:g:c1gAAOSwGypfu1vo
- ESP32 inkl. Bluetooth, WLAN und Display (für ca. 15€ bei Amazon): https://www.amazon.de/gp/product/B076P8GRWV/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
- Kabel, RGB-LED, rote LED, Buzzer, Löteisen, Heißklebepistole, 3D-Drucker, alte Handyhülle.

Die Bestellungen können dauern. Daher am Anfang auch direkt die Platinen bestellen. So ist alles rechtzeitig da.

Die Gerber-Files für die Platinen findest du hier ([GitHub](https://github.com/Geiger-Counter/controller/tree/master/files)). Ich hab meine für ca. 15$ bei https://jlcpcb.com/ bestellt.

Die .slt-Datei für das Case findest du hier: [GitHub](https://github.com/Geiger-Counter/controller/tree/master/files).

Wenn du alles hast, kann es schon an den Zusammenbau gehen.

#### Schritt 2: Zusammenbau

An die Platinen lötest du die benötigten Teile an (LEDs, Buzzer, etc.). Zudem auch gleich passende Kabel. Diese Kabel direkt auch an den ESP anlöten (entsprechende PINs findest du im [ControllerRepo auf GitHub](https://github.com/Geiger-Counter/controller) unter *sketch.ino*). Die Platinen und den ESP kannst du nun mittels Heißkleber im Gehäuse anbringen.

Auch die 3 PINs am Geiger-Zähler-Kit (5V, GND und VIN) mit Kabeln am ESP anlöten. 

<img src="/swh-19-20-marcoc/img/geiger_counter/open_case.JPG" />

Nun die Software für den ESP von GitHub herunterladen ([GeigerCounter Controller](https://github.com/Geiger-Counter/controller)) mittels ArduinoIDE oder VSCode-Plugin aufspielen.

Wenn alles richtig angeschlossen wurde, sollte der Controller nun schon laufen. 

Um die App auszuprobieren, auch diese einfach von GitHub herunterladen ([GeigerCounter App](https://github.com/Geiger-Counter/ios)) und per XCode auf dein iPhone aufspielen.

Die App sollte nun deinen GeigerCounter erkennen und sich mit ihm verbinden können. 

<img src="/swh-19-20-marcoc/img/geiger_counter/app/select.PNG" />

<img src="/swh-19-20-marcoc/img/geiger_counter/app/info_with_ping.PNG" />

Bei Problemen gerne per Issue in GitHub melden.