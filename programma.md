# GESTIONE DEI DATI NEI PARCHI NAZIONALI ALPINI - PROGRAMMA

## MODULO 1: Gestione dei dati e database  

### Parte 1: Introduzione alla gestione dei dati [1.5 ore]
#### Problemi tipici nella gestione dei dati [Presentazione]
	- Versioni dei dati
	- Documentazione dei dati
	- Accessibilità dei dati
	- Preservazione dei dati
	- Gestione di utenze multiple
	- Formalizzazione dei dati
	- Qualità dei dati
#### Requisiti per la gestione dei dati [Presentazione]
	- Completezza
	- Validità
	- Correttezza
	- Coerenza
	- Documentazione
	- Accessibilità
	- Opzioni per l'archiviazione dei dati
#### Piano per la gestione dei dati [Presentazione]  
	- Acquisizione dei dati
	- Controllo di qualità dei dati
	- Archiviazione dei dati
	- Aggiornamento dei dati
	- Accessibilità dei dati
	- Responsabile dei dati
	- Costo per la gestione dei dati

### Parte 2: Introduzione ai database relazionali [2.5 ore]  
#### Cosa è un database relazione [Presentazione]
	- Definizione di database relazionale
	- Cosa è PostgreSQL
	- Architettura Server-Client
	- Schemi
	- Tabelle
	- Tipi di dato
	- Vincoli ai dati
	- Vincoli fra tabelle
	- Modello dati ER
	- Utenti
	- Viste
	- Trigger e funzioni
	- Cosa è il linguaggio SQL
#### Visualizzare e gestire i dati di un database [DEMO + Esercizi]
	- Installare PgAdmin
	- Collegarsi a un database
	- Collegarsi ai database di ogni Parco
	- Collegarsi al database biodiversità condiviso
	- Gli elementi dell'interfaccia
	- Gli oggetti del database
	- Visualizzare i dati
	- Visualizzare la struttura dell tabelle
	- Visualizzare i dati spaziali
	- SQL per creare oggetti
	- SQL per interrogare i dati
	- Eseguire una query
	- Modificare i dati
	- Esportare i dati
	- Collegarsi al database con QGIS
	- Collegarsi al database con R
	- Collegarsi al database con Libre Office
	- Esercizi riassuntivi

## MODULO 2: Introduzione a SQL

### Organizzazione dei dati del Progetto Biodiversità [0.5 ora; Demo]
	- Modello dati Biodiversità
	- Review della struttura delle singole tabelle
	- Tassonomia nei database Biodiversità
	- Tabelle esterne
	- Tabelle di Look up
	- VIEWS del database biodiversità condiviso
	- Modello dati di altri data sets dei parchi

### Comandi SQL base per interrogare i dati [3.5 ore; Lezione + Esercizi]
2.1 SELECT, FROM
2.2 WHERE, =
2.3 AND, OR
2.4 IN
2.5 !=
2.6 NULL
2.7 Alias
2.8 ORDER BY, LIMIT
2.9 DISTINCT
2.10 INTEGER, FLOAT, CHARACTER VARYING, TEXT, BOOLEAN
2.11 CAST
2.12 Esercizi ricapitolativi

## MODULO 3: Comandi SQL per interagire con le tabelle in modo complesso

### Comandi SQL per unire dati da tabelle diverse e per raggrupparli [3 ore; Lezione + Esercizi]
3.1 DATE, TIMESTAMP, EXTRACT, TIMEZONE
3.2 JOIN di tabelle
3.3 LEFT JOIN
3.4 LIKE
3.5 GROUP BY
3.6 HAVING
3.7 COALESCE
3.8 CASE
3.9 Creare una VIEW
### Dati spaziali [1 ora;  Lezione + Esercizi]
3.10 Oggetti spaziali in PostGIS
3.11 Creare un punto a partire dalle coordinate
3.12 Sistemi di riferimento e coordinate
3.13 Trovare in quale comune ricade un punto
3.14 Riproiettare un punto
3.15 Creare una VIEW e visualizzarla in QGIS

## MODULO 4: Gestione e aggiornamento del database
### Comandi SQL avanzati [1 ora;  Lezione + Esercizi]
4.1 INSERT
4.2 UPDATE
4.3 DELETE
4.4 Subquery used in FROM statements
4.5 Subquery used in WHERE statements
4.6 WINDOW functions

### Inserimento di nuovi dati [2,5 ore; Demo]
4.7 Controllo preliminare dei dati
4.8 Verifica della completezza e correttezza dei dati nei fogli di calcolo
4.9 Formattazione dei dati per l'importazione
4.10 Creazione di una tabella di importazione nel database
4.11 Importazione dei dati nel database
4.12 Validazione dei valori dei dati
4.13 Verifica della coerenza dei dati
4.14 Caricamento dei dati nella tabella finale
4.15 Estensione del database con nuovi dataset

### Manutenzione del Database [0.5 ore; Demo]
4.16 Backup e restore
4.17 Gestione dei permessi
