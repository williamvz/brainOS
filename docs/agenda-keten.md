# Hoe een afspraak in Tana belandt

De agenda die de Dagstart leest is niet de bron. Er zitten twee koppelingen
tussen Outlook en Tana, elk met hun eigen ritme, en allebei lopen ze maar één
of twee keer per dag. Dat verklaart het meeste van wat er mis kan gaan.

Deze notitie beschrijft wat er is waargenomen — niet wat ergens
gespecificeerd staat. De tijden komen uit de `created`-stempels van
september 2026.

## De keten

| Wie | Wanneer | Wat |
| --- | --- | --- |
| iOS-automatisering op Williams telefoon | 05:00, dagelijks | kopieert de afspraken van **morgen** uit Outlook naar Google Calendar |
| Tana's Google Calendar-koppeling | rond 09:00 | maakt per afspraak een node onder Library → Google Calendar Events, voor **vandaag en morgen** |
| Dagstart | 06:00 | leest Google Calendar, verrijkt die nodes en bouwt de Meetings-index, voor **vandaag en morgen** |

## Wat daaruit volgt

**De agenda in Google is hooguit één dag vers.** De iOS-automatisering draait
één keer, om 05:00, en kijkt dan naar morgen. Wordt er daarna in Outlook iets
verzet, afgezegd of toegevoegd, dan ziet Google dat pas bij de volgende run.
De Dagstart kan dus een afspraak tonen die inmiddels is afgezegd, of een
nieuwe missen. Dat is een bekende beperking, geen bug in de skill: klopt de
agenda niet met wat William zegt dat er staat, dan is dit vrijwel altijd de
oorzaak. Een frequentere sync (bijvoorbeeld elk uur) zou dit oplossen; dat
staat geparkeerd als toekomstig klusje.

**Om 06:00 bestaan de nodes van vandaag al, die van morgen meestal niet.**
De koppeling draait rond 09:00 en pakt dan vandaag én morgen mee. De nodes
voor vandaag zijn dus gisterochtend gemaakt en staan er ruim op tijd; die
voor morgen komen pas ná de Dagstart. Daarom doet stap 9 beide dagen: wat
vandaag nog een platte regel in de index moest blijven, hangt morgenochtend
alsnog aan zijn node.

**Fysieke afspraken krijgen geen node.** Alles wat een Teams-link heeft
verschijnt als node; afspraken zonder online-link lijken te worden
overgeslagen — waargenomen bij een zitting in Rotterdam en een dag op
kantoor in Amsterdam, allebei met deelnemers en allebei zonder node. De
terugvalregel in stap 9 (platte regel in de index, geen eigen node maken) is
dus geen randgeval maar dagelijkse kost.

**Twee schrijvers maken twee blokken.** Zowel de koppeling als de Dagstart
kan een node met een datumveld maken, en alles met een datumveld verschijnt
in de agendaweergave. Daarom maakt de Dagstart er zelf geen meer: de node van
de koppeling is leidend en wordt alleen verrijkt. Zie stap 9.

## De tijdzoneval

Een datumveld zonder tijdzone slaat Tana op als UTC. Schrijf je de
Amsterdamse kloktijd rauw weg (`2026-09-14 11:00`), dan staat de afspraak
twee uur te laat in de agendaweergave — in de winter één uur.

Het venijn zit in de controle: lees je het veld terug, dan geeft de MCP de
opgeslagen wandklok terug zónder tijdzone — `11:00`, precies wat je bedoelde
— terwijl de weergave 13:00 toont. Teruglezen bevestigt de fout dus in plaats
van hem te betrappen. Zo heeft dit van 31 augustus tot 14 september 2026
onopgemerkt kunnen doorlopen.

Moet er ooit tóch een datum geschreven worden, doe het dan als expliciet
UTC-bereik met `Z`:

```bash
date -u -d "@$(TZ=Europe/Amsterdam date -d '2026-09-14 11:00' +%s)" '+%Y-%m-%dT%H:%MZ'
# 2026-09-14T09:00Z
```

Let op de omweg via `+%s`: `TZ=Europe/Amsterdam date -u -d '...'` lijkt
hetzelfde te doen maar zet ook het *parsen* op UTC, en geeft de tijd dus
onveranderd terug.

Controleer een tijd nooit door het datumveld terug te lezen. Alle tijden in
de briefing en de index komen uit Google Calendar, nooit uit het
Tana-datumveld.
