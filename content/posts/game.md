---
title: "ESP Game"
date: 2019-12-10T21:50:27+01:00
draft: false
---

## ESP32-Game

### Spielidee

Mithilfe des ESP32 sollte ein Sensor in einem Spiel verwendet werden. Nach einiger Zeit und dem Ausprobieren von verschiedenen Sensoren, entschieden wir uns den Ultraschall-Sensor zu benutzen. 

Um daraus ein Spiel zu konstruieren, würde man zwei "Controller" benötigen. An jedem dieser Controller sollte ein Ultraschall-Sensor angebracht sein. Der ESP gibt über seinen Display eine Distanz aus, welcher jeder Spieler bestmöglich erreichen soll. Eine beispielhafte Ausführung soll die Funktionsweise demonstrieren:

<strong>Ausführung:</strong>

<ul>
    <li>Das Spiel wird von einem "Master-Controller" gestartet. Dieser legt zufällig einen Distanzwert fest. (Bsp: 20 cm).</li>
    <li>Die "Slave-Controller" erhalten diesen Wert und das Spiel kann beginnen</li>
    <li>Alle Controller zeigen am Display die geforderte Distanz an</li>
    <li>Der Spieler muss nun seine Hand oder einen Gegenstand in die gewünschte Position vor dem Sensor bringen (Optimal natürlich 20 cm entfernt)</li>
    <li>Mit einem Klick auf einen Button am Controller wird die gemessene Distanz gespeichert und an den Master gesendet</li>
    <li>Der Master berechnet nun, welcher Controller am nächsten am gewünschten Wert ist</li>
    <li>Das Ergebnis wird an alle Controller mitgeteilt und es wird an allen Controllern angezeigt, ob gewonnen/verloren wurde und was die Differenz war</li>
</ul>

### Implementation

#### Game-State, Sensor und Button

Um die oben dargestellte Ausführung auch in der Software darstellen zu können, haben wir verschiedene States eingreichtet, in welchen sich das Spiel befinden kann. Diese wurden mittels ```Enum```realisiert. Zunächst mussten wir den Sensor und den Button mit dem ESP32 zum laufen bringen. Das ging relativ schnell und es gab keine großen Schwierigkeiten (Aufbau in Bild 1 dargestellt).

<img src="/swh-19-20-marcoc/img/esp/sensor_esp32.jpg" />

So konnten wir auch relativ schnell Distanzen mit dem Ultraschall-Sensor messen. Auch die Game-Logik war relativ schnell implementiert:

{{< highlight cpp >}}

long distance_mem = -1;
long local_distance;

enum State {
    WAITING = 0;
    RUNNING = 1;
    FINISHED = 2;
    START = 3;
} state;

void setup() {
    // ...
    distance_mem = -1;
    // ...
}

void loop() {
    switch(state) {
        case START: {
            int distance_mem = random(0,250);
            state = RUNNING;
            break;
        }
        case RUNNING: {
            measure_distance();
            break;
        }
        case FINISHED: {
            state = WAITING;
            break;
        }
    }
}

void measure_distance() {

  digitalWrite(TRIGGER_PIN, LOW);
  delay(5);
  digitalWrite(TRIGGER_PIN, HIGH);
  delay(10);
  digitalWrite(TRIGGER_PIN, LOW);

  long time = pulseIn(ECHO_PIN, HIGH);
  long distance = (time/2) * 0.03432;

  local_distance = distance;

}

{{< /highlight >}}

Um den das Drücken des Buttons zu erfassen nutzten wir die ```attachInterrupt``` Methode

{{< highlight cpp >}}
void setup() {
    //...

    attachInterrupt(BTN_PIN, btnPressed, RISING);

    //...
}

void btnPressed() {
    switch(state) {
        case WAITING:
            state = START;
            break;
        case RUNNING:
            state = FINISHED;
            break;
        default:
            break;  
    }
}

{{< /highlight >}}

#### Kommunikation

Das Thema Kommunikation hat uns jedoch viel Zeit und Nerven gekostet. Die Idee war: ein ESP32 eröffnet einen WiFi-Access-Point und einen Web Server. Dann können sich die anderen ESP32 per WLAN mit dem "Master-Controller" verbinden und über HTTP-Requests kommunizieren. Den Master-Controller haben wir auch erfolgreich eingerichtet. Per Smartphone und MacBook konnten wir uns mit dem WLAN verbinden und an die IP-Adresse des ESP32 erfolgreich HTTP-Request schicken. Leider hat dies mit einem anderen ESP32 nie funktioniert. So sah unser Code aus:

{{< highlight cpp >}}
#include <WiFi.h>
#include <WebServer.h>

const char* ssid = "ESP_GAME";
const char* password = "12345";

WebServer server(80);

void setup() {

    // ...

    WiFi.softAP(ssid, password);

    Serial.println(WiFi.softAPIP());

    server.on("/status", status);
    server.begin();

    // ...
}

void loop() {
    server.handleClient();
    // ...
}

void status() {
  if(state == RUNNING) {
    server.send(200, "text/plain", distance_mem + ", start");  
  } else {
    server.send(404, "text/plain", "error");
  }

}
{{< /highlight >}}

Die Zeit im Labor hat leider nicht ausgereicht um die Fehlerquelle zu finden. Daher haben wir uns eine Woche später noch einmal getroffen um das Problem zu beheben um endlich das Spiel zu testen. Da wir keinen ESP32 zu Hand hatten, nutzten wir meine eigenen ESP8266. Doch auch hier traten viele Probleme auf. So unterstütze die aktuelle ESP8266-Library keine Interrupts mehr. Daher mussten wir nach langem Suchen eine frühere Version aufspielen um die Button-Interrupts wieder nutzen zu können. 

Zudem trat wieder das Problem mit den HTTP-Requests auf. Leider haben wird dann nach Stunden voller Tutorials, GitHub-Issue-Diskussionen, zahlreichen Versions- und Library-Wechseln aufgegeben (Testaufbau um die beiden ESP8266 erfolgreich kommunizieren zu lassen in Bild 2). 

<img src="/swh-19-20-marcoc/img/esp/kommunikation_esp8266.jpg" />

Es war uns nicht möglich erfolgreich einen GET-Request an den Master-Controller zu schicken. Mit normalen Smartphones und PCs (mit unterschiedlichen Betriebssystemen) war das alles kein Problem und funktionierte. Mit dem ESP8266 wollte es jedoch einfach nicht funktioneren.


