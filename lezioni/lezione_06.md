<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

#### Lezione 6
## SQL AVANZATO

Autore: Ferdinando Urbano  

---
In questa lezioni si continua l'esplorazione del linguaggio SQL aggiungendo comandi più avanzati, in particolare per visualizzare contemporaneamente dati provenienti da più tabelle e raggruppare le righe per ottenere statistiche basate su determinati valori.


### GROUP BY
In alcuni casi può essere utile raggruppare le righe di una tabella secondo alcuni criteri. Ad esempio, per vedere quanti individui di ogni specie sono stati trovati in un monitoraggio, o quanti individui sono stati trovati durante un controllo per ogni plot. Questo è possibile con SQL in modo semplice tramite il comando `GROUP BY` che divide le righe restituite dall'istruzione `SELECT` in gruppi con lo stesso valore di una o più tabelle (ad esempio il plot o la specie). Per ogni gruppo, si può poi applicare una funzione di aggregazione ai valori delle colonne utilizzate come criteri per raggruppare le righe. Le principali funzioni di aggregazione (per i numeri) sono `COUNT` (conta quante righe sono state raggruppate), `SUM` (fai la somma di tutti i valori di quel campo, ad esempio il numero di individui), `MIN` (il valore minimo di quel campo per tutto il gruppo), `MAX` (il valore massimo di quel campo per tutto il gruppo), `AVG` (il valore medio di quel campo per tutto il gruppo, ad esempio il numero medio di individui trovati in ogni controllo dei plot), `STDDEV` (deviazione standard).  
La clausola `GROUP BY` deve apparire subito dopo la clausola `FROM` o `WHERE`. La clausola `GROUP BY` è seguita dal nome di una colonna o dalla lista di colonne, separate da virgola.  
La struttura generale del comando è:  
```sql
SELECT colonna_x, funzione_aggregazione(colonna_y)
FROM tabella_x
GROUP BY colonna_x;
```  

Le colonne specificate dopo `SELECT` devono essere elencate nella lista definita dopo `GROUP BY` o associate a una funzione di aggregazione, altrimenti la query non verrà eseguita e verrà restituito un errore.  
Se si vogliono raggruppare le righe in base ad alcuni campi ma non si devono calcolare statistiche sui campi aggregati, invece del comando `GROUP BY` si può usare il comando `DISTINCT` visto nella lezione precedente.  

Il modo più semplice per capire cosa fa e come funziona `GROUP BY` è guardare un esempio. Supponiamo di voler sapere per ogni specie il numero totale di lepidotteri trovati in tutti i monitoraggi di tutti i parchi; per fare questo devo specificare che voglio "raggruppare" le mie righe per *animale_code* (una riga per ogni specie animale o altro livello tassonomico) e poi che voglio calcolare la somma del numero di individui trovati per quella specie (*animale_code*). Per facilitare la lettura dei dati voglio anche chiedere che il risultato sia restituito mettendo in ordine alfabetico i nomi degli animali. Questa richiesta si può tradurre inizialmente come "Raggruppa tutte le righe della tabella *biodiversita.lepidotteri_monitoraggio* e calcola il numero totali di individui". La traduzione in SQL di questa richiesta è:  

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

Se invece voglio raggruppare per specie e per parco, devo inserire entrambi i criteri dopo `GROUP BY`. In questo caso invece che ordinare per specie, voglio ordinare per numero complessivo di individui. Voglio anche sapere quanti record facevano parte di ogni gruppo (quindi in quanti transetti è stata trovata ogni specie):

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
Il comando `HAVING` può essere usato in associazione con `GROUP BY` per filtrare il risultato del raggruppamento escludendo i gruppi che non soddisfano una specifica condizione di aggregazione. La clausola `HAVING` applica la condizione alle righe del gruppo dopo l'applicazione del comando `GROUP BY`, mentre il comando `WHERE` applica la condizione per le righe individuali prima dell'applicazione della clausola `GROUP BY`. Con riferimento agli esempi precedenti, se volessi escludere tutte le specie che hanno meno di 100 individui complessivi (risultato della somma di tutti i record di quella specie) dovrei aggiungere `HAVING SUM(numero_totale) > 100`, mentre se volessi escludere tutti i record raccolti prima del 2015 dovrei aggiungere `WHERE data_controllo > '2015-01-01'` (la prima clausola elimina dai risultati i gruppi che non soddisfano la condizione mentre la seconda limita i record che sono raggruppati). La condizione `HAVING` si mette dopo `GROUP BY` (quindi dopo `WHERE`, infatti una condizione che viene logicamente applicata dopo).


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

#### ESERCIZIO
> Estrarre dalla tabella *biodiversita.view_lepidotteri_specie_presenza* il numero totale di individui (*esemplari_totali*) per ogni famiglia (*family_name*) che ha almeno 300 individui in totale (criterio da applicare sulla somma totale).

> Estrarre dalla tabella *biodiversita.view_lepidotteri_specie_presenza* il numero totale di individui (*esemplari_totali*) per ogni genus (*genus_name*) escludendo le specie che hanno meno di 100 individui (criterio da applicare per definire i record che devono essere inclusi nella somma).

### UNION
Per ora abbiamo sempre interrogato una tabella alla volta. In questa e nelle prossime sezioni cominceremo a combinare più tabelle insieme. Il primo e più semplice comando che può utilizzare dati da tabelle diverse è `UNION`. Questo comando combina il risultato di due (o più)  `SELECT` in un unico set di risultati. In sostanza è come incollare i risultati di una query sotto quelli di un'altra per formare un'unica tabella; perché questo sia possibile il numero e l'ordine delle colonne nella lista di `SELECT` di entrambe le query devono essere gli stessi (non si può accodare una tabella di 6 colonne a una tabella di 5 colonne) e i tipi di dati devono essere compatibili (non si può accodare una colonna di numeri ad una di testo). Il nome di ogni campo verrà preso dalla prima query.  

La struttura generale di una query con `UNION` è:
```sql
SELECT colonna1, colonna2 FROM tabella 1
UNION
SELECT colonna1, colonna2 FROM tabella 2;
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
> Creare una tabella che contiene i campi *parco_code*, *plot_code* e *data_controllo* di tutti i record della tabella *biodiversita.lepidotteri_controllo* e della tabella *biodiversita.ortotteri_controllo*.

### JOIN
Una query di `UNION` attacca una tabella sotto un'altra, ma le query che generano le due tabelle rimangono distinte. Una singola query però coinvolgere più tabelle contemporaneamente, combinando le informazioni archiviate in ognuna di essere. Ad esempio, se si vogliono sapere le condizioni meteo in cui sono strati trovati gli individui di una specie bisogna estrarre una parte del dato dalla tabella di monitoraggio (la specie) e una parte dalla tabella di controllo (le condizioni meteo). Queste due tabelle possono essere collegate perché hanno dei campi in comune (parco, plot e data).  
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

Quando sono coinvolte più tabelle, è una buona pratica identificare ogni colonna con il nome della tabella cui appartiene + '.' + nome della colonna (*nome_tabella.nome_campo*), analogamente a quanto si fa con le tabelle a cui ci si riferisce usando la forma *nome_schema.nome_tabella*. Questo è obbligatorio se lo stesso nome di colonna è usato in due tabelle diverse, altrimenti il database non saprebbe a quale tabella ci riferiamo. Se il nome della tabella è lungo si possono usare degli `ALIAS` di tabella per semplificare il codice.  
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

L'approccio dell'unione di due tabelle spiegato sopra si può utilizzare anche per unire più di due tabelle.

#### ESERCIZIO
> Scrivere una query che mostra nella stessa tabella il nome della famiglia (*family_name*), della classe (*class_name*) e dell'ordine (*order_name*) utilizzando le tabelle *basedata.scientific_name_family* e *basedata.scientific_name_class*.

> Visualizzare tutti gli ortotteri che sono stati identificati (tabella *biodiversita.ortotteri_monitoraggio*) in un controllo con un impatto di pascolo (*pascolo_impatto_code* non nullo nella tabella *biodiversita.ortotteri_controllo*).

### LEFT JOIN
Nell'esempio precedente, solo i record della prima e della seconda tabella che corrispondono alle condizioni di join sono inclusi nei risultati. Usando una diversa sintassi di `JOIN` è possibile includere TUTTI i record della prima tabella e solo i record della seconda tabella che corrispondono alle condizioni di unione. Questo risultato si ottiene con l'uso di `LEFT JOIN`. Questo tipo di join è utile in molte situazioni. Ad esempio, io posso avere dei controlli di un plot in cui non sono stati trovati individui. Quando faccio un join con la tabella di monitoraggio, la tabella dei controlli senza specie in un `JOIN` normale verrebbero esclusi perché non hanno un corrispettivo nei monitoraggi. Utilizzando `LEFT JOIN` invece vengono inclusi tutti i record della prima tabella e quelli che soddisfano la condizione di `JOIN` della seconda tabella. Nel caso in cui nella seconda tabella non ci sia una corrispondenza a una riga della prima colonna, i campi della seconda tabella rimangono vuoti nel risultato. Ordinando il risultato per le colonne della seconda tabella è immediatamente possibile verificare quali sono i controlli in cui non sono stati trovati individui.

```sql
SELECT
  con.parco_code,
  con.plot_code,
  con.data_controllo,
  con.controllo_esito_code,
  mon.plot_code,
  mon.data_controllo,
  mon.animale_code,
  mon.numero_totale
FROM
  biodiversita.lepidotteri_controllo AS con LEFT JOIN
  biodiversita.lepidotteri_monitoraggio AS mon
ON
  con.plot_code = mon.plot_code AND
  con.parco_code = mon.parco_code AND
  con.data_controllo = mon.data_controllo
ORDER BY
  mon.plot_code,
  mon.data_controllo,
  con.plot_code,
  con.data_controllo;
```

Invertendo l'ordine delle tabelle (monitoraggio a sinistra del LEFT JOIN e controllo a destra), si può ad esempio controllare che i dati di un monitoraggio corrispondono a un controllo (nel caso del nostro database questo è sicuramente vero perché abbiamo una chiave esterna che lo garantisce).

#### ESERCIZIO
> Verificare che tutti i plot dei parchi sono stati (o meno) utilizzati per il monitoraggio degli ortotteri (tutti i plot della lista in tabella *biodiversita.plot* hanno associati dei record della tabella *biodiversita.ortotteri_controllo*).  

Una possibile soluzione per verificare la correttezza della risposta all'esercizio è riportata qui sotto:

```sql
SELECT a.parco_code, a.plot_code, b.data_controllo
FROM biodiversita.plot a
LEFT JOIN biodiversita.ortotteri_controllo b
ON a.parco_code = b.parco_code and a.plot_code = b.plot_code
ORDER BY b.data_controllo DESC;
```

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

È possibile estrarre parte di un timestamp (ad esempio mese, anno, minuto). In particolare, `epoch` è molto utile in quanto trasforma una data in secondi da un momento specifico nel tempo. Così facendo, è possibile trattare i riferimenti temporali come numeri interi utilizzando tutte le funzioni tipiche degli interi, ed eventualmente riconvertendo poi il risultato a un tipo di dato temporale.

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

In alcuni casi, nei dati raccolti sul campo per alcuni record la data non è disponibile, ma c'è solo l'anno. In questo caso, nel database si può avere un campo data (per quando questa è presente) e un campo anno (come numero intero) nel caso questa sia l'unica informazione disponibile.  

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
Una nuova tabella può essere costruita in due modi: attraverso l'interfaccia grafica di PgAdmin che chiederà in sequenza tutte le informazioni necessarie (nome della tabella, nome e tipo di dato di ogni colonna, chiave primaria, eventuali chiavi esterne e vincoli, permessi associati alla tabella) che tradurrà poi nel codice SQL che la genera; oppure scrivere direttamente il codice SQL. In questo secondo caso è ad esempio molto utile selezionare una tabella nel menù ad albero del pannello di sinistra di PgAdmin e visualizzare il tab *SQL* . Da qui si può copiare il codice che ha generato quella tabella e incollarlo per poi modificarlo a piacere.  
Nella versione più semplice (con definiti alcuni campi e la sola chiave primaria) il codice SQL è del tipo:  

```sql
CREATE TABLE nome_schema.nome_tabella
(
  campo1 integer,
  campo2 character varying,
  campo3 date,
  CONSTRAINT nome_tabella_pk PRIMARY KEY (campo1)
);
```

Il nome di ogni oggetto (tabella ,chiave primaria, chiavi esterne) deve essere univoco nel database, quindi se un nome è già usato il database restituirà un messaggio di errore.  
Come accennato in precedenza, si sconsiglia di usare lettere maiuscole e caratteri speciali nei nomi di campi e tabelle.  
Un altro metodo per creare una tabella a partire da una query SQL è usare il comando `CREATE TABLE nome_schema.nome_tabella AS`. In questo modo la tabella generata dalla query verrà trasformata in una tabella con il nome prescelto. In un secondo momento si deve poi aggiungere la chiave primaria. Un esempio è riportato qui sotto:

```sql
CREATE TABLE temp.tabella_test AS
SELECT  
  parco_code,
  plot_code,
  data_controllo
FROM
  biodiversita.lepidotteri_controllo
WHERE
  parco_code = 'pns';
```

E qui aggiungo una chiave primaria:
```sql
ALTER TABLE temp.tabella_test
ADD CONSTRAINT tabella_test_pk PRIMARY KEY (parco_code, plot_code);
```

### Creare una VIEW
Creare una *VIEW* è una operazione molto semplice: è sufficiente scrivere la query SQL che genera la tabella desiderata e farla precedere dal comando che specifica quale deve essere il nome (e lo schema) della tabella: `CREATE VIEW nome_schema.nome_vista AS` (in modo analogo a quanto fatto con la creazioni di una tabella da una query). Per maggiori informazioni si può consultare la documentazione di PostgreSQL: **[VIEWS](https://www.postgresql.org/docs/devel/static/sql-createview.html)**.
In questo esempio viene creata una view analoga alla tabella dell'esempio precedente:

```sql
CREATE view temp.view_test AS
SELECT  
  parco_code,
  plot_code,
  data_controllo
FROM
  biodiversita.lepidotteri_controllo
WHERE
  parco_code = 'pns';
```  

---

[**Lezione 7.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_07.md) Dati spaziali - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_07.html)</ins>]
