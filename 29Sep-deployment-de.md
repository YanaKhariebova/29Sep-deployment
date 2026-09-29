# Backend-Deployment: Von localhost in die Produktion

**Ziel:** Im Laufe dieses Kurses hast du leistungsfähige Backend-APIs mit Node.js, Express, Datenbanken (MongoDB/PostgreSQL) und ORMs/ODMs (Prisma/Mongoose) entwickelt. Bisher liefen diese ausschließlich auf deinem eigenen Computer. Bei dieser Aufgabe recherchierst du, wie du deine Anwendung aus deiner lokalen Umgebung ins Web bringst, damit sie für alle zugänglich ist.

Recherchiere die folgenden Fragen und beantworte sie. Sei darauf vorbereitet, deine Ergebnisse zu besprechen und deine Deployment-Pläne mit der Klasse zu teilen.

---

## Teil 1: Deployment-Konzepte und Grundlagen

1. **Was ist Deployment?** Erkläre mit deinen eigenen Worten, was Backend-Deployment ist und warum es für eine Anwendung in der Produktion unverzichtbar ist.
2. **Der Deployment-Prozess:** Was passiert im Hintergrund während des Deployments – vom Pushen deines Codes zu GitHub bis zu dem Moment, in dem deine API erfolgreich auf eine Anfrage im Web antwortet?
3. **Die Einschränkungen von localhost:** Warum eignet es sich nicht für echte Nutzerinnen und Nutzer, eine Anwendung auf `localhost` auszuführen (oder deinen eigenen Computer rund um die Uhr eingeschaltet zu lassen)?
4. **Trennung von Zuständigkeiten:** Warum ist es in der Branche Standard, deinen Servercode (Express) und deine Datenbank (PostgreSQL oder MongoDB) in der Produktion auf getrennten verwalteten Diensten zu hosten, statt beides auf demselben Server zu betreiben?

---

## Teil 2: Plattformüberblick und Optionen für Lernende

1. **Plattformrecherche:** Recherchiere mindestens **zwei** Plattformen für das Hosting von Backend-Anwendungen (z. B. Render, Railway, Fly.io, Vercel, Koyeb) und mindestens **zwei** Anbieter für Database-as-a-Service (DBaaS) (z. B. MongoDB Atlas, Neon, Supabase, Aiven).
   - _Welche konkreten kostenlosen Tarife bieten sie für einen Node.js-Stack mit Mongoose oder Prisma?_
2. **Kostenanalyse:** Vergleiche die Kosten und Preismodelle dieser Plattformen, sobald die kostenlosen Nutzungslimits überschritten werden oder eine Kreditkarte erforderlich ist.
   - _Welche Optionen sind für Lernende am sichersten, wenn sie unerwartete Kosten vermeiden möchten?_

---

## Teil 3: Limits kostenloser Tarife einfach erklärt

Hosting-Plattformen setzen technische Limits für ihre kostenlosen Tarife. Erkläre in einfachen Worten, was die folgenden technischen Ressourcenlimits bedeuten, und beschreibe, welche Auswirkungen es auf Nutzerinnen und Nutzer hat, wenn ein Limit erreicht wird.

1. **RAM-/Speicherlimits** (z. B. 512 MB)
2. **Cold Starts / Ruhephasen / Inaktivitäts-Timeouts** (z. B. der Server wird nach 15 Minuten ohne Nutzung „heruntergefahren“)
3. **Rechenstunden und CPU-Kontingente** (z. B. 750 kostenlose Ausführungsstunden pro Monat)
4. **Datenbankspeicher und Limits für aktive Verbindungen** (Recherchiere auch, wie ORMs wie Prisma oder Mongoose die maximale Anzahl an Datenbankverbindungen beeinflussen.)
5. **Ausgehender Datentransfer / Bandbreite** (z. B. 5 GB pro Monat)

---

## Teil 4: Recherche zum praktischen Deployment und Deployment-Plan

Wähle einen Anwendungshost und einen Datenbankhost aus deiner Recherche in Teil 2 aus. Lies die offizielle Dokumentation der Anbieter und schreibe für eine Mitschülerin oder einen Mitschüler eine klare Schritt-für-Schritt-Anleitung, in der genau erklärt wird, wie ein Projekt mit diesem Stack bereitgestellt wird.

Deine Anleitung muss Folgendes abdecken:

- [ ] Wie du die entfernte Datenbank bereitstellst und den sicheren Verbindungsstring bzw. die Verbindungs-URI erhältst.
- [ ] Wie du Umgebungsvariablen (wie Geheimnisse in `.env`) sicher auf der Hosting-Plattform einrichtest, ohne sie in die Versionsverwaltung aufzunehmen.
- [ ] Welche Build- und Startbefehle (z. B. `npm install`, `npx prisma generate`, `npm start`) die Plattform ausführen muss, um die App zu starten.
- [ ] Wie du ein GitHub-Repository verknüpfst, damit bei jedem Push automatisch ein neues Deployment erfolgt.
- [ ] Wie du testest und überprüfst, dass die öffentliche URL deiner bereitgestellten Anwendung erfolgreich Daten aus der entfernten Datenbank lesen und in sie schreiben kann.

---

## Teil 5: Checkliste vor dem Deployment

Bevor du Code in die Produktion bringst, musst du sicherstellen, dass deine App sicher und effizient ist. Recherchiere und erstelle eine umfassende Checkliste vor dem Deployment. Nenne konkrete Maßnahmen für die folgenden Kategorien:

- **Sicherheit:** Wie solltest du API-Schlüssel und CORS-Ursprünge verwalten und Sicherheits-Header einrichten (z. B. mit `helmet` oder Rate Limiting)?
- **Datenbankverwaltung:** Wie gehst du in der Produktion richtig mit Datenbankänderungen um (z. B. `prisma migrate deploy` statt `db push`) und wie richtest du Indizes ein?
- **Fehlerbehandlung und Logs:** Wie verhinderst du, dass sensible Stacktraces von Serverfehlern öffentlichen Nutzerinnen und Nutzern angezeigt werden? Wo findest du die Live-Logs deiner Anwendung nach dem Deployment?
- **Umgebung einrichten:** Wie solltest du `devDependencies` und lokale Testkonfigurationen bereinigen, damit dein Production-Build schlank bleibt?

## Teil 6: Abgabe

Diese Aufgabe ist **heute, am 29. September 2026 bis 23:59Uhr**, fällig. Reiche deine ausgearbeitete Recherche als Repository auf GitHub ein und teile den Link mit deiner Lehrkraft. Dein Repository muss Folgendes enthalten:

- Eine `README.md`-Datei mit deinen Antworten zu den Teilen 1–3.
- Eine `deployment-blueprint.md`-Datei mit deiner Schritt-für-Schritt-Anleitung zum Deployment aus Teil 4.
- Eine `pre-deployment-checklist.md`-Datei mit deiner umfassenden Checkliste aus Teil 5.
