---
name: paperless
description: >
  Paperless-ngx Inbox verarbeiten und Dokumente klassifizieren.
  Aktiviert wenn nach Paperless, Inbox-Verarbeitung oder
  Dokumentenklassifizierung gefragt wird.
allowed-tools:
  - Bash
  - Read
---

# Paperless-ngx Inbox-Klassifizierung

Dieser Skill verarbeitet die Paperless-ngx Inbox: Claude liest den OCR-Text jedes Dokuments, versteht den Inhalt, und setzt die richtigen Metadaten via API.

## Konfiguration

Vor dem ersten Einsatz müssen zwei Umgebungsvariablen gesetzt sein:

```bash
export PAPERLESS_URL="http://your-paperless-host:port"
export PAPERLESS_TOKEN="your-api-token-here"
```

Der Token wird in Paperless unter **Einstellungen > API-Token** generiert.

Alternativ kannst du diese Werte in einer `.env`-Datei im Projektverzeichnis speichern oder den Token sicher im macOS Keychain ablegen:

```bash
# Token im Keychain speichern (macOS)
security add-generic-password -a "paperless" -s "paperless-api-token" -w "your-token"

# Token aus Keychain lesen
PAPERLESS_TOKEN=$(security find-generic-password -a "paperless" -s "paperless-api-token" -w)
```

## API-Referenz

**Basis-URL:** `$PAPERLESS_URL/api`
**Auth-Header:** `Authorization: Token $PAPERLESS_TOKEN`

### Endpunkte

| Aktion | Methode | Endpunkt |
|--------|---------|----------|
| Dokumente auflisten | GET | `/documents/?page_size=100` |
| Dokument-Details | GET | `/documents/{id}/` |
| Dokument aktualisieren | PATCH | `/documents/{id}/` |
| Korrespondenten auflisten | GET | `/correspondents/?page_size=100` |
| Korrespondent anlegen | POST | `/correspondents/` |
| Dokumententypen auflisten | GET | `/document_types/?page_size=100` |
| Tags auflisten | GET | `/tags/?page_size=100` |

### Beispiel-Befehle

**Inbox-Dokumente abrufen:**
```bash
PAPERLESS_URL="${PAPERLESS_URL:-http://localhost:8000}"
PAPERLESS_TOKEN="${PAPERLESS_TOKEN:-your-token}"

# Inbox-Tag-ID herausfinden
curl -s "$PAPERLESS_URL/api/tags/?page_size=100" \
  -H "Authorization: Token $PAPERLESS_TOKEN" | \
  python3 -c "import sys, json; [print(f'{t[\"id\"]}: {t[\"name\"]} (inbox={t.get(\"is_inbox_tag\",False)})') for t in json.load(sys.stdin)['results']]"

# Inbox-Dokumente abrufen (ersetze 1 mit der echten Inbox-Tag-ID)
curl -s "$PAPERLESS_URL/api/documents/?tags__id=1&page_size=100" \
  -H "Authorization: Token $PAPERLESS_TOKEN" | python3 -m json.tool
```

**Dokument-Content lesen (OCR-Text):**
```bash
curl -s "$PAPERLESS_URL/api/documents/{id}/" \
  -H "Authorization: Token $PAPERLESS_TOKEN" | python3 -c "
import sys, json
doc = json.load(sys.stdin)
print(f'Titel: {doc[\"title\"]}')
print(f'Korrespondent: {doc[\"correspondent\"]}')
print(f'Typ: {doc[\"document_type\"]}')
print(f'Content (erste 3000 Zeichen):')
print(doc['content'][:3000])
"
```

**Dokument-Metadaten setzen (PATCH):**
```bash
curl -s -X PATCH "$PAPERLESS_URL/api/documents/{id}/" \
  -H "Authorization: Token $PAPERLESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "correspondent": 5,
    "document_type": 1,
    "tags": [2, 7],
    "title": "Rechnung Firma XY Januar 2025"
  }'
```

## Workflow

Bei Aufruf von `/paperless` führt Claude folgende Schritte aus:

### Schritt 1: Erreichbarkeit pruefen

```bash
PAPERLESS_URL="${PAPERLESS_URL:-http://localhost:8000}"
PAPERLESS_TOKEN="${PAPERLESS_TOKEN:-}"

if [ -z "$PAPERLESS_TOKEN" ]; then
  # Aus Keychain lesen falls nicht gesetzt (macOS)
  PAPERLESS_TOKEN=$(security find-generic-password -a "paperless" -s "paperless-api-token" -w 2>/dev/null || echo "")
fi

curl -s -o /dev/null -w "%{http_code}" "$PAPERLESS_URL/api/tags/?page_size=1" \
  -H "Authorization: Token $PAPERLESS_TOKEN" --connect-timeout 5
```

Erwartete Antwort: `200`. Bei Fehler: Meldung ausgeben, Abbruch.

### Schritt 2: Aktuelle Metadaten laden

Alle Korrespondenten, Dokumententypen und Tags via API abrufen, um die aktuellen IDs zu haben:

```bash
PAPERLESS="$PAPERLESS_URL/api"
AUTH="Authorization: Token $PAPERLESS_TOKEN"

echo "=== KORRESPONDENTEN ===" && \
curl -s "$PAPERLESS/correspondents/?page_size=200" -H "$AUTH" | python3 -c "
import sys, json
data = json.load(sys.stdin)
for c in data['results']:
    print(f\"{c['id']}: {c['name']}\")
"

echo "=== DOKUMENTENTYPEN ===" && \
curl -s "$PAPERLESS/document_types/?page_size=100" -H "$AUTH" | python3 -c "
import sys, json
data = json.load(sys.stdin)
for t in data['results']:
    print(f\"{t['id']}: {t['name']}\")
"

echo "=== TAGS ===" && \
curl -s "$PAPERLESS/tags/?page_size=100" -H "$AUTH" | python3 -c "
import sys, json
data = json.load(sys.stdin)
for t in data['results']:
    print(f\"{t['id']}: {t['name']} (inbox={t.get('is_inbox_tag', False)})\")
"
```

### Schritt 3: Inbox-Dokumente abrufen

Den Inbox-Tag aus Schritt 2 identifizieren (wo `is_inbox_tag=True`) und damit Dokumente abrufen:

```bash
INBOX_TAG_ID=1  # Durch echte ID aus Schritt 2 ersetzen

curl -s "$PAPERLESS_URL/api/documents/?tags__id=$INBOX_TAG_ID&page_size=100&ordering=-added" \
  -H "Authorization: Token $PAPERLESS_TOKEN" | python3 -c "
import sys, json
data = json.load(sys.stdin)
print(f'Inbox-Dokumente: {data[\"count\"]}')
for doc in data['results']:
    print(f\"  ID {doc['id']}: {doc['title']} (Typ: {doc['document_type']}, Korr: {doc['correspondent']})\")
"
```

Bei mehr als 10 Dokumenten: In Batches von 10 verarbeiten.

### Schritt 4: Pro Dokument klassifizieren

Für jedes Inbox-Dokument:

1. **OCR-Text lesen** (Feld `content`, auf ~3000 Zeichen kürzen)
2. **Analysieren:** Korrespondent, Dokumententyp, Person-Tags, Bereichs-Tags, Titel bestimmen
3. **Bei neuem Korrespondent:** Via POST anlegen, ID merken
4. **Metadaten setzen:** Via PATCH

```bash
# Neuen Korrespondenten anlegen
curl -s -X POST "$PAPERLESS_URL/api/correspondents/" \
  -H "Authorization: Token $PAPERLESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Firmenname GmbH",
    "matching_algorithm": 4,
    "match": "(?i)firmenname"
  }'
```

### Schritt 5: Zusammenfassung

Am Ende eine Tabelle ausgeben:

| # | Dokument-ID | Titel (neu) | Korrespondent | Typ | Tags | Status |
|---|-------------|-------------|---------------|-----|------|--------|
| 1 | 101 | Rechnung Firma XY Jan 2025 | Firma XY | Rechnung | Person, Kategorie | Klassifiziert |
| 2 | 102 | ... | ... | ... | ... | Uebersprungen |

## Klassifizierungsregeln

### Grundprinzipien

1. **Ein Korrespondent pro Dokument** — der Absender/Aussteller
2. **Ein Dokumententyp pro Dokument** — die Dokumentart
3. **Mindestens ein Person-Tag** — wer ist betroffen
4. **Inbox-Tag entfernen** bei erfolgreicher Klassifizierung
5. **Titel anpassen** — kurz, beschreibend, ohne Dateinamen-Artefakte

### Korrespondent bestimmen

- Absender, Briefkopf, Logo, Domain, Firmenstempel im OCR-Text suchen
- Mit bestehenden Korrespondenten abgleichen (case-insensitive)
- Neuen Korrespondenten anlegen wenn kein bestehender passt
- Kein Korrespondent erkennbar? Inbox-Tag belassen, Dokument üerspringen

### Titel-Format

```
[Dokumentart] [Absender] [Zeitraum]
```

Beispiele:
- "Rechnung Telefonanbieter Januar 2025"
- "Kontoauszug Hausbank Q4 2024"
- "Versicherungspolice Krankenversicherung 2025"

### Tags: Wichtiger Hinweis

Das `tags`-Feld bei PATCH **ersetzt alle bestehenden Tags**. Die neue Tag-Liste muss alle gewünschten Tags enthalten (Person + Bereich + sonstige). Den Inbox-Tag NICHT in die Liste aufnehmen — damit wird er entfernt.

## Nachklassifizierung

Bei Aufruf mit `/paperless reclassify`: Statt Inbox-Tag werden Dokumente ohne Korrespondent oder Dokumententyp gesucht:

```bash
# Dokumente ohne Korrespondent
curl -s "$PAPERLESS_URL/api/documents/?correspondent__isnull=true&page_size=100" \
  -H "Authorization: Token $PAPERLESS_TOKEN"

# Dokumente ohne Dokumententyp
curl -s "$PAPERLESS_URL/api/documents/?document_type__isnull=true&page_size=100" \
  -H "Authorization: Token $PAPERLESS_TOKEN"
```

## Sicherheitsmassnahmen

- **Nur Metadaten setzen** — niemals Dokumente löschen oder Dateien verändern
- **OCR-Text auf ~3000 Zeichen kürzen** — reicht für Klassifizierung, spart Kontext
- **Batches von max. 10 Dokumenten** — Kontext-Fenster schonen
- **Bei Unsicherheit: Inbox-Tag belassen** — lieber üerspringen als falsch klassifizieren
