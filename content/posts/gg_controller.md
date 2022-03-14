---
title: "GeigerCounter: Der Controller"
date: 2021-03-22T20:20:16+01:00
draft: false
---

## GeigerCounter: Der Controller

Das Herzstück meines Projektes stellt der Controller dar. Hier erfährst du alles über den Prozess zum fertigen Controller, welche Probleme es dabei gab und wie der Controller implementiert wurde.

### Die Grundlagen

Im vorherigen Kapitel hatte ich schon erwähnt, dass ich mit dem ESP32 von MakerHawk arbeiten möchte. Die Vorteile:

- BLE (Bluetooth Low Energy) integriert
- WLAN integeriert
- Display integriert
- genügend PINS

<img src="/swh-19-20-marcoc/img/geiger_counter/esp32.jpg" />

Zudem ist nach vielen Wochen des Wartens, das Geiger-Zähler-Kit gekommen. Nach einigen Test war klar: das System funktioniert und kann integriert werden.

#### Implementation

Das Geiger-Zähler-Kit benötigt eigentlich nur 3 Anschlüsse: 5V, GND und ein PIN um einen erkannten Zerfall zu übermitteln. 5V und GND waren einfach anschzuschließen. Der Signal-PIN machte allerdings einige Probleme: auf das Signal kann nicht einfach in einer Loop gehorcht werden. Es musste mittels Interrupt darauf gewartet werden:

**MainHandler.cpp**

{{< highlight cpp >}}

void setup() {
    attachInterrupt(digitalPinToInterrupt(IMP_PIN), impulse, FALLING);
}

void impulse() {
    signalHandler->impulse(100);
    unsigned long time = millis();
    detections.add(time);
    if(detections.size() > 999) {
        detections.shift();
    }
    bluetoothServer->decay_impulse(time);
}

{{< /highlight >}}


Bei jedem Zerfall wird die Funktion *impulse* aufgerufen. In dieser Implementation wird danach sofort das Signal initiert (LED blinkt und Buzzer wird ausgelöst; für 100 ms). Zudem wird der aktuelle Timestamp in einer Liste gespeichert (mit maximal 999 Einträgen; aus diesen Werten werden dann später die CPM (Counts per Minute) und die Microsievert pro Stunde berechnet.). Zudem wird per Bluetooth direkt ein Notify abgegeben. Dadurch erfährt die App (siehe nächsten Beitrag) von einem Zerfall und kann diesen ausgeben und anzeigen.

Info: die ersten Implementation hat nur den Timestamp gespeichert. 

Im Loop werden dann aus diesen Timestamps der aktuelle Wert für die CPM und Microsievert berechnet (siehe **Calculator.cpp**).

<img src="/swh-19-20-marcoc/img/geiger_counter/zwischenstand.JPG" />

*Der vollständige Code befindet sich auf [Github](https://github.com/Geiger-Counter/controller)*

Zudem wurde der BLEServer mit allen Characteristics und Services implementiert. Auch die Signalausgabe, LED anzeige, Displayanzeige und Callbackhandler wurden implementiert.
Um die vollständige Implementation zu erklären, ist hier leider nicht genügend Platz.

#### Platinen & Case

Um die Buttons, die LEDs und den Buzzer anzubringen, habe ich mich entschlossen eigene Platinen zu entwerfen. Mittels günstiger Produktion in China, waren diese auch nach nur 6 Wochen bei mir:

**Button-Patine**

*eingebaut, aber nicht verwendet (siehe Probleme)*

<img src="/swh-19-20-marcoc/img/geiger_counter/button_platine.JPG" />

Die Platine hat 4 Anschlüsse: 5V, GND, SIGNAL BTN 1 & SIGNAL BTN 2

**Signal-Patine**

Die Signal-Platine gibt den aktuellen Status aus. Eine RGB-LED (mit 3 Anschlüssen) gibt die Höhe der Radioaktivität an (Grün = OK, Geld = gefährlich, Rot = tötlich). Zudem gibt es einen Anschluss für den Buzzer und die Signal LED. Beide werden bei einem erkannten Zerfall aktiviert.

<img src="/swh-19-20-marcoc/img/geiger_counter/signal_platine.JPG" />

Die Platine hat 6 Anschlüsse: 5V, GND, SIGNAL (BUZZER & LED), LED (R, G, B).

Die Gerber-Files für beide Platinen befinden sich auch im GitHub-Repository.

**3D-Case**

<img src="/swh-19-20-marcoc/img/geiger_counter/running_case.JPG" />

Um das Ganze auch in einem Case unterzubringen, habe ich in Fusion 360 ein TopCase erstellt (nach vielen Fehlversuchen). Als Boden dient eine alte Handyhülle, welche zu meinem Smartphone passt. In den TopCase wurden Öffnungen für die beiden Platinen (zwei Buttons & zwei LEDs) erstellt. Der Buzzer wurde nach innen gerichtet.

Zudem gibt es eine Öffnung für das Zählrohr (um auch Alpha- und Beta-Strahlung messen zu können) und für den Display des ESP.

<img src="/swh-19-20-marcoc/img/geiger_counter/buttons.JPG" />

<img src="/swh-19-20-marcoc/img/geiger_counter/buttons_2.JPG" />

<img src="/swh-19-20-marcoc/img/geiger_counter/open_case.JPG" />

### Der fertige Controller

Der Controller gibt über den Bildschirm die aktuellen Strahlenwerte aus. Zudem gibt eine LED die Stärke der Strahlung an. Eine weitere LED und ein interner Buzzer geben bei jedem Zerfall ein kurzes Signal ab.

**Video**

<video src="/swh-19-20-marcoc/img/geiger_counter/controller.MOV" width="500px" controls>

#### Probleme

<img src="/swh-19-20-marcoc/img/geiger_counter/last_draft.JPG" />

Doch es gab auch einige Probleme und Irrwege:

**BLE**

Der gesamte Komplex BLE ist ein Problem an sich. Die Dokumentation für ESP ist nicht besonders gut, verschiedene Versionen unterscheiden sich in der Methodik und die schlechte Fehlerausgabe des ESP gibt seinen Teil dazu. Nach Stunden an Core Failures und Abstürzen kann ich sagen: der ESP ist nicht für BLE geeignet. Zumindest nicht so wie ich es nutzen möchte. Daher musste das Feature: per Button den BLE Server an/auszuschalten und per App Einstellungen am Controller vorzunehmen leider aus dem Projekt fliegen. Manchmal lief es, um dann nur wenige Minuten später durchgehend neuzustarten.

**Button Interrupts**

Das Interrupt Handling des ESP hat mir auch weitere schlaflose Nächte bereitet. Meistens hat der Button Interrupt auch vollkommen normal funktioniert. Manchmal hat er aber den ganzen ESP lahm gelegt. Ohne erkennbaren Grund. Aufgrund dieser Probleme und des vorher geschilderten (BLE) habe ich in der Endfassung auf Buttons ganz verzichtet. In den Schalt-Boards und in den Commits ist die Implementation aber noch zu finden. Falls jemand Lust hat das irgendwann mal richtig zu implementieren.

