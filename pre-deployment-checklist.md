# Pre-Deployment-Checkliste

Bevor eine Backend-Anwendung in die Produktion geht, muss sie sicher, stabil und sauber konfiguriert sein. Diese Checkliste hilft dabei, häufige Fehler und Sicherheitsprobleme vor dem Live-Start zu vermeiden.

---

## 1 Sicherheit

- [ ] Alle geheimen Werte wie API-Schlüssel, JWT-Secrets, Datenbank-URIs und OAuth-Tokens in Umgebungsvariablen speichern.
- [ ] Keine sensiblen Werte direkt im Code oder im Git-Repository speichern.
- [ ] Eine lokale `.env`-Datei nur für die Entwicklung verwenden und sie in `.gitignore` aufnehmen.
- [ ] Auf der Hosting-Plattform nur sichere Secret-Variablen im Dashboard oder über die Admin-Oberfläche setzen.
- [ ] CORS strikt konfigurieren: nur erlaubte Origins akzeptieren, keine offene `*`-Konfiguration in der Produktion.
- [ ] Nur die notwendigen HTTP-Methoden und Header erlauben.
- [ ] Sicherheits-Header mit `helmet` aktivieren, z. B. `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy` und `HSTS`.
- [ ] Rate Limiting für Login, Registrierung und öffentliche API-Endpunkte einbauen.
- [ ] Eingaben validieren und bereinigen, um SQL-Injection, NoSQL-Injection und andere Angriffe zu verhindern.
- [ ] HTTPS erzwingen, damit die Kommunikation zwischen Client und Server verschlüsselt ist.
- [ ] Debug-Modus in der Produktion deaktivieren.
- [ ] Keine vollständigen Stacktraces oder internen Fehlerdetails an Nutzer weitergeben.

## 2 Datenbankverwaltung

- [ ] In der Produktion keine `db push`-Operationen verwenden; stattdessen `prisma migrate deploy` nutzen.
- [ ] Alle Migrationen vor dem Start der App ausführen, damit Datenbank und Code konsistent sind.
- [ ] Backups und Wiederherstellungsstrategie planen.
- [ ] Indizes für häufig abgefragte Felder setzen, z. B. `email`, `username`, `createdAt`, Fremdschlüssel und häufig genutzte Filter.
- [ ] Verbindungen zur Datenbank kontrollieren und Pooling passend konfigurieren.
- [ ] Die maximale Anzahl aktiver Verbindungen beachten, da ORMs wie Prisma oder Mongoose mehrere Verbindungen öffnen können.
- [ ] Bei größeren Datenmengen prüfen, ob zusätzliche Indizes oder Schema-Optimierungen notwendig sind.
- [ ] Read/Write-Lasten und Performance-Engpässe vor dem Live-Start testen.

## 3 Fehlerbehandlung und Logs

- [ ] Ein globales Fehler-Handling in Express einbauen, damit Serverfehler sauber abgefangen werden.
- [ ] Benutzerfreundliche Fehlermeldungen an Clients senden, aber interne Details nur in Server-Logs speichern.
- [ ] Logs mit Zeitstempel, Request-ID, Statuscode und relevanten Metadaten versehen.
- [ ] Keine Passwörter, Tokens, API-Schlüssel oder persönlichen Daten in Logs schreiben.
- [ ] Nach dem Deployment die Live-Logs der Hosting-Plattform prüfen, z. B. im Bereich „Logs“ von Render, Railway, Fly.io oder ähnlichen Diensten.
- [ ] Monitoring-Tools wie Sentry oder zentrale Log-Services für Fehleranalyse nutzen.
- [ ] Health-Checks und Status-Endpunkte einrichten, damit der Server nach dem Start überprüfbar ist.
- [ ] Produktionsfehler sauber dokumentieren, damit Wiederholungen vermieden werden.

## 4 Umgebung einrichten

- [ ] Nur Laufzeit-Abhängigkeiten für die Produktion installieren.
- [ ] `devDependencies` nicht im Production-Build verwenden.
- [ ] Wenn möglich `npm ci --omit=dev` oder eine vergleichbare Production-Installation nutzen.
- [ ] Lokale Test- und Debug-Konfigurationen nicht in die Live-Umgebung übernehmen.
- [ ] Separate Konfigurationen für `development` und `production` nutzen.
- [ ] `NODE_ENV=production` setzen, damit der Server keine Entwicklungs-Features aktiviert.
- [ ] Unbenutzte Dateien, Testdatenbanken, Seed-Skripte und lokale Test-Tools vor dem Deployment entfernen.
- [ ] Port, Datenbank-URL, Secret-Keys, App-Mode und Build-Prozess vor dem Start prüfen.
- [ ] Build und Runtime sauber trennen: `npm install`, Prisma-Generate, Build und Start separat kontrollieren.
- [ ] Nur die benötigten Dateien für den Runtime-Betrieb behalten, damit die App schlank und stabil bleibt.

## 5 Abschlussprüfung vor dem Deployment

- [ ] App lokal mit Produktionsumgebung testen.
- [ ] Datenbank-Migrationen erfolgreich ausführen.
- [ ] HTTPS und CORS funktionieren wie erwartet.
- [ ] Geschützte Endpunkte sind richtig abgesichert.
- [ ] API-Antworten sind sauber und enthalten keine sensiblen Details.
- [ ] Logs sind lesbar und enthalten keine geheimen Informationen.
- [ ] Hosting-Umgebung hat alle notwendigen Umgebungsvariablen gesetzt.
- [ ] Startbefehl funktioniert ohne Fehler.
- [ ] Die öffentliche URL antwortet korrekt und kann Daten lesen und schreiben.

---

## Kurzfazit

Vor dem Deployment sollte man nicht nur den Code testen, sondern auch Sicherheit, Datenbankmigrationen, Fehlerbehandlung, Logs und die Production-Umgebung kontrollieren. Nur so kann eine Anwendung zuverlässig, sicher und stabil im Web laufen.
