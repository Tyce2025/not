# Prompt: Arbeitsbeginn-Prüfung mit Übergabe-Commit (Cloud)

Für eine Claude-Cloud-Sitzung (claude.ai/code) im Repository `Tyce2025/tycetoolz-agents`.
Schritte 1–4 sind rein lesend; einzige Änderung ist die Übergabe in Schritt 5, die als
eigener Branch gepusht wird.

Voraussetzungen der Sitzung (sonst bleiben Schritte offen und werden so berichtet):
- Repository mit Schreibzugriff angehängt (für Push und Pull Request).
- Paketinstallation erlaubt (`pip install -r requirements.txt`, sonst fehlt pytest).
- Zugangsdaten als Umgebungsvariablen der Cloud-Umgebung und freigegebene Domains nach
  `docs/cloud-arbeitsweise.md` Abschnitt 3 und 4. Neue Werte greifen erst in einer neuen Sitzung.

```text
Arbeitsbeginn nach dem Skill tycetoolz-uebergabe, in der Cloud. Schritte 1–4 sind rein lesend: nichts ändern, nichts buchen, nichts an Lexware, eBay, Amazon oder HiDrive schreiben. Die einzige erlaubte Änderung ist die Übergabe in Schritt 5.

1. Übergabe lesen
   - AGENTS.md (= CLAUDE.md), dann UEBERGABE.md in der Repo-Wurzel, dazu docs/cloud-arbeitsweise.md und docs/cloud-umzug-2026-10-31.md.
   - Die Bereichsübergaben aus der Bereichstabelle, mindestens docs/handoffs/buchhaltung/AKTUELL.md.
   - git status, git log --oneline -15, git branch -a und git ls-tree -r --name-only HEAD. Der Klon ist flach: Fehlt für den Abgleich Historie, mit git fetch --depth=200 origin main nachladen, nicht --unshallow.
   - Offene Pull Requests und Cloud-Branches (claude/*, codex/*) nennen, die noch nicht in main sind. Nur melden, nicht zusammenführen.
   - Die Übergabe gegen Git abgleichen: Gibt es die genannten Commits und Dateien? Sind die genannten Prüfberichte unter docs/regelpruefung/ vorhanden?
   - Zusammenfassen: geprüfter Gesamtstand, je Bereich Stand und genau ein nächster Schritt, offene Freigaben, Abweichungen zwischen Übergabe und Git. Zahlen aus der Übergabe als „laut Übergabe vom …“ kennzeichnen, nicht als aktuell ausgeben.

2. Skills auflisten
   - Die in dieser Sitzung verfügbaren Skills nennen, gruppiert nach Buchhaltung, Plattformen (eBay/Amazon/Lexware) und Sonstiges.
   - Die Ordner mit SKILL.md unter .claude/skills/ und skills/ zählen und mit den geladenen vergleichen: fehlende, zusätzliche und kaputte Verweise melden. Konto-Skills (anthropic-skills:*) getrennt ausweisen.

3. Tests ausführen
   - Falls pytest fehlt: pip install -r requirements.txt.
   - python3 -m pytest tests/ -q -p no:cacheprovider (immer mit Pfad tests/).
   - Ergebnis mit Anzahl bestanden/fehlgeschlagen/übersprungen und der Ausgabe der Fehlschläge berichten. Übersprungene Tests wegen fehlender Belege (Fixture lokaler_belegbestand) und fehlendem ~/.codex sind in der Cloud erwartet; getrennt ausweisen. Nichts reparieren, nichts überspringen.

4. Zugänge lesend prüfen
   - Umgebungsvariablen nach der Positivliste in scripts/mac/cloud_zugangsdaten_export.py: je Name nur vorhanden/fehlt. data/env.enc und .env gibt es in der Cloud nicht.
   - Erreichbarkeit der freigegebenen Domains aus docs/cloud-arbeitsweise.md Abschnitt 3: Antwortet der Dienst selbst oder der Proxy (403 vom Proxy = nicht freigegeben)?
   - Anmeldung nach Abschnitt 7, nur Abrufe ohne Datenänderung: Lexware GET /v1/profile; eBay Token-Erneuerung; Amazon LWA-Token und marketplaceParticipations; HiDrive whoami und Ordnerliste des Belegpfads.
   - HiDrive: Falls bei der Token-Erneuerung ein neuer Refresh-Token zurückkommt, das melden (Rotation, siehe Abschnitt 6) und den Wert nicht ausgeben.
   - Je Zugang berichten: erreichbar ja/nein, Anmeldung gültig ja/nein, Ablaufdatum von Token oder Secret, falls ablesbar (Amazon-Secret-Rotation laut CLAUDE.md fällig 20.03.2027).
   - Keine Secrets, Tokens oder Kundendaten ausgeben, nur vorhanden/fehlt/abgelaufen.
   - IMAP (imap.strato.de:993) ist aus der Cloud laut Übergabe vom 01.11.2026 nicht erreichbar; nur kurz bestätigen.
   - Zusätzlich: git remote -v zeigt origin auf Tyce2025/tycetoolz-agents? Ist .env in .gitignore, und ist buchhaltung/* weiterhin ignoriert (git check-ignore)?

5. Übergabe schreiben, committen und pushen
   - In docs/handoffs/buchhaltung/AKTUELL.md einen neuen Abschnitt oben einfügen:
     „## <Datum> — Arbeitsbeginn-Prüfung in der Cloud (Claude)“ mit:
     - Auftrag: lesende Prüfung von Übergabe, Skills, Tests und Zugängen, keine Freigabe erteilt oder verbraucht.
     - Geprüfter Commit (git rev-parse HEAD vor der Prüfung) und Branch.
     - Befund Übergabe ↔ Git: Abweichungen konkret benennen. Eine abweichende Übergabe wird als „Berichtigung“ ausgewiesen, alte Abschnitte nicht überschreiben oder löschen.
     - Skills: Anzahl und Abweichungen zwischen Repo-Fassung und geladener Fassung.
     - Tests: Befehl und Ergebnis mit Zahlen. Fehlschläge mit Testnamen, erwartete Cloud-Überspringer getrennt.
     - Zugänge: Tabelle Zugang | erreichbar | Anmeldung gültig | Ablauf. Keine Secrets.
     - Offene Punkte (auch: was nur der Inhaber erledigen kann, z. B. Domains, Zugangsdaten, Belege nach HiDrive).
     - Genau ein nächster Schritt.
   - UEBERGABE.md nur anfassen, wenn sich der Gesamtstatus geändert hat (z. B. Zugang jetzt gültig oder abgelaufen, Tests rot). Dann oben einen neuen Abschnitt ergänzen, bestehende Abschnitte nicht löschen.
   - Nur diese Übergabedateien committen, auf dem Branch dieser Sitzung (nicht direkt auf main):
       git add -- docs/handoffs/buchhaltung/AKTUELL.md [UEBERGABE.md nur falls geändert]
       git commit -m "übergabe: Arbeitsbeginn-Prüfung Cloud <Datum> (Übergabe, Skills, Tests, Zugänge)" \
           -m "Co-Authored-By: Claude <Modell> <noreply@anthropic.com>"
   - Pushen: git push -u origin <Sitzungsbranch>, danach Pull Request als Entwurf gegen main anlegen. Nicht selbst zusammenführen.
   - Kontrolle: git show --stat HEAD enthält nur die Übergabedateien, git show HEAD:docs/handoffs/buchhaltung/AKTUELL.md zeigt den neuen Abschnitt oben, git status ist sauber, der Branch ist auf origin.

Am Ende eine kurze Tabelle mit Übergabe, Skills, Tests, Zugänge und Commit, jeweils Status plus Befund, mit dem Hash des Übergabe-Commits und dem Link zum Pull Request. Danach anhalten und auf meine Anweisung warten.
```
