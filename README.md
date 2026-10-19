[README.md](https://github.com/user-attachments/files/33665270/README.md)
# Elevator Status / Aufzug-Status

**English** | [Deutsch](#deutsch)

Live site / Live-Seite: https://kaethe-leichter.github.io/elevator/

---

## English

### What is this?

A tiny website that tells you one thing: **does the elevator work right now?**

Open the page and read the answer:

- **YES** (green): the elevator is working.
- **NO** (red): the elevator is out of order.

No login, no app, nothing to install. Just check https://kaethe-leichter.github.io/elevator/ before you go.

### Pages

| Page | Purpose |
| --- | --- |
| `index.html` | The answer (YES or NO) and since when it has been like this. Refreshes automatically. |
| `stats.html` | History: current status, how long it has been active, last change, number of changes, share of working time this month, 12-month chart and a yearly overview. |
| `admin.html` | Password-protected switch for the facility team to change the status. |

### For the facility team: changing the status

1. Open `admin.html` (https://kaethe-leichter.github.io/elevator/admin.html).
2. Enter the password you received by email.
3. Use the switch. Everyone sees the new status within 1 to 2 minutes.

### How it works

There is no server. The status is stored in `data/history.json`, a list of changes with timestamps:

```json
[
  { "status": false, "at": "2026-10-19T09:31:03Z" },
  { "status": true,  "at": "2026-10-20T08:00:00Z" }
]
```

- The current status is the last entry. All statistics are calculated in the browser from this list.
- The admin page adds an entry by committing to this repository through the GitHub API, using a GitHub token.
- The token is stored encrypted (AES-256-GCM, key derived from the password with PBKDF2) in `assets/vault.js`. Entering the correct password decrypts it in the browser.
- The public pages read the static files served by GitHub Pages, so a change appears once Pages has rebuilt, usually within 1 to 2 minutes.

### Setup from scratch

1. Create a public repository and upload all files, keeping the `assets` and `data` folders. The default branch must be `main`.
2. Go to Settings, Pages, choose "Deploy from a branch", then `main` and `/ (root)`.
3. Create a fine-grained token (GitHub, Settings, Developer settings, Personal access tokens), limited to this repository, with **Contents: Read and write** and the longest expiry available.
4. Upload `setup.html`, open it on the live site, paste the token and choose the admin password. It saves `assets/vault.js`. Then delete `setup.html`.
5. Test `admin.html` with the password.

### Customising

- **Labels:** change `yes` and `no` in `CFG` at the top of `assets/common.js` (for example `JA` and `NEIN`).
- **Colours and layout:** edit `assets/style.css`.
- **Custom domain:** set `CFG.owner` and `CFG.repo` in `assets/common.js`.

### Maintenance

- **Token expiry:** when the token expires, the switch fails with "Write failed". Create a new token and run `setup.html` again.
- **Changing the password:** run `setup.html` again.
- **Resetting the history:** replace `data/history.json` with a single entry, for example `[{"status": true, "at": "2026-10-19T00:00:00Z"}]`.

### Limitations

- The password check happens in the browser. The encrypted token is public in the repository, so a determined person could try to guess the password offline. The token only has access to this repository.
- If two people toggle at the same moment, the second attempt fails and can be repeated.
- The history is visible to anyone who can open the repository.

---

## Deutsch

### Was ist das?

Eine kleine Website, die nur eines verrät: **Funktioniert der Aufzug gerade?**

Seite öffnen und die Antwort ablesen:

- **YES** (grün): Der Aufzug funktioniert.
- **NO** (rot): Der Aufzug ist außer Betrieb.

Kein Login, keine App, keine Installation. Einfach vorher https://kaethe-leichter.github.io/elevator/ ansehen.

### Seiten

| Seite | Zweck |
| --- | --- |
| `index.html` | Die Antwort (YES oder NO) und seit wann das so ist. Aktualisiert sich automatisch. |
| `stats.html` | Verlauf: aktueller Status, wie lange er aktiv ist, letzte Änderung, Anzahl der Änderungen, Betriebsanteil im aktuellen Monat, 12-Monats-Diagramm und Jahresübersicht. |
| `admin.html` | Passwortgeschützter Schalter, mit dem das Facility Management den Status ändert. |

### Für das Facility Management: Status ändern

1. `admin.html` öffnen (https://kaethe-leichter.github.io/elevator/admin.html).
2. Das Passwort eingeben, das per E-Mail gesendet wurde.
3. Den Schalter umlegen. Alle sehen den neuen Status innerhalb von 1 bis 2 Minuten.

### So funktioniert es

Es gibt keinen Server. Der Status steht in `data/history.json`, einer Liste von Änderungen mit Zeitstempel:

```json
[
  { "status": false, "at": "2026-10-19T09:31:03Z" },
  { "status": true,  "at": "2026-10-20T08:00:00Z" }
]
```

- Der aktuelle Status ist der letzte Eintrag. Alle Statistiken werden im Browser aus dieser Liste berechnet.
- Die Admin-Seite fügt einen Eintrag hinzu, indem sie über die GitHub-API in dieses Repository schreibt. Dafür wird ein GitHub-Token verwendet.
- Das Token liegt verschlüsselt (AES-256-GCM, Schlüssel per PBKDF2 aus dem Passwort abgeleitet) in `assets/vault.js`. Mit dem richtigen Passwort wird es im Browser entschlüsselt.
- Die öffentlichen Seiten lesen die statischen Dateien von GitHub Pages. Eine Änderung erscheint daher, sobald Pages neu gebaut hat, meist nach 1 bis 2 Minuten.

### Einrichtung von Grund auf

1. Ein öffentliches Repository erstellen und alle Dateien hochladen, mit den Ordnern `assets` und `data`. Der Standard-Branch muss `main` heißen.
2. Unter Settings, Pages "Deploy from a branch" wählen, dann `main` und `/ (root)`.
3. Ein Fine-grained Token erstellen (GitHub, Settings, Developer settings, Personal access tokens), nur für dieses Repository, mit **Contents: Read and write** und der längsten möglichen Laufzeit.
4. `setup.html` hochladen, auf der Live-Seite öffnen, das Token einfügen und das Admin-Passwort wählen. Die Seite speichert `assets/vault.js`. Danach `setup.html` löschen.
5. `admin.html` mit dem Passwort testen.

### Anpassen

- **Beschriftung:** `yes` und `no` in `CFG` am Anfang von `assets/common.js` ändern (zum Beispiel `JA` und `NEIN`).
- **Farben und Layout:** `assets/style.css` bearbeiten.
- **Eigene Domain:** `CFG.owner` und `CFG.repo` in `assets/common.js` setzen.

### Wartung

- **Token abgelaufen:** Läuft das Token ab, schlägt der Schalter mit "Write failed" fehl. Ein neues Token erstellen und `setup.html` erneut ausführen.
- **Passwort ändern:** `setup.html` erneut ausführen.
- **Verlauf zurücksetzen:** `data/history.json` durch einen einzelnen Eintrag ersetzen, zum Beispiel `[{"status": true, "at": "2026-10-19T00:00:00Z"}]`.

### Einschränkungen

- Die Passwortprüfung findet im Browser statt. Das verschlüsselte Token ist im Repository öffentlich, daher könnte jemand versuchen, das Passwort offline zu erraten. Das Token hat nur Zugriff auf dieses Repository.
- Schalten zwei Personen gleichzeitig um, schlägt der zweite Versuch fehl und kann wiederholt werden.
- Der Verlauf ist für alle sichtbar, die das Repository öffnen können.
