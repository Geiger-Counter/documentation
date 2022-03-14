---
title: "GeigerCounter: Die App"
date: 2021-03-22T20:20:23+01:00
draft: false
---

## GeigerCounter: Die App

<img src="/swh-19-20-marcoc/img/geiger_counter/app/info.PNG" />

Um nun die Radioaktivität sichtbar zu machen, brauchen wir noch eine App, welche mit dem zuvor erstellten Controller spricht und die übermittelten Daten visualisiert.

### Die Grundlagen

Da ich selbst ein iPhone besitze, war es von Anfang an klar, dass ich eine App für iOS programmieren möchte. Jedoch hatte ich das noch nie getan. Da ich Erfahrungen mit React hatte, entschloss ich mich die App mit React Native zu implemnetieren.

Da die Kommunikation per BLE und Augmented Reality leider nicht sehr gut von React Native unterstütz werden, habe ich das ganze Projekt nach einigen Wochen wieder umgekrempelt und habe die Implementation mit Swift, SwiftUI und ARKit durchgeführt.

**Was soll die App machen?**

Die App soll alle BLE-Geräte in der Umgebung erkennen. Sollte ein aktiver GeigerCounter-Controller darunter sein, soll eine Verbindung mit diesem hergestellt werden können. Nach der Verbindung soll die App die aktuellen CPM und Microsievert anzeigen. Zudem soll in Echtzeit ein Signal für einen Zerfall angezeigt werden und die aktuelle Strahlenbelastung.

Vervollständigend, gibt es die Option per AR die Radioaktivität sichtbar zu machen.

### Implementation

Zur Implementation lässt sich sehr viel und auch sehr wenig sagen. Den gesamten Quellcode der App gibt es auf [GitHub](https://github.com/Geiger-Counter/ios). Probleme gab es im Besondere mit dem mir unbekannten Swift. Hier werden doch einige Dinge sehr anders gehandhabt, als in anderen Programmiersprachen. Nach einigen Wochen Eingewöhnungszeit, klappe es aber relativ gut.

<img src="/swh-19-20-marcoc/img/geiger_counter/app/search.PNG" />

Die App sucht zu Beginn nach BLE-Geräten in der Umgebung. Wenn ein Gerät gefunden wird, wird überprüft, ob die gewünschten Services angeboten werden. Sollte dem so sein, kann eine Verbindung mit dem Gerät hergestellt werden.

<img src="/swh-19-20-marcoc/img/geiger_counter/app/select.PNG" />

Nachdem die Verbindung geglückt ist, gelangt der Nutzer in ein neues Menü. Dort werden die aktuellen Zahlen und Daten (in Echtzeit) des Controllers angegeben. Zudem wird über die aktuelle Strahlendosis inforiert. Bei einem registrierten Zerfall, leuchtet ein kleiner roter Punkt auf.

<img src="/swh-19-20-marcoc/img/geiger_counter/app/info_with_ping.PNG" />

Oben rechts gibt es die Möglichkeit in den "Augmented Reality"-Modus zu wechseln. Hier wird die Kamera aktiviert und auf Basis der aktuellen Strahlenbelastung die Radioaktivität als Partikel simuliert. Die Partikel bestehen hier aus dem Warnsymbol für Radioaktivität.

<img src="/swh-19-20-marcoc/img/geiger_counter/app/ar.PNG" />

### Probleme

Besonders das Handling der BLE-Verbindung sowie die Lernkurve für das AR-Kit waren große Herausforderungen. Erschwerend kam auch die unbekannte Programmiersprache hinzu. Allerdings muss ich auch sagen: mit Swift lies es sich einfache programmieren als mit C++ im ESP. Das lag allerdings vor allem an den um Welten besseren Fehlermeldungen.

### Die fertige App

Die App läuft sehr stabil und alles funktioniert relativ gut. Ein kleines Problem lies sich aber am Ende nicht mehr fixen: in der "AR"-Ansicht, hat die Echtzeit-Übertragung leider am Ende etwas Probleme gemacht.

**Video**

<video src="/swh-19-20-marcoc/img/geiger_counter/app/app_running.mp4" width="500px" controls>
