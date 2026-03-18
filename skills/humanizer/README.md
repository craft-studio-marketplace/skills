# Humanizer

Ein Claude Code Skill, der KI-typische Schreibmuster erkennt und entfernt. Basiert auf Wikipedias umfassendem Leitfaden zu "Signs of AI writing".

## Installation

```bash
# Empfohlen: Repository klonen
git clone https://github.com/craft-studio-marketplace/claude-skills.git
cp -r claude-skills/skills/humanizer ~/.claude/skills/

# Alternativ: Nur SKILL.md kopieren
mkdir -p ~/.claude/skills/humanizer
curl -o ~/.claude/skills/humanizer/SKILL.md \
  https://raw.githubusercontent.com/craft-studio-marketplace/claude-skills/main/skills/humanizer/SKILL.md
```

## Verwendung

```
/humanizer

[Text hier einfuegen]
```

Oder direkt in der Konversation:

```
Bitte humanize diesen Text: [Text]
```

## Erkannte Muster (24 insgesamt)

### Inhalt
- Bedeutungsinflation ("marking a pivotal moment")
- Bekanntheitsnachweise ("cited in NYT, BBC, FT")
- Oberflaechen-Analysen mit -ing-Endungen
- Werbliche Sprache ("nestled within the breathtaking region")
- Vage Quellenangaben ("Experts believe")
- Formelhafte Herausforderungsabschnitte

### Sprache
- KI-Vokabular (additionally, testament, landscape, showcase)
- Kopula-Vermeidung (serves as, stands as, boasts)
- Negative Parallelismen ("It's not just X, it's Y")
- Dreierregel-Uebernutzung
- Synonymkreisel (protagonist, main character, central figure)
- Falsche Bereiche ("from X to Y")

### Stil
- Gedankenstrich-Uebernutzung
- Fettschrift-Uebernutzung
- Inline-Header-Listen
- Title Case in Ueberschriften
- Emojis
- Typografische Anfuehrungszeichen

### Kommunikation
- Chatbot-Artefakte ("I hope this helps!")
- Knowledge-Cutoff-Disclaimer
- Sykophantischer Ton ("Great question!")
- Fuellphrasen ("In order to")
- Uebertriebene Absicherung ("could potentially possibly")
- Generische Schlusswoerter

## Beispiel

**Vorher (KI-typisch):**
> AI-assisted coding serves as an enduring testament to the transformative potential of large language models, marking a pivotal moment in the evolution of software development.

**Nachher:**
> AI coding assistants speed up some tasks. In a 2024 study by Google, developers using Codex completed simple functions 55% faster than a control group.

## Lizenz

MIT
