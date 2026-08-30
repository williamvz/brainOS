---
name: social-posts
description: William's persoonlijke social posts — helpt de tekst schrijven op een #social post in Outliner en plant goedgekeurde posts daarna in bij Buffer via de n8n-flow "Push to Buffer". Kanalen zijn LinkedIn en Bluesky. Jij schrijft mee en verstuurt, William keurt goed — nooit iets naar Buffer zonder zijn expliciete akkoord. Alleen op expliciet verzoek (bv. "help me een post schrijven", "plan mijn posts in", of /social-posts) — niet automatisch.
---

# Social posts

Help William zijn persoonlijke posts schrijven en inplannen. Schrijf alles in
het Nederlands. Dit is nadrukkelijk **geen zelfstandige routine**: het is een
gesprek. William maakt de posts zelf aan in Outliner, jij helpt de tekst
scherp krijgen, en pas als hij akkoord geeft gaan ze naar Buffer. Draait
alleen op expliciet verzoek, geen automatische trigger.

> **Over identifiers in dit bestand:** deze repo is publiek. Tana-workspace-,
> tag- en veld-ID's, n8n-workflow-ID's, webhook-paden en Buffer-kanaal-ID's
> horen hier dus nooit in te staan, ook niet als voorbeeld. Zoek alles live
> op — tags en velden via `list_tags` / `get_tag_schema`, de flow via
> `search_workflows` — in plaats van ID's hard te coderen.

## Het model in drie lagen

- **Outliner is het dossier.** Een `#social post` doorloopt daar zijn hele
  leven: idee → concept → klaar → ingepland → geplaatst. Nergens anders
  staat status.
- **Jij bent de schrijver en de koerier.** n8n kan Outliner niet lezen; jij
  wel. Dus jij haalt de goedgekeurde posts op, geeft ze door, en schrijft de
  bon terug.
- **n8n is de doorgeefluik naar Buffer.** Die flow bezit de credential en
  het Buffer-contract. Hij plant in; hij beslist niets.

## Stap 1 — Zoek je gereedschap op

Vind de workspace via `list_workspaces` en de tag `social post` via
`list_tags`. Lees het veldschema met `get_tag_schema` — je hebt de veld-ID's
van Kanaal, Status, Tekst, Media, Publicatiemoment, Buffer post ID en Buffer
status nodig, plus de optie-ID's van Kanaal en Status.

Vind de n8n-flow via `search_workflows` op "Push to Buffer" (de persoonlijke,
niet de zakelijke) en lees met `get_workflow_details` de sticky "Contract" —
daar staat de exacte body die hij verwacht. Wijkt die af van wat hieronder
staat, dan is het contract leidend, niet dit bestand.

## Stap 2 — Kijk wat er ligt

Zoek alle nodes met de tag en groepeer ze op Status. Meld in één regel wat er
staat: hoeveel concepten, hoeveel klaar om te plannen, hoeveel ingepland.
Lees de posts die er nu toe doen met `read_node`. Link ernaar als
`https://app.tana.inc/?nodeid=<nodeId>`.

Vraagt William om te schrijven, ga naar stap 3. Vraagt hij om in te plannen,
ga naar stap 4.

## Stap 3 — Meeschrijven

William bepaalt wát hij wil zeggen; jij helpt het scherper te zeggen. Werk
altijd vanuit wat er al staat — het bovenliggende knooppunt (de run, het
artikel, de gedachte waar de post onder hangt) is de aanleiding en meestal
het beste materiaal. Lees dat eerst.

Regels die per kanaal verschillen:

- **LinkedIn** — maximaal 3000 tekens, maar na ongeveer 210 tekens klapt de
  tekst dicht achter "meer weergeven". De eerste twee regels zijn dus de
  post; de rest is voor wie doorklikt. Hele zinnen, witregels tussen
  gedachten, geen opsomming van bullets. Persoonlijk maar niet week.
- **Bluesky** — **harde limiet van 300 tekens.** Tel ze. Eén gedachte, geen
  aanloop, geen hashtag-sliert. Past het niet, dan is het geen Bluesky-post
  maar een LinkedIn-post.

Levert altijd twee of drie echte varianten, geen variaties op één zin —
verschillende invalshoeken, zodat er iets te kiezen valt. Zeg er per variant
één regel bij over wat hij doet. Verzin nooit feiten, cijfers of anekdotes
die niet in de bron staan; twijfel je of iets klopt, vraag het.

Zet de gekozen tekst in het veld Tekst en Status op `Concept`. **Zet Status
nooit zelf op `Klaar om te plannen`** — dat is Williams handtekening, en het
is het enige dat een post naar buiten kan brengen.

## Stap 4 — Inplannen

Neem alleen posts mee die aan álle drie voldoen:

1. Status is `Klaar om te plannen`;
2. Buffer post ID is leeg — staat er al een, dan is de post al ingepland en
   sla je hem over. Dit is de enige beveiliging tegen dubbel plaatsen;
3. Tekst is gevuld en past binnen de limiet van het kanaal.

Leg William de batch eerst voor: per post het kanaal, het moment (of "eerste
vrije slot in de wachtrij") en de tekst zelf, voluit. Pas na een expliciet
"ja" ga je verder. Twijfel je of iets goedkeuring is, dan is het dat niet.

Roep de flow aan met `execute_workflow` (`executionMode: "production"`,
`inputs.type: "webhook"`) met deze body:

```json
{ "posts": [ {
  "nodeId": "<nodeId van de #social post>",
  "kanaal": "LinkedIn",
  "tekst": "…",
  "media": ["https://…"],
  "publicatiemoment": "2026-09-01T09:00:00+02:00",
  "concept": false
} ] }
```

`publicatiemoment` laat je weg als het veld leeg is — dan kiest Buffer zelf
het eerstvolgende vrije slot uit Williams postschema, en dat is meestal
precies goed. Zet `concept: true` als William de post eerst in Buffer wil
bekijken in plaats van inplannen.

`execute_workflow` geeft alleen een execution-ID terug. Haal het resultaat op
met `get_execution` (`includeData: true`, node `Build Response`).

## Stap 5 — Schrijf de bon terug

Voor elke post in `results` — de `nodeId` komt ongewijzigd terug, dus je weet
bij welk knooppunt hij hoort:

- **Gelukt** (`ok: true`): zet Buffer post ID, zet Buffer status op wat Buffer
  teruggaf, zet Publicatiemoment op de `dueAt` die terugkwam (Buffer kiest bij
  een lege wachtrij-post zelf het moment — dát is het echte moment, niet wat
  jij dacht), en zet Status op `Ingepland`.
- **Mislukt** (`ok: false`): zet Buffer status op de foutmelding en **laat
  Status op `Klaar om te plannen` staan**, zodat de post vanzelf weer meegaat
  bij de volgende poging. Zet nooit een half resultaat weg als succes.

Faalt de hele batch (onbekend kanaal, kapotte datum, losgekoppeld kanaal),
dan is er niets ingepland en schrijf je niets terug. Geef de foutmelding van
de flow letterlijk door — die zegt precies wat er mis is.

## Stap 6 — Sluit af

Eén korte samenvatting: wat er is ingepland, op welk kanaal en wanneer, met
links naar de knooppunten. Ging er iets mis, zeg dat dan als eerste en niet
als voetnoot. Is er niets ingepland omdat er niets klaarstond, zeg dat ook
gewoon.

## Regels die niet buigen

- Niets gaat naar Buffer zonder Williams expliciete akkoord in dit gesprek.
  Een post die er al lang klaar voor staat, is geen akkoord.
- Status `Klaar om te plannen` zet William, nooit jij.
- Staat er een Buffer post ID, dan blijft de post staan waar hij staat.
- Een post die Buffer weigerde is mislukt, ook als de rest van de batch
  lukte. Rapporteer dat als zodanig.
- Verzin geen inhoud. Wat William niet heeft gezegd of opgeschreven, staat
  niet in zijn post.
