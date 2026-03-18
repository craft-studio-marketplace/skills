# MarkItDown

Ein Claude Code Skill zur Konvertierung von Dateien und Office-Dokumenten in Markdown. Nutzt Microsofts MarkItDown-Bibliothek.

## Installation

```bash
# Repository klonen
git clone https://github.com/craft-studio-marketplace/skills.git craft-studio-skills
cp -r craft-studio-skills/skills/markitdown ~/.claude/skills/

# Oder nur SKILL.md
mkdir -p ~/.claude/skills/markitdown
curl -o ~/.claude/skills/markitdown/SKILL.md \
  https://raw.githubusercontent.com/craft-studio-marketplace/skills/main/skills/markitdown/SKILL.md
```

### Python-Paket installieren

```bash
pip install 'markitdown[all]'
```

## Verwendung

Der Skill wird automatisch aktiviert bei Erwaehnung von Dokumentkonvertierung, PDF zu Markdown, oder Dateiextraktion:

```
Konvertiere diese PDF in Markdown: bericht.pdf
Extrahiere den Text aus diesem Word-Dokument: proposal.docx
```

## Unterstuetzte Formate

| Format | Beschreibung |
|--------|--------------|
| PDF | Vollstaendige Textextraktion |
| DOCX | Word-Dokumente mit Tabellen und Formatierung |
| PPTX | PowerPoint-Folien mit Notizen |
| XLSX | Excel-Tabellen |
| Bilder | JPEG, PNG, GIF, WebP mit OCR |
| Audio | WAV, MP3 mit Transkription |
| HTML | Webseiten |
| CSV, JSON, XML | Strukturierte Daten |
| ZIP | Archivinhalt |
| EPUB | E-Books |
| YouTube | Videountertitel abrufen |

## Lizenz

MIT — basiert auf [microsoft/markitdown](https://github.com/microsoft/markitdown)
