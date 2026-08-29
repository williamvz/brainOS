---
name: weekly-roundup
description: William's wekelijkse terugblik en vooruitblik — wat er de afgelopen week gebeurd is (agenda, mail, afgeronde/openstaande taken uit Notion en Tana) plus een korte blik op de komende week. Platte-tekst samenvatting in de chat, geen artifact. Alleen op expliciet verzoek (bv. "wekelijkse review", "hoe was mijn week", of /weekly-roundup) — niet automatisch.
---

# Weekly Roundup

Een handmatige weekreview voor William. Draait alleen op expliciet verzoek —
geen automatische trigger.

## Periode bepalen

Bepaal de afgelopen week (maandag t/m zondag) op basis van `currentDate` uit de
sessiecontext, tenzij William een andere week bedoelt. Bepaal ook de komende
week voor de vooruitblik.

## Wat op te halen

1. **Agenda — Google Calendar**
   Overzicht van de afgelopen week: aantal meetings, opvallende/grote
   afspraken. Plus de belangrijkste afspraken van de komende week.

2. **Mail — Gmail**
   Belangrijke/ster-gemarkeerde mail en threads uit de afgelopen 7 dagen die
   nog een reactie nodig hebben. Negeer ruis (nieuwsbrieven, automatische
   notificaties).

3. **Taken — Notion**
   Wat is afgerond deze week (pagina's/taken met status "Done" of vergelijkbaar,
   updated binnen de week) versus wat nog open staat of is doorgeschoven.

4. **Taken — Tana (Outliner)**
   Loop de dag-nodes van de afgelopen week na (via de calendar-nodes) en maak
   onderscheid tussen afgevinkte en nog openstaande taken/notities.

Als een bron leeg of onbereikbaar is: sectie weglaten of in één regel noteren,
nooit invullen met aannames.

## Output

Platte tekst in de chat, geen artifact. Nederlands, ongeveer deze structuur
(laat lege secties weg):

```
🗓️ Week in vogelvlucht (DD/MM–DD/MM)
Kort, 2–3 zinnen: hoe zag de week eruit qua drukte/thema's.

✅ Afgerond
- ...

⏳ Nog open / doorgeschoven
- ...

📬 Mail die nog aandacht vraagt
- ...

🔭 Vooruitblik komende week
- Belangrijkste afspraken/deadlines.
```

Wees eerlijk over gaten in de data (bv. "geen Notion-taken met een datum deze
week gevonden") in plaats van iets te verzinnen om de sectie te vullen.
