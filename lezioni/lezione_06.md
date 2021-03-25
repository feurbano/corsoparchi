<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 5
## SQL AVANZATO

Autore: Ferdinando Urbano  

---
In questa lezioni si continua l'esplorazione del linguaggio SQL aggiungendo comandi più avanzati, in particolare per visualizzare contemporaneamente dati provenienti da più tabelle e raggruppare le righe per ottenere statistiche basate su determinati valori.


### GROUP BY
In alcuni casi può essere utile raggruppare le righe di una tabella secondo alcuni criteri. Ad esempio, per vedere quante individui di ogni specie sono stati trovati in un monitoraggio, o quanti individui sono stati trovati durante un controllo per ogni plot. Questo è possibile con SQL in modo semplice tramite il comando `GROUP BY` che divide le righe restituite dall'istruzione `SELECT` in gruppi con lo stesso valore di una o più tabelle (ad esempio il plot o la specie). Per ogni gruppo, si può poi applicare una funzione di aggregazione ai valori delle colonne che non sono usate come criteri per raggruppare le righe. Le principali funzioni di aggregazione (per i numeri) sono `COUNT` (conta quante righe sono state raggruppate), `SUM` (fai la somma di tutti i valori di quel campo, ad esempio il numero di individui), `MIN` (il valore minimo di quel campo per tutto il gruppo), `MAX` (il valore massimo di quel campo per tutto il gruppo), `AVG` (il valore medio di quel campo per tutto il gruppo, ad esempio il numero medio di individui trovati in ogni controllo dei plot), `STDDEV` (deviazione standard).  
La clausola `GROUP BY` deve apparire subito dopo la clausola `FROM` o `WHERE`. La clausola `GROUP BY` è seguita da una colonna o da una lista di colonne separate da virgola.  
La struttura generale del comando è:  
```sql
SELECT colonna_x, funzione_aggregazione(colonna_y)
FROM tabella_x
GROUP BY colonna_x;
```  

Le colonne specificate dopo `SELECT` devono essere elencate nella lista definita dopo `GROUP BY` o associate a una funzione di aggregazione, altrimenti la query non verrà eseguita e verrà restituito un errore.  
Se si vuole raggruppare le righe in base ad alcuni campi ma non si devono calcolare statistiche sui campi aggregati, invece del comando `GROUP BY` si può usare il comando `DISTINCT` visto nella lezione precedente.  

Il modo più semplice per capire cosa fa e come funziona `GROUP BY` è guardare un esempio. Supponiamo di voler sapere per ogni specie il numero totale di lepidotteri trovati in tutti i monitoraggi di tutti i parchi. Per fare questo devo specificare che voglio "raggruppare" le mie righe per *animale_code* (una riga per ogni animale) e poi che voglio calcolare la somma del numero di individui trovati per quella specie. Per facilitare la lettura dei dati voglio anche chiedere che il risultato sia restituito mettendo in ordine alfabetico i nomi degli animali. Questa richiesta si può tradurre inizialmente come "Raggruppa tutte le righe della tabella *biodiversita.lepidotteri_monitoraggio* e calcola il numero totali di individui". La traduzione in SQL di questa richiesta è:  

```sql
SELECT
  animale_code, SUM(numero_totale)
FROM
  biodiversita.lepidotteri_monitoraggio
GROUP BY
  animale_code
ORDER BY
  animale_code;
```

Se invece voglio raggruppare per specie e per parco, devo inserire entrambi i criteri dopo `GROUP BY`. In questo caso invece che ordinare per specie, voglio ordinare per numero complessivo di utenti. Voglio anche sapere quanti record facevano parte di ogni gruppo (quindi in quanti transetti è stata trovata ogni specie):

```sql
SELECT
  parco_code, animale_code, SUM(numero_totale), COUNT(numero_totale)
FROM
  biodiversita.lepidotteri_monitoraggio
GROUP BY
  parco_code, animale_code
ORDER BY
  SUM(numero_totale) DESC;
```

#### ESECIZIO
> Verificare quanti individui totali sono stati contati in ogni parco nel monitoraggio degli ortotteri (*biodiversita.lepidotteri_monitoraggio*).

> Contare quanti plot ci sono in ogni parco.  

### HAVING
Il comando `HAVING` può essere usato in associazione con `GROUP BY` per filtrare il risultato del raggruppamento escludendo i gruppi che non soddisfano una specifica condizione di aggregazione. La clausola `HAVING` applica la condizione alle righe del gruppo dopo l'applicazione del comando `GROUP BY`, mentre il comando `WHERE` applica la condizione per le righe individuali prima dell'applicazione della clausola `GROUP BY`. Con riferimento agli esempi precedenti, se volessi escludere tutte le specie che hanno meno di 100 individui complessivi (risultato della somma di tutti i record di quella specie) dovrei aggiungere `HAVING SUM(numero_totale) > 100`, mentre se volessi escludere tutti i record raccolti prima del 2015 dovrei aggiungere `WHERE data_controllo > '2015-01-01'` (la prima condizione elimina dai risultati i gruppi che non soddisfano la condizione mentre la seconda condizione limita i record che sono raggruppati). La condizione `HAVING` si mette dopo `GROUP BY` (quindi dopo `WHERE`, infatti una condizione che viene logicamente applicata dopo).


```sql
SELECT
  animale_code, SUM(numero_totale)
FROM
  biodiversita.lepidotteri_monitoraggio
WHERE
  data_controllo > '2015-01-01'
GROUP BY
  animale_code
HAVING
  SUM(numero_totale) > 100
ORDER BY
  SUM(numero_totale) DESC;
```

#### ESECIZIO
> Estrarre dalla tabella *biodiversita.view_lepidotteri_specie_presenza* il numero totale di individui (*esemplari_totali*) per ogni famiglia (*family_name*) che ha almeno 300 individui in totale (criterio da applicare sulla somma totale).

> Estrarre dalla tabella *biodiversita.view_lepidotteri_specie_presenza* il numero totale di individui (*esemplari_totali*) per ogni genus (*family_name*) escludendo le specie che hanno meno di 100 individui (criterio da applicare per definire i record che devono essere inclusi nella somma).

### UNION
Per ora abbiamo sempre interrogato una tabella alla volta. In questa e nelle prossime sezioni cominceremo a combinare più tabelle insieme. Il primo e più semplice comando che si può utilizzare dati da tabelle diverse è `UNION`. Questo comando combina il risultato di due (o più)  `SELECT` in un unico set di risultati. In sostanza è come incollare i risultati di una query sotto quelli di un'altra per forare un'unica tabella. Perché questo sia possibile il numero e l'ordine delle colonne nella lista di `SELECT` di entrambe le query devono essere gli stessi (non si può accodare una tabella di 6 colonne a una tabella di 5 colonne) e i tipi di dati devono essere compatibili (non si può accodare una colonna di numeri a una colonna di testo). Il nome di ogni campo verrà preso dalla prima query.  

La struttura generale di una query con `UNION` è:
```sql
SELECT colonna1, colonna2 FROM tabella 1
UNION
SELECT colonna1, colonna2 FROM tabella 1;
```  

L'operatore UNION può mettere le righe del risultato della prima query prima, dopo o tra le righe del risultato della seconda query.
Per ordinare le righe nel set di risultati finale, si usa la clausola `ORDER BY` nella seconda query.

In questo esempio `UNION` unisce in una sola tabella i record della tabella *biodiversita.view_lepidotteri_specie_presenza* con quelli della tabella *biodiversita.view_ortotteri_specie_presenza*:

```sql
SELECT
  animale_code, esemplari_totali
FROM
  biodiversita.view_lepidotteri_specie_presenza
UNION
SELECT
  animale_code, esemplari_totali
FROM
  biodiversita.view_ortotteri_specie_presenza
ORDER BY
animale_code;
```  

In pratica, si usa spesso l'operatore `UNION` per combinare dati da tabelle simili,
L'operatore `UNION` rimuove tutte le righe duplicate dal set di dati combinato. Per mantenere le righe duplicate, si usa invece l'operatore `UNION ALL`.  

#### ESERCIZIO
> Creare una tabella che contiene i campi *parco_code*, *plot_code* e *data_controllo* di tutti i record della tabella *biodiversita.lepidotteri_controllo* e della tabella * biodiversita.lepidotteri_controllo*.

### JOIN
Una query di `UNION` attacca una tabella sotto un'altra, ma le query che generano le due tabelle rimangono distinte. Una singola query però coinvolgere più tabelle contemporaneamente, combinando le informazioni archiviate in ognuna di essere. Ad esempio, se si vogliono sapere le condizioni meteo in cui sono strati trovati degli individui bisogna estrarre una parte del dato dalla tabella di monitoraggio (la specie) e una parte dalla tabella di controllo (le condizioni meteo). Queste due tabelle possono essere collegate perché hanno dei campi in comune (parco, plot e data).  
Una query che accede a tabelle diverse contemporaneamente è chiamata `JOIN`. Ci sono diverse sintassi che possono essere usate per collegare le tabelle. Quella più semplice ha questa struttura:  

```sql
SELECT
  tabella1.colonna_x, tabella1.colonna_y,
  tabella2.colonna_w, tabella2.colonna_z
FROM
  tabella1,
  tabella2
WHERE
  tabella1.colonna_comune = tabella2.colonna_comune;
```  

Un esempio concreto è riportato qui sotto, dove si collegano le tabelle con la tassonomia della specie e del genus, in modo da visualizzare specie, genus e famiglia. Il campo in comune fra queste due tabelle è *genus_name*:

```sql
SELECT
  species_name,
  scientific_name_species.genus_name,
  family_name
FROM
  basedata.scientific_name_species,
  basedata.scientific_name_genus
WHERE
  scientific_name_species.genus_name = scientific_name_genus.genus_name;
```

Il join illustrato nell'esempio può essere scritto anche in questa forma alternativa ed equivalente:

```sql
SELECT
  species_name,
  scientific_name_species.genus_name,
  family_name
FROM
  basedata.scientific_name_species INNER JOIN
  basedata.scientific_name_genus
ON
  (scientific_name_species.genus_name = scientific_name_genus.genus_name);
```

Quando sono coinvolte più tabelle, è una buona pratica qualificare il nome di ogni colonna con il nome della tabella ("nome_tabella.nome_campo") a cui appartiene analogamente a quanto si fa con le tabelle a cui ci si riferisce usando la forma "nome_schema.nome_tabella". Questo è obbligatorio se lo stesso nome di colonna è usato in due tabelle diverse, altrimenti il database non saprebbe a quale ci riferiamo. Se il nome della tabella è lungo si possono usare degli `ALIAS` di tabella per semplificare il codice.  
In questo esempio, più complesso perché le tabelle sono legate da 3 campi, il codice SQL genera la tabella che mette insieme i dati meteo e le determinazioni per i lepidotteri:

```sql
SELECT
  con.plot_code,
  con.parco_code,
  con.data_controllo,
  con.cielo_copertura_code,
  con.vento_quantita_code,
  mon.animale_code,
  mon.numero_totale
FROM
  biodiversita.lepidotteri_controllo AS con,
  biodiversita.lepidotteri_monitoraggio AS mon
WHERE
con.plot_code = mon.plot_code AND
con.parco_code = mon.parco_code AND
con.data_controllo = mon.data_controllo;
```

Si possono unire più di due tabelle, usando lo stesso approccio visto con il caso di 2 sole tabelle.

#### ESERCIZIO
> Scrivere una query che mostra nella stessa tabella il nome della famiglia (*family_name*), della classe (*class_name*) e dell'ordine (*order_name*) utilizzando le tabelle *basedata.scientific_name_family* e *basedata.scientific_name_class*.

> Visualizzare tutti gli ortotteri che sono stati identificati (tabella *biodiversita.ortotteri_monitoraggio*) in un controllo con un impatto di pascolo (*pascolo_impatto_code* non nullo nella tabella *biodiversita.ortotteri_controllo*).

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
