# Visual Explainer

Ein Claude Code Skill, der komplexe Informationen als schoen gestaltete, selbst-enthaltsame HTML-Seiten visualisiert — anstelle von ASCII-Art und Box-Zeichen.

## Installation

```bash
# Empfohlen: Repository klonen (beinhaltet Templates und Referenzen)
git clone https://github.com/craft-studio-marketplace/claude-skills.git
cp -r claude-skills/skills/visual-explainer ~/.claude/skills/

# Prompt-Templates für Slash-Commands (optional)
cp ~/.claude/skills/visual-explainer/prompts/*.md ~/.claude/prompts/ 2>/dev/null || true
```

## Verwendung

Der Skill wird automatisch aktiviert wenn du nach Diagrammen, Architekturen, Flows oder Visualisierungen fragst. Er greift auch proaktiv ein, wenn eine komplexe ASCII-Tabelle entstehen wuerde.

```
Zeichne ein Diagramm unserer Authentifizierungs-Architektur
Erklaer mir diesen Code visuell
Vergleiche diese zwei Ansaetze als Tabelle
```

### Slash-Commands (mit Prompt-Templates)

| Command | Funktion |
|---------|----------|
| `/generate-web-diagram` | HTML-Diagramm zu einem beliebigen Thema |
| `/diff-review` | Visuelles Code-Review mit Architekturvergleich |
| `/plan-review` | Plan gegen Codebase prüfen mit Risikoanalyse |
| `/project-recap` | Mentales Modell für Kontext-Wechsel |
| `/fact-check` | Behauptungen in Dokumenten gegen Code verifizieren |

## Was wird generiert

- Selbst-enthaltsame `.html` Datei (keine externen Abhaengigkeiten ausser CDN)
- Dark/Light Theme Unterstuetzung
- Interaktive Mermaid-Diagramme mit Zoom und Pan
- Responsive Design

11 unterstuetzte Diagrammtypen: Architektur, Flowchart, Sequenz, Datenfluss, ER/Schema, State Machine, Mind Map, Datentabellen, Timeline, Dashboard

Output-Pfad: `~/.agent/diagrams/`

## Anforderungen

- Browser zum Anzeigen der generierten HTML-Dateien
- Optional: [surf-cli](https://github.com/nicobailon/surf-cli) für KI-generierte Illustrationen

## Credits

Basiert auf [nicobailon/visual-explainer](https://github.com/nicobailon/visual-explainer).

## Lizenz

MIT
