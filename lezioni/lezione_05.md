<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 5
## SQL E COMANDI SQL BASE

Autore: Ferdinando Urbano  

---

SQL (sigla che sta per Structured Query Language)  ́e un linguaggio testuale standard per ope-rare con le basi di dati.  Standard vuol dire che  ́e (quasi) indipendente la particolare database scelto (Oracle, Microsoft SQL Server, PostgreSQL, Mysql, etc.).  Il linguaggio  ́e funzionale (un solo costrutto esegue le operazioni specificate), non imperativo (non ci sono variabili o elenchi di operazioni), anche se una sua estensione (il PL-SQL) permette di dichiarare funzioni in modo imperativo.  Un’introduzione al linguaggio richiederebbe un corso annuale:  in questa breve nota si vuole dare una breve descrizione alla struttura del linguaggio, in modo che poi sia possibile introdurne la parte propriamente spaziale.  Inizieremo col vedere gli elementi di base (tipi di dato:  numeri e parole), passeremo quindi alla definizione dei dati (schemi, colonne e tabelle), alle operazioni di inserimento e modifica dei dati, quindi all’interrogazione degli stessi.  Per finire faremo un breve accenno agli elementi avanzati:  indici, chiavi e relazioni.

Il numero delle operazioni che si possono fare sul database aumenta esponenzialmente con un po' di familiarità con SQL. Per chi fosse interessato, esistono tantissimi tutorial gratuiti online. Ad esempio (tra i tanti):  

* [PostgreSQL official tutorial](https://www.postgresql.org/docs/current/static/tutorial.html)  
* [postgresqltutorial](http://www.postgresqltutorial.com/)  
* [w3resource](https://w3resource.com/PostgreSQL/tutorial.php)  
* [sqlbolt](https://sqlbolt.com/)  
* [webcheatsheet](http://webcheatsheet.com/sql/interactive_sql_tutorial/)  
* [www.sql.org](www.sql.org)  

Per una introduzione all'estensione spaziale PostGIS, una fonte utile è:  

* [PostGIS Intro by BoundlessGeo](http://workshops.boundlessgeo.com/postgis-intro/)  

Per questioni complesse, gli operatori e i collaboratori del Parco possono sempre chiedere il supporto all'amministratore o allo sviluppatore del database.


### PgAdmin SQL editor
  aprire editor
  struttura editor
  semplice query
  aprire più tab
  chiudere un tab

### SELECT, FROM

### WHERE, =

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
