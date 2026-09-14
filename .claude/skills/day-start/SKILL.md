---
name: day-start
description: William's ochtendbriefing "Dagstart" — agenda uit Google Calendar, afspraken als meeting-nodes in Outliner (Tana) met een Meetings-index en agenda-voorstellen voor 1-op-1's en terugkerende meetings, openstaande taken, keuzes die vandaag om een beslissing vragen, NOS-nieuws, Buienradar-weer, en beurzen + portefeuille. Levert een gestileerde pagina als Artifact, een regel in de dagnotitie, en een pushbericht. Alleen op expliciet verzoek (bv. "start mijn dag", "wat staat er vandaag", "goedemorgen", of /day-start) — niet automatisch.
---

# Dagstart

Maak Williams dagelijkse ochtendbriefing "Dagstart". Schrijf alles in het
Nederlands. Werk zelfstandig af — er is niemand om vragen aan te stellen.
Draait alleen op expliciet verzoek, geen automatische trigger.

> **Over identifiers in dit bestand:** deze repo is publiek. Agenda-ID's,
> Tana-workspace-/node-ID's, de artifact-URL, en concrete
> portefeuillegegevens horen hier dus nooit in te staan, ook niet als
> voorbeeld. Waar de live sessie die gegevens al kent (via connectors, eerder
> in het gesprek, of Claude's geheugen), gebruik ze dan gewoon — maar schrijf
> ze niet terug naar dit bestand.

## Stap 1 — Tijd en plaats

Bepaal eerst de datum en tijd in Europe/Amsterdam via bash
(`TZ=Europe/Amsterdam date`). "Vandaag" is die datum.

## Stap 2 — Agenda (Google Calendar)

Haal de events van vandaag 00:00 t/m morgen 00:00 (Europe/Amsterdam) op uit de
agenda's die van William zijn — doorgaans een privé-agenda, een werkagenda en
een gezinsagenda. Gebruik `list_calendars` om ze te vinden in plaats van
ID's hard te coderen. De werkagenda levert vaak elke afspraak dubbel aan;
ontdubbel op titel plus starttijd voordat je verder werkt. Sorteer
chronologisch, hele-dag-events bovenaan. Bewaar per event de `htmlLink`. Kijk
ook even naar morgen: als er iets is dat vandaag voorbereiding vraagt, noem
dat bij Beslissingen.

## Stap 3 — Taken (Outliner / Tana)

Zoek open taken met een deadline vandaag of eerder, niet afgevinkt, niet in de
prullenbak. Vind eerst zelf de relevante tag, velden en workspace via
`list_workspaces`, `list_tags` en `get_tag_schema` — hardcodeer geen
workspace- of veld-ID's. Lees elke overgebleven taak met `read_node` om
Prioriteit, bovenliggende area/project, deadline en herhaling op te halen.
Sorteer op prioriteit (P1–P4) en dan op hoe lang de deadline verlopen is.
Link naar een taak als `https://app.tana.inc/?nodeid=<nodeId>`.

Terugkerende taken gedragen zich anders dan gewone: elke afronding laat een
afgevinkte kopie met de oude deadline achter, dus dezelfde naam staat vaak
tientallen keren in de graph. Alleen de onafgevinkte is de levende taak — tel
per naam één keer en laat de historie buiten de lijst. Hoe dat mechanisme
precies werkt staat in `docs/outliner-taken.md` in deze repo; lees dat als je
twijfelt over wat je ziet.

**Triage.** Draai daarna de zoekopdracht *Inbox* — zoek hem live op als
search-node met die naam, hardcodeer geen ID. Die zoekt geen open taken maar
onaffe: vastgelegd, maar nog niet zo ingevuld dat ze gepland kunnen worden
(geen area of project, geen prioriteit, geen deadline zonder dat ze someday
zijn, of een terugkerende taak zonder `Occurrence`). Rapporteer de uitkomst
als één regel met het aantal — nooit als lijst, dat is werk voor de
weekafsluiting. Springt er iets uit dat vandaag echt een keuze vraagt — een
terugkerende taak die niet meer doorrolt, of een stapel die boven de tien
uitkomt — neem dat dan mee naar stap 4.

## Stap 4 — Beslissingen

Dit is het belangrijkste blok en het vraagt oordeel, geen opsomming. Noem
maximaal drie dingen waar William vandaag een keuze in moet maken. Goede
kandidaten: een verlopen P1, een taak waarvan de deadline aantoonbaar niet
klopt, een stapel achterstallige taken die om een besluit vraagt (verzetten
of laten staan), een afspraak morgen die vandaag voorbereiding nodig heeft,
twee afspraken die elkaar overlappen. Formuleer elk als een echte vraag met
één zin toelichting en een link naar het item. Zijn er geen echte
beslissingen, laat het blok dan weg — verzin er nooit een.

## Stap 5 — Nieuws (NOS RSS, via WebFetch — nooit curl of python)

Gebruik de RSS-feeds van de NOS:
- `https://feeds.nos.nl/nosnieuwsalgemeen` — het hoofdnieuws
- `https://feeds.nos.nl/nieuwsuuralgemeen` — achtergrond, alleen als het
  hoofdnieuws mager is

Vraag per feed om titel, publicatiedatum en -tijd, beschrijving en link van de
eerste tien items. Deze twee fetches mag je parallel doen.

Kies daaruit de zes tot acht koppen die er voor vandaag toe doen. Schrijf per
kop een samenvatting van één tot twee zinnen, maximaal veertig woorden: de
kern plus het detail dat de kop niet geeft. Geen tussenkopjes, geen
opsomming, gewoon lopende zinnen. De beschrijving uit de feed is meestal
genoeg; is die te dun, haal dan het artikel zelf op via de link uit de feed.
Bewaar per item de publicatietijd (HH:MM) en de artikellink.

Verzin nooit een samenvatting uit alleen een kop. Levert een feed niets
bruikbaars op, schrijf dan één regel dat het nieuws vanochtend niet op te
halen was.

## Stap 6 — Weer (Buienradar, via WebFetch)

Haal `https://data.buienradar.nl/2.0/feed/json` op. Vraag om:
- `forecast.weatherreport`: titel en samenvatting (de redactionele
  vooruitblik van Buienradar);
- `forecast.fivedayforecast`: per dag datum, min- en maxtemperatuur,
  regenkans, zonkans, windrichting en windkracht, en de omschrijving;
- de actuele meting van het dichtstbijzijnde station bij Williams eigen
  locatie: temperatuur, gevoelstemperatuur, windrichting, windkracht in Bft
  en de weersomschrijving.

Schrijf één alinea over vandaag — wat het weer betekent voor de dag zoals
die in de agenda staat, niet alleen de cijfers — en zet daaronder de tabel
met de komende vijf dagen. Staat er een waarschuwing in het weerbericht
(onweer, windstoten, gladheid, hitte), zet die bovenaan.

## Stap 7 — Markten en portefeuille

Om 06:35 zijn alle beurzen dicht. De AEX en de Europese ETF's staan op de
slotkoers van de vorige handelsdag, de Amerikaanse indices en aandelen op de
slotkoers van vannacht. Zeg dat er expliciet bij, met de datum van de stand.
Is het weekend of een beursfeestdag, meld dan dat de beurs gesloten was en
van welke dag de standen zijn; reken de portefeuille dan gewoon door, maar
zeg erbij dat er sinds vrijdag niets veranderd is.

**7a. De grote markers**
- `https://www.google.com/finance/markets/indexes/europe` — deze ene pagina
  geeft in één keer Dow Jones, S&P 500, Nasdaq, DAX, FTSE 100, CAC 40, Nikkei
  en meer, elk met stand, mutatie en percentage. Pak hieruit Dow Jones en
  Nasdaq; noem S&P 500 en DAX alleen als er iets opvallends is.
- `https://www.google.com/finance/quote/AEX:INDEXEURO` — de AEX staat níét
  op die overzichtspagina, dus die haal je apart op.

Google Finance heeft een strakke rate-limit. Haal Google-pagina's één voor
één op, nooit twee in hetzelfde bericht. Krijg je een 429, wacht dan met
`sleep 160` in bash en probeer opnieuw, maximaal twee keer. Lukt het dan nog
steeds niet, laat die stand weg en zeg op de pagina dat hij niet opgehaald
kon worden — verzin nooit een koers en presenteer nooit een oude als de
stand van vandaag.

**7b. Wisselkoers**
`https://api.frankfurter.dev/v1/latest?base=EUR&symbols=USD` — de
ECB-referentiekoers EUR/USD, nodig om Amerikaanse posities naar euro's om te
rekenen. (`api.frankfurter.app` stuurt door naar `.dev`; gebruik meteen
`.dev`.)

**7c. De portefeuille — lees eerst Outliner, hardcode niets**

William houdt zijn beleggingen bij in een Financieel-area in Outliner. Lees
elke ochtend opnieuw (zoek de juiste nodes via `search_nodes`/`list_tags` in
plaats van ID's hard te coderen):
- de beleggingsrekening: waarde, totaal ingelegd, peildatum;
- de posities: symbool/ISIN en beurs per positie;
- de meest recente peiling van die posities: aantal stuks en
  gemiddelde aankoopkoers per positie.

Voegt William een positie toe, verkoopt hij er een of verandert het aantal
stuks, dan loopt dat zo vanzelf mee. Neem nooit aantallen over uit een
eerdere Dagstart of uit dit skill-bestand — er staan hier bewust geen
voorbeeldposities of -bedragen in.

Koersen ophalen — in deze volgorde:
1. **Amerikaanse aandelen** via stockanalysis.com
   (`https://stockanalysis.com/stocks/<ticker>/`, kleine letters). Geen
   rate-limit, dus meerdere posities mag je parallel in één bericht ophalen.
   Gebruik stockanalysis.com nóóit voor Europese ETF's — daar lopen die
   koersen soms weken achter.
2. **Europese ETF's eerst via de API van Börse Frankfurt.** Snel, geen
   rate-limit, dus ook parallel op te halen:
   `https://api.boerse-frankfurt.de/v1/data/quote_box/single?isin=<ISIN>&mic=XETR`
   Je krijgt `lastPrice`, `changeToPrevDayAbsolute`,
   `changeToPrevDayInPercent` en `timestampLastPrice`.
   **Controleer altijd `timestampLastPrice`.** Sommige fondsen worden op
   Xetra nauwelijks verhandeld en geven een koers van weken terug. Is
   `timestampLastPrice` niet van de laatste handelsdag, gooi die koers weg
   en haal die ene positie op via stap 3.
   Dit is de Xetra-notering; die kan een fractie van een procent afwijken
   van Amsterdam. Voor een ochtendbriefing is dat prima — zet in de
   voettekst welke bron je per positie gebruikt hebt als het er meer dan
   één was.
3. **Wat er in stap 2 afviel, haal je bij Google Finance op** — één voor
   één, met de pauzes uit het kader in 7a:
   `https://www.google.com/finance/quote/<SYMBOOL>:<BEURS>`. Beurscodes bij
   Google wijken af van wat Outliner soms noteert: EAM → AMS, NDQ → NASDAQ,
   NSY → NYSE, Xetra → ETR (en als ETR niets geeft, probeer AMS).

Rekenen:
- Waarde per positie = aantal stuks × koers. Amerikaanse posities in
  dollars deel je door de EUR/USD-koers uit 7b.
- Dagmutatie per positie in euro's = aantal stuks × het koersverschil van
  vandaag (of waarde × dagmutatie-percentage).
- Cashsaldo: het verschil tussen de rekeningwaarde en de som van de posities
  bij de laatste peiling in Outliner. Leid dat af, hardcode het niet, en tel
  het mee in het totaal.
- Portefeuillewaarde = som van de posities + cashsaldo.
- Ongerealiseerd resultaat = portefeuillewaarde − totaal ingelegd, ook als
  percentage.

Reken netjes na en rond op de pagina af op hele euro's.

Wat je toont — houd het rustig. Het blok is: één regel met de
portefeuillewaarde, de dagmutatie in euro's en procenten, en het
ongerealiseerde resultaat sinds inleg. Daaronder hooguit drie posities die
er die dag echt uitspringen: de grootste beweger in euro's, een positie die
meer dan drie procent beweegt, of een positie waar een lopende open vraag
uit de Financieel-area aan hangt. Geen volledige tabel van alle posities —
die hoort bij een maandelijkse routine, niet bij een ochtendbriefing.
Beweegt er niets van betekenis, schrijf dan één zin dat het een rustige dag
was en laat de posities weg.

Raakt de stand van vandaag aantoonbaar aan een open scenario uit Williams
eigen Financieel-notities, dan mag je daar hooguit één zin over schrijven.
Nooit vaker dan één zo'n zin per ochtend, en nooit als aansporing om iets te
doen. Je bent geen financieel adviseur en dit is geen advies.

Deze stap is read-only: schrijf niets terug naar de Financieel-area en maak
geen waardepeilingen aan. Dat blijft Williams eigen, aparte routine.

## Stap 8 — De pagina

Schrijf een self-contained HTML-bestand en publiceer het met de
Artifact-tool. Gebruik dezelfde artifact-URL als de vorige ochtend zodat de
link stabiel blijft — lees die eerst terug met de Artifact-tool
(`action: "read"`) en gebruik hem als sjabloon: dezelfde CSS, dezelfde
opbouw, alleen nieuwe inhoud. (Die URL hoort niet in dit bestand thuis —
gebruik de URL die je al kent van vorige keren.) Titel `<title>Dagstart</title>`,
favicon ☕.

Het ontwerp ligt vast — houd het exact aan zodat de pagina elke ochtend
hetzelfde aanvoelt:
- Fonts via Google Fonts: Bricolage Grotesque (koppen), Source Serif 4
  (lopende tekst), IBM Plex Mono (tijden, cijfers, labels). Altijd een
  echte fallback-stack.
- Licht: bg `#EEF1F4`, surface `#FFFFFF`, surface-sunk `#E4E8ED`, ink
  `#101620`, ink-soft `#57616F`, ink-faint `#8A94A2`, line `#D8DEE6`,
  accent `#12566E`, accent-soft `#DCEAF0`, kritiek `#99223A`, waarschuwing
  `#8A4A0B`, goed `#2C6350`, bron-geel `#B8890B`.
- Donker: bg `#0D1117`, surface `#151B23`, surface-sunk `#1D242E`, ink
  `#E6EAF0`, ink-soft `#9BA5B4`, ink-faint `#6C7686`, line `#262E39`,
  accent `#6FB9D6`, accent-soft `#17303C`, kritiek `#E08497`, waarschuwing
  `#D9A05B`, goed `#7FC0A8`, bron-geel `#E3B44A`.
- Definieer alle kleuren als tokens op kale `:root` (licht), herdefinieer
  ze in `@media (prefers-color-scheme: dark)` met de guard
  `:root:not([data-theme="light"])`, en nog eens in `:root[data-theme="dark"]`.
  Geef `body` een expliciete achtergrond uit een token. Nooit een kleur die
  alleen binnen een media- of `[data-theme]`-blok bestaat.
- Eén kolom, max-width 640px, mobiel eerst. Volgorde: kop → Vandaag (agenda)
  → Beslissingen → Open taken → Nieuws → Weer → Markten & portefeuille →
  voettekst met bronnen.
- Onder Open taken sluit één regel in ink-faint de sectie af met de uitkomst
  van de triage uit stap 3 (aantal onaffe taken en projecten). Is er niets
  onaf, laat die regel dan weg.
- Elke sectiekop heeft rechts een klein bronlabel in monospace: "Google
  Agenda", "Outliner", "NOS", "Buienradar", "Outliner + koersen".
- De kop bestaat uit: een monospace regel met datum (en plaats, als de
  agenda dat verraadt); één serif-kop van maximaal twee regels die de dag
  benoemt zoals een vriend dat zou doen (de vorm van de dag, of het één
  ding dat hem bijzonder maakt — niet allebei); en één zin eronder die zegt
  wat er vandaag van hem gevraagd wordt.
- Heeft een afspraak een agenda-voorstel gekregen, zet de punten dan compact
  onder die agendaregel: hooguit drie, elk één regel, in ink-soft. Het
  volledige voorstel staat in Outliner — de pagina toont alleen genoeg om
  hem eraan te herinneren.
- Een nieuwsitem is: links de publicatietijd (HH:MM) in bron-geel
  monospace, rechts de kop (Bricolage Grotesque, klikbaar naar de
  artikellink) met daaronder de samenvatting in Source Serif 4, kleur
  ink-soft, regelbreedte maximaal 52 tekens.
- Het blok Markten & portefeuille: bovenaan drie cijfers naast elkaar —
  AEX, Dow Jones, Nasdaq — elk met stand en dagmutatie in procenten, mutatie
  in de kleur goed of kritiek, cijfers in monospace met tabular-nums.
  Daaronder een dunne lijn en dan de portefeuille: de waarde groot in
  monospace, daarnaast de dagmutatie in euro's en procenten, en op een
  tweede regel in ink-soft het ongerealiseerde resultaat sinds inleg.
  Daaronder hooguit drie regels met de opvallende posities: naam links,
  dagmutatie rechts. Sluit af met één regel in ink-faint die zegt van welke
  datum en welk moment de standen zijn en welke bronnen je gebruikt hebt.
- Tijden en cijfers in monospace met `font-variant-numeric: tabular-nums`.
  Brede tabellen in een container met `overflow-x: auto`.
- Geen emoji als sectiemarkering, geen gradients, geen kaarten met
  afgeronde accentbalken.

Lege blokken: heeft de agenda niets, schrijf dan één rustige zin in plaats
van een leeg kader. Zijn er geen open taken, zeg dat dan ook zo — dat is
goed nieuws.

## Stap 9 — Afspraken als meeting-nodes

Zet elke echte afspraak van vandaag als losse node onder de calendar-node van
vandaag, naast de Dagstart-node uit stap 11. Alleen afspraken met andere
mensen — sla blokken over die geen meeting zijn: schoolrit, focusblok, lunch,
sport, reistijd. Bij twijfel: geen deelnemers of alleen jezelf is geen meeting.

Zoek de juiste supertags en velden elke ochtend opnieuw op via `list_tags` en
`get_tag_schema`. Hardcodeer geen tag-, veld- of node-ID's in dit bestand —
deze repo is publiek.

Welke tag:
- Een afspraak met precies één andere persoon krijgt de 1-op-1-meeting-tag,
  met het team-member-veld gevuld met de bestaande #person-node van die
  persoon. Bestaat die persoon nog niet als node, gebruik dan de gewone
  meeting-tag en laat het veld leeg — maak nooit een nieuwe persoonsnode aan.
- Alle andere afspraken krijgen de gewone meeting-tag.

Vul per node:
- de datum als tijdsbereik in UTC, met een `Z` achter elke tijd:
  `JJJJ-MM-DDTuu:mmZ/JJJJ-MM-DDTuu:mmZ`. Zet die met `set_field_content` op
  het datumveld — niet als `[[date:...]]` in de tana-paste. Lees hieronder
  waarom: dit is de enige vorm die in Tana op de juiste plek in de dag landt;
- de Teams-joinlink uit de uitnodiging;
- de agendalink: de `htmlLink` uit Google Calendar, waarbij je de spatie in de
  `eid`-parameter vervangt door `%20` — anders is de link stuk;
- een omschrijving met de organisator en de deelnemersnamen als platte tekst.
  Zet deelnemers nooit als `[[referenties]]`: dat maakt tientallen lege
  persoonsnodes aan.

**Tijdzone — de valkuil die zichzelf verbergt.** Een datumveld zonder
tijdzone slaat Tana op als UTC. Schrijf je de Amsterdamse kloktijd rauw weg
(`2026-09-14 11:00`), dan staat de afspraak in Tana twee uur te laat in de
zomer en één uur in de winter. Het venijn zit in de controle: lees je het
veld terug, dan geeft de MCP de opgeslagen wandklok terug zónder tijdzone —
`11:00`, precies wat je wilde zien — terwijl de agendaweergave hem op 13:00
zet. Teruglezen bevestigt de fout dus in plaats van hem te betrappen.

Reken de tijd uit Google Calendar daarom altijd eerst om naar UTC en zet er
`Z` achter:

| Google Calendar (Europe/Amsterdam) | wat je wegschrijft |
| --- | --- |
| 11:00–11:45, zomertijd (UTC+2) | `2026-09-14T09:00Z/2026-09-14T09:45Z` |
| 11:00–11:45, wintertijd (UTC+1) | `2026-11-16T10:00Z/2026-11-16T10:45Z` |

Bepaal de offset per datum in plaats van hem vast te nemen — eind maart en
eind oktober klopt een vaste twee uur niet meer. Google Calendar geeft de
offset zelf al mee in `start.dateTime` (`2026-09-14T11:00:00+02:00`); reken
daarmee, of laat bash het doen:

```bash
# Amsterdamse kloktijd -> UTC-instant
date -u -d "@$(TZ=Europe/Amsterdam date -d '2026-09-14 11:00' +%s)" '+%Y-%m-%dT%H:%MZ'
# 2026-09-14T09:00Z
```

Let op de omweg via `+%s`: `TZ=Europe/Amsterdam date -u -d '...'` lijkt
hetzelfde te doen maar is het niet — `-u` zet ook het *parsen* op UTC, dus
die vorm geeft de tijd onveranderd terug en je denkt dat je hebt omgerekend.

Twee regels die hieruit volgen:
- Controleer een tijd nooit door het datumveld terug te lezen. Dat geeft de
  UTC-wandklok en niet wat William in zijn agenda ziet. Wil je echt
  verifiëren, kijk dan in de agendaweergave van Tana zelf.
- De Meetings-index en alle tijden in de briefing blijven gewoon Amsterdamse
  tijd. Die haal je uit Google Calendar, nooit uit het Tana-datumveld — dan
  kan de UTC-opslag er ook niet in lekken.

Staat er een echt doel of een vraag in de uitnodiging, zet die dan in het
purpose-veld (gewone meeting) of het prep-veld (1-op-1). Botst de afspraak
met een andere, zet dat als losse regel eronder.

Werk idempotent: kijk eerst of er voor vandaag al meeting-nodes bestaan
voordat je iets aanmaakt. Zoek daarbij op titel door de hele workspace, niet
alleen onder de dagnode — de tweede schrijver zet zijn nodes ergens anders
neer, dus een controle die alleen naar de dagnode kijkt vindt nooit iets en
meldt altijd "nog niets aanwezig".

Houd er rekening mee dat je niet de enige schrijver bent. William heeft een
aparte Google Calendar Events-koppeling die per afspraak zelf een node onder
Library zet, mét datumveld, dus die verschijnt ook in de agendaweergave.
Die koppeling synchroniseert later op de ochtend dan deze skill draait: als
jij om 07:00 langskomt bestaat de node van vandaag meestal nog niet. Je kunt
er dus niet op vertrouwen dat je hem vindt, en "gewoon even controleren" lost
de dubbeling niet op. Vind je hem wél, hang je aanvullingen — purpose,
agenda-voorstel, botsingen — dan onder die bestaande node en maak geen
tweede.

Zet daarna één node "Meetings" onder de calendar-node van vandaag, met
daaronder een referentie naar elke meeting-node van die dag, chronologisch en
met de tijd ervoor: `08:30–09:30 — [[Titel^nodeId]]`. Dat is de index — de
dagnode blijft leesbaar en één klik brengt hem in het gesprek zelf. Ook hier
idempotent: bestaat die Meetings-node al, werk hem dan bij in plaats van een
tweede toe te voegen.

Zijn er geen echte afspraken vandaag, sla deze stap dan stil over.

## Stap 10 — Agenda-voorstel voor 1-op-1's en terugkerende meetings

Bij gesprekken die zich herhalen kun je vooraf zien wat er speelt. Stel daar
een agenda voor — een vóórstel, geen besluit: het blijft Williams gesprek.

**Voor welke gesprekken.** Elke 1-op-1 van vandaag, en elke terugkerende
meeting waarvan je een eerdere instantie met dezelfde titel terugvindt. Niet
voor eenmalige afspraken van iemand anders. Stuurde de organisator zelf al
een agenda of doel mee, stel dan alleen voor wát William inbrengt — een
vergadering van een ander is niet aan jou om in te delen.

**Waar je het vandaan haalt**, in volgorde van sterkte:
1. Openstaande items met de discuss-tag voor die persoon (het
   team-member-veld). Oudere items hebben dat veld soms niet — match dan op
   de voornaam in de titel. Zet erbij hoe lang het al wacht: iets van vier
   maanden geleden is óf urgent óf dood, en dat verschil is zelf een
   agendapunt.
2. Openstaande action items uit de vorige instantie van hetzelfde gesprek.
3. Taken die aan de ander gedelegeerd zijn, en taken uit de area of het
   project van die persoon.
4. Een doel-1-op-1 van die persoon — niet elke week, wel als er weken niets
   over gezegd is of als er iets aan bewoog.
5. Notitie-1-op-1-items van na het vorige gesprek.
6. Mail: een thread met die persoon die nog op antwoord wacht. Alleen voor
   1-op-1's, en alleen als er echt iets openstaat.
7. De samenvatting van het vorige gesprek — wat bleef daar hangen.

**Hoe je het samenstelt.** Dit is het deel dat oordeel vraagt:
- Drie tot vijf punten, niet meer. Een half uur is een half uur.
- Sorteer op wat het duurst is om over te slaan, niet op chronologie.
- Eén regel per punt, met een referentie naar waar het vandaan komt. Geen
  betoog: dit wordt twee minuten voor het gesprek gelezen.
- Zet apart wat William van de ander wil en wat de ánder van hém wacht. Een
  1-op-1 waarin alleen zijn eigen vragen staan is een statusupdate, geen
  gesprek. Zoek dus actief naar wat bij hem ligt: taken die aan hem
  gedelegeerd zijn, mail waar hij niet op antwoordde, een toezegging van
  vorige keer.
- Kijk naar de verhouding via het reporting-to-veld op de #person-node.
  Rapporteert de ander aan William, dan hoort er periodiek iets in over hun
  doelen en hoe het met ze gaat, niet alleen over lopende zaken. Rapporteert
  William aan de ander, dan zijn het vooral beslissingen die hij nodig heeft
  en dingen die hij moet melden. Zijn ze gelijken, dan gaat het over
  afstemming.
- Schrijf feiten en vragen, nooit een oordeel over de persoon. Dit gaat over
  echte collega's, in een systeem dat hij kan delen.
- Verzin nooit een punt dat je niet kunt herleiden. Vind je niets, schrijf
  dan dat je niets vond — een leeg voorstel is eerlijker dan een gevuld.

**Waar het landt.** Als kind van de meeting-node één node "Agenda-voorstel"
met de punten eronder. Het prep-veld (1-op-1) en het agenda- of purpose-veld
(gewone meeting) laat je met rust zodra er iets in staat — dat is van William
zelf of van de organisator. Is zo'n veld leeg, dan mag je er één regel
context in zetten, zoals stap 9 al beschrijft.

**Houd het betaalbaar.** Op een dag met negen afspraken is dit anders te veel
werk. Doe elke 1-op-1, en daarnaast hooguit de drie terugkerende meetings die
er vandaag het meest toe doen. Voor de rest volstaat de meeting-node zonder
voorstel.

## Stap 11 — Node in de dagnotitie

Zet in Williams dagelijkse journaal-structuur in Outliner (de calendar-node
van vandaag) één node "Dagstart — <dag> <datum>" met daaronder: de link naar
de pagina, één regel over de agenda, de beslissingen als losse kinderen, één
regel met het aantal open taken en de areas, de vier belangrijkste
nieuwskoppen elk met hun samenvatting van één zin, één regel weer, één regel
markten (AEX, Dow Jones, Nasdaq met hun dagmutatie) en één regel
portefeuille (waarde, dagmutatie in euro's, ongerealiseerd resultaat).
Verwijs naar taken met `[[Naam^nodeId]]` zodat het echte referenties worden.
Bestaat er al een Dagstart-node onder vandaag, werk die dan bij in plaats
van een tweede toe te voegen.

De meetings staan al onder de dagnode uit stap 9 — herhaal ze hier niet.
Noem in de agendaregel hooguit welke gesprekken een agenda-voorstel kregen.

## Stap 12 — Pushbericht

Stuur William een pushbericht van één zin met de kern van vandaag en de
link naar de pagina.
