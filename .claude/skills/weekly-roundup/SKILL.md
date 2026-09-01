---
name: weekly-roundup
description: William's wekelijkse "Weekafsluiting" volgens de Grip-methode van Rick Pastoor — bereidt een terugblik op agenda, inboxen, taken en areas voor, plus een vooruitblik en kandidaat-prioriteiten. Jij bereidt voor, William oordeelt en kiest — vul nooit zijn conclusies in. Levert een gestileerde pagina als Artifact, een regel in de weeknotitie, en een pushbericht. Alleen op expliciet verzoek (bv. "weekafsluiting", "hoe was mijn week", of /weekly-roundup) — niet automatisch.
---

# Weekafsluiting

Bereid Williams wekelijkse weekafsluiting voor volgens de Grip-methode van
Rick Pastoor. Schrijf alles in het Nederlands. Werk zelfstandig af — er is
niemand om vragen aan te stellen. **Jij bereidt voor — William oordeelt en
kiest. Vul dus nooit zijn conclusies in.** Draait alleen op expliciet
verzoek, geen automatische trigger.

> **Over identifiers in dit bestand:** deze repo is publiek. Agenda-ID's,
> Tana-workspace-/tag-/veld-ID's en de artifact-URL horen hier dus nooit in
> te staan, ook niet als voorbeeld. Zoek tags en velden op hun naam op via
> `list_tags` / `get_tag_schema` in plaats van ID's hard te coderen, en
> hergebruik de artifact-URL die je al kent van vorige keren.

## Tijd en plaats

Bepaal eerst datum, tijd en weeknummer in Europe/Amsterdam via bash
(`TZ=Europe/Amsterdam date '+%Y-%m-%d %H:%M %A week %V'`). "Deze week" loopt
van maandag 00:00 t/m zondag 23:59 van de huidige ISO-week.

## Stap 1 — Terugblik op de agenda (Google Calendar)

Haal de events van maandag t/m vandaag op uit de agenda's die van William
zijn — doorgaans een privé-agenda, een werkagenda en een gezinsagenda. Vind
ze via `list_calendars`. Vat de week samen als wat er gebeurde, niet als een
agenda-uitdraai. Zoek naar het patroon: was dit een week van overleggen, van
maken, van reizen, van thuis zijn? Noem expliciet wat nog opvolging vraagt —
een gesprek zonder afgesproken vervolg, een afspraak die is verzet, een
besluit dat is blijven hangen.

## Stap 2 — Inboxen

- **Gmail**: tel de ongelezen berichten in de inbox (`in:inbox is:unread`).
  Groepeer ze grofweg (nieuwsbrieven, administratie, echte post van mensen)
  zodat William ziet wat er in één klap weg kan en wat een besluit vraagt.
- **Outliner-inbox**: lees de capture-inbox van de workspace en tel de
  echte items (lege knoopjes tellen niet mee).

## Stap 3 — Takenlijst (Outliner)

Zoek open taken met een verlopen deadline (deadline vóór maandag komende
week), niet afgevinkt, niet in de prullenbak. Zoek ook wat er deze week is
afgerond — noem het aantal en de paar die er echt toe deden. Een
weekafsluiting die alleen achterstand toont, is geen afsluiting.

Kijk actief naar drie dingen: taken die al meer dan twee weken over datum
zijn (verzetten, delegeren of schrappen), taken die dubbel of bijna dubbel
in het systeem staan, en vage taken zonder werkwoord.

Pas op bij dat tweede punt: een terugkerende taak laat bij elke afronding een
afgevinkte kopie achter, dus dezelfde naam komt tientallen keren voor. Dat is
historie, geen dubbele taak — meld het niet als opruimwerk. Zie
`docs/outliner-taken.md` in deze repo.

Link naar een taak als `https://app.tana.inc/?nodeid=<nodeId>`.

## Stap 4 — Areas met wekelijks ritme

Zoek de areas met review-ritme "Wekelijks". Lees per area het veld Doel en
Standaard. Zet de standaard er letterlijk bij — dat is waar William aan
toetst. Waar de agenda of de taken van deze week iets zeggen over een
standaard (bijvoorbeeld: een standaard vraagt elke week één echt gesprek met
een vriend en er staat niets in de agenda), zeg dat er eerlijk bij. Oordeel
niet namens hem.

## Stap 5 — Vooruitkijken

Haal uit dezelfde agenda's de komende veertien dagen. Let op en benoem:
afspraken die voorbereiding vragen, reistijd die ontbreekt, twee afspraken
die overlappen, een dag die overvol staat, en of er nog echt lege ruimte in
de week zit (Pastoor: twintig procent). Meld ook als een agenda opvallend
leeg is — dat is óf ruimte óf een agenda die niet is bijgewerkt, en het
verschil is belangrijk.

## Stap 6 — Kandidaat-prioriteiten

Formuleer maximaal drie kandidaten voor de twee prioriteiten van komende
week, elk als een echte vraag met één zin toelichting. Goede kandidaten: het
ene ding dat de rest van de maand makkelijker maakt, een verlopen taak waar
iemand anders op wacht, iets uit een area waarvan de standaard al weken niet
gehaald wordt. Kies niet voor hem — leg de keuze voor.

## Stap 7 — Loslaten

Geen data, alleen de velden waar hij zelf in schrijft.

## De pagina

Schrijf een self-contained HTML-bestand en publiceer het met de
Artifact-tool. Gebruik dezelfde artifact-URL als vorige week zodat de link
stabiel blijft — lees die eerst terug met de Artifact-tool (`action: "read"`)
en gebruik hem als sjabloon: dezelfde CSS, dezelfde structuur, alleen nieuwe
inhoud. Titel `<title>Weekafsluiting</title>`, favicon 🌙.

Het ontwerp ligt vast — houd het exact aan zodat de pagina elke vrijdag
hetzelfde aanvoelt:
- Fonts via Google Fonts: Bricolage Grotesque (koppen), Source Serif 4
  (lopende tekst), IBM Plex Mono (tijden, cijfers, labels). Altijd een
  echte fallback-stack.
- Licht: bg `#F1EFEA`, surface `#FFFFFF`, sunk `#E6E2DA`, ink `#191713`,
  ink-soft `#5A554C`, ink-faint `#8C877C`, line `#DBD5CA`, accent `#3B5D48`
  (mos), accent-soft `#DEE8E0`, kritiek `#96253C`, waarschuwing `#8A5210`,
  goed `#2F6350`.
- Donker: bg `#100F0D`, surface `#191816`, sunk `#22211E`, ink `#ECE8E1`,
  ink-soft `#A19B90`, ink-faint `#6F6A61`, line `#2B2926`, accent `#8CB99C`,
  accent-soft `#1D2B23`, kritiek `#DF8497`, waarschuwing `#D5A05C`, goed
  `#7EC0A7`.
- Alle kleuren als tokens op kale `:root` (licht), herdefinieer in
  `@media (prefers-color-scheme: dark)` met de guard
  `:root:not([data-theme="light"])`, en nog eens in `:root[data-theme="dark"]`.
  Body krijgt een expliciete achtergrond uit een token. Nooit een kleur die
  alleen binnen een media- of `[data-theme]`-blok bestaat.
- Eén kolom, max-width 680px, mobiel eerst. Kop met monospace-stempel
  (weeknummer, datumbereik, "Grip · Rick Pastoor"), één serif-kop die de
  week benoemt zoals een vriend dat zou doen, en één zin eronder.
- Daarna de zeven stappen, genummerd 01 t/m 07 in monospace in een
  linkerkolom van 44px, elk met titel, de Grip-instructie in cursief, en
  dan de voorbereide data in een kader. Elke stap sluit af met een
  afvinkje ("Afgelopen") dat in localStorage bewaard wordt, netjes in
  try/catch, met een unieke sleutel per week.
- Stap 6 en 7 eindigen met een gestippeld kader waarin de Outliner-velden
  staan waar William zelf in schrijft.
- Geen emoji als sectiemarkering, geen gradients, geen afgeronde kaarten
  met accentbalken.

Lege blokken: is er niets te melden, schrijf dan één rustige zin in plaats
van een leeg kader. Nul verlopen taken is goed nieuws — zeg dat ook zo.

## De node in Outliner

Haal de weeknode op met `get_or_create_calendar_node` (granularity week,
datum van vandaag) en zet er via `import_tana_paste` één node onder:
"Weekafsluiting — week `<nr>`" met de Weekafsluiting-tag (zoek de tag op via
`list_tags`, hardcodeer geen tag-ID). Vul alleen de velden die je uit data
kunt afleiden:
- Datum:: `<datum van vandaag>`
- Reviewpagina:: de artifact-link

Laat deze velden leeg — die zijn van William: De week in één zin, Hoe ging
het met mij, Grootste prestatie, Wat kan beter, Prioriteit 1, Prioriteit 2,
Losgelaten tot maandag.

Zet als kinderen onder de node: één regel per stap met de kern van wat je
vond (agenda, inboxen, taken, areas, vooruitblik), en de kandidaat-
prioriteiten als losse kinderen. Verwijs naar taken met `[[Naam^nodeId]]`
zodat het echte referenties worden. Bestaat er al een weekafsluiting onder
deze weeknode, werk die dan bij in plaats van een tweede toe te voegen.

## Tot slot

Stuur William een pushbericht van één zin met de kern van de week en de
link naar de pagina.
