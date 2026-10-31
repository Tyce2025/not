# Prompt: Arbeitsbeginn-Prüfung mit Übergabe-Commit

Für eine lokale Claude-Code-Sitzung im Repository `tycetoolz-agents` auf dem Mac.
Schritte 1–4 sind rein lesend; einzige Änderung ist die Übergabe in Schritt 5.

```text
Arbeitsbeginn nach dem Skill tycetoolz-uebergabe. Schritte 1–4 sind rein lesend: nichts ändern, nichts buchen, nichts an Lexware, eBay oder Amazon schreiben. Die einzige erlaubte Änderung ist die Übergabe in Schritt 5.

1. Übergabe lesen
   - AGENTS.md bzw. CLAUDE.md, dann UEBERGABE.md in der Repo-Wurzel.
   - Die Bereichsübergaben aus der Bereichstabelle, mindestens docs/handoffs/buchhaltung/AKTUELL.md.
   - git log --oneline -15 und git ls-tree -r --name-only HEAD. Nacktes git status ist hier unzuverlässig (eigener Index). Fremde offene Änderungen nur melden, nicht anfassen.
   - Die Übergabe gegen Git abgleichen: Gibt es die genannten Commits und Dateien? Sind die genannten Prüfberichte unter docs/regelpruefung/ vorhanden?
   - Zusammenfassen: geprüfter Gesamtstand, je Bereich Stand und genau ein nächster Schritt, offene Freigaben, Abweichungen zwischen Übergabe und Git. Zahlen aus der Übergabe als „laut Übergabe vom …“ kennzeichnen, nicht als aktuell ausgeben.

2. Skills auflisten
   - Die verfügbaren Skills nennen, gruppiert nach Buchhaltung, Plattformen (eBay/Amazon/Lexware) und Sonstiges.
   - Zusätzlich die gepflegten Fassungen unter skills/ im Repo mit den geladenen vergleichen und Abweichungen melden.

3. Tests ausführen
   - Den Testbefehl selbst ermitteln (README, pyproject.toml, package.json, Makefile, tests/) und vollständig ausführen.
   - Ergebnis mit Anzahl bestanden/fehlgeschlagen/übersprungen und der Ausgabe der Fehlschläge berichten. Nichts reparieren, nichts überspringen.

4. Zugänge lesend prüfen
   - Lexware Office API, eBay API, Amazon SP-API: nur Abrufe, die keine Daten ändern (z. B. Profil, Token-Gültigkeit, eine Liste mit Limit 1).
   - Je Zugang berichten: erreichbar ja/nein, Anmeldung gültig ja/nein, Ablaufdatum von Token oder Secret, falls ablesbar.
   - Keine Secrets, Tokens oder Kundendaten ausgeben, nur vorhanden/fehlt/abgelaufen.
   - Zusätzlich: Hat das Repo ein Remote (git remote -v)? Ist .env in .gitignore, und ist buchhaltung/* weiterhin ignoriert?

5. Übergabe schreiben und committen
   - In docs/handoffs/buchhaltung/AKTUELL.md einen neuen Abschnitt oben einfügen:
     „## <Datum> — Arbeitsbeginn-Prüfung (Claude)“ mit:
     - Auftrag: lesende Prüfung von Übergabe, Skills, Tests und Zugängen, keine Freigabe erteilt oder verbraucht.
     - Geprüfter Commit (git rev-parse HEAD vor der Prüfung).
     - Befund Übergabe ↔ Git: Abweichungen konkret benennen. Eine abweichende Übergabe wird als „Berichtigung“ ausgewiesen, alte Abschnitte nicht überschreiben oder löschen.
     - Skills: Anzahl und Abweichungen zwischen Repo-Fassung und geladener Fassung.
     - Tests: Befehl und Ergebnis mit Zahlen. Fehlschläge mit Testnamen.
     - Zugänge: Tabelle Zugang | erreichbar | Anmeldung gültig | Ablauf. Keine Secrets.
     - Offene Punkte.
     - Genau ein nächster Schritt.
   - UEBERGABE.md nur anfassen, wenn sich der Gesamtstatus geändert hat (z. B. Zugang abgelaufen, Tests rot). Dann die Bereichszeile fortschreiben; einen bestehenden „## Letzte Übergabe — …“ zu „## Vorherige Übergabe — …“ umbenennen, nicht löschen.
   - Nur diese Übergabedateien committen, auf dem Weg aus dem Protokoll:
       export GIT_INDEX_FILE=/tmp/idx_$$
       git read-tree HEAD
       git add -- docs/handoffs/buchhaltung/AKTUELL.md [UEBERGABE.md nur falls geändert]
       git -c user.name="Dawid Cwielong" -c user.email="info@tycetoolz.de" \
           commit -m "übergabe: Arbeitsbeginn-Prüfung <Datum> (Übergabe, Skills, Tests, Zugänge)" \
           -m "Co-Authored-By: Claude <Modell> <noreply@anthropic.com>"
   - Danach zurückgebliebene Locks nach .git/_stale/ umbenennen (.git/HEAD.lock, .git/index.lock, .git/objects/**/tmp_obj_*). „unable to unlink“-Warnungen sind normal.
   - Kontrolle: git rev-parse HEAD zeigt den neuen Commit, git show --stat HEAD enthält nur die Übergabedateien, git show HEAD:docs/handoffs/buchhaltung/AKTUELL.md zeigt den neuen Abschnitt oben.
   - Nicht pushen, keine Sicherung anlegen. Das Repo hat kein Remote, und dafür gibt es keinen Auftrag.

Am Ende eine kurze Tabelle mit Übergabe, Skills, Tests, Zugänge und Commit, jeweils Status plus Befund, mit dem Hash des Übergabe-Commits. Danach anhalten und auf meine Anweisung warten.
```
