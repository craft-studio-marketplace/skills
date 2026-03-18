# Skills einreichen

Danke für dein Interesse, einen Skill beizutragen. Skills werden manuell geprüft und müssen folgende Anforderungen erfüllen.

## Mindestanforderungen

Jeder eingereichte Skill braucht:

- `SKILL.md` mit gültigem YAML-Frontmatter
- `README.md` mit Beschreibung, Installationsanleitung und Beispiel
- Keine hardcodierten Pfade, IP-Adressen oder Credentials
- Lokaler Test mit mindestens einem Beispiel-Output

## Frontmatter-Pflichtfelder

Die SKILL.md muss folgendes YAML-Frontmatter enthalten:

```yaml
---
name: mein-skill
description: Kurze Beschreibung — wird als Trigger-Text verwendet
allowed-tools:
  - Read
  - Write
---
```

Zulässige Kategorien: `productivity`, `dev`, `writing`, `domain-specific`

## Namenskonventionen

- Lowercase, keine Leerzeichen: `mein-skill` nicht `Mein Skill`
- Präfix für domänenspezifische Skills: `crm-`, `erp-`, `finance-` usw.
- Eindeutig und beschreibend

## Qualitätskriterien

Geprüft wird:

- Funktioniert der Skill wie beschrieben?
- Ist die Dokumentation vollständig und verständlich?
- Keine Sicherheitsrisiken (keine API-Keys, keine sensiblen Daten)?
- Kein Konflikt mit bestehenden Skills?

## Pull-Request-Prozess

1. Fork dieses Repository
2. Branch erstellen: `git checkout -b add-skill-mein-skill`
3. Skill in `skills/mein-skill/` ablegen
4. PR erstellen mit ausgefüllter Checkliste
5. CI-Checks abwarten
6. Review durch Craft Studio

## Fragen

Offene Fragen gerne als GitHub Issue erstellen.
