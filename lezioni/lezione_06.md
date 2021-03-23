<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 5
## SQL AVANZATO

Autore: Ferdinando Urbano  

---
In questa lezioni si continua l'esplorazione del linguaggio SQL aggiungendo comandi più avanzati, in particolare per visualizzare contemporaneamente dati provenienti da più tabelle e raggruppare le righe per ottenere statistiche basate su determinati valori.


### GROUP BY

The `GROUP BY` clause divides the rows returned from the `SELECT` statement into groups. For each group, you can apply an aggregate function to the values of the columns that are nto used as criteria to group the rows. The main aggregate functions (at least for numbers) are `COUNT`, `SUM`, `MIN`, `MAX`, `AVG`, `STDDEV`. For example SUM calculates the sum of items and COUNT gets the number of items in the groups.

Se si vuole raggruppare le righe in base ad alcuni campi ma non si devono calcolare statistiche sui campi aggregati, invece del comando `GROUP BY` si può usare il comando `DISTINCT` visto nella lezione precedente.

The GROUP BY clause must appear right after the FROM or WHERE clause. Followed by the GROUP BY clause is one column or a list of comma-separated columns. You can also put an expression in the GROUP BY clause.

```sql
SELECT column_x, aggregate_function(column_y)
FROM table_x
GROUP BY column_x;
```

The following statement illustrates the syntax of the GROUP BY clause.
```sql
SELECT
  sex,
  count(animals_id) as number_animals
FROM
  main.animals
GROUP BY
  sex;
```

```sql
SELECT
  gps_sensors_id,
  max(roads_dist)
FROM
  main.gps_data_animals
GROUP BY
  gps_sensors_id;
```

##### EXERCISE

* Count how many records you have per animal with coordinates not null (table *main.gps_data_animals*) and the average altitude.

### HAVING

We often use the `HAVING` clause in conjunction with the `GROUP BY` clause to filter group rows that do not satisfy a specified condition. The `HAVING` clause sets the condition for group rows created by the `GROUP BY` clause after the GROUP BY clause applies while the `WHERE` clause sets the condition for individual rows before `GROUP BY` clause applies. This is the main difference between the `HAVING` and `WHERE` clauses, as illustrated in the examples.

```sql
SELECT
  animals_id,
  count(animals_id),
  avg(roads_dist)
FROM
  main.gps_data_animals
WHERE
  roads_dist < 900
GROUP BY
  animals_id
ORDER BY
  animals_id;
```

```sql
SELECT
  animals_id,
  count(animals_id),
  avg(roads_dist)::integer
FROM
  main.gps_data_animals
GROUP BY
  animals_id
HAVING
  avg(roads_dist) < 900
ORDER BY
  animals_id;
```


### UNION
Per ora abbiamo sempre interrogato una tabella alla volta. In questa e nelle prossime sezioni cominceremo a combinare più tabelle insieme.  
L'operatore `UNION` combina i set di risultati di due o più istruzioni `SELECT` in un unico set di risultati.

La seguente illustra la sintassi dell'operatore UNION che combina gli insiemi di risultati di due query.

Per combinare i set di risultati di due query usando l'operatore `UNION`, le query devono essere conformi alle seguenti regole:

    Il numero e l'ordine delle colonne nella lista di selezione di entrambe le query devono essere gli stessi.
    I tipi di dati devono essere compatibili.

L'operatore UNION rimuove tutte le righe duplicate dal set di dati combinato. Per mantenere le righe duplicate, si usa invece l'operatore `UNION ALL`.

PostgreSQL UNION con clausola ORDER BY

L'operatore UNION può mettere le righe del risultato della prima query prima, dopo o tra le righe del risultato della seconda query.

Per ordinare le righe nel set di risultati finale, si usa la clausola `ORDER BY` nella seconda query.

In pratica, si usa spesso l'operatore `UNION` per combinare dati da tabelle simili,

```sql
SELECT
FROM
UNION
SELECT
FROM
ORDER BY ;
```

#### ESERCIZIO
>

### JOIN di tabelle
Finora, le nostre query hanno avuto accesso solo a una tabella alla volta. Le query possono accedere a più tabelle contemporaneamente, coinvolgendo le informazioni di tutte le tabelle coinvolte. Una query che accede a più righe della stessa tabella o di tabelle diverse in una sola volta è chiamata join query. Ci sono diverse sintassi che possono essere usate. Quella più semplice è illustrata nel prossimo esempio. Se voglio visualizzare tutti i record della tabella main.gps_data_animals che appartengono ad animali maschi, devo includere tutte le informazioni nella tabella main.animals dove è memorizzato il sesso e poi devo selezionare le coppie di righe dove questi animals_id (che è presente in entrambe le tabelle e li "collega") corrispondono.

SELEZIONA
  animali.animals_id,
  animali.sesso,
  gps_data_animals.acquisition_time,
  gps_data_animals.longitude,
  gps_data_animali.latitudine
DA
  main.gps_data_animals,
  main.animals
DOVE
  animals.animals_id = gps_data_animals.animals_id;

Quando sono coinvolte più tabelle, è una buona pratica qualificare il nome di ogni colonna con il nome della tabella a cui appartiene. Questo è obbligatorio se lo stesso nome di colonna è usato in due tabelle diverse.

Il join illustrato nell'esempio può essere scritto anche in questa forma alternativa ed equivalente:

SELECT
  animali.animali_id,
  animali.sesso,
  gps_data_animals.acquisition_time,
  gps_data_animali.longitudine,
  gps_data_animali.latitudine
DA
  main.gps_data_animals
INNER JOIN
  main.animals
ON (animals.animals_id = gps_data_animals.animals_id);

ESERCIZIO

    Recuperare il nome di ogni animale con il timestamp della sua prima e ultima posizione

### LEFT JOIN
Nell'esempio precedente, solo i record della prima e della seconda tabella che corrispondono alle condizioni della join sono inclusi nei risultati. Usando la sintassi JOIN è possibile includere TUTTI i record della prima tabella e solo i record della seconda tabella che corrispondono alle condizioni di unione. Questo si ottiene con l'uso di LEFT JOIN. Questo tipo di join è utile in molte situazioni.

SELEZIONA
  lu_age_class.age_class_code,
  lu_age_class.age_class_description,
  animali.nome,
  animali.sesso
DA
  lu_tables.lu_age_class
LEFT JOIN
  main.animals
SU
  lu_age_class.age_class_code = animals.age_class_code;

In questo caso, vengono riportati anche i codici delle classi di età senza animali associati.
#### ESERCIZIO
> Contare quanti animali sono inclusi nella tabella main.animals per ogni specie elencata nella tabella lu_tables.lu_species (riportare '0' invece di NULL se non ci sono animali per quella specie).

### DATE, TIMESTAMP, EXTRACT, TIMEZONE
Per i dati di monitoraggio raccolti dai Parchi, il tempo e lo spazio sono spesso informazioni importanti. Qui introduciamo un nuovo tipo di dati specifico per le informazioni sulla data/ora temporale, con associato uno specifico formato e un set di operazioni possibili: **[timestamp](http://www.postgresql.org/docs/devel/static/datatype-datetime.html)**, cioè data, ora e la loro combinazione (con indicazione o meno del fuso orario).  
In particolare, in PostgreSQL si hanno i seguenti tipi di dati temporali:  
* Timestamp
* Timestamp with time zone
* Date
* Time
* Time with time zone
* Interval

Il timestamp unisce la data e l'ora del giorno, necessari per identificare un momento specifico nel tempo. Inoltre, lo stesso momento può essere espresso in modo diverso a seconda del paese del mondo in cui ci si trova. Infatti, per identificare senza ambiguità un momento nel tempo è necessario specificare il fuso orario. Questo ha un'analogia con il sistema di riferimento per gli oggetti spaziali. Il fuso orario può essere non specificato ma in questo caso può essere problematico unire i dati con altre fonti (ad esempio, sensori o dati satellitari) o con banche dati provenienti da altre parti del mondo, così come avviene per le coordinate senza specificato il sistema di riferimento.  
Quando si usa un tipo di dato timestamp with time zone *"il valore memorizzato internamente è sempre UTC (Universal Coordinated Time, tradizionalmente noto come Greenwich Mean Time, GMT). Un valore di input che ha un fuso orario diverso viene convertito in UTC usando l'offset appropriato per quel fuso orario. Se nessun fuso orario è indicato nella stringa di input, allora si presume che sia nel fuso orario indicato dal parametro TimeZone del sistema, e viene convertito in UTC usando l'offset per il fuso orario."*  
Inoltre *"PostgreSQL non memorizza il fuso orario di provenienza con il timestamp. Invece converte da e verso il fuso orario di ingresso e di uscita"*.

Quando si interrogano i dati si specifica il fuso orario in cui visualizzarli (fuso orario del server di default). Per vedere il fuso orario di default del server si può eseguire questa query:

```sql
SHOW TIME ZONE;
```

Qui alcuni esempi del diverso comportamento del timestamp con o senza fuso orario (`now()` è la funzione che restituisce il timestamp corrente):

```sql
SELECT
 now() ora_qui,
 now() AT TIME ZONE 'America/Los_Angeles' ora_in_la,
 '2018-06-17 10:00:00'::timestamp no_tz,
 '2018-06-17 10:00:00'::timestamp with time zone tz_no_specificato,
 '2018-06-17 10:00:00+02'::timestamp with AS tz_specificato,
 now()::date AS date,
 now()::time AS time;
```

PostgreSQL ha anche un altro tipo di dato temporale: `interval`. Un intervallo descrive una durata, ad esempio un mese o due settimane, o anche un millisecondo, ed è il tipo di dato generato quando si fa la differenza fra due timestamp (o date):

```sql
SELECT
 interval '1 month' esempio1,
 interval '2 weeks' esempio2,
 2 * interval '1 week' esempio3,
 78389 * interval '1 ms' esempio4,
 now()-'2018-05-17 10:00:00+00'::timestamp with time zone esempio_diff,
 '2021-03-28'::date - '2020-03-28'::date date_diff;
```

Si possono usare gli intervalli per aggiungere o sottrarre tempo a un timestamp:

```sql
SELECT
 now(),
 now() + interval '1 hour' example_operation;
```

È possibile estrarre parte di un timestamp (ad esempio mese, anno, minuto). In particolare, `epoch` è molto utile in quanto trasforma una data in secondi da un momento specifico nel tempo. Così facendo, è possibile trattare i riferimenti temporali come numeri interi utilizzando tutte le funzioni tipiche degl interi, ed eventualmente riconvertendo poi il risultato a un tipo di dato temporale.

```sql
SELECT
 now(),
 extract (year from now()) as year,
 extract (month from now()) as month,
 extract (doy from now()) as doy,
 extract (epoch from now()) as epoch,
 extract (epoch from now()) - extract (epoch from (now() - interval '1 minute')) difference_seconds;
```

Molti altri strumenti per gestire il tempo sono descritti nella **[documentazione di PostgreSQL](https://www.postgresql.org/docs/devel/static/functions-datetime.html)**.  

In alcuni casi, nei dati raccolti sul campo per alcuni record la data non è disponibile, ma c'è solo l'anno. In questo caso, nel database si può avere un campo data (per quando questa è presente) e un campo anno (come numero intero) nel caso questa si l'unica informazione disponibile.  

#### ESERCIZIO
> Quanti sono i record della tabella *biodiversita.lepidotteri_monitoraggio* che sono stati raccolti nel 2014 (si può estrarre l'anno dalla data, chiedere che questa sia uguale al valore prescelto e poi vedere quante righe vengono restituite)?

### COALESCE

La funzione `COALESCE` restituisce il primo dei suoi argomenti che non è nullo. `NULL` viene restituito solo se tutti gli argomenti sono nulli. In pratica, viene utilizzato per sostituire il valore NULL con un valore predefinito quando i dati vengono richiesti per la visualizzazione.  
In questo esempio vengono estratti i nomi delle specie e il numero di individui totali e di maschi dalla tabella *biodiversita.lepidotteri_monitoraggio*. In un campo aggiuntivo, rinominato con un alias, i record che hanno valore nullo nel campo *numero_maschi* vengono rimpiazzati con il valore 0.  

```sql
SELECT  
  animale_code,
  numero_totale,
  numero_maschi,
  coalesce(numero_maschi,0) AS numero_maschi_0null
	FROM biodiversita.lepidotteri_monitoraggio;
```
#### ESERCIZIO
> Visualizzare i dati della tabella *biodiversita.lepidotteri_controllo* sostituendo il valore NULL nel campo *pascolo_impatto_code* con la stringe 'Impatto non noto'.

### CASE

Il comando SQL `CASE` è un'espressione condizionale generica, simile alle istruzioni *if/else* (se/allora) in altri linguaggi di programmazione. All'interno di questa espressione è possibile definire una condizione (`WHEN`). Se questa condizione si realizza, allora viene restituito un valore specificato nell'espressione (`WHEN`), altrimenti viene valutata l'espressione successiva fino alla fine dell'espressione (`END`).  
La sintassi completa è `CASE WHEN criteria THEN value END`, dove possono esserci più `WHEN criteria THEN value`.  
Qui sotto è riportato un esempio di codice che in base al valore del campo *dati_qualita_code* della tabella *biodiversita.lepidotteri_monitoraggio* restituisce una stringa di testo che descrive il livello di qualità del dato.

```sql
SELECT  
  animale_code,
  note,
  dati_qualita_code,
CASE
  WHEN dati_qualita_code = 1 THEN 'Dato confermato'
  WHEN dati_qualita_code in(3,4) THEN 'Dato con margini di dubbio'
  WHEN dati_qualita_code = 201 THEN 'Dato non confermato'
END as descrizione_qualita
FROM
  biodiversita.lepidotteri_monitoraggio
ORDER BY
  dati_qualita_code desc;
```  

#### ESERCIZIO
> Visualizzare i campi *animale_code* e *numero_totale* della tabella *biodiversita.lepidotteri_controllo* aggiungendo un campo testo 'individui > 10' quando il campo *numero_totale* è < 10 e 'individui >= 10' quando il campo *numero_totale* è >= 10.

### Creare una tabella

codice sql
da una query + chiave primaria

### Creare una VIEW
Creare una *VIEW* è una operazione molto semplice: è sufficiente scrivere la query SQL che genera la tabella desiderata a farla precedere da comanda che specifica quale deve essere il nome (e lo schema) della tabella: `CREATE VIEW nome_schema.nome_vista AS`.  
In questo esempio viene creata la view XXX nello schema YYY in base al comando di select specificato nel codice:


[**Lezione 7.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_07.md) Dati spaziali - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_07.html)</ins>]
