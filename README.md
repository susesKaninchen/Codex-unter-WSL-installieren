# Codex (OpenAI) – Installation unter Windows mit WSL

Diese Anleitung ist **extra langsam & detailliert** geschrieben. Du musst nichts über Linux wissen. Folge den Schritten **der Reihe nach**. Am Ende startest du Codex zum ersten Mal.

---

## 0) Was wir gleich machen

* **WSL** installieren (das ist „Linux in Windows“). Wir nehmen **Ubuntu**.
* Ein paar **Grundbegriffe** (Wie öffne ich eine Konsole? Wie kopiere ich Befehle?).
* **Ubuntu** aktualisieren und **wichtige Werkzeuge** installieren (Python, Git, Compiler, nützliche Tools).
* **Node.js** installieren (braucht Codex).
* **Codex CLI** installieren und **anmelden**.
* **Erster Test** + **VS Code Terminal** benutzen.

> **Tipp:** Befehle kannst du markieren und **kopieren** (`Strg + C`) und in die Konsole **einfügen** (`Strg + V`). In Ubuntu/WSL geht auch Rechtsklick → Einfügen.

---

## 1) WSL & Ubuntu installieren (einmalig)

### 1.1 Konsole als Administrator öffnen

1. Klicke unten links auf die **Start‑Taste** (Windows‑Logo).
2. Tippe **„Terminal“** oder **„PowerShell“**.
3. Rechtsklick auf **Windows Terminal** (oder **PowerShell**) → **Als Administrator ausführen**.
4. Frage der Benutzerkontensteuerung (**„Möchten Sie zulassen…“**) mit **Ja** bestätigen.

### 1.2 WSL installieren

In dem **blauen** (Admin‑)Fenster tippst/fügt du ein und drückst **Enter**:

```powershell
wsl --install -d Ubuntu
```

* Das aktiviert WSL und lädt **Ubuntu** herunter.
* **Wenn eine Neustart‑Aufforderung kommt**: bitte **neu starten**.

### 1.3 Ubuntu das erste Mal starten

1. Nach dem Neustart: **Startmenü** öffnen → **Ubuntu** tippen → **Ubuntu** anklicken.
2. Es erscheint ein schwarzes Fenster. **Beim ersten Start** richtet Ubuntu sich ein (1–2 Minuten).
3. Du wirst nach einem **Benutzernamen** gefragt (z. B. `marco`) und einem **Passwort**.

   * Beim Tippen des Passworts **siehst du keine Zeichen** – das ist normal. Tippen, **Enter**.

### 1.4 Prüfen, ob WSL 2 aktiv ist (optional, gut zu wissen)

In **Ubuntu** (dem schwarzen Fenster) eingeben:

```bash
wsl.exe -l -v
```

In der Liste sollte bei **Ubuntu** die Version **2** stehen. Falls nicht:

```powershell
# im Windows- (Admin-)Terminal
wsl --set-version Ubuntu 2
```

---

## 2) Mini‑Crashkurs Konsole

* **`pwd`** zeigt, wo du gerade bist.
* **`ls`** listet Dateien/Ordner. Mit `ls -la` siehst du mehr Details.
* **`cd ORDNERNAME`** wechselt in einen Ordner, **`cd ..`** geht eins zurück, **`cd ~`** geht in dein „Zuhause“.
* **Windows‑Laufwerke** sind in Ubuntu unter **`/mnt`**. Beispiel: `C:` = **`/mnt/c`**.

  * In deinen Windows‑Benutzerordner kommst du so:

    ```bash
    cd /mnt/c/Users/DEINNAME
    ```

---

## 3) Ubuntu aktualisieren & Grund‑Werkzeuge installieren

Alle folgenden Befehle bitte **in Ubuntu** ausführen (nicht in PowerShell):

```bash
# Paketlisten aktualisieren & Updates installieren
sudo apt update && sudo apt upgrade -y

# Nützliche Tools, Compiler, Python & Co.
sudo apt install -y \
  build-essential pkg-config make \
  git curl wget unzip zip \
  ripgrep jq tree fzf fd-find \
  python3 python3-venv python3-pip python-is-python3 pipx \
  nano vim ca-certificates gnupg lsb-release

# Komfort: aus "fdfind" wird der gewohnte Befehl "fd"
sudo ln -s "$(command -v fdfind)" /usr/local/bin/fd 2>/dev/null || true
```

> **Warum das?** Damit Codex Dateien finden/anzeigen darf, kleine Programme bauen kann (Compiler) und **Python** verfügbar ist.

---

## 4) Node.js (LTS) installieren – benötigt für Codex

Wir nehmen die einfache, stabile Variante.

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
node -v
npm -v
```

Die letzten beiden Zeilen zeigen dir die Versionen – **keine Fehlermeldung** = alles gut.

> **Hinweis:** Wenn `curl` „command not found“ sagt, hast du Schritt 3 noch nicht ausgeführt.

---

## 5) Codex CLI installieren

```bash
npm install -g @openai/codex
# Test
codex --version
```

> Falls eine **„EACCES/Permission denied“**‑Meldung erscheint, probiere einmal:
>
> ```bash
> sudo npm install -g @openai/codex
> ```

---

## 6) Bei Codex anmelden

Starte Codex:

```bash
codex
```

* Wähle **„Sign in with ChatGPT“** und folge dem Browser‑Login.
* Öffnet sich kein Browser? Installiere den Brücken‑Helfer und versuche es erneut:

  ```bash
  sudo apt install -y wslu
  ```

  Danach erneut `codex` starten. (Zur Not kannst du Anmelde‑Links auch mit `wslview URL` öffnen.)

**Alternative (API‑Key):**

```bash
export OPENAI_API_KEY="DEIN-KEY-HIER"
echo 'export OPENAI_API_KEY="DEIN-KEY-HIER"' >> ~/.bashrc && source ~/.bashrc
```

---

## 7) Erster Test (Hello Codex)

1. **Testordner** anlegen und hineingehen:

   ```bash
   mkdir -p ~/code/codex-test && cd ~/code/codex-test
   ```
2. **Codex starten**:

   ```bash
   codex
   ```
3. Beispiel‑Aufgabe eingeben (Deutsch ist okay):

   > *„Lege eine Datei `hello.py` an, die `Hallo aus Codex!` ausgibt und führe sie aus.“*

   Du siehst, welche Schritte Codex plant. Bestätige, wenn du gefragt wirst.

---

## 8) VS Code mit WSL benutzen (optional, aber bequem)

### 8.1 VS Code installieren & öffnen

* Installiere **Visual Studio Code** unter Windows (falls noch nicht vorhanden).
* Öffne **VS Code**.

### 8.2 In VS Code ein Terminal öffnen

* Menü: **Ansicht → Terminal** (oder **Strg + \`**).
* Rechts oben im Terminal ist ein kleines Drop‑down. Wähle dort **„Ubuntu (WSL)“**.

  * Fehlt die Option? Installiere in VS Code die Erweiterung **„WSL“** (Microsoft) und starte VS Code neu.

### 8.3 Einen Ordner aus WSL in VS Code öffnen

* In Ubuntu in den gewünschten Ordner wechseln (z. B. `~/code/codex-test`) und eingeben:

  ```bash
  code .
  ```

  Beim ersten Mal installiert VS Code automatisch den **Server** in WSL. Bestätige eventuelle Hinweise.

> **Merke:** Befehle im **VS‑Code‑Terminal (Ubuntu/WSL)** sind **genau dieselben** wie im separaten Ubuntu‑Fenster.

### 8.4 WSL aus jedem Terminal/Editor starten (Tipp)

Du kannst WSL auch direkt aus **jedem** Terminal starten: Windows Terminal, PowerShell, Eingabeaufforderung (cmd) **und** die integrierten Terminals in VS Code, JetBrains & Co.

```powershell
wsl -d Ubuntu
```

* Verfügbare Linux‑Distributionen anzeigen: `wsl -l -v`
* Eine bestimmte Distribution starten: `wsl -d <Name>` (z. B. `wsl -d Ubuntu`)
* Einen einzelnen Befehl in WSL ausführen (ohne eine Shell offen zu halten):

  ```powershell
  wsl -d Ubuntu -- echo Hallo aus WSL
  ```
* Wenn `wsl` **nicht gefunden** wird: Öffne ein **Windows‑Terminal (Admin)** und führe einmalig `wsl --install` aus (siehe Schritt 1).

---

## 9) Häufige Stolpersteine (mit schnellen Lösungen)

* **„wsl: Befehl nicht gefunden“ in PowerShell** → Du bist nicht im **Admin‑Terminal** oder Windows ist zu alt. Öffne **Windows Terminal (Admin)** und führe `wsl --install` aus. Danach neu starten.
* **„Ubuntu startet nicht“** → Windows einmal **neu starten**. Danach im Startmenü erneut **Ubuntu** öffnen.
* **„node: command not found“** → Schritt 4 nochmal ausführen.
* **„codex: command not found“** → Schritt 5 erneut ausführen. Prüfe mit `npm -v`, ob npm vorhanden ist.
* **Browser öffnet sich nicht** → `sudo apt install -y wslu` und Login erneut versuchen.
* **WSL neu starten** (hilft oft):

  ```powershell
  # in Windows (kein Ubuntu)
  wsl --shutdown
  ```

  Danach **Ubuntu** neu öffnen.

---

## 10) Nächste Schritte

Codex ist jetzt bereit. Als Nächstes erstellen wir eine **Benutzungs‑Anleitung** (Prompts, Freigabe‑/Approval‑Modi, sinnvolle Workflows) und anschließend **Agents** (AGENTS.md) mit Beispielen.

---

### Bonus: Kurze Befehl‑Übersicht

```text
pwd              # aktueller Ordner
ls               # Dateien/Ordner anzeigen
cd NAME          # in Ordner wechseln
cd ..            # einen Ordner zurück
cd ~             # ins Home-Verzeichnis
mkdir NAME       # Ordner anlegen
cat DATEI        # Datei anzeigen
python3 DATEI.py # Python-Skript ausführen
node -v / npm -v # Versionen prüfen
```

---

# Teil 2: Benutzung, Best‑Practices & Agents (für Anfänger)

## 1) Codex im Projekt starten (Schritt für Schritt)

1. **Projektordner öffnen** (in Ubuntu/WSL):

   ```bash
   cd /pfad/zu/deinem/projekt
   ```

   *Tipp:* Falls dein Code auf **C:** liegt, kommst du so hin: `cd /mnt/c/Users/DEINNAME/…`
2. **Codex starten**:

   ```bash
   codex
   ```
3. **Erstes Gespräch**:

   * Zum **nur Lesen/Planen**: Tippe `/approvals` und wähle **Read Only** (Lesemodus).
   * Bitte Codex z. B.: *„Gib mir einen Überblick über dieses Projekt. Welche Pakete werden verwendet? Welche Ordner sind wichtig?“*
4. **Gezielt arbeiten**: Stelle immer **eine konkrete Aufgabe**:

   * *„Erstelle eine Datei `README.md` mit Kurzanleitung (Install, Start, Tests).“*
   * *„Finde alle Stellen mit `TODO:` und schlage eine Priorisierung vor.“*

> **Warum Read Only zuerst?** So lernst du Code & Struktur kennen, **ohne** dass direkt etwas geändert wird. Du schaltest später gezielt mehr Rechte frei.

---

## 2) Approval‑Modi & Sandbox verstehen

Codex hat verschiedene Freiheitsgrade ⇒ **du steuerst, wie viel automatisch passiert**:

* **Suggest (Vorschlagen)**: Codex **liest** Dateien und **schlägt** Edits/Kommandos vor. **Du bestätigst**.
* **Auto Edit (Auto‑Bearbeiten)**: Codex darf **Dateien schreiben/ändern**, fragt aber **vor Shell‑Kommandos**.
* **Full Auto (Vollautomatik)**: Codex **liest/schreibt** und **führt Kommandos** im **aktuellen Arbeitsordner** aus.

**Modus prüfen/ändern (im Chat):** `/approvals` öffnen und Modus wählen. Fürs Planen: **Read Only**. Für kleine Refactorings: **Auto Edit**. Für schnelle Prototypen: **Full Auto** (vorsichtig nutzen).

> **Sicherheit:** Aktionen sind auf den **aktuellen Projektordner** begrenzt. Netzwerkzugriffe und Arbeiten außerhalb des Ordners erfordern üblicherweise **deine Bestätigung**.

**Nicht‑interaktiv (Skript‑/CI‑Art):**

```bash
codex exec "fixe den fehlgeschlagenen CI‑Build"
```

---

## 3) Gute Prompts = bessere Ergebnisse (Rezepte)

**Allgemein:**

* **Kontext geben** (Framework, Version, Ordnerpfade, Ziel).
* **Akzeptanzkriterien** nennen (z. B. „Tests grün, Lint sauber, README aktualisiert“).
* **Klein starten**, dann erweitern (inkrementell).
* **Artefakte benennen** (Dateinamen, Ordner, Befehle).

**Vorlagen:**

* **Planen lassen**

  > *„Erstelle einen **Schritt‑für‑Schritt‑Plan**, um \<Ziel> umzusetzen. Lass uns erst den Plan prüfen, bevor du Änderungen vorschlägst.“*

* **Refactoring**

  > *„Refaktoriere `src/utils/date.ts` für bessere Lesbarkeit. **Keine Logikänderungen**. Schlage Unit‑Tests vor.“*

* **Fehler beheben**

  > *„Hier ist der Fehlerlog (unten). **Diagnose zuerst**, dann Fix‑Vorschlag. Nutze bestehende Patterns. Akzeptanz: `npm test` grün.“*

* **Dokumentation**

  > *„Erstelle eine `README.md`‑Sektion **Getting Started** (Install, Start, Tests). Beziehe dich auf `package.json`‑Scripts.“*

**Do/Don’t kurz:**

* ✅ *Do:* konkrete Dateien nennen, Logs anhängen, Erwartungen klar formulieren.
* ❌ *Don’t:* „Mach es besser“ ohne Kontext; zu große Aufgaben in einem Rutsch; geheime Schlüssel posten.

---

## 4) Typische Workflows (mit Beispielen)

### 4.1 Feature umsetzen (sicher)

1. **Branch anlegen**

```bash
git checkout -b feature/login
```

2. **Plan im Read‑Only**: `/approvals` → Read Only → *„Plane die Implementierung eines einfachen Login‑Flows…“*
3. **Umsetzung**: `/approvals` → Auto Edit → *„Lege `src/auth/` an, implementiere …, passe Routen an.“*
4. **Tests & Lint**: *„Führe `npm test` aus und behebe Fehler. Dann `npm run lint`.“*
5. **Diff prüfen**

```bash
git status
git diff --stat
git diff
```

6. **Committen**

```bash
git add -A
git commit -m "feat(auth): basic login flow with tests"
```

### 4.2 Bugfix aus Log reproduzieren

* Im Chat: *„Hier ist der Stacktrace (unten). Reproduziere ihn lokal und schlage minimalen Fix vor.“*
* Danach `git add -p` nutzen, um Änderungen **teilweise** zu übernehmen.

### 4.3 Dokumentation generieren

* *„Lies `src/` und `package.json`. Erzeuge `README.md` mit **Install/Start/Tests** und einer **Ordnerübersicht**.“*

---

## 5) Git – der sichere Umgang (Anfänger)

**Einmalig konfigurieren** (falls noch nicht geschehen):

```bash
git config --global user.name "Dein Name"
git config --global user.email "you@example.com"
```

**Repository initialisieren** (falls neu):

```bash
git init
```

**Status & Änderungen ansehen**

```bash
git status
git diff [DATEI]
```

**Ausgewählt übernehmen**

```bash
git add -p   # interaktiv stagen
```

**Committen & Historie**

```bash
git commit -m "kurze, sinnvolle Nachricht"
git log --oneline --graph --decorate -n 10
```

**Remote setzen (GitHub) & Push**

```bash
git remote add origin https://github.com/DEINNAME/DEINREPO.git
git push -u origin HEAD
```

**Pull Request (optional, GitHub‑CLI):**

```bash
# gh vorher installieren und einloggen
gh pr create --fill --base main --head feature/login
```

**Mit Codex zusammen:** Bitte Codex, **Commit‑Nachrichten** oder eine **PR‑Beschreibung** vorzuschlagen (du bestätigst vor dem Schreiben).

---

## 6) AGENTS.md – der „Spickzettel“ für Agenten

**Was ist das?** Eine Datei `AGENTS.md` im **Projektwurzelordner**, die Agenten erklärt, **wie** mit deinem Projekt zu arbeiten ist (Setup, Regeln, Tests, Limits).

**Minimalbeispiel (`AGENTS.md`):**

```markdown
# AGENTS.md – Projektleitfaden

## Ziel & Kontext
- Web‑App für Workshop‑Anmeldung (Node 22, React 18, Vite)

## Setup & Start
- Install: `npm ci`
- Dev: `npm run dev`
- Test: `npm test`

## Architektur
- `src/` React‑App, `server/` Express‑API

## Code‑Regeln
- ESLint + Prettier; keine `any`; Funktionen < 50 Zeilen

## Tests
- Vitest; neue Features brauchen mind. 1 Test

## Grenzen & Sicherheit
- **Keine** direkten Änderungen unter `infra/` ohne Rücksprache
- Secrets über `.env.local` (niemals ins Repo)
```

**So nutzt du es:**

* Lege `AGENTS.md` **im Repo** an/ab. Starte Codex im Projektordner und sag explizit:

  > *„Lies die Datei `AGENTS.md` im Projekt und befolge die Regeln.“*
* Bei Unterordnern kannst du zusätzliche `AGENTS.md` anlegen (z. B. in `server/`). Weise Codex darauf hin:

  > *„Lies zusätzlich `server/AGENTS.md`. Priorisiere projektweite Regeln.“*

> **Hinweis:** Je nach Codex‑Version wird `AGENTS.md` **nicht immer automatisch** geladen. Nenne die Datei daher **aktiv** im ersten Prompt (z. B. „Lies AGENTS.md…“).

---

## 7) Nützliche Tipps aus der Praxis

* **Arbeite iterativ**: Erst **Plan**, dann kleine Schritte. Nach jedem Schritt `git status` prüfen.
* **Explizite Grenzen**: „Arbeite **nur** in `src/` und **ändere keine** Konfigurationen.“
* **Akzeptanztests**: „Das Ergebnis gilt als fertig, wenn `npm test` grün ist.“
* **Diffs erklären lassen**: „Zeige mir die **Diffs** und erkläre die Änderungen kurz, bevor wir committen.“
* **Performance & Sicherheit**: „Achte auf O(n) statt O(n^2) bei `processData`. Keine Secrets in Logs.“
* **VS Code**: Terminal auf **Ubuntu (WSL)** stellen; `code .` öffnet den Ordner grafisch.

---

## 8) Häufige Probleme beim Benutzen

* **„Warte auf Freigabe…“**: Öffne `/approvals` und erhöhe temporär den Modus (z. B. **Auto Edit**), oder bestätige einzelne Vorschläge manuell.
* **Login klemmt**: `wslview`/`wslu` installieren (siehe Installationsteil) und erneut anmelden.
* **Konfiguration**: Nutzer‑Einstellungen liegen (je nach Version) unter `~/.codex/` (z. B. `config.toml`).
* **Zu viele Änderungen**: Bitte um **Patch in Teilschritten** oder nutze `git add -p`.

---

## 9) Checkliste „Sicher arbeiten“

* [ ] Im **richtigen Ordner** gestartet?
* [ ] **Read Only** zum Erkunden genutzt?
* [ ] **AGENTS.md** vorhanden & erwähnt?
* [ ] **Tests** definiert und ausführbar?
* [ ] **Git‑Branch** für die Aufgabe erstellt?
* [ ] Änderungen geprüft (**git diff**) & nur Gewolltes committet?

–––

**Weiter geht’s gern mit konkreten Beispielen aus deinem Projekt (Stack, Ordnerstruktur, CI). Sag Bescheid, dann passe ich Prompts & `AGENTS.md` exakt darauf an.**
