---
name: yearly-review
description: William's "Jaarplandag" volgens de Grip-methode van Rick Pastoor — de grootste van de drie reviews (reken op een halve dag). Bereidt een maand-voor-maand terugblik, doelenbeoordeling, area-review, kwartaal-vergelijking en energiecurve voor, gevolgd door Pastoors brainstormvragen en de vragen voor de doelen van het nieuwe jaar. Jij levert materiaal en vragen, William schrijft de conclusies — vul nooit zijn verhaal of doelen in. Levert een gestileerde pagina als Artifact, een regel in de jaarnotitie, een pushbericht én een e-mail. Alleen op expliciet verzoek (bv. "jaarplandag", "jaaroverzicht", of /yearly-review) — niet automatisch.
---

# Jaarplandag

Bereid Williams jaarplandag voor volgens de methode uit Grip van Rick
Pastoor. Schrijf alles in het Nederlands. Werk zelfstandig af — er is
niemand om vragen aan te stellen. Dit is de grootste van de drie reviews —
reken op een halve dag voor William, en bereid het navenant grondig voor.
**Jij levert het materiaal en de vragen; William schrijft de conclusies. Vul
nooit zijn verhaal of zijn doelen in.** Draait alleen op expliciet verzoek,
geen automatische trigger.

> **Over identifiers in dit bestand:** deze repo is publiek. Agenda-ID's,
> Tana-workspace-/tag-/veld-ID's en de artifact-URL horen hier dus nooit in
> te staan, ook niet als voorbeeld. Zoek tags en velden op hun naam op via
> `list_tags` / `get_tag_schema` in plaats van ID's hard te coderen, en
> hergebruik de artifact-URL die je al kent van vorige keren.

## Tijd

Bepaal datum en tijd in Europe/Amsterdam via bash. Het jaar dat je
beoordeelt is het lopende kalenderjaar, van 1 januari t/m vandaag. Deze
review vervangt de kwartaalreview van Q4 en zet meteen de doelen voor Q1
van het nieuwe jaar.

## Terugkijken — stap 1 t/m 5

**Stap 1, jaaroverzicht.** Haal per maand de events op uit de agenda's die
van William zijn — doorgaans privé, werk en gezin, te vinden via
`list_calendars`. Schrijf per maand drie tot vijf zinnen: wat gebeurde er
werkelijk? Dit is het materiaal waar de rest op rust, dus doe dit goed en
volledig — twaalf maanden. Haal er ook bij: de taken die dit jaar zijn
afgerond, de afgeronde projecten, en de dagnotities (de dagnodes onder
Daily notes, inclusief de Dagstart-nodes van elke ochtend — dat is een jaar
aan dagelijkse observaties dat nergens anders bestaat).

Maak daaruit twee lijsten, zoals Pastoor voorschrijft: wat ging goed, en
wat ging niet goed. Nog geen oordeel, alleen ophalen.

**Stap 2, doelen beoordelen.** Zoek de kwartaaldoelen van dit jaar. Zet ze
per kwartaal met hun status (Area, Hoe ziet succes eruit, Status). Zoek ook
naar oudere "persoonlijk doel"-nodes voor het geval er nog jaardoelen buiten
dit systeem staan. Zijn er geen doelen gezet, meld dat feitelijk en zonder
verwijt — de vraag is dan waarom ze er niet kwamen.

**Stap 3, per area terugkijken.** Haal alle actieve areas op. Lees per area
Doel en Standaard. Zet per area het doel en de standaard náást wat er dit
jaar werkelijk gebeurde (afspraken, taken, projecten die eraan hingen).
Waar die twee ver uit elkaar liggen, zeg dat — dat is precies waar het
gesprek met zichzelf zit. Sla geen area over, ook niet de stille.

**Stap 4, per kwartaal teruglopen.** Haal de vier kwartaalreviews op en de
weekafsluitingen van dit jaar. Zet de vier kwartalen naast elkaar met wat
hij zelf toen opschreef. Zoek naar patronen die per week onzichtbaar bleven:
een seizoen waarin het steeds misging, een periode die achteraf beter was
dan hij toen voelde. Het veld "Hoe ging het met mij" over ~52 weken is een
energiecurve over een heel jaar — geef die als een leesbaar overzicht weer
(bijvoorbeeld een strook van 52 blokjes, één per week, gekleurd naar
antwoord, met de maanden eronder).

**Stap 5, het jaar schrijven.** Geen data, alleen het veld waarin hij zelf
schrijft.

## Brainstormen — stap 6

Geen data. De vragen van Pastoor, letterlijk: ben ik tevreden over mijn
werk? Slaap ik goed? Wat wil ik leren? Wie wil ik vaker zien? Wat zou ik
doen als geld geen rol speelde? Waar zou ik spijt van hebben als dit jaar
precies zo nog eens ging?

Voeg hooguit twee eigen vragen toe die volgen uit wat je in het jaar zag, en
maak duidelijk dat het brainstormen is: geen haalbaarheidstoets, geen
prioriteiten.

## Doelen zetten — stap 7 en 8

**Stap 7**: de vragen voor de doelen van het nieuwe jaar, met de nadruk op
Q1. Een heel jaar is te lang om concreet te plannen; het eerste kwartaal
niet.

**Stap 8**: de areas en hun standaarden bijstellen. Zet per area de huidige
standaard erbij zodat hij hem kan herschrijven.

## De pagina

Schrijf een self-contained HTML-bestand en publiceer het met de
Artifact-tool. Gebruik dezelfde artifact-URL als vorig jaar zodat de link
stabiel blijft — lees die eerst terug met de Artifact-tool
(`action: "read"`) en gebruik hem als sjabloon: dezelfde CSS, dezelfde
structuur, alleen nieuwe inhoud. Titel `<title>Jaarplandag</title>`,
favicon 🕯️.

Het ontwerp in het kort:
- Fonts via Google Fonts: Bricolage Grotesque (koppen), Source Serif 4
  (lopende tekst), IBM Plex Mono (labels en cijfers), altijd met echte
  fallback-stack.
- Licht: bg `#EFEEF2`, surface `#FFFFFF`, sunk `#E3E1EA`, ink `#16141B`,
  ink-soft `#545061`, ink-faint `#86828F`, line `#D5D2DE`, accent `#4B4176`
  (indigo), accent-soft `#E3E0EF`, kritiek `#96253C`, waarschuwing
  `#8A5210`, goed `#2F6350`.
- Donker: bg `#0E0D12`, surface `#17161D`, sunk `#201E28`, ink `#E9E7F0`,
  ink-soft `#9D98AB`, ink-faint `#6D6879`, line `#292734`, accent `#A79CD9`,
  accent-soft `#211D31`, kritiek `#DF8497`, waarschuwing `#D5A05C`, goed
  `#7EC0A7`.
- Alle kleuren als tokens op kale `:root` (licht), herdefinieer in
  `@media (prefers-color-scheme: dark)` met guard
  `:root:not([data-theme="light"])`, en nog eens in `:root[data-theme="dark"]`.
  Body krijgt een expliciete achtergrond uit een token.
- Eén kolom, max-width 680px, mobiel eerst. Drie fases met een
  monospace-label en een dunne lijn ernaast: Terugkijken (± 2 uur),
  Brainstormen (± 1 uur), Doelen zetten (± 1 uur). Daaronder de acht
  genummerde stappen (01 t/m 08) in een linkerkolom van 44px.
- Vervang het inleidende kader "Deze pagina is nog leeg van data" door één
  zin die zegt over welk jaar je hebt teruggekeken.
- De maand-voor-maand terugblik is deze keer lang; geef die een eigen
  ritme met de maandnaam in monospace links en de alinea rechts, zodat de
  pagina scanbaar blijft.
- Geen emoji als sectiemarkering, geen gradients, geen afgeronde kaarten
  met accentbalken. Brede blokken in een container met
  `overflow-x: auto`.

## De node in Outliner

Haal de jaarnode op met `get_or_create_calendar_node` (granularity year) en
zet er via `import_tana_paste` één node onder: "Jaarreview — `<jaar>`" met
de Jaarreview-tag (zoek de tag op via `list_tags`, hardcodeer geen tag-ID).
Vul alleen wat uit data volgt:
- Reviewpagina:: de artifact-link

Laat leeg — die zijn van William: Het jaar in een verhaal, Wat ik meeneem,
Wat ik achterlaat, Thema voor komend jaar.

Zet als kinderen: de twaalf maandsamenvattingen, de twee lijsten (ging goed
/ ging niet goed), de doelen met hun status, de areas met doel en standaard
naast wat er gebeurde, en de vier kwartalen naast elkaar. Maak ook een lege
kop "Doelen Q1 `<nieuw jaar>`" aan waaronder William zijn kwartaaldoel-nodes
hangt.

## Tot slot

Stuur William een pushbericht van één zin met de kern van het jaar en de
link naar de pagina, en stuur hem dezelfde link per e-mail — deze review
wil hij waarschijnlijk op een groter scherm openen.
