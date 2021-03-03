### Introduzione alla gestione dei dati per i Parchi Nazionali Alpini Italiani
## PROGRAMMA DEL CORSO

### MODULO 1: Introduzione alla gestione dei dati e database  

#### Lezione 1. Gestione dei dati: problemi e soluzioni [1,5 ora; Presentazione]
	- Problemi tipici nella gestione dei dati
	  - Versioni dei dati
	  - Documentazione dei dati
	  - Accessibilità dei dati
	  - Preservazione dei dati
	  - Gestione di utenze multiple
	  - Formalizzazione dei dati
	  - Qualità dei dati
	  - Riuso dei dati
	- Requisiti per la gestione dei dati
	  - Completezza
	  - Validità
	  - Correttezza
	  - Coerenza
	  - Documentazione
	  - Accessibilità
	- Opzioni per l'archiviazione dei dati
	  - CSV file e documentazione
	  - Fogli di calcolo
	  - Database locali
	  - Archivi web condivisi
	  - Database centralizzati
	- Principali caratteristiche dei database relazionali
	  - a
	  - b
	  - c
	- Piano per la gestione dei dati
	  - Acquisizione dei dati
	  - Controllo di qualità dei dati
	  - Archiviazione dei dati
	  - Aggiornamento dei dati
	  - Accessibilità dei dati
	  - Responsabile dei dati
	  - Costo per la gestione dei dati

#### Lezione 2. Introduzione ai database relazionali [0,5 ora; Presentazione]
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

#### Lezione 3. Visualizzare e gestire i dati di un database [2 ore; DEMO + Esercizi]
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

### MODULO 2: Introduzione a SQL

#### Lezione 4. Organizzazione dei dati del Progetto Biodiversità [0.5 ora; Demo]
	- Modello dati Biodiversità
	- Review della struttura delle singole tabelle
	- Tassonomia nei database Biodiversità
	- Tabelle esterne
	- Tabelle di Look up
	- VIEWS del database biodiversità condiviso
	- Modello dati di altri data sets dei parchi

#### Lezione 5. Comandi SQL base per interrogare i dati [3.5 ore; Lezione + Esercizi]
1. SELECT, FROM
2. WHERE, =
3. AND, OR
4. IN, NOT IN
5. !=, >, <
6. NULL, IS NULL, IS NOT NULL
7. Alias
8. ORDER BY, LIMIT
9. DISTINCT
10. INTEGER, FLOAT, CHARACTER VARYING, TEXT, BOOLEAN
11. CAST
12. Esercizi ricapitolativi

### MODULO 3: Comandi SQL per interagire con il database in modo complesso

#### Lezione 6. Comandi SQL per unire dati da tabelle diverse e per raggrupparli [3 ore; Lezione + Esercizi]
1. DATE, TIMESTAMP, EXTRACT, TIMEZONE
2. JOIN di tabelle
3. LEFT JOIN
4. LIKE
5. GROUP BY
6. HAVING
7. COALESCE
8. CASE
9. Creare una VIEW

#### Lezione 7. Dati spaziali [1 ora; Lezione + Esercizi]
1. Oggetti spaziali in PostGIS
2. Creare un punto a partire dalle coordinate
3. Sistemi di riferimento e coordinate
4. Trovare in quale comune ricade un punto
5. Riproiettare un punto
6. Visualizzare una view in QGIS

### MODULO 4: Gestione e aggiornamento del database

#### Lezione 8. Comandi SQL avanzati [1 ora; Lezione + Esercizi]
1. INSERT
2. UPDATE
3. DELETE
4. Subquery used in FROM statements
5. Subquery used in WHERE statements
6. WINDOW functions

#### Lezione 9. Inserimento di nuovi dati [2,5 ore; Demo]
- Controllo preliminare dei dati
- Verifica della completezza e correttezza dei dati nei fogli di calcolo
- Formattazione dei dati per l'importazione
- Creazione di una tabella di importazione nel database
- Importazione dei dati nel database
- Validazione dei valori dei dati
- Verifica della coerenza dei dati
- Caricamento dei dati nella tabella finale
- Estensione del database con nuovi dataset
- Esercitazione pratica con i dati sugli uccelli raccolti dal PNDB

#### Lezione 10. Manutenzione del Database [0.5 ore; Lezione + Esercizi]
- Backup
- Restore
- Creazione di utenti
- Creazione di gruppi
- Assegnazione dei permessi
