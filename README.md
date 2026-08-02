# Kosten-Motitor-App

Minimalistische Mobile-App zur persönlichen Finanzkontrolle – schnelle Ausgabenerfassung, Beleg-Scan per KI, Budgetwarnungen und automatisierte Fixkosten. Design im Arctic Look: klar, luftig, mit Primärfarbe #2B7FE0 und Akzent #3C82DC.
Dateien
|Datei               |Beschreibung                                                |
|--------------------|------------------------------------------------------------|
|`kosten-monitor.jsx`|Die App selbst (React-Artefakt, läuft direkt im Claude-Chat)|

Funktionen
	•	Kosten-Übersicht – Dashboard mit Gesamtsumme des Monats, Budget-Fortschrittsbalken pro Kategorie und den letzten Ausgaben.
	•	Ausgaben-Erfasser – Ein-Klick-Formular mit Betrag, Kategorie, Datum. Der Kamera-Button scannt Kassenbons: Ein Foto wird an die Anthropic-Vision-API geschickt, die Betrag und Datum automatisch ausliest. Ist der Beleg unscharf/unlesbar, springt die App auf manuelle Eingabe zurück.
	•	Kategorie-Manager – Kategorien anlegen, bearbeiten, löschen. Sind noch Ausgaben mit einer Kategorie verknüpft, fragt die App: neu zuweisen oder archivieren (Soft-Delete, Verlauf bleibt erhalten).
	•	Budget-Planer – Monatslimits pro Kategorie festlegen, plus ein Fixkosten-Modul für wiederkehrende Buchungen (Miete, Abos). Der Tag im Monat wird bei kürzeren Monaten automatisch angepasst (z. B. 31. → letzter Tag im Februar).
	•	Auswertungs-Analyse – Tortendiagramm der Kategorienverteilung und Balkendiagramm zum Monatsvergleich (letzte 6 Monate), mit Monats-Navigation.
	•	Budgetwarnungen – In-App-Banner bei 80 % und 100 % Budgetauslastung, Schwelle einstellbar.
	•	Tägliche Erinnerung – Uhrzeit für die Erinnerung zur Ausgabenerfassung ist in den Einstellungen konfigurierbar (Zahnrad oben rechts).

Datenmodell

Kategorien:  id, name, icon, budgetLimit, deleted
Ausgaben:    id, amount, date, categoryId, receiptPhoto, isFixed, fixedTemplateId
Fixkosten:   id, name, categoryId, amount, dayOfMonth, active
Einstellungen: notifTime, notifEnabled, warn80, warn100


Speicherung
Die App nutzt den persistenten Key-Value-Storage der Artefakt-Umgebung (window.storage). Daten bleiben zwischen Sitzungen erhalten und sind privat (nicht mit anderen geteilt). Es findet keine externe Datenübertragung statt – außer beim Beleg-Scan, wo das Foto einmalig an die Anthropic-API zur Texterkennung gesendet wird.
Bekannte Grenzen (Artefakt-Umgebung)
	•	Keine echten Push-Benachrichtigungen außerhalb der App – die eingestellte Erinnerungszeit ist aktuell nur eine gespeicherte Einstellung, keine OS-Notification. Für echte Push-Benachrichtigungen bräuchte es eine native App (z. B. via Flutter, Swift oder React Native/Expo).
	•	OCR läuft über die Anthropic-Vision-API (kein dediziertes On-Device-OCR) – funktioniert gut bei klaren Fotos, kann aber wie jede KI-Erkennung bei sehr schlechten Aufnahmen daneben liegen. Ergebnis vor dem Speichern immer kurz prüfen.
	•	Die App ist ein Prototyp/MVP zum Testen der Idee – für eine App-Store-Veröffentlichung müsste sie in ein natives oder Cross-Platform-Framework überführt werden.
Weiterentwicklung – nächste Schritte
	1.	Umsetzung als native App (Flutter/SwiftUI) für echte Push-Notifications und Offline-Datenbank (z. B. SQLite).
	2.	Export/Backup-Funktion (CSV/Excel) für die Auswertung.
	3.	Mehrwährungsunterstützung, falls relevant.


