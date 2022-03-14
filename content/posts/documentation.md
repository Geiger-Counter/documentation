---
title: "Dokumentation"
date: 2019-10-23T14:29:55+02:00
draft: false
---

## Static Page mit Hugo

#### Schritt 1 | Installation und Konfiguration von Hugo

Da ich ein MacBook benutze, habe ich mich entschieden Hugo mittels Homebrew zu installieren. Dazu gab ich im Terminal einfach <code>brew install hugo</code> ein. Nach einem kleinen Test, ob Hugo auch korrekt installiert wurde (mittels <code>hugo version</code>; siehe Screenshot 1), habe ich ein neues Project initialisiert (<code>hugo new site documentation</code>).

![Screenshot 01] (/swh-19-20-marcoc/img/documentation/shot_01.png "Screenshot 01")

Als nächster Schritt stand die Auswahl eines Themes an. Zuerst musste dafür ein git Repository in meinem Projekt-Ordner initialisiert werden (<code>git init</code>). Zudem habe ich eine .gitignore-Datei hinzugefügt (erstellt auf https://www.gitignore.io/api/hugo) um unerwünschte Dateien auf dem Repository auszuschließen. 

Als Theme habe ich mir auf https://themes.gohugo.io das Theme "Ananke Gohugo Theme" ausgewählt und mittels <code>git clone https://github.com/budparr/gohugo-theme-ananke.git themes/ananke</code> in mein Projekt eingefügt. 

Abschließend habe ich meinen ersten Post verfasst (diesen). Erstellt wurde die .md Datei mit dem Befehl <code>hugo new posts/documentation.md</code>.

Die Installation und das Einrichten von Hugo verlief ohne große Probleme. Das ganze wurde dann umgehend getestet. Dazu wurde ein lokaler Test-Server aufgesetzt (<code>hugo server -D</code>; siehe Screenshot 2).

![Screenshot 2] (/swh-19-20-marcoc/img/documentation/shot_02.png "Screenshot 02")

#### Schritt 2 | GitLab konfigurieren & erster deploy

Um den Blog zu veröffentlichen, musste zuerst ein GitLab-Project erstellt werden (verwendet wurde dazu GitLab auf den Servern der Uni). Nachdem das neue Projekt erstellt wurde (SWH-19-20-MarcoC), habe ich die SSH/GPG-Keys erstellt und git auf meinem lokalen Rechner konfiguriert. Damit auch das deloyen ohne Probleme funktioniert, habe ich die gitlab-ci.yml von moodle heruntergeladen und zum Projekt hinzugefügt. Anschließend musste noch die <code>config.toml</code> Datei bearbeitet werden. Damit es mit der Base-URL keine Probleme gibt wurde die korrekte URL dort eingetragen.

Um nun die Seite zum ersten Mal live zu sehen, wurden alle Dateien git hinzugefügt (<code>git add .</code>) und commited. Nach dem pushen habe ich auf GitLab überprüft ob auch der Job korrekt ausgeführt wird. Nachdem das der Fall war, stand der finale Test an: die URL aufrufen (https://luxo.informatik.uni-ulm.de/swh-19-20-marcoc/) und es zeigte sich, dass alles funktionierte:

![Screenshot 03] (/swh-19-20-marcoc/img/documentation/shot_03.png "Screenshot 03")

#### Zusammenfassung

Die Installation und das Einrichten von hugo, sowie dem erstellen und veröffentlichen des ersten Blog-Eintrages funktionierten bei mir überraschend gut. Einige meiner Kollegen hatten da weniger Glück. Ein Problem trat aber auch bei mir auf. Da ich nicht daran gedacht hatte, die Option ``drafts`` im Post von ``true`` in ``false`` zu ändern, wurde mein Post nicht angezeigt. Nach dem Ändern, commiten und pushen funktionierte jedoch alles wie gewollt.

