
# Reflexion zur Jenkins CI Pipeline Aufgabe

### Welche Schritte waren notwendig, um Jenkins lokal mit Docker zum Laufen zu bringen?
Um Jenkins lokal mit Docker zu starten, musste ich zunächst Docker installiert und gestartet haben. Danach habe ich den offiziellen Jenkins LTS Docker-Container mit folgendem Befehl gestartet:
```
docker run --name my-simple-jenkins -p 8080:8080 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```
Dieser Befehl startet Jenkins, mappt den Port 8080 und nutzt ein Docker-Volume für Persistenz der Daten. Anschließend habe ich Jenkins im Browser unter `http://localhost:8080` geöffnet, das Initialpasswort aus den Container-Logs ausgelesen und die erste Einrichtung inklusive Plugin-Installation und Admin-Benutzer abgeschlossen.

---

### Was ist der Zweck der Datei Jenkinsfile, und wo muss sie im Verhältnis zu deinem Anwendungscode liegen?
Die Jenkinsfile definiert die Pipeline, also die automatisierten Schritte, die Jenkins bei jedem Build ausführen soll. Sie beschreibt z.B. das Auschecken des Codes, Installations- und Build-Prozesse. Die Jenkinsfile liegt im Wurzelverzeichnis des Projekts, also auf der gleichen Ebene wie die `package.json`, sodass Jenkins sie leicht beim Pipeline-Lauf finden und ausführen kann.

---

### Beschreibe die zwei Hauptstages, die du in deiner Pipeline definiert hast, und was der Hauptzweck der Steps in jeder Stage ist.
- **Checkout Stage:** In dieser Stage holt Jenkins den aktuellen Quellcode aus dem konfigurierten Git-Repository. Der Step `checkout scm` sorgt dafür, dass die neueste Version des Codes bereitsteht.
- **Install & Build Stage:** Hier wird der Build-Prozess der React-Webanwendung ausgeführt. Zunächst werden alle npm-Abhängigkeiten installiert (`npm ci`), danach wird die Anwendung mit `npm run build` für die Produktion gebaut.

---

### Wie hast du in Jenkins konfiguriert, von welchem Git-Repository und welchem Branch der Code für die Pipeline geholt werden soll?
In der Pipeline-Konfiguration im Jenkins-Job habe ich unter dem Abschnitt "Pipeline" die Option „Pipeline script from SCM“ gewählt, dann als SCM „Git“ ausgewählt. Dort habe ich die URL meines GitHub-Repositories eingetragen und den Branch `main` angegeben, der gebaut werden soll. So weiß Jenkins, woher es den Code holt und welchen Branch es verwendet.

---

### Was ist der Unterschied zwischen dem checkout scm Step in deiner Pipeline und dem git clone Befehl, den du manuell im Terminal ausführen würdest?
Der `checkout scm` Step in Jenkins ist ein abstrakter Schritt, der den Quellcode aus dem im Job definierten SCM holt. Er kümmert sich automatisch um das korrekte Einchecken, Fetching und Handling von Branches, basierend auf der Jenkins-Konfiguration. Der manuelle `git clone` Befehl hingegen ist ein einfacher Git-Befehl, der ein neues Repository lokal klont, ohne die zusätzlichen Kontextinformationen und Verwaltungsfunktionen, die Jenkins bereitstellt.

---
