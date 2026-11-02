# Prompt: Zuarbeit am Mac für die Cloud-Sitzung

Für eine Claude-Sitzung **am Mac** (Claude-Desktop-App oder `claude` im Terminal) im Ordner
`~/Documents/tycetoolz-agents` (Repository `Tyce2025/tycetoolz-agents`). Stand 02.11.2026.

Zweck: Die Cloud-Sitzung (claude.ai/code) kommt an den Mac nicht heran. Diese Sitzung erledigt
am Mac, was dafür fehlt, und schreibt das Ergebnis in die Übergabe, damit die Cloud weiterarbeiten
kann.

Was der Inhaber **selbst** erledigt, weil kein Agent an diese Stellen kommt:
- `EBAY_REFRESH_TOKEN` in der Cloud-Umgebung eintragen (Titelleiste, Umgebung, Edit). Der Wert
  gehört in Anführungszeichen: `EBAY_REFRESH_TOKEN="v^1.1#…"`. Danach eine neue Cloud-Sitzung starten.
- Remote Control für diesen Ordner einschalten, damit die Cloud den Mac künftig selbst nutzen kann.
  Das geht in der Claude-Desktop-App unter Einstellungen, Claude Code, Remote Control, oder mit
  `claude remote-control` im Terminal in diesem Ordner.
- Konto-Skills `tycetoolz-hook-generator` und `lexware-office` in den claude.ai-Einstellungen
  durch die Repo-Fassung ersetzen.

```text
Zuarbeit am Mac für die Cloud, nach dem Skill tycetoolz-uebergabe und AGENTS.md. Antworten, Commits und Doku auf Deutsch.

Grundregeln für diese ganze Sitzung:
- Keine Zugangsdaten, Tokens, Schlüssel oder Passwörter ausgeben, protokollieren, committen oder in den Chat schreiben. Nur vorhanden/fehlt, Länge oder ok/Fehler melden.
- data/env.enc, .env, der Schlüsselbund-Schlüssel und config/ bleiben auf dem Mac. Sie werden weder nach HiDrive noch nach GitHub kopiert.
- Nichts löschen, verschieben oder überschreiben, weder am Mac noch in HiDrive. Originalbelege bleiben unverändert.
- Lexware, eBay, Amazon, PayPal und ELSTER werden nicht angefasst.
- Bricht ein Schritt ab, den Fehler ohne Werte melden und mit dem nächsten Schritt weitermachen.

1. Stand holen
   - Übergabe lesen: UEBERGABE.md, oberste drei Abschnitte.
   - git status. Sind fremde Änderungen offen, nichts zurücksetzen und nichts committen. Dann nur melden und bei Schritt 2 weitermachen.
   - Sonst: git pull origin main. Erwartet wird mindestens der Commit 65d74b4. Den Kopf-Commit (git log --oneline -1) melden.

2. openssl für env.enc klären (für die spätere Umstellung in agents/utils/env_loader.py, Abschnitt 2 der Sicherheitsprüfung)
   - which -a openssl, dazu openssl version für jeden Treffer, auch /usr/bin/openssl. Falls vorhanden, auch $(brew --prefix)/bin/openssl.
   - Den Schlüssel wie env_loader.py aus dem Schlüsselbund holen. Er darf nie in einer Befehlszeile stehen.
   - Für jedes gefundene openssl prüfen, ob es data/env.enc mit den Parametern aus env_loader.py (aes-256-cbc, -pbkdf2, -iter 100000) entschlüsselt. Den Schlüssel dabei über -pass stdin oder -pass fd:N übergeben, die Ausgabe nach /dev/null. Melden: Pfad, Version, entschlüsselt ja/nein, -pass stdin geht ja/nein, -pass fd:N geht ja/nein.
   - Nichts an env.enc ändern und keine neue Verschlüsselung erzeugen.

3. eBay-Refresh-Token prüfen (für die Cloud)
   - /usr/bin/python3 scripts/mac/cloud_zugangsdaten_export.py ohne Zusatz ausführen. Melden: Quelle, Anzahl vorhanden/fehlt, und ob bei EBAY_REFRESH_TOKEN der Hinweis „enthält #“ erscheint.
   - Die Länge von EBAY_REFRESH_TOKEN aus der Quelle melden, nur als Zahl, ohne Wert.
   - Nicht --zwischenablage und nicht --anzeigen aufrufen. Das Eintragen in der Cloud-Umgebung macht der Inhaber selbst.

4. Belege nach HiDrive hochladen
   Freigabe des Inhabers für genau diesen Schritt: FREIGABE HIDRIVE-UPLOAD BELEGE 2026-11-02. Sie gilt nur für das Hochladen in den neuen Zielordner unten, ohne Überschreiben und ohne Löschen.
   a) Bestand feststellen, nur lesend:
      - Zentrale Belegablage: python3 -c "from agents.storage.document_layout import document_root; print(document_root())". Melden: Pfad, Anzahl Dateien, Größe in MB, ob _register/belegregister.csv vorhanden ist und wie viele Zeilen sie hat.
      - Repo-Ordner buchhaltung/: Anzahl Dateien und Größe in MB.
      - Zusammen mehr als 2 GB oder einer der Ordner fehlt: hier anhalten und den Inhaber fragen.
   b) Prüfliste erzeugen: Für jede Datei beider Bestände relativer Pfad, Größe und SHA-256. Als CSV ablegen unter ~/Documents/tycetoolz-hidrive-upload-2026-11-02/ (außerhalb des Repos, nicht committen). Dateinamen können Kundennamen enthalten und gehören deshalb nicht nach Git und nicht in den Chat.
   c) Hochladen mit agents/storage/hidrive_agent.py (Zugang über load_secure_env):
      - Zentrale Ablage nach  <HiDrive-Home>/tycetoolz/belegablage/  mit unveränderter Ordnerstruktur.
      - Repo-Ordner buchhaltung/ nach  <HiDrive-Home>/tycetoolz/repo-buchhaltung/  mit unveränderter Ordnerstruktur.
      - Nur in diese neuen Ordner. Die vorhandenen HiDrive-Ordner buchhaltung, buchhaltung(1), tycetoolz-agents und backups nicht anfassen.
      - Kein on_exist="overwrite" und kein "autoname". Gibt es eine Datei schon, wird sie übersprungen und als Konflikt gemeldet. Vorher in der HiDrive-API-Doku oder im Code prüfen, welcher Wert das bewirkt.
      - .DS_Store auslassen und die Zahl melden. Alles andere unverändert hochladen.
      - Beide Prüflisten-CSVs zusätzlich nach <HiDrive-Home>/tycetoolz/ hochladen, damit die Cloud die SHA-256 gegenprüfen kann.
   d) Kontrolle: Je Zielordner Anzahl und Gesamtgröße in HiDrive gegen die Prüfliste vergleichen. Melden: hochgeladen, übersprungen, Konflikte, Fehler, Abweichung 0 ja/nein.
   e) Falls hidrive_agent beim Token-Abruf einen neuen Refresh-Token speichert (Rotation): melden, ohne Wert. Dann muss der Inhaber HIDRIVE_REFRESH_TOKEN auch in der Cloud-Umgebung ersetzen.

5. Tests
   - /usr/bin/python3 -m pytest tests/ -q -p no:cacheprovider. Ergebnis mit Zahlen melden. Nichts reparieren.

6. Übergabe schreiben, committen und pushen
   - In UEBERGABE.md oben einen neuen Abschnitt einfügen: „# <Agent>, 02.11.2026: Zuarbeit am Mac für die Cloud“ mit den Ergebnissen der Schritte 1–5. Nur Zahlen, Pfade der Zielordner, Versionen und ja/nein. Keine Dateinamen aus den Belegen, keine Werte. Danach genau ein nächster Schritt: Die Cloud prüft die SHA-256 gegen die Prüfliste und die eBay-Anmeldung.
   - Nur UEBERGABE.md committen, auf einem eigenen Branch (nicht main): git switch -c mac/zuarbeit-cloud-2026-11-02
     git commit -m "Übergabe: Zuarbeit am Mac für die Cloud (openssl, eBay-Token, HiDrive-Upload)" -m "Co-Authored-By: <Agent und Modell> <noreply@anthropic.com>"
   - git push -u origin mac/zuarbeit-cloud-2026-11-02, dann einen Pull Request als Entwurf gegen main anlegen. Nicht selbst zusammenführen.
   - Kontrolle: git show --stat HEAD enthält nur UEBERGABE.md, git status ist sauber.

Am Ende eine kurze Tabelle mit Stand, openssl, eBay-Token, HiDrive-Upload, Tests und Commit, jeweils mit Status und Befund, dazu der Link zum Pull Request. Danach anhalten.
```
