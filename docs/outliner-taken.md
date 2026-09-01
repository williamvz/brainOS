# Taken in Outliner (Tana)

Naslag bij de skills die taken lezen of afronden — `day-start` (stap 3),
`weekly-roundup` (stap 3) en `monthly-energy` (stap 6). Beschrijft hoe `#task`
en `#recurring task` zich tot elkaar verhouden, hoe een terugkerende taak wordt
afgerond — met de hand én vanuit een skill — en wat dat betekent voor elke
zoekopdracht die taken ophaalt.

> **Over identifiers in dit bestand:** deze repo is publiek. Hieronder staan
> alleen namen van tags, velden en commando's — nooit tag-, veld- of
> node-ID's. Zoek ze live op via `list_tags` en `get_tag_schema`.

## `#task` en `#recurring task`

`#recurring task` **extend `#task`**. Het erft dus elk veld — bovenliggende
area/project, deadline, prioriteit, gedelegeerd aan, someday, taakstatus — en
voegt er precies één aan toe: **`Occurrence`**, de herhaalafstand als
relatieve datumstring.

Twee gevolgen die er in de praktijk toe doen:

- Een zoekopdracht op `#task` levert **ook** terugkerende taken op. Wil je ze
  gescheiden houden, filter dan expliciet op de recurring-tag.
- Het echte verschil zit niet in de velden maar in het **commando**: alleen
  `#recurring task` heeft *Complete — Complete and reschedule*.

## Een terugkerende taak afronden

Vink het selectievakje **niet** aan — dan is de taak definitief dicht en komt
er niets voor terug. Draai in plaats daarvan het commando **Complete**
(*Complete and reschedule*) op de node, via de commandoregel.

Dat commando doet vier dingen op een rij:

1. zet de done-status op *Done*;
2. **dupliceert** de node;
3. zet de done-status van het origineel terug op *Not done*;
4. schuift `Deadline Date` vooruit — met de huidige deadline als referentie en
   `Occurrence` als relatieve datumstring.

Netto blijft het **originele knooppunt leven**: onafgevinkt, met de volgende
deadline. Wat achterblijft is een **afgevinkte kopie met de oude deadline**,
als sibling op dezelfde plek — dat is de historie van het ritme.

Het commando pakt alleen aan als aan alle vier de voorwaarden is voldaan: de
node draagt de recurring-tag, `Deadline Date` is gevuld, `Occurrence` is
gevuld, en de taak is nog niet afgevinkt. Mist er één, dan gebeurt er niets.

`Occurrence` moet een relatieve datumstring zijn die Tana kan lezen:
`In seven days`, `In four weeks`, `In one month`, `In one year`. Een
frequentie als `Every week` is géén relatieve datum — daar loopt stap 4 op
stuk en valt de reeks stil.

## Complete and reschedule nabootsen

*Complete — Complete and reschedule* is een Tana-commando en zit niet in de
MCP-koppeling. Een skill die een terugkerende taak wil afronden kan het dus
niet aanroepen — en `check_node` op het origineel is precies het verkeerde:
dat sluit de reeks definitief.

De vier stappen zijn wel na te bootsen, want ze zijn deterministisch. Netto
moet er hetzelfde staan als na het commando: een levend origineel met de
volgende deadline, en een afgevinkte kopie met de oude.

1. **Kopieer de taak** als sibling op dezelfde plek: dezelfde naam, dezelfde
   recurring-tag, en dezelfde veldwaarden — inclusief de **oude**
   `Deadline Date`. `import_tana_paste` koppelt velden op naam, dus je hebt
   geen veld-ID's nodig.
2. **Vink de kopie af** met `check_node`. Dat is de historie.
3. **Schuif de deadline van het origineel vooruit**: reken `Occurrence` uit
   met de oude `Deadline Date` als referentie (`In one month` op 31 augustus
   wordt 30 september) en zet die met `set_field_content` op het origineel.
4. **Laat het origineel onafgevinkt** en laat het taakstatusveld met rust —
   het commando raakt dat ook niet aan.

Lees daarna beide nodes terug en controleer dat het klopt: één afgevinkte
kopie met de oude deadline, één levend origineel met de nieuwe.

Twee dingen die je niet moet doen als het misgaat: een half aangemaakte kopie
laten staan (ruim hem op), en het origineel alsnog afvinken "om het af te
maken". Lukt de nabootsing niet, laat de taak dan gewoon open staan en zeg dat
het commando er met de hand overheen moet. Een taak die nog open staat is een
kleiner probleem dan een reeks die stilvalt.

## Wat dit betekent voor zoekopdrachten

- **Afgevinkte duplicaten zijn historie, geen dubbele taken.** Een
  terugkerende taak die twintig keer is afgerond, staat twintig keer in de
  graph. Alleen de onafgevinkte is de levende taak. Meld ze nooit als "dubbel
  in het systeem" — het is juist het bewijs dat het ritme draait.
- **Tel per naam één keer.** Ontdubbel op naam voordat je terugkerende taken
  opsomt.
- **Het vinkje is de done-status, het taakstatusveld niet.** Het commando
  raakt dat veld niet aan; op levende terugkerende taken staat het gewoon op
  `Not Started`. Filter op de done-status, nooit op dat veld.

## De triage-zoekopdracht "Inbox"

Op de Task Management-pagina staat een zoekopdracht **Inbox** die geen open
taken zoekt maar **onaffe** taken: alles wat is vastgelegd maar nog niet zo
is ingevuld dat het gepland kan worden.

```
( ( (Tagged:task) AND (NOT is:Done)
    AND ( (Parent area or Project==Not set)
          OR ( (Deadline Date==Not set) AND (Someday==No) )
          OR (Priority==Not set) ) )
  OR ( (Tagged:recurring task) AND (NOT is:Done)
       AND ( (Parent area or Project==Not set)
             OR (Deadline Date==Not set)
             OR (Occurrence==Not set)
             OR (Priority==Not set) ) )
  OR ( (Tagged:project) AND (Parent Area==Not set) AND (Area Status==Active) ) )
```

(De query zoals de builder hem genereert, alleen opnieuw ingesprongen om hem
leesbaar te houden.)

Per tak:

| Tak | Vangt |
| --- | --- |
| `#task` | open taken zonder area/project, zonder prioriteit, of zonder deadline terwijl ze niet als *someday* geparkeerd zijn |
| `#recurring task` | hetzelfde, plus terugkerende taken zonder `Occurrence` — die kunnen niet doorrollen |
| `#project` | actieve projecten die niet onder een area hangen |

De someday-uitzondering in de eerste tak is het scharnier: een taak zonder
deadline is pas een probleem als je hem niet bewust hebt geparkeerd. In de
tweede tak vervalt die uitzondering met opzet — een terugkerende taak hóórt
een deadline te hebben, anders staat het ritme stil.

> **De derde tak vangt op dit moment niets.** `Area Status` is een veld van
> `#area`, niet van `#project`, en geen enkele projectnode draagt het; met
> `Parent Area == Not set` in dezelfde clausule valt er ook via de area niets
> op te zoeken. Wil je die tak laten werken, laat `Area Status == Active`
> dan vallen en houd `(Tagged:project) AND (Parent Area == Not set)` over.
