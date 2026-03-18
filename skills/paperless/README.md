# Paperless-ngx Skill

Ein Claude Code Skill zur automatischen Klassifizierung und Verwaltung von Dokumenten in Paperless-ngx.

## Was macht dieser Skill?

Der Skill verbindet Claude Code mit deiner Paperless-ngx Instanz. Beim Aufruf von `/paperless`:

1. Lädt alle Dokumente aus der Inbox
2. Liest den OCR-Text jedes Dokuments
3. Bestimmt Korrespondent, Dokumententyp und Tags
4. Setzt die Metadaten automatisch via API
5. Zeigt eine Zusammenfassung aller klassifizierten Dokumente

## Anforderungen

- Laufende [Paperless-ngx](https://docs.paperless-ngx.com/) Instanz
- API-Token (in Paperless unter Einstellungen > API-Token generieren)
- `curl` und `python3` (auf den meisten Systemen vorinstalliert)

## Installation

```bash
git clone https://github.com/craft-studio-marketplace/claude-skills.git
cp -r claude-skills/skills/paperless ~/.claude/skills/
```

## Konfiguration

```bash
# Option 1: Umgebungsvariablen
export PAPERLESS_URL="http://192.168.1.100:8010"
export PAPERLESS_TOKEN="dein-api-token"

# Option 2: macOS Keychain (empfohlen)
security add-generic-password -a "paperless" -s "paperless-api-token" -w "dein-api-token"
```

## Verwendung

```
/paperless
```

Der Skill lädt die Inbox, analysiert jeden OCR-Text und setzt Metadaten automatisch. Bei Unsicherheit wird das Dokument üersprungen (Inbox-Tag bleibt).

```
/paperless reclassify
```

Klassifiziert Dokumente ohne Korrespondent oder Dokumententyp nach.

## Anpassung

Der Skill arbeitet mit deinen bestehenden Korrespondenten, Dokumententypen und Tags in Paperless — er lädt sie bei jedem Aufruf frisch. Neue Korrespondenten legt er automatisch an wenn nötig.

Die Klassifizierungslogik kann durch Ergänzung des `SKILL.md` an deine spezifischen Bedürfnisse angepasst werden.

## Lizenz

MIT
