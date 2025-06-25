
# Simple Webapp CI Pipeline mit Jenkins

## Beschreibung der Aufgabe
Diese Aufgabe umfasst das Erstellen einer einfachen React-Webanwendung, das Einrichten eines Git-Repositories, sowie das Aufsetzen einer Jenkins-Pipeline zur automatisierten Integration (CI). Ziel ist es, den Build-Prozess der Webanwendung automatisch bei Code-Änderungen auszuführen.

---

## Webanwendung
- Basis: React-App mit Vite als Build-Tool
- Projektname: `simple-webapp`
- Build-Befehl: `npm run build`
- Repository enthält den Quellcode und eine Jenkinsfile zur Pipeline-Definition

---

## Jenkins-Pipeline Konfiguration

### Jenkins Master starten
Der Jenkins-Server wird als Docker-Container betrieben:
```bash
docker run --name my-simple-jenkins -p 8080:8080 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

### Jenkins initial einrichten
- Jenkins Web UI unter `http://localhost:8080` aufrufen
- Initial-Admin-Passwort aus Container-Logs auslesen
- Empfohlene Plugins installieren
- Ersten Admin-Benutzer anlegen

### Pipeline Job anlegen
- Im Jenkins Dashboard auf **„New Item“** klicken
- Name: `SimpleWebApp-CI`
- Job-Typ: `Pipeline`

### SCM Integration
- Pipeline-Definition: `Pipeline script from SCM`
- SCM: `Git`
- Repository URL: (dein Git-Repo, z.B. `https://github.com/dein-benutzername/simple-webapp.git`)
- Branch: `*/main` (oder entsprechend)
- Script Path: `Jenkinsfile`

### NodeJS Plugin installieren
- Unter `Jenkins verwalten > Plugins` das NodeJS Plugin installieren
- Unter `Jenkins verwalten > Tools` NodeJS hinzufügen und automatische Installation aktivieren (z.B. NodeJS 24.x)

### Jenkinsfile (Pipeline-Skript)
Die Pipeline enthält mindestens zwei Stages:
- **Checkout**: Holt den Code aus dem Git-Repository
- **Install & Build**: Führt `npm ci` und `npm run build` aus

Beispiel-Jenkinsfile:
```groovy
pipeline {
  agent any

  tools {
    nodejs 'node16'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Install & Build') {
      steps {
        sh 'npm ci'
        sh 'npm run build'
      }
    }
  }
}
```

---

## Pipeline ausführen
- Jenkins Job öffnen (`SimpleWebApp-CI`)
- Links auf **„Build Now“** klicken
- Build-Prozess in **Console Output** überwachen
- Erfolgreicher Build endet mit `Finished: SUCCESS`

---

## Aufräumen (optional)
- Jenkins Container stoppen:
```bash
docker stop my-simple-jenkins
```
- Jenkins Container + Daten löschen:
```bash
docker rm -v my-simple-jenkins
```

---

## Zusammenfassung
Mit dieser Pipeline wird bei jedem Push in das Git-Repository automatisch der Build der React-Webanwendung gestartet und überprüft. So wird sichergestellt, dass Änderungen immer erfolgreich gebaut werden.

---
