---
title: "3d Printing"
date: 2020-02-11T21:44:52+01:00
draft: false
---

## 3D-Druck

### Aufgabe

Es sollte für den eigenen Schreibtisch zuhause eine Kabelhalterung entworfen und gedruckt werden. Ziel war es zuerst korrekt Maß zu nehmen und dann die realen Objekte in Fusion360 als 3D-Objekte zu gestalten.

### Idee/Sketch

Meine Idee war es mein iPhone-Ladekabel als Vorlage zu nutzen. Leider kannte ich die Maße meines Schreibtisches daheim nicht und habe daher die Maße des Tisches vor Ort genutzt.

Den Halter sollte man an die Tischplatte klemmen können (mit zwei Öffnungen für Schrauben am Boden, um die Halterung noch fester zu befestigen). Auf der Oberseite sollten zwei Ladekabel angebracht werden können.

Zuerst wurde der Tisch und das Kabel ausgemeßen. Die Daten nutze ich um auf Papier einen ersten "Sketch" zu erstellen:

Die Seitenansicht auf die Halterung (am unteren Ende sind die Bohrlöcher für die Befestigungsschrauben zu sehen):
<img src="/swh-19-20-marcoc/img/3dprinting/drawing_1.JPG" />

Der Blick von "oben" auf die Kabelhalterungen:
<img src="/swh-19-20-marcoc/img/3dprinting/drawing_2.JPG" />

Die Maße des Kabelkopfes, welcher in die Halterung eingeklemmt werden sollte:
<img src="/swh-19-20-marcoc/img/3dprinting/drawing_3.JPG" />

Der Blick von "vorne" auf die gesamte Halterung:
<img src="/swh-19-20-marcoc/img/3dprinting/drawing_4.JPG" />

### Umsetzung in Fusion360

Nachdem die Maße genommen wurden, war es an der Zeit ein 3D-Objekt in Fusion360 zu erstellen. Die Installation von Fusion360 lief ohne Probleme ab. Das Gestalten der Form kostete aber einiges an Zeit und auch an Nerven. Besonders die "Bohrlöcher" für die Schrauben und das Ausschneiden der Form für die Kabelhalterung machten mir Probleme. 

Doch nach einiger Zeit und einigen Nerven war das erste 3D-Modell fertiggestellt:

<img src="/swh-19-20-marcoc/img/3dprinting/sketch_1.png" />

<img src="/swh-19-20-marcoc/img/3dprinting/sketch_1_ports.png" />

### Slicen

Um zu überprüfen ob das Objekt in den vorgegebenen 15 Minuten gedruckt werden kann, musste es zuerst mit Cura gesliced werden. Dazu wurde in Fusion360 das Projekt als ```.stl```Datei gespeichert und in Cura importiert (Version 1: <a href="/swh-19-20-marcoc/img/3dprinting/kabelhalterung_v1.stl">Kabelhalterung_v1.stl</a>).

Das erste Slicen war dann sehr ernüchternd. Der Druckprozess würde über 50 Minuten dauern. Viel zu lange. Also musste an den Druckeinstellungen gespielt werden (Schnelligkeit erhöhen, Fill verringern, etc.). Doch selbst die extremsten Einstellungen brachten die Druckzeit nicht unter 40 Minuten.

Also musste am 3D-Objekt etwas entfernt werden. Meine Idee war die Halterung mit den Bohrlöchern zu verkleinern und schmaler zu machen (Version 2: <a href="/swh-19-20-marcoc/img/3dprinting/kabelhalterung_v2.stl">Kabelhalterung_v2.stl</a>). Wieder wurde das Projekt exportiert und in Cura importiert. Doch auch jetzt war die Druckzeit noch bei über 25 Minuten. Es musste also noch mehr abgenommen werden.

So ging es noch einmal zurück zu Fusion360. Nun wurden noch die Ecken abgerundet und die Wände etwas schmaler (Version 3: <a href="/swh-19-20-marcoc/img/3dprinting/kabelhalterung_v3.stl">Kabelhalterung_v3.stl</a>). 

<img src="/swh-19-20-marcoc/img/3dprinting/sketch_2.png" />

Wieder exportiert und in Cura importiert, waren wir zwar immer noch nicht bei 15 Minuten, aber mit 20 Minuten konnten wir leben. Es konnte also ans Drucken gehen.

<img src="/swh-19-20-marcoc/img/3dprinting/sketch_in_cura.png" />

Dazu erstellte Cura aus dem vorgegebenen Objekt und den Druckeinstellungen eine GCODE-Datei (<a href="/swh-19-20-marcoc/img/3dprinting/UMO_Kabelhalterung.gcode">UMO_Kabelhalterung.gcode</a>).

Die ```.gcode``` Datei wurde auf eine SD-Karte übertragen und damit in den 3D-Drucker eingespielt.

### Drucken

Am Drucker wurde die Datei ausgewählt und der Druckprozess gestartet. Beim ersten Versuch verrutschte die Druckfläche jedoch und der Prozess musste gestoppt werden. Beim zweiten Versuch ging aber alles gut und die Form wurde gedruckt:

<img src="/swh-19-20-marcoc/img/3dprinting/printing_1.JPG" />

<img src="/swh-19-20-marcoc/img/3dprinting/printing_2.JPG" />

### Verbesserungen

Nach dem Drucken stellte sich heraus, dass die extremen Einstellungen in Cura nicht ohne Folgen blieben. Das Objekt war sehr unstabil. Auch war die Oberfläche nicht besonders schön:

<img src="/swh-19-20-marcoc/img/3dprinting/finished_1.JPG" />

<img src="/swh-19-20-marcoc/img/3dprinting/finished_2.JPG" />

<img src="/swh-19-20-marcoc/img/3dprinting/finished_3.JPG" />

Doch das Kabel konnte (mit etwas sanfter Gewalt) in die Halterung gesteckt werden:

<img src="/swh-19-20-marcoc/img/3dprinting/finished_with_cable.JPG" />

Mit einem 3D-Druck-Stift versuchte ich die Konstruktion noch etwas stabiler zu machen. Das gelange auch (aber nur in sehr begrenztem Umfang). Zudem fügte ich, um die Optik ein wenig zu verschönern, noch ein Smiley hinzu:

<img src="/swh-19-20-marcoc/img/3dprinting/finished_with_smile_and_cable.JPG" />