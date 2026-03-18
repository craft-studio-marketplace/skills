# Claude Code Skills — Marketplace

Ein kuratierter Marktplatz für Claude Code Skills. Hier findest du wiederverwendbare Skills für deinen KI-Assistenten — bereitgestellt von [Craft Studio](https://craft.studio) und der Community.

## Was ist ein Skill?

Ein Skill ist eine Markdown-Datei (`SKILL.md`), die Claude Code mit einem spezifischen Fachwissen ausstattet. Du kopierst die Datei in dein lokales Skills-Verzeichnis — fertig.

## Installation

```bash
# Skill klonen (empfohlen)
git clone https://github.com/craft-studio-marketplace/claude-skills.git
cp -r claude-skills/skills/humanizer ~/.claude/skills/

# Oder einzelne Skill-Datei kopieren
mkdir -p ~/.claude/skills/humanizer
cp skills/humanizer/SKILL.md ~/.claude/skills/humanizer/
```

Nach der Installation ist der Skill sofort in Claude Code verfügbar. Ruf ihn auf mit `/skillname` oder beschreibe die Aufgabe in natürlicher Sprache.

## Skills

| Skill | Kategorie | Beschreibung |
|-------|-----------|--------------|
| [humanizer](skills/humanizer/) | Schreiben | KI-typische Schreibmuster erkennen und entfernen |
| [visual-explainer](skills/visual-explainer/) | Produktivität | Systeme, Code und Daten als HTML-Seiten visualisieren |
| [markitdown](skills/markitdown/) | Produktivität | Dateien und Dokumente in Markdown konvertieren |
| [paperless](skills/paperless/) | Domänenspezifisch | Paperless-ngx Inbox klassifizieren und verwalten |

## Beitragen

Skills können via Pull Request eingereicht werden. Lies zuerst [CONTRIBUTING.md](CONTRIBUTING.md) für Anforderungen und Qualitätskriterien.

---

Craft Studio — KI-Kurse und Beratung für Berufstätige und KMU.
