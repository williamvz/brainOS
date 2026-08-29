---
name: day-start
description: William's ochtendbriefing — vandaag's agenda uit Google Calendar, mail die aandacht vraagt uit Gmail, en openstaande taken uit Notion en Tana (Outliner). Levert een platte-tekst samenvatting in de chat, geen artifact. Gebruik dit alleen als William er expliciet om vraagt (bv. "start mijn dag", "wat staat er vandaag", "goedemorgen", of /day-start) — niet automatisch bij elke vraag over zijn agenda.
---

# Day Start

Een handmatige ochtendbriefing voor William. Draait alleen op expliciet verzoek —
er is geen automatische trigger voor deze skill.

## Wat op te halen

Gebruik de datum uit de sessiecontext (`currentDate`) als "vandaag", tenzij William
een andere dag noemt. Ga uit van tijdzone Europe/Amsterdam tenzij anders blijkt.

1. **Agenda — Google Calendar**
   Haal vandaag's events op (primaire kalender, en eventuele andere relevante
   kalenders als die bestaan). Sorteer chronologisch. Noteer tijd, titel, en
   voor meetings met meerdere deelnemers kort wie erbij zit als dat nuttig is.

2. **Mail — Gmail**
   Zoek ongelezen en belangrijke mail van de laatste ~24 uur
   (bv. `is:unread newer_than:1d` en `is:important is:unread`). Selecteer alleen
   threads die om actie of aandacht vragen — negeer nieuwsbrieven, notificaties
   en cc-only threads. Vat elke relevante thread in één regel samen: afzender,
   onderwerp, wat er gevraagd wordt.

3. **Taken — Notion**
   Zoek naar taken/pagina's die relevant zijn voor vandaag: due today, of open
   items in een taken-/projectendatabase als William die heeft. Gebruik
   `notion-search` om eerst te ontdekken welke databases er zijn als dat nog
   niet bekend is.

4. **Taken — Tana (Outliner)**
   Gebruik `get_or_create_calendar_node` voor vandaag en lees de children
   (`get_children` / `read_node`) voor taken en losse notities die daar
   onder hangen.

Als een bron niet beschikbaar is (connector niet verbonden, geen resultaten),
sla die sectie stilzwijgend over of noteer in één regel dat de bron leeg/niet
bereikbaar was — verzin nooit content.

## Output

Platte tekst in de chat, geen artifact. Kort en scanbaar, Nederlands, ongeveer
deze structuur (laat lege secties weg):

```
📅 Agenda vandaag
- 09:00 — ...
- 14:00 — ...

📧 Mail die aandacht vraagt
- Van X: ... → actie: ...

✅ Taken (Notion + Tana)
- ...

🎯 Focus voor vandaag
1–3 prioriteiten, alleen als er uit agenda/mail/taken een duidelijke top-keuze
naar voren komt. Verzin geen prioriteiten die niet uit de data volgen.
```
