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
