# Notizen – Einsatzplanung-Demo (junited AUTOGLAS)

Stand: 12.11.2026. Diese Datei hält Hintergrund, Entscheidungen und Ergebnis fest, damit ein neuer Chat ohne Vorwissen weitermachen kann.

## Hintergrund
- Marke laut Logo: **junited AUTOGLAS** (Franchisegeber). Gespräch mit dem Inhaber des Franchise-Standorts **Wetzlar**.
- Ziel: dem Inhaber eine **kleine Quick-Demo** zeigen, was möglich ist. Kein fertiges Produkt, Bezahlung egal, ein Standort.
- Ablauf im Betrieb: Kunden vereinbaren Termine für Autoglas-Wechsel. Ein Teil kommt in die Werkstatt, zu den anderen fährt der Kfz-Mechaniker (Garage, Hof, …).
- Die Termine liegen in einem **Google Kalender** (Kunde, Datum, Adresse, Fahrzeugtyp/Kennzeichen). Der Fahrzeugtyp steht später bei der Terminvergabe im Eintrag.

## Fachliche Anforderungen (vom Auftraggeber)
**Mitarbeiter (5):** Login, sehen offene Termine, übernehmen selbst Termine (z. B. Mo, Di, Mi), dürfen Zusagen zurückziehen, haben eine To-do-Liste, setzen nach Erledigung einen Haken (im echten System würde nur das in den Kalender zurückgeschrieben).

**Zwei-Personen-Regel:** Bei manchen Fahrzeugen sind zwei Mitarbeiter nötig. Das soll aus dem Fahrzeug(schein) abgeleitet werden. Fehlt die zweite Zusage, sieht der Chef das im Dashboard (keine Benachrichtigung).

**Chef (nur Franchise-Nehmer):** Dashboard mit Auslastung pro Mitarbeiter (wer ist wo bei welchem Kunden), Terminen ohne Zusage, erledigten und offenen Terminen, Ampel (rot/gelb/grün). Kann Mitarbeiter auch selbst zuweisen.

## Entscheidungen für die Demo
- Reine statische **Web-App** (responsive), Deutsch, kein Backend, **Beispieldaten**, kein echter Google Kalender.
- Rollenwahl per Klick, **kein Passwort**.
- Daten liegen nur im **Browser** (localStorage). Änderungen sieht niemand sonst. Neuer Tag = Daten werden neu erzeugt, damit die Ampel zu "heute" passt.
- Zwei-Personen-Regel in der Demo über eine feste Fahrzeugliste (z. B. Tesla, BMW X5, Sprinter, Transporter, Audi Q7 = 2 Personen). **Keine echte Fahrzeugschein-Logik.**
- Ampel: rot = in 0–2 Tagen und nicht voll besetzt; gelb = später, nicht voll besetzt; grün = besetzt; grau = erledigt.
- Mitarbeiter-Kalender zeigt **nur offene** Termine (nicht besetzt, nicht erledigt, nicht schon selbst übernommen). Eigene Termine stehen unter "Meine Termine". Der Chef-Kalender zeigt alle.
- Demo-Leiste: "Termine neu mischen" (neue Termine, alle Zusagen leer) und "Zurücksetzen".
- Logo von der Auftraggeberseite eingebunden (`site/logo.png`).

## Ergebnis (umgesetzt)
- Rollenwahl: Chef oder Marco, Jonas, Sabine, Tim, Lena.
- Wochenkalender (Mo–Sa) mit Vor-Ort-/Werkstatt-Terminen, Blättern bis zum letzten Termin.
- Mitarbeiter: Termin übernehmen/zurückziehen, Doppelbelegung zur selben Zeit wird verhindert, "Meine Termine" mit Erledigt-Haken (erst bei voller Besetzung), Routenlink zu Google Maps.
- Chef: Kennzahlen (anklickbar, springt zur gefilterten Liste), Auslastung und Einsatzort pro Mitarbeiter, Terminliste mit Zuweisung per Auswahlliste.
- Behobene Fehler unterwegs: Zähler "Termine suchen Verstärkung" wich vom Kalender ab (Blätter-Grenze von 3 Wochen), jetzt identisch.
- Vom Auftraggeber nach Test mit "sieht gut aus" bewertet.

## Technik und Veröffentlichung
- Alles in `site/index.html` (HTML, CSS, JS in einer Datei), `site/logo.png`. Kein Build.
- Veröffentlichung über **GitHub Pages** mit dem Workflow `.github/workflows/pages.yml` (Pages-Source: "GitHub Actions"). Deploy läuft bei jedem Push auf `main`.
- URL (Demo): https://aozcorap.github.io/junited-autoglas/
- Arbeitsweise: Änderungen auf Branch `claude/focused-albattani-ly4pma`, dann Pull Request nach `main` (Squash-Merge). Nach einem Merge den Branch wieder von `main` neu aufsetzen.
- Bekannt: Der Workflow läuft auch auf dem Entwicklungsbranch und schlägt dort rot fehl, weil Pages nur von `main` deployt. Harmlos, lässt sich entfernen, indem der Branch aus `on.push.branches` gestrichen wird.

## Grenzen der Demo / offene Punkte für ein echtes System
- **Fahrzeugschein → 1 oder 2 Personen:** Der Fahrzeugschein enthält diese Info nicht. Es braucht eine gepflegte Zuordnung (Modell/Typschlüssel → Personenzahl). Größtes fachliches Risiko. Ob Wetzlar so eine Liste hat, ist unbekannt.
- **Google Kalender anbinden:** Lesen der Termine, Zurückschreiben des Erledigt-Hakens, Anmeldung und Freigaben. In der Demo bewusst weggelassen.
- **Gemeinsame Daten:** Echtes Backend/Datenbank nötig, damit Chef und Mitarbeiter denselben Stand sehen. Dazu echte Logins und Rollen.
- **Mehrere Standorte / Franchise-Nehmer:** Würde die Architektur deutlich ändern (Mandantenfähigkeit). Aktuell nicht vorgesehen.
- **Benachrichtigungen** (z. B. bei Rückzug einer Zusage): vom Auftraggeber ausdrücklich nicht gewünscht.
- Im echten Betrieb zu klären: Wer pflegt die Fahrzeugliste, wie lange dauern Termine (Demo: pauschal 2,5 Stunden Sperrzeit), was passiert bei Absagen.
- Nicht auf einem echten iPhone getestet, nur im Browser mit Handy-Breite.
