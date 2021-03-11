# Lezione 1. Gestione dei dati: problemi e soluzioni

Materiale in formato [<ins>[**PRESENTAZIONE**](https://docs.google.com/presentation/d/1XyWKH8GZxVzKnz5JL4B-3uMMULKOEaAzruW7Ps_FQeI/edit?usp=sharing)</ins>]  

### SOMMARIO

* Alcuni problemi tipici nella gestione dei dati
* Requisiti per la gestione dei dati
* Opzioni per l'archiviazione dei dati
* Principali caratteristiche dei database relazionali
* Piano per la gestione dei dati
* Perché i Parchi stanno creando un database

### 1. Alcuni problemi tipici nella gestione dei dati

* Difficoltà ad usare i dati per scopi specifici
* Qualità dei dati non verificata con errori non corretti
* Dati poco utilizzabili/accessibili dopo molto tempo
* Dati archiviati in più versioni
* Più persone lavorano su versioni diverse
* Documentazione comprensibile solo a chi l’ha fatta
* Complessità nella struttura dei file/dati

### 2.1 Requisiti per la gestione dei dati

* I dati devono essere completi
* I valori devono essere classi/range validi
* I valori devono essere corretti
* Le informazioni in un data set non si devono contraddire
* Ci deve essere una sola versione dei dati
* I dati/protocolli/metodi devono essere documentati
* I dati devono essere accessibili
* L’accesso ai dati deve essere protetto

### 2.2 Requisiti per la gestione dei dati

* Si deve poter estrarre i dati in base alle esigenze
* Più persone devono poter lavorare ai dati contemporaneamente
* I dati devono essere conservati per lunghi periodi
* I dati da gestire sono vari: tabelle, spaziali, temporali, …
* La gestione dei dati deve richiedere risorse ragionevoli
* La gestione dei dati deve essere fatta con risorse disponibili
* I dati sono usati con tool diversi: stats, report, GIS, …

### 3. Opzioni per l'archiviazione dei dati

* CSV file e documentazione (e.g. csv e documenti di testo)
* Fogli di calcolo (e.g. MS Excel, OO Calc)
* Database locali (e.g. MS Access)
* Archivi web condivisi (e.g. GBIF, Ornitho, Movebank)
* Database centralizzati (e.g. PostgreSQL, Oracle, MySQL)

### 4.1 Piano per la gestione dei dati: cos’è

* Un Data Management Plan è un documento da preparare all’inizio di un progetto (e poi da aggiornare) che descrive come devono essere gestiti i dati durante e dopo la loro raccolta.
* Fornisce una descrizione dei dati, dei protocolli di raccolta, degli standard usati, delle politiche di accesso, della conservazione di dati, e dei responsabili delle varie attività.
* Garantisce che i dati siano nel formato corretto, ben organizzati e meglio annotati.
* I dati potranno essere compresi anche da chi non li ha raccolti e che possano essere utilizzati anche in futuro.
* I dati possono essere formattati durante la raccolta per rendere più facile l’inserimento in un database.

### 4.2 Piano per la gestione dei dati: elementi chiave

* Per garantire che i dati siano di qualità, preservati e utilizzabili, la gestione dei dati deve essere una delle attività integranti delle attività dei Parchi
* Acquisizione dei dati
* Controllo di qualità dei dati
* Archiviazione dei dati
* Aggiornamento dei dati
* Accessibilità dei dati
* Responsabile dei dati
* Costo per la gestione dei dati

### 4.3 Piano per la gestione dei dati: ciclo di vita dei dati

* Pianificare: definizione dei dati che saranno raccolti e come saranno gestiti
* Raccogliere: osservazioni di operatori o sensori e digitalizzazione
* QA/QC: la qualità dei dati assicurata con controlli e strutturata nel formato/strumento adeguato
* Analizzare: i dati vengono analizzati/utilizzati (output scientifici o gestionali)
* Descrivere: i dati sono documentati utilizzando gli appropriati standard per metadati
* Conservare: i dati sono presentati a un appropriato archivio a lungo termine
* Scoprire: i dati vengono trovati e acquisiti, insieme alle informazioni rilevanti sui dati (metadati)
* Integrare: i dati provenienti da fonti diverse sono combinati per essere facilmente analizzati
* Analizzare: i dati vengono analizzati/utilizzati

### 5.1 Principali caratteristiche dei database relazionali

* I tipi di dato sono definiti in modo esplicito
* I valori ammessi dei dati possono essere ristretti
* Gli oggetti e le loro relazioni sono formalizzati
* Non c’è ridondanza dei dati (normalizzazione)
* Si può accedere al database da remoto
* Viene gestita la concorrenza di accessi

### 5.2 Principali caratteristiche dei database relazionali

* Si possono assegnare livelli di accesso diversi
* Si possono fare backup automatici
* Si possono archiviare enormi quantità di dati
* Possibilità di interrogare i dati con SQL
* I dati vengono processati velocemente

### 5.3 Principali caratteristiche dei database relazionali

* Il sistema è centralizzato (richiede un server)
* Ha una struttura server/client (piattaforma modulare)
* Si possono utilizzare i dati con qualsiasi software
* I dati sono archiviati in modo sicuro
* Standard industriali per i formati dei dati
* Interoperabilità tra diversi database

### 5.4 Principali caratteristiche dei database relazionali

* Tecnologia consolidata
* Gestione dei dati spaziali
* Gestione dei dati temporali
* Necessarie conoscenze di base per utilizzo
* Necessarie conoscenze avanzate per gestione

### 5.5 Principali caratteristiche dei database relazionali

* Garantisce l’integrità dei dati
* Si prevengono errori di inserimento
* Si hanno solo informazioni "pulite"
* Si formalizza l'informazione
* Aumenta i livelli di sicurezza
* I dati non vengono corrotti per errori degli operatori
* I dati non possono essere usati da non autorizzati
* Permette il riutilizzo dei dati
* Uso dei dati per diverse applicazioni
* Uso dei dati sul lungo termine
* Integrazione dei dati con altri data sets
* Documentazione dei dati

### 5.6 Principali caratteristiche dei database relazionali

* Previene la duplicazione dei dati e migliora accesso
* I dati possono essere usati da più persone insieme
* I dati possono essere usati da dovunque
* I dati possono essere facilmente condivisi
* Si possono gestire dataset grandi e complessi
* Grande capienza di storage
* Modelli di dati con relazioni formali complesse
* Performance nell'uso dei dati

### 6.1 Perché i Parchi stanno creando un database

* Rendere fruibili i dati raccolti (adesso e in passato)
* Preservare i dati sul lungo periodo
* Verificare e migliorare la qualità dei dati
* Armonizzare i dati raccolti all’interno del parco
* Conservati in un unico archivio
* Rendere più efficiente la gestione dei dati
* Facilitare la connessione con altri progetti/network/istituzioni
* Semplificare l’eventuale integrazione con dati di altre fonti (remote sensing, modelli climatici, etc)
* PERCHÉ DI FATTO NON CI SONO ALTERNATIVE (sul medio e lungo periodo) compatibili con il mandato dei Parchi

### 6.2 Progetto Biodiversità

* I dati del Progetto Biodiversità sono stati tutti integrati nei database dei rispettivi Parchi
* La struttura dei data set nei vari database è consistente
* I dati sono stati armonizzati
* I dati hanno avuto simili processi di verifica della qualità
* La coerenza dei protocolli di raccolta dati fra Parchi è stata verificata
* Ottimizzazione delle procedure di registrazione dei dati di campo
* I database dei parchi sono separati ma interoperabili
* I dati sono condivisi in un database “virtuale” comune legato ai database dei singoli Parchi
* Esperienza pilota per future collaborazioni anche con altri parchi
