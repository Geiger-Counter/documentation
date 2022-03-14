---
title: "GeigerCounter: Von der Idee zum Plan"
date: 2021-03-22T19:21:41+01:00
draft: false
---

## GeigerCounter: Von der Idee zum Plan

### Die Idee

Ziel des zweiten Halbjahres der Vorlesung "Sketching with Hardware" sollte die Gestaltung, Produktion und Implementation unseres eigenen Projektes werden. Zu Beginn war natürlich die Frage nach einer Idee drängend. Zum Glück hatte ich eines Nachts einen Traum: ich konnte Radioaktivität sehen. 

Die Idee hat mich so fasziniert, dass ich sie sofort eingebracht habe. Und glücklicherweise wurde mir auch gestatten, diese Idee umzusetzen. Doch wie genau soll das gelingen, und wie sieht der grobe Plan aus?

### Das Konzept

Ich möchte einen Geiger-Zähler an mein Smartphone anschließen, um dann mittels Bluetooth die Messwerte auf eben jenes zu übertragen. In einer App sollen die Messwerte dann in Echtzeit dargestellt werden. Zudem soll es möglich sein mittels Augmented Reality die Radioaktivität in der Umgebung anzuzeigen.

### Die Recherche

#### Radioaktivität & Geiger-Zähler

Zu aller Erst musste ich mein Wissen über Radioaktivität auffrischen. Radioaktivität kann mitteils eines Geiger-Zählers gemessen werden (meist in Mirkosievert pro Stunde). Das Problem: der Geiger-Zähler kann nur angeben, welche Strahlung in der Umgebung herscht. Allerdings nicht, aus welcher Richtung die Strahlung kommt.

Gleich zu Beginn direkt dieser Rückschlag. Doch ich wollte mein Projekt nicht aufgeben. Das Problem beschränkt sich auch Gamma-Strahlung. Durch eine geeignete Öffnung in meinen Gerät kann die Alpha- und Beta-Strahlung einigermaßen richtig bestimmt werden. Auch die Gamma-Strahlung sollte gemessen und angegeben werden (wobei Mikrosievert keinen Unterschied bei Alpha-, Beta- oder Gammastrahlung aufzeigen), jedoch kann dich Richtung aus welcher diese Strahlung kommt, nicht angegeben werden.

#### Materialien

Einen Geiger-Zähler selbst zu bauen fiel schnell aus der Planung. Zum einen ist die Konstruktion nichts für Anfänger. Zum anderen werden realtiv hohe Spannungen erzeugt, welche ich als Amateur nicht unbedingt handeln möchte.

Die Recherche ging also weiter: ich brauchte einen Geiger-Zähler, der relativ klein ist um auch gut verbaut zu werden. Zudem sollte er einigermaßen einfach an einen Arduino oder ESP angeschlossen werden. 

Nach einer längeren Suche fiel mein Blick auf ein Set auf Ebay ([ähnlicher Artikel](https://www.ebay.de/itm/Assembled-DIY-Geiger-Counter-Kit-Module-Nuclear-Radiation-Detector-M3X5/274586907280?hash=item3feea7b290:g:c1gAAOSwGypfu1vo)). Das Kit war bereits fertig zusammengebaut (siehe Bedenken zum eigenen Zusammenbau weiter oben), brauchte bei Versand aus China jedoch zwei Monate.

Zudem wollte ich einen ESP32 nutzen, da nur bei diesem Controller die Bluetooth-Funktion bereits integriert war. Um auch Informationen anzeigen zu können, sollte auch ein kleiner Display integriert sein. Meine Wahl fiehl auf den [MakerHawk ESP32](https://www.amazon.de/gp/product/B076P8GRWV/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1).


<img src="/swh-19-20-marcoc/img/geiger_counter/esp32.jpg" />


Alle weitere Materialen (Kabel, LEDs, Breadboard, etc.) bezog ich aus meinem eigenen Arduino-Starter-Kit.

### Der Plan

Die Materialen waren beschafft (oder kamen in wenigen Wochen) und die Recherche abgeschlossen. Nun war es an der Zeit einen genaueren Plan aufzustellen:

**GeigerCounter Controller**

- ESP32 mit WiFi, BLE und Display
- Geiger-Zähler-Kit
- Status-LED (GRÜN/GELB/ROT für aktuelle Strahlung)
- Buzzer (Signal bei Zerfall im Zählrohr)
- rote LED (Signal bei Zerfall im Zählrohr)
- 3D-gedruckted Case
- Case per Handyhülle an Smartphone anbringbar

- Button für BLE (*optional*)
- Button für Signal (*optional*)

**GeigerCounter App**
*App für iOS*

- Suchfunktion für BLE GeigerCounter
- Echtzeitanzeige von Messwerten
- Signal bei Zerfall im Zählrohr
- Einordnung der Readioaktivität
- Radioaktivität sichtbar per Augmented Reality (AR)

- Einstellungen für die Buttons am Controller (*optional*)

