---
name: quarterly-review
description: William's kwartaalreview — voortgang op doelen/OKR's, belangrijkste resultaten en openstaande zaken van het kwartaal, gebaseerd op Google Calendar, Gmail, Notion en Tana (Outliner). Platte-tekst samenvatting in de chat, geen artifact. Alleen op expliciet verzoek (bv. "kwartaalreview", "hoe ging dit kwartaal", of /quarterly-review) — niet automatisch.
---

# Quarterly Review

Een handmatige kwartaalreview voor William. Draait alleen op expliciet verzoek —
geen automatische trigger.

## Periode bepalen

Bepaal het lopende (of het meest recent afgelopen, als William dat bedoelt)
kalenderkwartaal op basis van `currentDate` uit de sessiecontext:
Q1 jan–mrt, Q2 apr–jun, Q3 jul–sep, Q4 okt–dec.

## Wat op te halen

Een kwartaal is te lang om event-voor-event door te lopen — focus op patronen
en de grote lijn, niet op een volledige agenda-opsomming.

1. **Doelen/OKR's — Notion & Tana**
   Zoek naar een doelen-, OKR- of projectendatabase/-tag in Notion en Tana
   (bv. #goal, #okr, of een "Doelen"-database). Rapporteer per doel kort de
   status: op koers, vertraagd, afgerond, of gestopt. Als William geen
   expliciete doelenstructuur bijhoudt, zeg dat eerlijk en sla deze sectie
   over in plaats van iets te verzinnen.

2. **Agenda — Google Calendar**
   Globaal beeld van het kwartaal: drukte-patroon, terugkerende
   samenwerkingen/projecten die uit de meeting-titels naar voren komen.

3. **Mail — Gmail**
   Grote/lopende threads of onderwerpen die het kwartaal domineerden.

4. **Notion & Tana — resultaten**
   Belangrijke pagina's/projecten die zijn afgerond of aanzienlijk zijn
   opgeschoven binnen het kwartaal, en wat nog open of vertraagd is.

## Output

Platte tekst in de chat, geen artifact. Nederlands, ongeveer deze structuur
(laat lege secties weg):

```
📊 Q[n] [jaar] in vogelvlucht
2–4 zinnen: belangrijkste thema's van het kwartaal.

🎯 Voortgang op doelen
- Doel X: op koers / vertraagd / afgerond — korte toelichting.

✅ Belangrijkste resultaten
- ...

⚠️ Openstaand / vertraagd
- ...

🔭 Vooruitblik volgend kwartaal
- Wat er waarschijnlijk op de agenda komt, op basis van wat nog openstaat.
```

Als er geen expliciete doelenstructuur is gevonden, meld dat kort en stel
optioneel voor om er een op te zetten in Notion of Tana — vul de sectie niet
met giswerk.
