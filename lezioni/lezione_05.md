<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 5
## SQL E COMANDI SQL BASE

Autore: Ferdinando Urbano  

---

SQL, che sta per Structured Query Language, e il linguaggio che si utilizza per interagire (interrogare i dati, manipolare i dati, creare degli oggetti) con i database relazionali. SQL è un linguaggio standard, il che vuol dire che è (quasi) indipendente dal particolare database scelto (ad esempio PostgreSQL, Oracle, Microsoft SQL Server, Mysql). SQL, per quanto sia un linguaggio di programmazione e quindi richieda esperienza per essere usato al pieno delle sue potenzialità, è comunque un linguaggio di alto livello, quindi semplice almeno per quanto riguarda i comandi principali. Infatti, mentre le query (codice SQL finalizzato a chiedere dati al database nella forma richiesta dall'utente) complesse possono essere difficili da progettare, SQL combina un insieme molto limitato di comandi che sono in qualche modo vicini al linguaggio naturale.  
L'obiettivo delle prossime lezione è fornire i rudimenti per ottenere dal database i dati desiderati. Alcuni elementi più avanzati saranno solo di interesse per le persone che devono più direttamente essere coinvolte nell'uso e aggiornamento del database. In particolare, le lezioni 5 e 6 si concentrano sul recupero dei dati da un database esistente. In lezioni successive si vedrà come usare SQL per creare nuovi oggetti e per manipolare gli oggetti spaziali.

Il numero delle operazioni che si possono fare sul database aumenta esponenzialmente all'aumentare della conoscenza di SQL. Per gli utenti che desiderano sviluppare ulteriormente le proprie conoscenza in materia, esistono tantissimi tutorial gratuiti online. Ad esempio (tra i tanti):  

* [PostgreSQL official tutorial](https://www.postgresql.org/docs/current/static/tutorial.html)  
* [postgresqltutorial](http://www.postgresqltutorial.com/)  
* [w3resource](https://w3resource.com/PostgreSQL/tutorial.php)  
* [sqlbolt](https://sqlbolt.com/)  
* [webcheatsheet](http://webcheatsheet.com/sql/interactive_sql_tutorial/)  
* [www.sql.org](www.sql.org)  

Per una introduzione all'estensione spaziale PostGIS, una fonte di informazioni utile è:  

* [PostGIS Intro by BoundlessGeo](http://workshops.boundlessgeo.com/postgis-intro/)  

In futuro, per l'uso e aggiornamento dei database dei singoli Parchi, gli operatori e i collaboratori potranno eventualmente chiedere il supporto tecnico al curatore e al gestore del database del proprio Parco.

### PgAdmin SQL editor
Per poter inviare una richiesta al database tramite una query SQL, si deve aprire la finestra SQL (o SQL editor) del client che si utilizza, nel nostro caso PgAdmin. L'icona del SQL editor è nella barra dei menù in alto a sinistra (vedi figura sotto). La finestra SQL verrà aggiunta alla lista delle schede (Le schede con gli SQL editor non vanno confuse con la scheda chiamata *SQL* che è utilizzata da PaAdmin per mostrare il codice SQL che genera l'oggetto selezionato nel menù ad albero del pannello di sinistra). Si possono aprire quante finestre SQL si vogliono, navigando fra poi fra le finestre come in qualsiasi pannello a schede. Ogni scheda può essere chiusa cliccando sul bottone (X) in alto a destra. Si sconsiglia di aprire troppe schede per non rendere la navigazione troppo complicata.  

[![](materiale/l05_sql_editor.png)](https://github.com/feurbano/corsoparchi/blob/main/lezioni/materiale/l05_sql_editor.png?raw=true)  

L'editor è strutturato in due sezioni. In quella superiore si può scrivere il codice SQL, mentre in quella inferiore vengono visualizzati i risultati ottenuti dal database (cioè, i dati in formato tabellare). Nella sezione superiore, a destra può esserci una sottosezione *Scratch Pad*, la cui utilità è limitata e può essere chiuso per non prendere spazio all'editor SQL.  

Una volta aperta la finestra di SQL


### SELECT, FROM

  create script

#### Esercizio

### WHERE, =

#### Esercizio

### AND, OR

### IN, NOT IN

### !=, >, <

### NULL
, IS NULL, IS NOT NULL

### Alias
di colonne e tabelle AS

### ORDER BY, LIMIT

### DISTINCT

### Stringhe, numeri, booleiani
INTEGER, FLOAT, CHARACTER VARYING, TEXT, BOOLEAN, SERIAL number  

### CAST
Cambiare tipo di dato:

### LIKE
Ricerca nelle stringhe di testo:

### Scaricare i dati
di una query in un file .csv

### Esercizi ricapitolativi

---
[**Lezione 6.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_06.md) Comandi SQL avanzati - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_06.html)</ins>]
