GESTIONE DEI DATI NEI PARCHI NAZIONALI ALPINI
-----------------------------------------------

```diff
- IN EVOLUZIONE -
```

MODULO 1: Gestione dei dati [1 ora; Presentazione]
- A cosa serve un database
	- Problemi gestione dei dati
	- Versioni dei dati
	- Formalizazione dei dati
	- Documentazione dei dati
	- Accessibilità dei dati
	- Preservazione dei dati
	- Gestione di utenze mutiple
	- Opzioni per l'archiviazione dei dati
- Requisiti dei dati
	- Completi
	- Validi
	- Corretti
	- Coerenti
	- Espliciti
	- Comprensibili
	- Codificati
- Piano per la gestione dei dati
	- Acquisizione dei dati
	- Controllo di qualità dei dati
	- Archiviazione dei dati
	- Aggiornamento dei dati
	- Accessibilità dei dati
	- Responsabile dei dati
	- Costo per la gestione dei dati

MODULO 2: Introduzione ai database relazionali [0.5 ora; Presentazione]
- Cosa è un database relazionale
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

MODULO 3: Visualizzare e gestire i dati di un database [2.5 ore; DEMO]
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

-----------------------------------------------

MODULO 3: SQL base [4 ore; Esercitazione]
- SELECT, FROM
- WHERE, =
- AND, OR
- IN
- !=
- NULL
- Alias
- ORDER BY, LIMIT
- DISTINCT
- JOIN di tabelle

-----------------------------------------------

MODULO 3: Dati del progetto biodiversità [1 ora; Demo]
- Modello dati biodiversità
- Review della struttura delle singole tabelle
- Tassonomia nel database biodiversità
- Tabelle esterne
- Look up tables
- VIEWS database biodiversità condiviso
- Modello dati di altri data sets dei parchi

-----------------------------------------------

MODULO 5: SQL intermedio [3 ore; Esercitazione]
- CASE
- CAST
- LIKE
- GROUP BY
- HAVING
- COALESCE
- LEFT JOIN
- Dati temporali (date, time, timezone, EXTRACT)
- Oggetti spaziali in PostGIS
- Creare un punto a partire dalle coordinate
- Sistemi di riferimento e coordinate
- INSERT, UPDATE, DELETE
- Subquery used in FROM statements
- Subquery used in WHERE statements
- Creare una VIEW
- WINDOW functions
- Cosa è un Trigger
- Trovare in quale comune ricade un punto

-----------------------------------------------

MODULO 6: Verifica dei dati e inserimento nel database [4 ore; DEMO]
Controllo preliminare dei dati
- Verifica della completezza e correttezza dei dati nei fogli di calcolo
- Formattazione dei dati per l'importazione
Creare tabelle
- Creazione di una tabella di importazione nel database
Importare i dati
- Importazione dei dati nel database
Validazione dei dati
- Validazione dei valori dei dati
- Verifica della coerenza dei dati
- Caricamento dei dati nella tabella finale
