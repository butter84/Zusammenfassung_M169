# Zusammenfassung_M169

**<u>Sie können einen Docker-Container ausführen und interaktiv verwenden.</u>**

docker run -it ubuntu bash


**Sie erkennen, warum Portweiterleitungen wichtig sind und können eine solche in Betrieb nehmen.**

docker run -p 8080:80 nginx

**Sie können mit Docker Volumes umgehen.**

docker run -v /daten:/app nginx

**Sie können Volumes benennen und in einem definierten Verzeichnis speichern.**

docker volume create meinvolume
docker run -v meinvolume:/app nginx

**Sie verstehen den Standardaufbau eines Netzwerks mit Docker.**

docker network ls --> zeigt aktive Netzwerke.

**Sie kennen die wichtigsten Befehle um Docker zu administrieren, dazu zählen Platzbedarf von Images und Containern ermitteln, Container und Images löschen, Volumes verwalten, Gesamtüberblick erhalten und Ungenutzten Speicher freigeben.**

Platzbedarf prüfen: docker system df
Container löschen: docker rm <container>
Image löschen: docker rmi <image>
Volumes verwalten: docker volume ls, docker volume rm <volume>
Speicher freigeben: docker system prune

**Sie kennen die Syntax von Dockerfiles.**

→ Ein Dockerfile beschreibt, wie ein Docker-Image gebaut wird.
Wichtige Befehle:

FROM: Basis-Image wählen (z.B. ubuntu, node, nginx)

RUN: Befehle ausführen (z.B. Software installieren)

COPY: Dateien ins Image kopieren

CMD oder ENTRYPOINT: Startbefehl im Container

**Sie können das Beispiel von Dockerfiles nachvollziehen.**

Beispiel:

Dockerfile
Kopieren
Bearbeiten
FROM ubuntu
RUN apt update && apt install -y nginx
CMD ["nginx", "-g", "daemon off;"]
→ Baut ein Ubuntu-Image mit installiertem Nginx.

**Sie können eigene Dockerfiles erstellen, testen und dokumentieren.**

→ Datei schreiben, z.B. Dockerfile (keine Endung).
→ Image bauen:

bash
Kopieren
Bearbeiten
docker build -t meinimage .
→ Container starten und testen:

bash
Kopieren
Bearbeiten
docker run -p 8080:80 meinimage
→ Dokumentation: Kommentiere im Dockerfile mit #, damit andere verstehen, was passiert.

**Sie kennen den Einsatzzweck von Docker compose.**

 Mehrere Container (z.B. Webserver + Datenbank) einfach zusammen starten, vernetzen und verwalten.
→ Keine 5 separate docker run-Befehle mehr, alles zentral in einer Datei.

**Sie verstehen den Aufbau einer YAML-Datei und können eigene YAML-Dateien erstellen.**

 YAML ist eine lesbare Struktur (Einrückung wichtig!).
→ Beispiel:

yaml
Kopieren
Bearbeiten
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: geheim
→ Zwei Container: Nginx und MySQL.

**Sie können Docker compose anwenden.**

Starten: docker-compose up -d
Stoppen: docker-compose down
Logs anschauen: docker-compose logs

**Sie verstehen wie in Docker compose mit Passwörtern oder sonstigen geheimen Informationen umgegangen wird.**

Nicht direkt im YAML schreiben!

Besser .env-Datei nutzen:

env
Kopieren
Bearbeiten
MYSQL_ROOT_PASSWORD=geheim
und im YAML:

yaml
Kopieren
Bearbeiten
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
Oder secrets verwenden (noch sicherer, komplexer).



**MINI Projekt**

## Docker Image builden
Im Verzeichnis den Ordner samplesite/ erstellen und in diesem Ordner die HTML/CSS etc. Inhalte einfügen

`docker build -t webprojekt .`

## Docker Container starten
docker run -d -p 8443:443 -p 8085:80 --name webprojekt webprojekt

## Docker Container starten (sinnvoller)
Das Verzeichnis samplesite als Volume einbinden, ebenso das Apache2-Logfile -> Änderungen können im Host unter samplesite/ vorgenommen werden

`docker run -d -p 8443:443 -p 8085:80 --name webprojekt -v /home/vmadmin/samplesite/:/var/www/html -v /home/vmadmin/:/var/log/apache2 -h webtest webprojekt`
