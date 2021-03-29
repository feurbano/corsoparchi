<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 9
## CONTROLLO E IMPORTAZIONE DI UN NUOVO DATASET: DIMOSTRAZIONE PRATICA

Autore: Ferdinando Urbano  

---

In questa ultima lezione viene data una dimostrazione pratica di alcuni dei passaggi tipici del processo di controllo e importazione di informazioni disponibili in formato foglio di calcolo.  
Come esempio di riferimento vengono utilizzati i dati raccolti sugli uccelli nell'ambito del Progetto Biodiversità dal Parco Nazionale delle Dolomiti Bellunesi. I file messi a disposizione del Parco hanno una struttura abbastanza semplice ideale per mostrare la procedura senza problemi tecnici eccessivi e presentano, sebbene in misura abbastanza limitata, molti dei problemi che si incontrano nella verifica di qualità dei dati.  
Questa lezione viene presentata interamente online e in questa pagina si riporta solo una lista dei principali passi che vengono illustrati nella dimostrazione. Per le spiegazioni complete si deve vedere la registrazione della video lezione.

### ESEMPIO DI RIFERMENTO: FILE BIODIVERSITÀ UCCELLI PNDB 2013-2014-2018-2019
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
  - Operartore mancante
  - ...

- Creare tabelle di importazione
  - Lista campi dai csv
  - Creazione chiave primaria
  - Definizione dei tipi di dati
  - SQL creazione tabella
  - Creazione delle tabelle PLOT e ASCOLTO nello schema TEMP

- Importazione dei dati dal CSV
  - 1 da interfaccia
  - 1 da riga di comando

- Verifica conformità struttura degli altri Parchi

- Verifica e correzione dati con SQL
  -
  -

- Utilizzo codici corretti
	-

- Verifica delle note per informazioni aggiuntive
	-

---

#### FINE DEL CORSO
