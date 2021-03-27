<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

### Introduzione alla gestione dei dati per i Parchi Nazionali Alpini Italiani
## PROGRAMMA DETTAGLIATO DEL CORSO
---

### MODULO 1: Introduzione alla gestione dei dati e database  

#### Lezione 1. Gestione dei dati: problemi e soluzioni
- Problemi tipici nella gestione dei dati
- Requisiti per la gestione dei dati
- Opzioni per l'archiviazione dei dati
- Piano per la gestione dei dati
- Principali caratteristiche dei database relazionali
- Perché i Parchi stanno creando i propri database

#### Lezione 2. Introduzione ai database relazionali
- Cosa è un database
  - Cosa è un database relazionale
  - Cosa è PostgreSQL
  - Cosa è PgAdmin
  - Cosa è PostGIS
  - Architettura Server-Client
  - Database server
- Principali oggetti di un database
  - Schemi
  - Tabelle
  - Tipi di dato
  - Utenti
  - Viste
  - Trigger e funzioni
- Vincoli formali
  - Vincoli ai dati
  - Vincoli fra tabelle
- Modello dati
  - Modello Entità-Relazioni
  - Normalizzazione
- Il linguaggio di un database relazionale
  - Il linguaggio SQL

#### Lezione 3. Visualizzare e gestire i dati di un database
- Installare PgAdmin
- Collegarsi a un database
- Cambiare le impostazioni di PgAdmin
- Collegarsi ai database di ogni Parco
- Collegarsi al database biodiversità condiviso
- Gli elementi dell'interfaccia
- Gli oggetti del database
- Visualizzare i dati
- Visualizzare la struttura delle tabelle
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

---

### MODULO 2: Introduzione a SQL

#### Lezione 4. Organizzazione dei dati del Progetto Biodiversità
- Modello dati Biodiversità
- Review della struttura delle singole tabelle
- Tassonomia nei database Biodiversità
- Tabelle esterne
- VIEWS del database biodiversità condiviso
- Modello dati di altri data sets dei parchi

#### Lezione 5. Comandi SQL base per interrogare i dati
1. SELECT, FROM
2. WHERE, =
3. AND, OR
4. IN, NOT IN
5. !=, >, <
6. NULL, IS NULL, IS NOT NULL
7. Alias di colonne e tabelle AS
8. ORDER BY, LIMIT
9. DISTINCT
10. INTEGER, FLOAT, CHARACTER VARYING, TEXT, BOOLEAN, SERIAL number
11. Cambiare tipo di dato: CAST
12. Ricerca nelle stringhe di testo: LIKE
13. Scaricare i dati di una query in un file .csv
14. Esercizi ricapitolativi

---

### MODULO 3: Comandi SQL per interagire con il database in modo complesso

#### Lezione 6. Comandi SQL per unire dati da tabelle diverse e per raggrupparli
1. DATE, TIMESTAMP, EXTRACT, TIMEZONE
2. UNION
3. JOIN di tabelle
4. LEFT JOIN
5. GROUP BY
6. HAVING
7. COALESCE
8. CASE
9. Creare una tabella
10. Creare una VIEW

#### Lezione 7. Dati spaziali
2. Visualizzazione dei dati spaziali in PgAdmin
3. Creare un punto a partire dalle coordinate
4. Sistemi di riferimento e coordinate
5. Trovare in quale comune ricade un punto
6. Riproiettare le coordinate di un punto
7. Visualizzare una view in QGIS

---

### MODULO 4: Gestione e aggiornamento del database

#### Lezione 8. Comandi SQL avanzati
1. Inserimento di nuovi dati: INSERT
2. Inserimento di nuovi dati: COPY
3. Inserimento di nuovi dati: /COPY
4. Aggiornamento di dati: UPDATE
5. Cancellazione di dati: DELETE
6. Subquery con FROM
7. Subquery con WHERE
8. Funzioni WINDOW

#### Lezione 9. Controllo e importazione di un nuovo dataset
1. Controllo preliminare dei dati
2. Verifica della completezza e correttezza dei dati nei fogli di calcolo
3. Formattazione dei dati per l'importazione
4. Creazione di una tabella di importazione nel database
5. Importazione dei dati nel database
6. Validazione dei valori dei dati
7. Verifica della coerenza dei dati
8. Caricamento dei dati nella tabella finale
9. Documentazione della qualità dei dati
10. Estensione del database con nuovi dataset
11. Esercitazione pratica con i dati sugli uccelli raccolti dal PNDB
