<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 9
## CONTROLLO E IMPORTAZIONE DI UN NUOVO DATASET: DIMOSTRAZIONE PRATICA

Autore: Ferdinando Urbano  

---

In questa ultima lezione viene data una dimostrazione pratica di alcuni dei passaggi tipici del processo di controllo e importazione di informazioni disponibili in formato foglio di calcolo.  
Come esempio di riferimento vengono utilizzati i dati raccolti sugli uccelli nell'ambito del Progetto Biodiversità dal Parco Nazionale delle Dolomiti Bellunesi. I file messi a disposizione del Parco hanno una struttura abbastanza semplice ideale per mostrare la procedura senza problemi tecnici eccessivi e presentano, sebbene in misura abbastanza limitata, molti dei problemi che si incontrano nella verifica di qualità dei dati.  
Questa lezione viene presentata interamente online e in questa pagina si riporta solo una lista dei principali passi che vengono illustrati nella dimostrazione. Per le spiegazioni complete si deve vedere la registrazione della video lezione.

#### Esempio di riferimento: file Biodiversità uccelli PNDB 2013-2014-2018-2019
File ricevuti dal Parco:

* 2019-Uccelli-Mon-Bd-COMPILAT-per-PARCO.xls
* Ornitofauna-PNDB-2014_PuntiAscolto.xls.xls
* Ornitofauna-rilievi-2015-ok.xls
* Uccelli-Mon Bd-2018-Compilato.xls

#### Elenco dei passi per il controllo e l'importazione dei dati nel database
- Analizzare distribuzione dei dati fra file
  - Un foglio di calcolo per anno

- Analizzare struttura fogli
  - 3 fogli: monitoraggio, controllo, legenda
  - Diversa lista dei dati

- Analizzare struttura dati
  - Struttura PLOT consistente
  - Struttura ASCOLTO
    - 10/20 minuti strutturato in modo diverso
    - Alcuni record vuoti

- Analisi dei singoli campi PLOT
  - Sole e Vento diverse codifiche
  - Manca operatore 2 anni
  - Campo Parco ridondante
  - Campo anno ridondante
  - Plot scritto in modo scorretto
  - Note aggiuntive e colori celle
  - Informazioni nelle note che dovrebbero essere nelle codifiche
  - Ora fine inconsistente
  - Data scritta in modo potenzialmente ambiguo
- Analisi dei singoli campi ASCOLTO
  - Campi Zona e Anno ridondanti
  - Data scritta in modo potenzialmente ambiguo
  - Comportamento non codificato
  - Osservazioni con vari codici equivalenti
  - Note riferite al PLOT e non all'ASCOLTO
  - Specie
    - Scritte con lettere maiuscole/minuscole
    - Stesso nome scritto in diverse varianti
    - Indeterminato non specificato
  - Distanza con simboli scritti con e senza spazi
  - 10/20 minuti scritto con codici (oltre che struttura) diversi

- Unire dati dai vari file in un unico file PLOT
  - Creare copia dei file
  - Creare struttura analoga fra i 4 fogli
  - Creare un unico foglio che mette insieme PLOT
  - Sistemazione formato Data
  - Correggo Plot
  - Correggo Ora Fine
  - Correggo codifiche Sole
  - Correggo codifiche Vento
  - Riempire dati mancanti Operatore
  - Esporto come CSV

- Unire dati dai vari file in un unico file ASCOLTO
  - Creare copia dei file
  - Creare struttura analoga fra i 4 fogli
  - Creare un unico foglio che mette insieme ASCOLTO
  - Sistemazione formato Data
  - Ordino per eliminare righe vuote
  - Verifica Specie: molte differenze, pulire nel database
  - Verifica Comportamento: molte differenze, pulire nel database
  - Verifica Osservazione: molte differenze, pulire nel database
  - Verifica N individui: 1 nullo
  - Verifica 10/20 minuti: 1 codice scorretto, non rilevato da convertire in NULL nel database
  - Nota da associare al PLOT, da fare nel database
  - Esporto come CSV

- Note per eventuali domande da fare
  - Operatore mancante
  - ...

- Creare tabelle di importazione
  - Lista campi dai .csv
  - Creazione chiave primaria
  - Definizione dei tipi di dati
  - SQL creazione tabella
  - Creazione delle tabelle PLOT e ASCOLTO nello schema TEMP

```sql
CREATE TABLE test.imp_plot
  (
  id serial,
  plot character varying,
  data date,
  ora_inizio character varying,
  ora_fine character varying,
  sole character varying,
  vento character varying,
  note text,
  operatore character varying,
  CONSTRAINT imp_plot_pk PRIMARY KEY (id)
  );
```

```sql
CREATE TABLE test.imp_ascolto
  (
  id serial,
  plot character varying,
  data date,
  specie character varying,
  comportamento character varying,
  n_individui integer,
  distanza character varying,
  osservazione character varying,
  x10_20minuti character varying,
  Note character varying,
  x10_min character varying,
  x20_min character varying,
  CONSTRAINT imp_ascolto_pk PRIMARY KEY (id)
  );
```

- Importazione dei dati dal CSV
  - 1 da interfaccia
  - 1 da riga di comando

```
"C:\\Program Files\\pgAdmin 4\\v4\\runtime\\psql.exe"
-U corso_user -h db.parco.gran-paradiso.g3wsuite.it
-p 2345 -d corso_parchi
```

```
\copy test.imp_plot
(plot, data, ora_inizio, ora_fine, sole, vento, note, operatore)
FROM 'C:\temp\plot.csv'
DELIMITER ';' CSV HEADER
```

```
\copy test.imp_ascolto
(plot, data, specie, comportamento, n_individui, distanza, osservazione, x10_20minuti, note, x10_min, x20_min)
FROM 'C:\temp\ascolto.csv'
DELIMITER ';' CSV HEADER
```

- Creazione tabelle definitive secondo protocollo

```sql
CREATE TABLE biodiversita.uccelli_controllo
(
  plot_code character varying,
  data_controllo date NOT NULL,
  ripetizione integer,
  time_start time without time zone,
  time_end time without time zone,
  cielo_copertura_code character varying,
  vento_quantita_code character varying,
  note text,
  dati_qualita_note text,
  dati_qualita_code integer,
  operatore1_code character varying,
  operatore2_code character varying,
  controllo_esito_code integer,
  CONSTRAINT uccelli_controllo_pk
    PRIMARY KEY (plot_code, data_controllo)
);
GRANT ALL ON TABLE biodiversita.uccelli_controllo
TO corso_user;
```

```sql
CREATE TABLE biodiversita.uccelli_monitoraggio
(
  uccelli_monitoraggio_id serial,
  plot_code character varying not null,
  data_controllo date not null,
  animale_code character varying not null,
  uccelli_numero integer not null,
  uccelli_comportamento_code character varying,
  uccelli_distanza_code character varying,
  uccelli_osservazione_tipo_code character varying,
  uccelli_periodo_rilevamento_type character varying,
  note text,
  dati_qualita_note text,
  dati_qualita_code integer,
  CONSTRAINT uccelli_monitoraggio_pk
    PRIMARY KEY (uccelli_monitoraggio_id),
  CONSTRAINT uccelli_monitoraggio_animali_fkey
  FOREIGN KEY (animale_code)
    REFERENCES biodiversita.biodiversita_animali (animale_code) MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION,
  CONSTRAINT uccelli_monitoraggio_controllo_fkey
    FOREIGN KEY (plot_code, data_controllo)
    REFERENCES biodiversita.uccelli_controllo (plot_code, data_controllo) MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION,
  CONSTRAINT uccelli_periodo_rilevamento_check
	  CHECK (uccelli_periodo_rilevamento_type in ('10min', '20min'))
);
ALTER TABLE biodiversita.uccelli_monitoraggio
    OWNER to corso_user;
```

- Verifica e correzione dati con SQL
  - Pulizia campo ora
  - Pulizia campi codificati
    - Cielo
    - Vento
    - Distanza
    - Osservazione
  - Riorganizzazione campo 10/20 minuti
  - Consistenza data-plot

```sql
SELECT
  imp_ascolto.plot, imp_ascolto.data, specie,
  n_individui, imp_plot.plot, imp_plot.data
FROM test.imp_ascolto
LEFT JOIN test.imp_plot
ON imp_ascolto.plot = imp_plot.plot
  AND imp_ascolto.data = imp_plot.data
WHERE imp_plot.plot is null
ORDER BY imp_ascolto.plot, imp_ascolto.data;
```

```sql
SELECT
  imp_ascolto.plot, imp_ascolto.data,
  imp_plot.plot, imp_plot.data
FROM test.imp_ascolto
RIGHT JOIN test.imp_plot
ON imp_ascolto.plot = imp_plot.plot
  AND imp_ascolto.data = imp_plot.data
WHERE imp_ascolto.plot is null
ORDER BY imp_plot.plot, imp_plot.data;
```

  - Verifica nome specie

```sql
SELECT specie, count(*) as numero
FROM test.imp_ascolto
GROUP BY specie
ORDER BY specie;
```

```sql
UPDATE test.imp_ascolto
SET
  specie = upper(substring(specie from 1 for 1)) ||
    substring(specie from 2 for length(specie));
```

```sql
UPDATE test.imp_ascolto
SET
  specie = TRIM(specie);
```

```sql
SELECT DISTINCT specie
FROM test.imp_ascolto
WHERE specie NOT IN
  (SELECT animale_code
   FROM biodiversita.biodiversita_animali)
ORDER BY specie;
```

```sql
WITH nuove_specie AS
(SELECT DISTINCT specie
FROM test.imp_ascolto
WHERE specie NOT IN
  (SELECT animale_code
   FROM biodiversita.biodiversita_animali)
ORDER BY specie)

SELECT animale_code, specie, similarity(animale_code, specie)
FROM nuove_specie, biodiversita.biodiversita_animali
WHERE similarity(animale_code, specie) > 0.6;
```

  - Verifica delle note per informazioni aggiuntive

- Importazione dei dati da tabelle temporanee a tabelle definitive

---
#### FINE DEL CORSO
