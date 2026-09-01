# Energiebronnen

Naslag bij de skill [`monthly-energy`](../.claude/skills/monthly-energy/SKILL.md).
Beschrijft per bron wat hij oplevert, of er een API is, en waar het misgaat.

> **Over identifiers en geheimen in dit bestand:** deze repo is publiek. Hier
> staan alleen namen van bronnen, endpoints uit openbare documentatie en de
> vorm van de gegevens — nooit sleutels, tokens, wachtwoorden, klantnummers,
> meterstanden of bedragen.

## De vier bronnen in één oogopslag

| Bron | Levert | API? | Hoe je erbij komt |
| --- | --- | --- | --- |
| Solarman | productie (kWh) | ja, Open API | App ID + App Secret, per mail toegekend |
| Tibber | afname (kWh), teruglevering (kWh), kosten (€) | ja, GraphQL | persoonlijk toegangstoken |
| 50Five (EVC-net) | gedeclareerde kWh, vergoeding (€) | nee | portaal, login vereist |
| Joulo | ERE-opbrengst (€) | nee | portaal, login vereist |

## Solarman

De Solarman Open API (`https://globalapi.solarmanpv.com`, documentatie op
`https://doc.solarmanpv.com/`) is aangevraagd en toegekend: de klantenservice
heeft App ID en App Secret per e-mail gestuurd. Zoek die mail in Gmail op de
Solarman-afzender plus "API" — en laat de sleutels daar staan.

De volgorde is token ophalen (`/account/v1.0/token`), installatie opzoeken
(`/station/v1.0/list`), en dan de maandopbrengst uit de history-endpoint van de
station-API. Veld- en parameternamen verschillen per API-versie; controleer ze
in de documentatie in plaats van ze uit je hoofd te typen. Een token is een
paar maanden geldig en vervalt bij een wachtwoordwijziging.

Het portaal (`globalhome.solarmanpv.com`) toont hetzelfde getal in de
maandweergave van de plant-gegevens, en is de terugvaloptie.

**Derde route: Home Assistant.** William heeft Home Assistant gekoppeld, maar
deelt op dit moment alleen klimaat-entiteiten met de assistent. Deelt hij de
omvormer- of zonne-sensor ook, dan is de maandproductie op te vragen met
`GetLiveContext` en is Solarman voor deze skill helemaal niet meer nodig.

## Tibber

De GraphQL-API zit op `https://api.tibber.com/v1-beta/gql` en werkt met een
persoonlijk toegangstoken (`Authorization: Bearer <token>`) dat je aanmaakt op
`https://developer.tibber.com/settings/access-token`. In de explorer op
`developer.tibber.com/explorer` test je een query voordat je hem in de skill
zet.

Wat de skill nodig heeft zit onder `viewer.homes`, met `resolution: MONTHLY`:
`consumption` geeft `from`, `to`, `consumption` (kWh) en `cost` (€),
`production` geeft `production` (kWh) en `profit` (€).

**De factuurmail is geen bron van bedragen.** Tibber mailt rond de zevende van
de volgende maand een factuur ("Factuur voor 1 <maand> — 31 <maand>") met het
bedrag in een PDF-bijlage. De Gmail-connector levert de mail en de bijlagenaam,
maar niet de inhoud van die bijlage. Gebruik de mail dus alleen als signaal dat
de maand is afgesloten; het bedrag komt uit de API.

**Leverancierswissel.** William is in de loop van het jaar op Tibber
overgestapt. Maanden van vóór die overstap komen niet uit deze API; in de tabel
staan ze als aparte rijen met de leverancier tussen haakjes.

## 50Five (EVC-net)

Geen publieke API. De cijfers staan in het EVC-net-portaal, in het
transactieoverzicht per eigen laadpunt per maand — één rapport geeft de
gedeclareerde kWh en het bedrag per maand naast elkaar. De terugkerende taak in
Outliner draagt de link met de juiste filters; alleen de begin- en einddatum
hoef je aan te passen.

Twee dingen om op te letten:

- **De laatste maand kan nog groeien.** 50Five verwerkt sessies met
  vertraging, dus een maand die net voorbij is kan later oplopen.
- **Bedrag en kWh horen in verhouding te staan.** Die verhouding is over de
  maanden heen opvallend stabiel. Wijkt hij sterk af, dan klopt er iets niet
  aan het uitgelezen bereik — meld dat in plaats van het getal over te nemen.

50Five stuurt geen maandoverzicht per e-mail; er is dus geen mailroute als
terugval.

## Joulo

Geen publieke API. De ERE-opbrengst staat in het Joulo-dashboard onder de
uitbetalingen; ook die link hangt aan de terugkerende taak. Joulo mailt vanaf
`noreply@joulo.nl`; als er een uitbetalingsmail voor de maand is, is dat de
snelste route.

Joulo betaalt niet elke maand uit. Een maand zonder uitbetaling is een lege
cel, geen nul.

## Wat er in de weg kan zitten

- **Netwerk.** Claude Code op het web draait achter een egress-proxy die deze
  vier hosts weigert (403 op CONNECT). Draai de skill dus in Cowork op Williams
  eigen machine. Kom je er niet bij, meld dat per bron — vul nooit een getal in
  dat je niet hebt opgehaald.
- **Login.** De twee portalen vragen een sessie met Williams inloggegevens. Zonder
  browser-tool met zijn profiel is de enige route: William de twee getallen
  vragen. Vraag dat in één keer, samen met wat je verder mist.
- **Timing.** Draai de skill na de zevende van de maand: dan is de
  Tibber-factuur binnen, staan de 50Five-transacties verwerkt en is de vorige
  maand overal definitief.
