## Teil 1: Deployment-Konzepte und Grundlagen

1. **Was ist Deployment?**

   Deployment bedeutet, eine Backend-Anwendung auf einem Online-Server bereitzustellen. Dadurch ist sie jederzeit über das Internet erreichbar.

2. **Der Deployment-Prozess:**

   Der Code wird zu GitHub gepusht. Die Hosting-Plattform lädt ihn herunter, installiert die Pakete, setzt die Umgebungsvariablen und startet den Server. Danach kann die API Anfragen über eine öffentliche URL beantworten.

3. **Die Einschränkungen von localhost:**

   localhost ist nur auf dem eigenen Computer erreichbar. Der Computer müsste ständig eingeschaltet und mit dem Internet verbunden sein. Außerdem ist er nicht für Sicherheit und viele Nutzer ausgelegt.

4. **Trennung von Zuständigkeiten:**

   Server und Datenbank werden getrennt gehostet, damit sie unabhängig verwaltet, gesichert und skaliert werden können. Verwaltete Datenbanken bieten außerdem Backups, Updates und besseren Schutz.

## Teil 2: Plattformüberblick und Optionen für Lernende

1. **Plattformrecherche:**  
   Für das Hosting von Backend-Anwendungen habe ich Render und Koyeb verglichen.
   - **Render:** Der kostenlose Tarif bietet 512 MB RAM, 0,1 CPU und 750 Betriebsstunden pro Monat. Nach 15 Minuten ohne Anfragen wird der Server pausiert.
   - **Koyeb:** Der kostenlose Tarif bietet 512 MB RAM, 0,1 vCPU und 2 GB SSD-Speicher. Nach einer Stunde ohne Traffic wird der Server pausiert.

   Als Datenbankanbieter habe ich MongoDB Atlas und Neon verglichen.
   - **MongoDB Atlas:** Der kostenlose M0-Tarif bietet 512 MB Speicher. Er eignet sich für Node.js-Projekte mit Mongoose.
   - **Neon:** Der kostenlose Tarif bietet eine PostgreSQL-Datenbank mit 0,5 GB Speicher, 100 CU-Stunden und 5 GB Datentransfer. Er eignet sich für Node.js-Projekte mit Prisma.

2. **Kostenanalyse:**  
   Bei Render kostet der kleinste kostenpflichtige Web-Service etwa 7 US-Dollar pro Monat. Bei Koyeb beginnen kostenpflichtige Instanzen bei ungefähr 1,61 US-Dollar pro Monat. Koyeb verlangt außerdem eine Kreditkarte.

   MongoDB Atlas bietet nach dem kostenlosen Tarif einen Flex-Tarif ab 0,011 US-Dollar pro Stunde an. Bei Neon werden Compute und Speicher nach Verbrauch berechnet.

   Für Lernende sind Render, MongoDB Atlas und Neon am sichersten, weil ihre kostenlosen Tarife ohne Kreditkarte verwendet werden können. Dadurch lassen sich unerwartete Kosten vermeiden. Bei Koyeb muss man vorsichtiger sein, weil eine Kreditkarte hinterlegt werden muss.

## Teil 3: Limits kostenloser Tarife einfach erklärt

Hosting-Plattformen setzen technische Limits für ihre kostenlosen Tarife. Diese Limits beeinflussen die Leistung und Verfügbarkeit einer Anwendung.

1. **RAM-/Speicherlimits:**  
   RAM wird benötigt, während die Anwendung läuft. Bei einem Limit von 512 MB können nur kleine Anwendungen ausgeführt werden. Wenn der Speicher voll ist, wird die Anwendung langsam, stürzt ab oder wird automatisch neu gestartet.

2. **Cold Starts / Ruhephasen / Inaktivitäts-Timeouts:**  
   Kostenlose Server werden nach einer bestimmten Zeit ohne Anfragen pausiert. Bei der nächsten Anfrage muss der Server zuerst neu starten. Dadurch kann die erste Antwort deutlich länger dauern.

3. **Rechenstunden und CPU-Kontingente:**  
   Rechenstunden bestimmen, wie lange eine Anwendung kostenlos laufen darf. Wenn beispielsweise 750 Stunden verbraucht sind, wird der Server bis zum nächsten Monat pausiert oder es entstehen zusätzliche Kosten. Ein kleines CPU-Kontingent führt außerdem bei vielen Anfragen zu langsameren Antworten.

4. **Datenbankspeicher und Limits für aktive Verbindungen:**  
   Der Datenbankspeicher bestimmt, wie viele Daten gespeichert werden können. Wenn das Speicherlimit erreicht ist, können möglicherweise keine neuen Daten mehr gespeichert werden.

   Das Verbindungslimit bestimmt, wie viele gleichzeitige Verbindungen zur Datenbank erlaubt sind. Bei zu vielen Verbindungen müssen Nutzer warten oder erhalten eine Fehlermeldung.

   Prisma und Mongoose verwenden Verbindungspools. Jede Instanz von `PrismaClient` erstellt einen eigenen Pool. Deshalb sollte eine Anwendung normalerweise nur eine gemeinsame Instanz verwenden. Bei Mongoose kann die Anzahl der Verbindungen mit `maxPoolSize` begrenzt werden. Zu große Verbindungspools können das Limit einer kostenlosen Datenbank schnell erreichen.

5. **Ausgehender Datentransfer / Bandbreite:**  
   Ausgehender Datentransfer sind Daten, die der Server an Nutzer sendet. Dazu gehören beispielsweise API-Antworten, Bilder und Dateien. Wenn das monatliche Limit von beispielsweise 5 GB erreicht wird, kann die Anwendung gesperrt werden, langsamer werden oder zusätzliche Kosten verursachen.
