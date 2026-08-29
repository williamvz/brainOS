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
- **[weekly-roundup](.claude/skills/weekly-roundup/SKILL.md)** — terugblik op
  de afgelopen week (afgerond/openstaand) plus een korte vooruitblik. Platte
  tekst in de chat, geen artifact.
- **[quarterly-review](.claude/skills/quarterly-review/SKILL.md)** —
  voortgang op doelen/OKR's en de belangrijkste resultaten van het kwartaal.
  Platte tekst in de chat, geen artifact.
- **[yearly-review](.claude/skills/yearly-review/SKILL.md)** — jaaroverzicht:
  hoogtepunten, voortgang op jaardoelen, patronen en richting voor volgend
  jaar. Platte tekst in de chat, geen artifact.

## Over persoonlijke gegevens in deze repo

Deze repo is publiek. Skills beschrijven de methode (welke bronnen, welke
volgorde, welke rekenregels, welk ontwerp) — nooit concrete identifiers zoals
agenda-ID's, workspace-/node-ID's, artifact-URL's, woonplaats of financiële
bedragen. Waar een skill die gegevens nodig heeft, haalt hij ze live op (via
`list_calendars`, `list_workspaces`, `search_nodes`, etc.) in plaats van ze
hard te coderen.
