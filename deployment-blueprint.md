# Deployment-Blueprint mit Render und Neon

## Gewählter Stack

- **Anwendungshost:** Render Web Service (Free)
- **Datenbankhost:** Neon Serverless PostgreSQL (Free)
- **Backend:** Node.js mit Express
- **ORM:** Prisma ORM mit `@prisma/adapter-pg`
- **Versionsverwaltung:** GitHub

Die Express-Anwendung wird aus einem GitHub-Repository auf Render bereitgestellt. Render verbindet sich über die geheime Umgebungsvariable `DATABASE_URL` mit der PostgreSQL-Datenbank bei Neon.

## 1. Voraussetzungen

- Ein GitHub-Konto und ein Repository mit dem Projektcode
- Ein kostenloses Neon-Konto
- Ein kostenloses Render-Konto
- Eine vorhandene `.env`-Datei für die lokale Entwicklung
- `.env` muss in `.gitignore` stehen und darf niemals committed werden

## 2. PostgreSQL-Datenbank bei Neon erstellen

1. Bei [Neon](https://console.neon.tech/) anmelden.
2. **New Project** auswählen.
3. Einen passenden Projektnamen eintragen.
4. Eine europäische Region auswählen, zum Beispiel Frankfurt.
5. Das Projekt erstellen.
6. Im Bereich **Connection Details** die gepoolte Verbindungszeichenfolge kopieren. Sie hat ungefähr dieses Format:

   ```env
   DATABASE_URL="postgresql://BENUTZER:PASSWORT@HOST/neondb?sslmode=require"
   ```

7. Die echte URL nur in `.env` und später in den geheimen Environment Variables von Render speichern. Sie darf weder in den Quellcode noch in GitHub eingefügt werden.

## 3. Datenbankschema und Testdaten bereitstellen

Die Neon-URL lokal als `DATABASE_URL` in `.env` eintragen. Danach im Projektverzeichnis ausführen:

```bash
npx prisma migrate deploy
```

`migrate deploy` führt ausschließlich bereits vorhandene Production-Migrationen aus. Im Gegensatz zu `prisma db push` verändert es das Schema kontrolliert und nachvollziehbar anhand des Ordners `prisma/migrations`.

Für eine neue Übungsdatenbank können anschließend die Testdaten eingefügt werden:

```bash
npm run prisma:seed
```

Ein Seed-Befehl kann vorhandene Daten verändern oder löschen. Er darf deshalb nicht unkontrolliert auf einer produktiven Datenbank ausgeführt werden.

## 4. Repository auf GitHub vorbereiten

Vor dem Push prüfen, dass `.env` ignoriert wird:

```bash
git check-ignore .env
git status
```

Danach die benötigten Projektdateien committen und in den Branch `main` pushen. Geheimnisse dürfen nicht Teil des Commits sein.

## 5. Render Web Service erstellen

1. Im [Render Dashboard](https://dashboard.render.com/) **New → Web Service** auswählen.
2. GitHub verbinden und das Repository mit der Backend-Anwendung auswählen.
3. Den Service folgendermaßen konfigurieren:

| Einstellung       | Wert                                                                                               |
| ----------------- | -------------------------------------------------------------------------------------------------- |
| Name              | Ein frei gewählter Service-Name                                                                                   |
| Region            | Frankfurt                                                                                          |
| Branch            | `main`                                                                                             |
| Language/Runtime  | Node                                                                                               |
| Build Command     | `npm ci --include=dev && npx prisma generate && npx prisma migrate deploy && npm prune --omit=dev` |
| Start Command     | `npm start`                                                                                        |
| Compute Plan      | Free                                                                                               |
| Health Check Path | `/`                                                                                                |

Bei einem kostenlosen Render Web Service steht kein separater Pre-Deploy Command zur Verfügung. Deshalb wird `prisma migrate deploy` hier im Build Command ausgeführt. Bei einem kostenpflichtigen Service sollte die Migration stattdessen als Pre-Deploy Command eingerichtet werden.

## 6. Umgebungsvariablen sicher einrichten

Unter **Advanced → Environment Variables** folgende Werte anlegen:

| Key            | Value                                     |
| -------------- | ----------------------------------------- |
| `DATABASE_URL` | Vollständige geheime Neon-Verbindungs-URL |
| `NODE_ENV`     | `production`                              |

`PORT` muss nicht manuell gesetzt werden. Render stellt diese Variable bereit und die Anwendung verwendet `process.env.PORT || 3000`.

Die Werte nicht in Logs, Screenshots, Dokumentation oder GitHub veröffentlichen. Falls eine Verbindungs-URL versehentlich veröffentlicht wurde, müssen die Zugangsdaten in Neon sofort erneuert werden.

## 7. Deployment starten und Logs prüfen

1. **Create Web Service** anklicken.
2. Im Tab **Deploys/Logs** kontrollieren, ob Abhängigkeiten installiert, der Prisma Client generiert und die Migrationen erfolgreich ausgeführt werden.
3. Auf die Meldung achten, dass der Server gestartet wurde und Render einen Port erkannt hat.
4. Nach erfolgreichem Deployment stellt Render eine öffentliche URL bereit, zum Beispiel:

   ```text
   https://SERVICE-NAME.onrender.com
   ```

Bei einem Fehler zuerst Build- und Runtime-Logs prüfen. Häufige Ursachen sind eine fehlende `DATABASE_URL`, ein nicht generierter Prisma Client oder eine falsche Startanweisung.

## 8. Automatische Deployments

Render aktiviert für ein verknüpftes GitHub-Repository standardmäßig Auto-Deploy. Jeder neue Push in den konfigurierten Branch `main` startet einen Build. Nur wenn Build und Migration erfolgreich sind, wird die neue Version veröffentlicht. Die Einstellung kann unter **Settings → Build & Deploy → Auto-Deploy** kontrolliert werden.

## 9. Öffentliche API testen

Zuerst die Startseite und einen lesenden Endpunkt aufrufen:

```bash
curl https://SERVICE-NAME.onrender.com/
curl https://SERVICE-NAME.onrender.com/api/resources
```

Danach einen Schreibvorgang testen:

```bash
curl -X POST https://SERVICE-NAME.onrender.com/api/resources \
  -H "Content-Type: application/json" \
  -d '{"name":"Testeintrag"}'
```

Zum Abschluss kontrollieren, ob der neue Datensatz aus Neon gelesen werden kann:

```bash
curl https://SERVICE-NAME.onrender.com/api/resources
```

Erwartet werden erfolgreiche HTTP-Statuscodes, JSON-Antworten und der neu erstellte Datensatz. Damit ist nachgewiesen, dass die öffentliche Render-Anwendung Daten aus der entfernten Neon-Datenbank lesen und in sie schreiben kann.

## 10. Hinweise zum kostenlosen Betrieb

- Render versetzt den kostenlosen Service nach 15 Minuten ohne Traffic in den Ruhezustand. Der erste Request kann deshalb ungefähr eine Minute dauern.
- Render besitzt ein flüchtiges Dateisystem. Dauerhafte Daten gehören in Neon und nicht in lokale Dateien des Web Service.
- Verbrauch und Limits regelmäßig in den Dashboards von Render und Neon kontrollieren.
- Für ein Lernprojekt keine Zahlungsmethode hinterlegen, wenn unerwartete nutzungsabhängige Kosten vollständig vermieden werden sollen.

## Quellen

- [Render: Web Services](https://render.com/docs/web-services)
- [Render: Deployments und Befehle](https://render.com/docs/deploys)
- [Render: Prisma ORM bereitstellen](https://render.com/docs/deploy-prisma-orm)
- [Render: kostenlose Dienste](https://render.com/docs/free)
- [Neon Console](https://console.neon.tech/)
- [Prisma: Production-Migrationen](https://www.prisma.io/docs/orm/prisma-client/deployment/deploy-database-changes-with-prisma-migrate)
