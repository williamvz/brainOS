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
| Tana's Google Calendar-koppeling | tussen 08:00 en 11:00, niet elke dag | maakt per afspraak een node onder Library → Google Calendar Events, voor **vandaag en morgen** |
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
De koppeling draait in de ochtend en pakt dan vandaag én morgen mee. De nodes
voor vandaag zijn dus gisterochtend gemaakt en staan er ruim op tijd; die
voor morgen komen pas ná de Dagstart. Daarom doet stap 9 beide dagen: wat
vandaag nog een platte regel in de index moest blijven, hangt morgenochtend
alsnog aan zijn node.

**Maar de koppeling slaat dagen over.** Tussen 14 en 16 september draaide zij
niet, en toen zij op de 16e om 10:26 alsnog liep, waren beide Dagstarts die
naar die dag keken (op de 15e en de 16e om 06:00) al geweest. De hele index
van 16 september bleef daardoor plat terwijl de nodes gewoon bestonden. Twee
dagen vooruitkijken is dus niet genoeg: stap 9 loopt daarom ook gisteren en
eergisteren na en vervangt platte regels alsnog door referenties.

**Elke afspraak krijgt een node, maar lang niet elke node een tag.** De
koppeling maakt voor alles wat in de agenda staat een node aan — ook voor
lunch, focusblokken en de schoolrit. De supertag komt daar los achteraan en
blijft vaak uit: op 17 september hadden alle zeven afspraken een node,
waarvan er drie getagd waren en vier niet. Er zit geen patroon in; het is
geen kwestie van wel of geen Teams-link.

Dat maakt de tag onbruikbaar als zoeksleutel. Zoek een agenda-node altijd op
datum (en dan op titel), nooit op tag, en lees de datum uit het datumveld in
plaats van uit de naam — alleen getagde nodes dragen hun datum in de naam.
Stap 9 beschrijft de werkwijze. Wie hier de tag als filter gebruikt, mist
stelselmatig de helft van de dag zonder dat er iets kapot lijkt.

**Op welke agenda een afspraak staat, zegt niets.** De iOS-automatisering
schrijft niet alles naar dezelfde agenda: ze verdeelt de dag over de
werkagenda en de privé-agenda. Die verdeling volgt geen eigenschap van de
afspraak — niet de organisator, niet de deelnemers, niet de locatie, en ook
niet of het een Teams-call of een fysieke afspraak is.

Ze is zelfs niet stabiel. Op 23 september 2026 liep dezelfde lijst van negen
afspraken twee keer, drie uur na elkaar; drie ervan kwamen de tweede keer op
de ándere agenda terecht. Per run schrijft de automatisering een blok naar de
ene agenda, klapt halverwege de lus één keer om, en laat de rest naar de
andere gaan — de ene keer na de vijfde afspraak, de andere keer na de zesde.
Het is dus een positiefout in de lus, geen keuze per afspraak.

Voor de Dagstart volgt daar tweeërlei uit. Lees altijd álle agenda's en voeg
ze samen: een werkafspraak kan net zo goed op de privé-agenda staan.
En ontdubbel over de agenda's heen in plaats van per agenda — draait de
automatisering twee keer op een dag, dan staat een afspraak dubbel, en die
twee kopieën staan lang niet altijd bij elkaar.

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
