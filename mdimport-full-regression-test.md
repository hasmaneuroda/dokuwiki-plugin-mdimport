# mdimport Full Regression Test

Diese Datei dient als vollständiger Funktionstest für das DokuWiki-Plugin `mdimport`.

Ziel des Tests:
- Die Reihenfolge von Absätzen, Codeblöcken und Tabellen muss erhalten bleiben.
- Markdown-Inline-Code muss als DokuWiki-Monospace importiert werden.
- DokuWiki-Syntax innerhalb von Inline-Code darf nicht erneut interpretiert werden.
- Listen müssen als echte DokuWiki-Listen gerendert werden.
- Links, Bilder, Tabellen, Zitate, Überschriften, horizontale Linien und fenced code blocks müssen plausibel konvertiert werden.

---

## 1. Überschriften

# H1 Überschrift

## H2 Überschrift

### H3 Überschrift

#### H4 Überschrift

##### H5 Überschrift

Normaler Absatz nach mehreren Überschriften.

---

## 2. Absätze und Zeilenfluss

Dies ist ein normaler Absatz mit mehreren Sätzen. Er sollte als normaler Fließtext importiert werden.

Dies ist ein zweiter Absatz. Zwischen diesem und dem vorherigen Absatz muss ein Abstand bleiben.

Dieser Absatz enthält einen
manuellen Zeilenumbruch im Markdown-Quelltext,
soll aber trotzdem als zusammenhängender Absatz behandelt werden.

---

## 3. Fett, kursiv und Kombinationen

**Dieser Text ist fett.**

*Dieser Text ist kursiv.*

__Dieser Text ist ebenfalls fett.__

_Dieser Text ist ebenfalls kursiv._

Text mit **fetter Hervorhebung** mitten im Satz.

Text mit *kursiver Hervorhebung* mitten im Satz.

Text mit **fett und `inline_code` nebeneinander**.

Text mit *kursiv und `inline_code` nebeneinander*.

---

## 4. Einfacher Inline-Code

Der Dienst heißt `sshd.service`.

Der Pfad ist `/etc/ssh/sshd_config`.

Die Option heißt `PasswordAuthentication`.

Der Befehl lautet `systemctl status sshd.service`.

Der Host heißt `halloHorst`.

Der Webpfad lautet `/var/www/virtual/foo/wiki.foo.tld/`.

---

## 5. Kritische Zeichen innerhalb von Inline-Code

Alle folgenden Werte müssen im gerenderten Artikel exakt sichtbar bleiben.

Das darf nicht unterstrichen werden: `__init__`

Das darf nicht kursiv werden: `foo_bar`

Das darf nicht kursiv werden: `_literal_`

Das darf nicht formatiert werden: `*literal*`

Das darf nicht fett werden: `**literal**`

Das darf kein DokuWiki-Link werden: `[[not:a:link]]`

Das darf kein DokuWiki-Bild werden: `{{not-an-image.png}}`

Das darf kein HTML oder DokuWiki-Codeblock werden: `<code bash>`

Das darf keinen Codeblock schließen: `</code>`

Das darf keine DokuWiki-Überschrift werden: `====== Keine Überschrift ======`

Das enthält DokuWiki-Monospace-Zeichen: `foo''bar`

Das enthält Prozentzeichen: `100% sicher`

Das enthält Nowiki-ähnliche Zeichen: `%%nowiki%%`

---

## 6. Shell-typische Inline-Code-Fälle

Variable: `$HOME`

Parameter Expansion: `${STATE}`

Command Substitution: `$(hostname -f)`

Test-Ausdruck: `[ -e "$MARKER" ]`

printf-Ausdruck: `printf '%s\n' "$HOST"`

awk-Ausdruck: `awk '{print $1}'`

find-Ausdruck: `find . -name '*.md' -print0`

sed-Ausdruck: `sed 's/foo/bar/g'`

systemd-Unit: `reboot-required-mail.service`

systemd-Timer: `reboot-required-mail.timer`

---

## 7. Sonderzeichen im Inline-Code

Kleiner/größer: `a < b && b > c`

Ampersand: `foo & bar`

Doppelte Quotes: `"quoted"`

Einfache Quotes: `'single quoted'`

Pfad mit Doppelpunkt: `$PATH:/usr/local/bin`

Klammern: `(foo) [bar] {baz}`

URL mit Querystring: `https://example.com/path?foo=bar&baz=qux`

---

## 8. Links

Normaler externer Link:

[Uberspace Manual](https://manual.uberspace.de/)

DokuWiki-Dokumentation:

[DokuWiki Syntax](https://www.dokuwiki.org/wiki:syntax)

Link mit Unterstrich in der URL:

[OpenBSD sshd_config](https://man.openbsd.org/sshd_config)

Link mit Inline-Code im sichtbaren Text:

[`sshd_config` documentation](https://man.openbsd.org/sshd_config)

Inline-Code direkt vor einem Link: `sshd.service` [DokuWiki](https://www.dokuwiki.org/)

Link direkt nach Inline-Code ohne Satzende: `sshd_config`[OpenBSD](https://man.openbsd.org/sshd_config)

---

## 9. Bilder

Hinweis: Die Bilder müssen nicht existieren. Hier wird primär getestet, ob die Markdown-Bildsyntax plausibel in DokuWiki-Syntax umgewandelt wird.

![Beispielbild](example-image.png)

![Diagramm mit Unterstrich im Dateinamen](network_diagram.png)

![`Inline-Code` im Alt-Text](image-with-code-alt.png)

---

## 10. Ungeordnete Listen

- Erster Listenpunkt.
- Zweiter Listenpunkt mit `inline_code`.
- Dritter Listenpunkt mit **fetter Hervorhebung**.
- Vierter Listenpunkt mit [Link](https://example.com/).
- Fünfter Listenpunkt mit kritischem Inline-Code: `__init__`.

Nach der Liste muss dieser Satz wieder normaler Fließtext sein.

---

## 11. Geordnete Listen

1. Datei `reboot-required-mail` erstellen.
2. Rechte mit `chmod 0755` setzen.
3. Service `reboot-required-mail.service` erstellen.
4. Timer `reboot-required-mail.timer` aktivieren.
5. Mit `systemctl list-timers` prüfen.

Nach der nummerierten Liste muss dieser Satz wieder normaler Fließtext sein.

---

## 12. Verschachtelte Listen

- Host vorbereiten
  - Paket `msmtp` installieren
  - Datei `/etc/msmtprc` prüfen
  - Testmail mit `mail -s "Test" user@example.com` senden
- DokuWiki vorbereiten
  - `conf/userstyle.css` sichern
  - neue Datei hochladen
  - `data/cache/*` leeren
- Git vorbereiten
  - Branch `fix-inline-code-conversion` prüfen
  - Commit mit `git show --stat` prüfen
  - Push mit `git push` durchführen

---

## 13. Gemischte Listen

1. Erster geordneter Punkt
   - Unterpunkt mit `inline_code`
   - Unterpunkt mit `foo_bar`
2. Zweiter geordneter Punkt
   - Unterpunkt mit [Link](https://example.com/)
   - Unterpunkt mit `[[not:a:link]]`
3. Dritter geordneter Punkt

---

## 14. Blockquotes

> Dies ist ein einfaches Zitat.
> Es besteht aus mehreren Zeilen.

> Zitat mit Inline-Code: `/run/reboot-required`
> und mit kritischem Inline-Code: `__init__`.

> Zitat mit Link: [DokuWiki](https://www.dokuwiki.org/)

---

## 15. Horizontale Linien

Text vor der horizontalen Linie.

---

Text nach der horizontalen Linie.

---

## 16. Tabelle mit Inline-Code

| Zweck | Befehl | Erwartung |
|---|---|---|
| Dienst prüfen | `systemctl status sshd` | Status wird angezeigt |
| Port prüfen | `ss -tulpn` | Listener sichtbar |
| Datei prüfen | `test -e /run/reboot-required` | Exit-Code auswertbar |
| Hash bilden | `sha256sum /run/reboot-required.pkgs` | Prüfsumme wird ausgegeben |
| Log lesen | `journalctl -u reboot-required-mail.service` | Service-Log sichtbar |
| Kritischer Code | `__init__` | Unterstriche bleiben erhalten |
| Linktext | [sshd_config](https://man.openbsd.org/sshd_config) | Link bleibt korrekt |

Dieser Absatz steht direkt unter der Tabelle und darf nicht oberhalb der Tabelle erscheinen.

---

## 17. Reihenfolge: Absatz vor Codeblock

Dieser Satz steht oberhalb des Bash-Codeblocks und muss nach dem Import auch oberhalb des Codeblocks stehen.

```bash
echo "Dieser Codeblock muss unter dem vorherigen Satz stehen."
systemctl status sshd.service
```

Dieser Satz steht unterhalb des Bash-Codeblocks und muss nach dem Import auch unterhalb des Codeblocks stehen.

---

## 18. Reihenfolge: Absatz vor Tabelle

Dieser Satz steht oberhalb der Tabelle und muss nach dem Import auch oberhalb der Tabelle stehen.

| Name | Wert |
|---|---|
| Dienst | `sshd.service` |
| Pfad | `/etc/ssh/sshd_config` |

Dieser Satz steht unterhalb der Tabelle und muss nach dem Import auch unterhalb der Tabelle stehen.

---

## 19. Bash-Codeblock mit Syntaxhighlighting

```bash
#!/bin/sh
set -eu

TO="deine.adresse@example.com"
FROM="alerts@example.com"

MARKER="/run/reboot-required"
PKGS="/run/reboot-required.pkgs"
STATE="/run/reboot-required-mail.sent"

[ -e "$MARKER" ] || exit 0

HOST="$(hostname -f 2>/dev/null || hostname)"
DATE="$(date -Is)"

if [ -s "$PKGS" ]; then
    SIG="$(sha256sum "$PKGS" | awk '{print $1}')"
else
    SIG="$(stat -c '%Y:%s' "$MARKER")"
fi

if [ -e "$STATE" ] && [ "$(cat "$STATE")" = "$SIG" ]; then
    exit 0
fi

{
    printf 'From: %s\n' "$FROM"
    printf 'To: %s\n' "$TO"
    printf 'Subject: [%s] Reboot erforderlich nach Updates\n' "$HOST"
    printf 'Content-Type: text/plain; charset=UTF-8\n'
    printf '\n'
    printf 'Server: %s\n' "$HOST"
    printf 'Zeitpunkt: %s\n' "$DATE"
    printf '\n'
    printf 'Auf diesem System ist nach Paketupdates ein Reboot erforderlich.\n'
    printf '\n'

    if [ -s "$PKGS" ]; then
        printf 'Auslösende Pakete laut %s:\n\n' "$PKGS"
        cat "$PKGS"
    fi
} | sendmail -t

printf '%s\n' "$SIG" > "$STATE"
```

---

## 20. Bash-Codeblock mit Backticks

Dieser Codeblock muss die Backticks unverändert behalten:

```bash
echo `date`
echo "Heute ist: `date +%F`"
RESULT=`hostname -f`
printf '%s\n' "$RESULT"
```

---

## 21. systemd Service

```ini
[Unit]
Description=Send mail when reboot is required after package upgrades
ConditionPathExists=/run/reboot-required

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/reboot-required-mail
```

---

## 22. systemd Timer

```ini
[Unit]
Description=Check reboot-required marker periodically

[Timer]
OnBootSec=5min
OnUnitActiveSec=1h
Persistent=true

[Install]
WantedBy=timers.target
```

---

## 23. CSS-Codeblock

```css
:root {
    --gb-bg0-soft: #32302f;
    --gb-bg1: #3c3836;
    --gb-fg1: #ebdbb2;
}

div.dokuwiki pre.code {
    background: var(--gb-bg0-soft);
    color: var(--gb-fg1);
    border: 1px solid var(--gb-bg1);
}
```

---

## 24. PHP-Codeblock

```php
<?php

declare(strict_types=1);

private function convertInline(string $text): string
{
    return preg_replace_callback(
        '/(?<!\\\\)(`+)([^\r\n]*?)(?<!`)\1(?!`)/',
        static function (array $matches): string {
            return "''%%" . $matches[2] . "%%''";
        },
        $text
    );
}
```

---

## 25. YAML-Codeblock

```yaml
site:
  name: "mool wiki"
  theme: "gruvbox-dark-soft"
  features:
    syntax_highlighting: true
    markdown_import: true
    inline_code_conversion: true
```

---

## 26. JSON-Codeblock

```json
{
  "service": "sshd",
  "port": 44475,
  "password_authentication": false,
  "allowed_users": ["mool"],
  "paths": {
    "config": "/etc/ssh/sshd_config",
    "unit": "/etc/systemd/system/sshd.service"
  }
}
```

---

## 27. SQL-Codeblock

```sql
SELECT hostname, service, port
FROM services
WHERE service = 'sshd'
  AND port = 44475
ORDER BY hostname ASC;
```

---

## 28. Markdown als Codeblock

Dieser Abschnitt testet, ob Markdown innerhalb eines Codeblocks nicht konvertiert wird.

```markdown
# Diese Überschrift darf nicht als echte Überschrift gerendert werden

Der Dienst heißt `sshd.service`.

**Das darf nicht fett werden.**

[Dieser Link](https://example.com/) darf im Codeblock kein echter Link werden.
```

---

## 29. HTML-artige Inhalte

Inline-Code mit HTML-ähnlichem Text:

`<div class="example">`

`<code bash>echo test</code>`

Normales HTML im Fließtext:

<div>Dies ist ein rohes HTML-div im Markdown-Test.</div>

---

## 30. Sehr lange Zeile im Codeblock

```bash
rsync -av --delete --exclude='.git/' --exclude='resources/' --exclude='public/' /home/mool/02_areas/workstation/projects/wiki-content/ user@host.example:/var/www/virtual/foo/wiki.foo.tld/data/pages/
```

---

## 31. Gemischter Realwelt-Abschnitt

Wenn die Testmail durchgegangen ist, dann wird der Reboot-Notifier erstellt:

```bash
sudo vim /usr/local/sbin/reboot-required-mail
```

Danach wird der Service angelegt:

```ini
[Unit]
Description=Send reboot-required notification mail
ConditionPathExists=/run/reboot-required

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/reboot-required-mail
```

Zum Schluss wird der Timer aktiviert:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now reboot-required-mail.timer
systemctl list-timers | grep reboot
```

Dieser Absatz muss nach dem letzten Codeblock stehen.

---

## 32. Sichtprüfungs-Checkliste

Nach dem Import müssen folgende Punkte stimmen:

- `sshd.service` wird als Inline-Code angezeigt.
- `__init__` bleibt sichtbar mit beiden Unterstrichen.
- `foo_bar` bleibt sichtbar mit Unterstrich und wird nicht kursiv.
- `*literal*` bleibt sichtbar mit Sternchen und wird nicht kursiv.
- `**literal**` bleibt sichtbar mit Sternchen und wird nicht fett.
- `[[not:a:link]]` wird nicht als DokuWiki-Link interpretiert.
- `{{not-an-image.png}}` wird nicht als DokuWiki-Bild interpretiert.
- `<code bash>` wird nicht als DokuWiki-Codeblock interpretiert.
- Listen werden als echte Listen gerendert.
- Tabellen werden als echte Tabellen gerendert.
- Der Satz oberhalb eines Codeblocks bleibt oberhalb des Codeblocks.
- Der Satz oberhalb einer Tabelle bleibt oberhalb der Tabelle.
- Backticks innerhalb von Bash-Codeblöcken bleiben erhalten.
- Links mit Unterstrich wie `sshd_config` werden nicht zu `sshd//config`.
- Fenced code blocks bekommen weiterhin Syntaxhighlighting.

---

## 33. Bekannter Edge Case: Multi-Backtick-Inline-Code

Dieser Abschnitt ist optional. Er testet CommonMark-Code-Spans mit mehrfachen Backticks. Falls dieser Abschnitt nicht korrekt importiert wird, bedeutet das nicht zwingend, dass der aktuelle Fix kaputt ist, weil vollständige CommonMark-Code-Span-Unterstützung nicht zwingend Teil des aktuellen Plugin-Fixes ist.

Sauberer Multi-Backtick-Code-Span:

`` echo `date` ``

Weiterer Multi-Backtick-Code-Span:

`` printf '`literal backtick`' ``

Noch ein Multi-Backtick-Code-Span:

`` command with `inner` backticks ``
