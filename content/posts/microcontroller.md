---
title: "Microcontroller"
date: 2019-11-20T16:36:23+01:00
draft: false
---

## Arduino Wetterstation

### Schritt 1 | Arduino mit Mac verbinden & Feuchtigkeits- und Temperatursensor einrichten

Die Einrichtung war ziemlich einfach: den Mac via USB mit dem Arduino verbinden. Die ArduinoIDE hat die Verbindung dann auch ziemlich schnell erkannt.

Um die Feuchtigkeit und Raumtemperatur zu messen wurde der DHT11 Sensor zum System hinzugefügt. Dazu wurde der Sensor zunächst auf dem Breadboard platziert.

<img src="/swh-19-20-marcoc/img/arduino/humidity_sensor.jpg" />

GRN (ground) und VCC (5V) wurden korrekt angeschlossen. Zudem wurde der Data-Pin mit Pin 4 auf dem Arduino verbunden. 

Das folgende Skript konnte dann damit die Daten für Luftfeuchtigkeit und Raumtemperatur aus dem Sensor auslesen und in der Konsole ausgeben:

{{< highlight cpp >}}
#include "DHT.h"

// Define PINs
#define DHTPIN 4

#define DHTTYPE DHT11

// Initialize the DHT library for the humidity and temperature sensor
DHT dht(DHTPIN, DHTTYPE);

void setup() {

  Serial.begin(9600);
  
  dht.begin();
  delay(2000);
}

void loop() {

  // read the humidity (in percent) and the temperature (in degree celcius) from the dht11
  float humidity = dht.readHumidity();
  float temp = dht.readTemperature();

  // Debug output
  Serial.print("Luftfeuchtigkeit: ");
  Serial.println(humidity);
  Serial.print("Temperature: ");
  Serial.println(temp);

  delay(500);

}
{{< /highlight >}}

<img src="/swh-19-20-marcoc/img/arduino/temp_humidity.png">

### Schritt 2 | Helligkeitssensor hinzufügen

Um weitere Daten von Sensoren zu erhalten, wurde ein Helligkeitssensor zum System hinzugefügt. Wieder wurden GRN und 5V korrekt angeschlossen.

Der Sensor wurde dieses Mal analog über PIN A2 ausgelesen. Dafür musste das Skript verändert werden. Zuerst wurde eine neue Konstante für den PIN hinzugefügt.

{{< highlight cpp >}}
#define BRIGHTNESS_PIN A2
{{< /highlight>}}

Das Signal des Helligkeitssensors musste noch angepasst werden. Zudem wurde der Wert wieder in der Konsole ausgegeben:

{{< highlight cpp >}}
int brightness = abs(1000 - analogRead(BRIGHTNESS_PIN));
{{< /highlight >}}

<img src="/swh-19-20-marcoc/img/arduino/brightness.png">

#### Schritt 3 | Mehr LED!

Um das System mit ein paar Zustands-Indikatoren zu erweitern, wurde eine LED hinzugefügt. Diese LED sollte angehen, wenn die Helligkeit im Raum niedrig ist und wieder ausgehen, sollte die Helligkeit im Raum ansteigen.

Doch die Installation der LED verlief leider nicht ganz reibungslos. Es wurde korrekt ein Wiederstand (220 &Omega;) verbaut und die Spannung angelegt, doch die LED leuchtete nicht.

Ein Lösungsansatz: Ohne Wiederstand versuchen. Leider hat die LED immer noch nicht funktioniert. Nun wurde klar: die LED wurde falsch herum angeschlossen. Beim erneuten Versuch wurde dann jedoch der Wiederstand vergessen, was zu einer defekten LED führte. Mit Wiederstand und korrektem Anschluss, funktionierte die LED dann jedoch doch wie gewünscht.

Die LED musste nun noch vom Arduino gesteuert werden. Dazu wurde sie an PIN 12 angelegt. Das Skript wurde dazu weiter geändert:

{{< highlight cpp >}}
#define LED_GREEN_PIN 12
{{< /highlight >}}


{{< highlight cpp >}}
pinMode(LED_GREEN_PIN, OUTPUT);
{{< /highlight >}}

Um die LED bei geeigneter Helligkeit an- oder auszumachen, musste in der loop noch folgende Bedingung eingefügt werden:

{{< highlight cpp >}}
  // If the brightness recognized by the sensor is below a certain point, the LED is turned on 
  if(brightness < 350) {
    digitalWrite(LED_GREEN_PIN, HIGH);
  } else {
    digitalWrite(LED_GREEN_PIN, LOW);
  }
{{< /highlight >}}

Der Test im Video zeigt, dass das Experiment geglückt ist:

<video src="/swh-19-20-marcoc/img/arduino/Helligkeitssensor.mp4" width="500px" controls>

### Schritt 4 | Warnung!

Eine weitere Idee war, ein Tonsignal zu erzeugen, wenn der Feuchtigkeitswert der Luft zu hoch wird. Dazu wurde ein Buzzer eingesetzt. Dieser wurde wieder korrekt angeschlossen und an PIN 8 mit dem Arduino verbunden:

{{< highlight cpp >}}
#define BUZZER_PIN 8
{{< /highlight >}}

{{< highlight cpp >}}
pinMode(BUZZER_PIN, OUTPUT);
{{< /highlight >}}

Bei einem Feuchtigkeitswert von über 60%, sollte der Buzzer angehen:

{{< highlight cpp >}}
  if(humidity > 60) {
    digitalWrite(BUZZER_PIN, HIGH);
  } else {
    digitalWrite(BUZZER_PIN, LOW);
  }
{{< /highlight >}}

Da der Buzzer jedoch dauerhaft einen Ton von sich gab (sehr nervig, siehe Video), musste etwas geändert werden. Mit einem Counter gab der Buzzer nur zu Beginn einer Erhöhung der Luftfeuchtigkeit einen Ton ab:

{{< highlight cpp >}}
  // If the humidity is over 60% the buzzer sends an signal.
  if(humidity > 60) {
    // To limit the time the buzzer is active => 4 iterations with 500 ms of delay = 2 seconds of buzzer
    if(timer < 4) {
      digitalWrite(BUZZER_PIN, HIGH);
    } else {
      digitalWrite(BUZZER_PIN, LOW);
    }
    timer++;
  } else {
    digitalWrite(BUZZER_PIN, LOW);
    timer = 0;
  }
{{< /highlight >}}

<video src="/swh-19-20-marcoc/img/arduino/Feuchtigkeitssensor.mp4" width="500px" controls>

### Schritt 5 | Temperaturanzeige

Abschließend sollte noch ein Display verwendet werden. Darauf sollte die aktuelle Raumtemperatur angezeigt werden. Das Einbinden war dank passender Library sehr einfach. Die Temperatur-Daten waren auch schon vorhanden (siehe Schritt 1). Damit konnte sehr schnell die Temperatur angezeigt werden:

<img src="/swh-19-20-marcoc/img/arduino/temperatur.JPG">
