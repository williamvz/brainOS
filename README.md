# brainOS

William's persoonlijke Cowork skills.

## Skills

Elke skill draait alleen op expliciet verzoek — geen automatische triggers.
Bronnen: Google Calendar, Gmail, Notion, Tana (Outliner) en — voor de
energiebalans — de API's en portalen van de energie- en laadpaaldiensten.

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
- **[monthly-energy](.claude/skills/monthly-energy/SKILL.md)** — de
  maandelijkse "Energiebalans": zonproductie, afname en kosten bij de
  energieleverancier, de vergoeding voor het laden en de opbrengst van de
  ERE-certificaten. Vult de maandtabel in Outliner, rondt de terugkerende taak
  af, en meldt of de maand geld kostte of opleverde. Gestileerde pagina als
  Artifact, plus een node in de maandnotitie en een pushbericht.

## Naslag

- **[docs/outliner-taken.md](docs/outliner-taken.md)** — hoe `#task` en
  `#recurring task` zich verhouden, hoe je een terugkerende taak afrondt
  zonder hem definitief te sluiten — met de hand of vanuit een skill — en de
  triage-zoekopdracht "Inbox" die onaffe taken en projecten vangt. Gedeelde
  naslag voor `day-start`, `weekly-roundup` en `monthly-energy`.
- **[docs/energie-bronnen.md](docs/energie-bronnen.md)** — de vier bronnen
  achter `monthly-energy`: welke een API hebben en welke alleen een portaal,
  wat elke bron oplevert, en waar het misgaat (netwerk, login, timing).

## Over persoonlijke gegevens in deze repo

Deze repo is publiek. Skills beschrijven de methode (welke bronnen, welke
volgorde, welke rekenregels, welk ontwerp) — nooit concrete identifiers zoals
agenda-ID's, workspace-/node-ID's, artifact-URL's, woonplaats of financiële
bedragen. Waar een skill die gegevens nodig heeft, haalt hij ze live op (via
`list_calendars`, `list_workspaces`, `search_nodes`, etc.) in plaats van ze
hard te coderen.
