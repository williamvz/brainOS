# brainOS

William's persoonlijke Cowork skills.

## Skills

Elke skill draait alleen op expliciet verzoek — geen automatische triggers.
Bronnen: Google Calendar, Gmail, Notion en Tana (Outliner).

- **[day-start](.claude/skills/day-start/SKILL.md)** — de "Dagstart"
  ochtendbriefing: agenda, openstaande taken, beslissingen die vandaag om een
  keuze vragen, nieuws, weer, en beurzen + portefeuille. Levert een
  gestileerde pagina als Artifact, een regel in de dagnotitie in Outliner, en
  een pushbericht.
- **[weekly-roundup](.claude/skills/weekly-roundup/SKILL.md)** — de
  "Weekafsluiting" volgens de Grip-methode van Rick Pastoor: agenda,
  inboxen, taken en areas worden voorbereid, William oordeelt en kiest zelf.
  Gestileerde pagina als Artifact, plus een regel in de weeknotitie en een
  pushbericht.
- **[quarterly-review](.claude/skills/quarterly-review/SKILL.md)** — de
  "Kwartaalreview" volgens Grip: terugblik per maand, doelenbeoordeling,
  energievragen en herijking van areas/standaarden, met kandidaat-doelen
  voor het volgende kwartaal. Gestileerde pagina als Artifact, plus een
  regel in de jaarnotitie en een pushbericht.
- **[yearly-review](.claude/skills/yearly-review/SKILL.md)** — de
  "Jaarplandag" volgens Grip, de grootste van de drie reviews: een volledige
  terugblik op het jaar, Pastoors brainstormvragen, en de vragen voor de
  doelen van het nieuwe jaar. Gestileerde pagina als Artifact, plus een
  regel in de jaarnotitie, een pushbericht én een e-mail.

## Naslag

- **[docs/outliner-taken.md](docs/outliner-taken.md)** — hoe `#task` en
  `#recurring task` zich verhouden, hoe je een terugkerende taak afrondt
  zonder hem definitief te sluiten, en de triage-zoekopdracht "Inbox" die
  onaffe taken en projecten vangt. Gedeelde naslag voor `day-start` en
  `weekly-roundup`.

## Over persoonlijke gegevens in deze repo

Deze repo is publiek. Skills beschrijven de methode (welke bronnen, welke
volgorde, welke rekenregels, welk ontwerp) — nooit concrete identifiers zoals
agenda-ID's, workspace-/node-ID's, artifact-URL's, woonplaats of financiële
bedragen. Waar een skill die gegevens nodig heeft, haalt hij ze live op (via
`list_calendars`, `list_workspaces`, `search_nodes`, etc.) in plaats van ze
hard te coderen.
