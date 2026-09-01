---
name: monthly-energy
description: William's maandelijkse energiebalans — zonproductie (Solarman), afname en kosten bij de energieleverancier (Tibber), laadvergoeding van de laadpaalbeheerder (50Five/EVC-net) en ERE-certificaten (Joulo). Vult de maandtabel in Outliner (Tana), rondt de terugkerende taak af, en meldt of de maand geld kostte of opleverde. Levert een gestileerde pagina als Artifact en een pushbericht. Alleen op expliciet verzoek (bv. "energierekening", "maandoverzicht energie", of /monthly-energy) — niet automatisch.
---

# Maandbalans energie

Zet één maand energie op een rijtje: wat de zon opbracht, wat er van het net
af kwam en wat dat kostte, wat de laadpaal aan vergoeding opleverde en wat de
ERE-certificaten opbrachten. Schrijf alles in het Nederlands. Werk zelfstandig
af waar het kan; vraag alleen als een bron echt niet te bereiken is en het
getal nergens anders vandaan komt.

> **Over identifiers en geheimen in dit bestand:** deze repo is publiek.
> Node-ID's, workspace-ID's, de artifact-URL, klantnummers, meterstanden,
> bedragen en vooral **API-sleutels, tokens en wachtwoorden** horen hier nooit
> in te staan — ook niet als voorbeeld. Waar een bron een sleutel nodig heeft,
> zegt dit bestand alleen wáár die te vinden is. Schrijf een sleutel die je
> onderweg tegenkomt nooit terug naar de repo, naar een Artifact, of naar een
> Outliner-node.

## Wat deze skill nodig heeft

Deze skill haalt gegevens van buiten. Draai hem in een sessie die het internet
op kan (Cowork op Williams eigen machine). In een afgeschermde omgeving —
Claude Code op het web draait achter een egress-proxy die deze hosts weigert —
komt hij niet bij de bronnen; ga dan niet gokken, maar meld per bron dat hij
niet bereikbaar was en vraag William om de getallen. Zie
`docs/energie-bronnen.md` voor het volledige bronnenlandschap.

## Stap 1 — Welke maand, en wat staat er al

Bepaal de datum in Europe/Amsterdam via bash (`TZ=Europe/Amsterdam date`).
Standaard verwerk je de **vorige volledige maand**: draai je in september, dan
gaat het over augustus. Noemt William zelf een maand, neem die.

Zoek de maandtabel live op in Outliner — een node met de laadkosten en de
vergoeding van de laadpaalbeheerder, met per maand een kindnode. Zoek hem via
`search_nodes` op naam of via de terugkerende taak uit stap 6; hardcodeer geen
node-ID. Lees hem met `read_node` (maxDepth 3).

Kijk goed naar wat je aantreft voordat je iets schrijft:

- **Welke velden gebruikt William?** Doorgaans: productie (zon, kWh), afname
  van het net (kWh), teruglevering (kWh), kosten energie (€), gedeclareerde
  laad-kWh, vergoeding van de laadpaalbeheerder (€), en de ERE-opbrengst (€).
  Niet elke maand heeft ze allemaal — dat is geen fout, dat is de historie.
  Voeg nooit een veld toe dat er nog niet is zonder het te melden.
- **Hoe heten de maanden?** Oudere rijen dragen het jaartal, recente rijen
  alleen de maandnaam. Volg wat je ziet. Wisselt William van leverancier
  midden in een maand, dan staan er twee rijen voor die maand met de
  leverancier tussen haakjes; splits alleen als de facturen dat afdwingen.
- **Bestaat de rij al?** Vaak vult William een maand halverwege alvast deels.
  Vul dan aan in plaats van een tweede rij te maken, en overschrijf een getal
  dat er al staat alleen als je bron het tegenspreekt — zeg dat dan ook.

Vanaf het jaar waarin een kale maandnaam voor de tweede keer langskomt, botsen
de rijen. Zet er vanaf dat moment het jaartal bij en meld dat je dat gedaan
hebt.

## Stap 2 — De bronnen

Vier bronnen, vier getallen-groepen. Haal ze op in deze volgorde; de eerste
twee zijn API's en gaan vanzelf, de laatste twee zijn portalen en vragen meer.

Doe per bron **eerst de API, dan het portaal, dan pas de vraag aan William**.
Verzin nooit een getal, en presenteer nooit een getal van vorige maand als dat
van deze.

### 2a. Zonproductie — Solarman

William heeft een eigen App ID en App Secret voor de Solarman Open API; die
zijn per e-mail toegekend door de Solarman-klantenservice (zoek in Gmail op de
afzender van Solarman en op "API"). Gebruik ze uit die bron — schrijf ze
nergens naartoe.

De API zit op `https://globalapi.solarmanpv.com`, gedocumenteerd op
`https://doc.solarmanpv.com/`. Grote lijn: haal een token op via
`/account/v1.0/token` (App ID en App Secret als queryparameters, account en
wachtwoord in de body — het wachtwoord gehasht zoals de documentatie
voorschrijft), zoek de installatie op via `/station/v1.0/list`, en vraag de
maandopbrengst op via de history-endpoint van de station-API met de maand als
tijdsbereik. Controleer de exacte veld- en parameternamen in de documentatie
voordat je ze gebruikt; ze verschillen per API-versie.

Wat je eruit haalt: **productie in kWh over de maand**.

Lukt de API niet, dan staat hetzelfde getal in het Solarman-portaal onder de
plant-gegevens, in de maandweergave.

Een derde route die het scrapen helemaal overbodig maakt: Home Assistant. Als
William zijn omvormer- of zonne-sensor daar deelt met de assistent, is de
maandproductie met `GetLiveContext` op te vragen. Op dit moment zijn er geen
energie-entiteiten gedeeld — noem het één keer als suggestie, niet elke maand.

### 2b. Afname, teruglevering en kosten — Tibber

Tibber heeft een echte GraphQL-API: endpoint `https://api.tibber.com/v1-beta/gql`,
met een persoonlijk toegangstoken uit `https://developer.tibber.com/settings/access-token`
als `Authorization: Bearer <token>`. Bestaat dat token nog niet, vraag William
er dan één keer om en laat hem het bewaren waar de andere sleutels staan —
niet in deze repo.

Vraag per maand op wat je nodig hebt, met `resolution: MONTHLY`: onder
`viewer.homes.consumption` staan `from`, `to`, `consumption` (kWh) en `cost`
(€), onder `viewer.homes.production` staan `production` (kWh) en `profit` (€).
Pak de node waarvan `from` in de doelmaand valt.

Wat je eruit haalt: **afname in kWh**, **teruglevering in kWh** en **kosten
energie in €**.

De kosten kun je kruiscontroleren met de maandfactuur die Tibber rond de
zevende van de volgende maand mailt ("Factuur voor 1 <maand> — 31 <maand>").
Die factuur is een PDF-bijlage; de Gmail-connector geeft je wel de mail en de
naam van de bijlage, maar niet de inhoud ervan. Gebruik de mail dus als
**bevestiging dat de maand is afgesloten**, niet als bron van het bedrag. Is
de factuurmail er nog niet, dan is de maand bij Tibber nog niet definitief —
zet het bedrag er wel in, maar noem het voorlopig.

Let op de leverancierswissel: kosten van vóór de overstap horen bij de oude
leverancier en komen niet uit deze API.

### 2c. Laadvergoeding — 50Five (EVC-net)

Hier is geen API. De vergoeding staat in het EVC-net-portaal, in het
transactieoverzicht per eigen laadpunt per maand; het rapporttype dat per maand
groepeert geeft in één scherm de gedeclareerde kWh en het bedrag per maand.
De taak in Outliner (stap 6) draagt de link met de juiste filters — pas de
begin- en einddatum aan naar het bereik dat je nodig hebt.

Het portaal vraagt een login. Heeft deze sessie een browser-tool met Williams
ingelogde profiel, gebruik die. Zo niet, vraag William om twee getallen: de
**gedeclareerde kWh** en de **vergoeding in €** voor de doelmaand. Vraag dat in
één keer, samen met wat je verder nog mist — niet per bron opnieuw.

De maandregel loopt achter: 50Five keert per maand af, maar de regel voor de
laatste maand kan nog groeien zolang niet alle sessies verwerkt zijn. Vergelijk
het bedrag met de gedeclareerde kWh — de verhouding tussen die twee is over de
maanden heen opvallend stabiel. Wijkt hij sterk af, meld dat als
aandachtspunt in plaats van het getal stil te slikken.

### 2d. ERE-certificaten — Joulo

Ook geen API. Joulo toont de opbrengst in het dashboard onder de
uitbetalingen; de taak in Outliner draagt die link. Zelfde aanpak als bij
50Five: browser-tool als die er is, anders vragen.

Joulo betaalt niet elke maand uit — een maand zonder uitbetaling is een lege
cel, geen nul die je moet verzinnen. Joulo mailt wel (afzender `noreply@joulo.nl`);
staat er een uitbetalingsmail in Gmail voor deze maand, gebruik die.

## Stap 3 — Rekenen

Reken twee dingen uit, allebei over de doelmaand:

- **Netto energie**: vergoeding laadpaal + ERE-opbrengst − kosten energie.
  Positief is winst, negatief is kosten. Dit is het getal waar het William om
  gaat.
- **Eigen verbruik tegenover laden**: afname van het net minus de gedeclareerde
  laad-kWh — wat het huis zelf verbruikte bovenop de auto.

Zet er twee regels context bij die alleen uit de tabel kunnen komen: hoe deze
maand zich verhoudt tot dezelfde maand vorig jaar (als die er is), en tot het
lopende jaargemiddelde. Rond bedragen af op hele euro's, kWh op hele
kilowattuur.

Klopt er iets niet — productie in de zomer die onder die van december ligt,
een vergoeding zonder gedeclareerde kWh, afname die verdubbelt zonder reden —
noem dat expliciet als aandachtspunt. Een maand die niet klopt is nuttiger dan
een maand die netjes lijkt.

## Stap 4 — De tabel bijwerken

Schrijf de maand weg als kindnode onder de maandtabel, met dezelfde
veldnamen als de bestaande rijen. Gebruik `import_tana_paste` met
`Veldnaam::waarde`-regels — dat koppelt op naam, zodat je geen veld-ID's hoeft
te hardcoderen.

Werk idempotent: bestaat de rij al, vul dan de lege velden aan in plaats van
een tweede rij te maken. Een bron die je niet kon ophalen laat je leeg — schrijf
nooit een 0 waar je "onbekend" bedoelt.

## Stap 5 — Nog een keer nalezen

Lees de tabel terug met `read_node` en controleer dat er precies één rij voor
de doelmaand staat, met de waarden die je bedoelde. Klopt er iets niet, herstel
het voordat je verder gaat.

## Stap 6 — De terugkerende taak afronden

De taak heet iets als "Kosten / baten laadpaal op een rijtje zetten", draagt de
recurring-tag, hangt onder de Huis-area en verwijst naar de maandtabel. Zoek hem
live op via `search_nodes`; hardcodeer geen node-ID.

**Vink het selectievakje van het origineel niet aan.** Dat sluit de reeks
definitief en er komt niets voor terug. Het commando *Complete — Complete and
reschedule* dat dit netjes doet, is een Tana-commando en niet via de
MCP-koppeling aan te roepen. Bootst het daarom stap voor stap na — de vier
stappen staan in `docs/outliner-taken.md`, onder "Complete and reschedule
nabootsen". Kort:

1. maak een kopie van de taak als sibling op dezelfde plek, met dezelfde tag,
   dezelfde velden en de **oude** deadline;
2. vink die kopie af — dat is de historie;
3. schuif op het origineel `Deadline Date` één `Occurrence` vooruit, met de
   oude deadline als referentie;
4. laat het origineel onafgevinkt en laat de taakstatus met rust.

Twijfel je of de kopie klopt, of lukt stap 1 niet volledig, **laat de taak dan
staan** en zeg tegen William dat hij het commando zelf moet draaien. Een reeks
die stilvalt is erger dan een taak die nog open staat.

## Stap 7 — De pagina

Schrijf een self-contained HTML-bestand en publiceer het met de Artifact-tool.
Gebruik dezelfde artifact-URL als vorige maand zodat de link stabiel blijft —
lees die eerst terug met `action: "read"` en gebruik hem als sjabloon. (Die URL
hoort niet in dit bestand thuis.) Titel `<title>Energiebalans</title>`,
favicon ⚡.

Het ontwerp volgt dezelfde taal als de Dagstart, zodat Williams pagina's één
familie blijven:

- Fonts via Google Fonts: Bricolage Grotesque (koppen), Source Serif 4
  (lopende tekst), IBM Plex Mono (cijfers, labels, eenheden). Altijd een echte
  fallback-stack.
- Licht: bg `#EEF1F4`, surface `#FFFFFF`, surface-sunk `#E4E8ED`, ink
  `#101620`, ink-soft `#57616F`, ink-faint `#8A94A2`, line `#D8DEE6`, accent
  `#12566E`, accent-soft `#DCEAF0`, kritiek `#99223A`, waarschuwing `#8A4A0B`,
  goed `#2C6350`, bron-geel `#B8890B`.
- Donker: bg `#0D1117`, surface `#151B23`, surface-sunk `#1D242E`, ink
  `#E6EAF0`, ink-soft `#9BA5B4`, ink-faint `#6C7686`, line `#262E39`, accent
  `#6FB9D6`, accent-soft `#17303C`, kritiek `#E08497`, waarschuwing `#D9A05B`,
  goed `#7FC0A8`, bron-geel `#E3B44A`.
- Definieer alle kleuren als tokens op kale `:root` (licht), herdefinieer ze in
  `@media (prefers-color-scheme: dark)` met de guard
  `:root:not([data-theme="light"])`, en nog eens in `:root[data-theme="dark"]`.
  Geef `body` een expliciete achtergrond uit een token. Nooit een kleur die
  alleen binnen een media- of `[data-theme]`-blok bestaat.
- Eén kolom, max-width 640px, mobiel eerst. Volgorde: kop → de maand in één
  getal → de vier bronnen → de tabel over twaalf maanden → aandachtspunten →
  voettekst met bronnen en peildatum.
- De kop: een monospace regel met de maand en het jaar; één serif-kop van
  maximaal twee regels die zegt wat voor maand het was; en één zin eronder met
  wat dat betekent.
- "De maand in één getal" is het nettoresultaat, groot in monospace, in de
  kleur goed (winst) of kritiek (kosten), met op een tweede regel in ink-soft
  hoe dat is opgebouwd: vergoeding plus ERE min kosten.
- De vier bronnen als vier regels: links de bron in monospace, rechts het
  getal met eenheid. Een bron die je niet kon ophalen krijgt een streepje in
  ink-faint en een reden in de voettekst — nooit een verzonnen waarde.
- De twaalf-maandentabel in een container met `overflow-x: auto`, cijfers in
  monospace met `font-variant-numeric: tabular-nums`. Markeer de nieuwe maand
  subtiel met de accentkleur.
- Elke sectiekop heeft rechts een klein bronlabel in monospace: "Solarman",
  "Tibber", "50Five", "Joulo", "Outliner".
- Geen emoji als sectiemarkering, geen gradients, geen kaarten met afgeronde
  accentbalken.

## Stap 8 — Node in Outliner

Zet onder de maand-calendarnode van de doelmaand één node "Energiebalans —
<maand> <jaar>" met daaronder: de link naar de pagina, één regel met het
nettoresultaat, één regel per bron met het getal, en de aandachtspunten als
losse kinderen. Verwijs naar de maandtabel en de taak met `[[Naam^nodeId]]`
zodat het echte referenties worden. Bestaat de node al, werk hem dan bij in
plaats van een tweede toe te voegen.

## Stap 9 — Pushbericht

Stuur William één zin: of de maand geld kostte of opleverde, met het bedrag en
de link naar de pagina. Kon je een bron niet ophalen, zeg dat in dezelfde zin —
niet in een tweede bericht.
