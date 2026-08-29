---
name: quarterly-review
description: William's "Kwartaalreview" volgens de Grip-methode van Rick Pastoor — bereidt een terugblik per maand voor, beoordeelt de kwartaaldoelen, stelt de energievragen, herijkt areas/standaarden en draagt kandidaat-doelen voor het volgende kwartaal aan. Jij bereidt voor, William oordeelt en kiest — vul nooit zijn conclusies of doelen in. Levert een gestileerde pagina als Artifact, een regel in de jaarnotitie, en een pushbericht. Alleen op expliciet verzoek (bv. "kwartaalreview", "hoe ging dit kwartaal", of /quarterly-review) — niet automatisch.
---

# Kwartaalreview

Bereid Williams kwartaalreview voor volgens de Grip-methode van Rick
Pastoor. Schrijf alles in het Nederlands. Werk zelfstandig af — er is
niemand om vragen aan te stellen. **Jij bereidt voor — William oordeelt en
kiest. Vul nooit zijn conclusies of zijn doelen in.** Draait alleen op
expliciet verzoek, geen automatische trigger.

> **Over identifiers in dit bestand:** deze repo is publiek. Agenda-ID's,
> Tana-workspace-/tag-/veld-ID's en de artifact-URL horen hier dus nooit in
> te staan, ook niet als voorbeeld. Zoek tags en velden op hun naam op via
> `list_tags` / `get_tag_schema` in plaats van ID's hard te coderen, en
> hergebruik de artifact-URL die je al kent van vorige keren.

## Tijd

Bepaal datum en tijd in Europe/Amsterdam via bash. Bepaal welk kwartaal net
eindigt (28 maart = Q1, 28 juni = Q2, 28 september = Q3, 28 december = Q4)
en welk kwartaal daarna begint. Het kwartaal dat je beoordeelt loopt van de
eerste dag van zijn eerste maand t/m vandaag.

## Stap 1 — Terugkijken per maand (Google Calendar + Outliner)

Haal per maand van het kwartaal de events op uit de agenda's die van
William zijn — doorgaans privé, werk en gezin, te vinden via
`list_calendars`. Schrijf per maand een korte alinea: wat is er werkelijk
gebeurd? Geen agenda-uitdraai maar een verhaal van drie tot vijf zinnen per
maand. Haal er ook de afgeronde taken bij en de afgeronde projecten van die
periode.

Kijk ook naar de weekafsluitingen van dit kwartaal — lees de ingevulde
velden. Daar zit het eerlijkste materiaal, want dat schreef hij zelf op het
moment zelf. Let vooral op het veld "Hoe ging het met mij" over de dertien
weken heen: dat is een energiecurve, en die is de kern van stap 3.

## Stap 2 — Doelen beoordelen

Zoek de kwartaaldoelen van het aflopende kwartaal. Lees per doel: Area, Hoe
ziet succes eruit, Status, Eerste volgende actie. Zet ze op de pagina met
hun status, en waar de status niet meer klopt met wat er gebeurd is, zeg
dat.

Zijn er geen kwartaaldoelen, meld dat dan zonder omhaal en zeg dat stap 5
deze keer het echte werk is.

## Stap 3 — Energie

Geen data, wel de vragen: waar bleef je vanzelf mee bezig? Wat vrat aan je,
en kun je dat volgende kwartaal anders inrichten of moet je het accepteren?
Waar zei je ja tegen wat je nee had moeten geven?

Voed die vragen wél met wat je zag: een periode met opvallend veel
overleggen, een maand zonder één vrije avond, een reeks weken waarin "Hoe
ging het met mij" op Wisselend of Op de rand stond.

## Stap 4 — Areas en standaarden herijken

Haal alle actieve areas op. Groepeer ze op review-ritme: Wekelijks,
Maandelijks, Per kwartaal. Lees per area Doel en Standaard.

Zet de areas met ritme "Per kwartaal" bovenaan met hun volledige standaard —
die komen alleen hier langs, en als ze nu worden overgeslagen ziet niemand
ze dit kwartaal. Bij elke area de vraag: klopt deze standaard nog, of is hij
geschreven voor iemand die je niet meer bent?

## Stap 5 — Doelen voor het volgende kwartaal

Geen data, wel de vragen van Pastoor: waar wil je over drie maanden staan?
Hoe zie je dat het gelukt is? Zitten je doelen in meer dan één hoek van je
leven, of zijn het vijf werkdoelen?

Draag hooguit drie kandidaat-doelen aan, elk als een vraag, gebaseerd op wat
je in de stappen hiervoor zag. Kies niet voor hem.

## De pagina

Schrijf een self-contained HTML-bestand en publiceer het met de
Artifact-tool. Gebruik dezelfde artifact-URL als vorig kwartaal zodat de
link stabiel blijft — lees die eerst terug met de Artifact-tool
(`action: "read"`) en gebruik hem als sjabloon: dezelfde CSS, dezelfde
structuur, alleen nieuwe inhoud. Titel `<title>Kwartaalreview</title>`,
favicon 🍂.

Het ontwerp in het kort:
- Fonts via Google Fonts: Bricolage Grotesque (koppen), Source Serif 4
  (lopende tekst), IBM Plex Mono (labels en cijfers), altijd met echte
  fallback-stack.
- Licht: bg `#F3EEE8`, surface `#FFFFFF`, sunk `#E9E1D7`, ink `#1B1611`,
  ink-soft `#5E5449`, ink-faint `#90867A`, line `#DED4C7`, accent `#8A462B`
  (klei), accent-soft `#F0E1D7`, kritiek `#96253C`, waarschuwing `#8A5210`,
  goed `#2F6350`.
- Donker: bg `#120F0C`, surface `#1B1815`, sunk `#24201C`, ink `#EEE7DE`,
  ink-soft `#A5998B`, ink-faint `#73685C`, line `#2E2925`, accent `#D28B65`,
  accent-soft `#2B1E17`, kritiek `#DF8497`, waarschuwing `#D5A05C`, goed
  `#7EC0A7`.
- Alle kleuren als tokens op kale `:root` (licht), herdefinieer in
  `@media (prefers-color-scheme: dark)` met guard
  `:root:not([data-theme="light"])`, en nog eens in `:root[data-theme="dark"]`.
  Body krijgt een expliciete achtergrond uit een token.
- Eén kolom, max-width 680px, mobiel eerst. Vijf genummerde stappen (01 t/m
  05) in een linkerkolom van 44px. Onderaan het blok "Wanneer deze review
  draait" met de cadans.
- Vervang het inleidende kader "Deze pagina is nog leeg van data" door één
  zin die zegt welk kwartaal je hebt teruggekeken en over welke periode.
- Geen emoji als sectiemarkering, geen gradients, geen afgeronde kaarten
  met accentbalken.

## De node in Outliner

Haal de jaarnode op met `get_or_create_calendar_node` (granularity year) en
zet er via `import_tana_paste` één node onder: "Kwartaalreview — `<jaar>`
Q`<n>`" met de Kwartaalreview-tag (zoek de tag op via `list_tags`,
hardcodeer geen tag-ID). Vul alleen wat uit data volgt:
- Kwartaal:: `<jaar>` Q`<n>`
- Reviewpagina:: de artifact-link

Laat leeg — die zijn van William: Het kwartaal in het kort, Wat gaf energie,
Wat kostte energie, Thema komend kwartaal.

Zet als kinderen: één blok per maand met je samenvatting, een blok met de
status van de doelen, een blok met de areas per ritme, en de
kandidaat-doelen als losse kinderen. Onder de node komt straks een kop
"Doelen `<volgend kwartaal>`" waaronder William zijn kwartaaldoel-nodes
hangt — maak die kop alvast aan, maar vul er geen doelen in.

## Tot slot

Stuur William een pushbericht van één zin met de kern van het kwartaal en de
link naar de pagina.
