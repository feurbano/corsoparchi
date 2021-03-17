<p align="center"> <img src="materiale/loghi.png" width="315" height="100"> </p>

#### Lezione 1
## INTRODUZIONE ALLA GESTIONE DEI DATI
---
Materiale in formato [<ins>[**PRESENTAZIONE**](https://docs.google.com/presentation/d/1XyWKH8GZxVzKnz5JL4B-3uMMULKOEaAzruW7Ps_FQeI/edit?usp=sharing)</ins>]  
Autore: Ferdinando Urbano  

### SOMMARIO

* Problemi tipici nella gestione dei dati
* Requisiti per la gestione dei dati
* Opzioni per l'archiviazione dei dati
* Principali caratteristiche dei database relazionali
* Piano per la gestione dei dati
* Perché i Parchi stanno creando un database

### Problemi tipici nella gestione dei dati

* Difficoltà ad usare i dati per scopi specifici
* Qualità dei dati non verificata con errori non corretti
* Dati poco utilizzabili/accessibili dopo molto tempo
* Dati archiviati in più versioni
* Più persone lavorano su versioni diverse
* Documentazione comprensibile solo a chi l’ha fatta
* Complessità nella struttura dei file/dati

### Requisiti per la gestione dei dati

* I dati devono essere completi
* I valori devono essere classi/range validi
* I valori devono essere corretti
* Le informazioni in un data set non si devono contraddire
* Ci deve essere una sola versione dei dati
* I dati/protocolli/metodi devono essere documentati
* I dati devono essere accessibili
* L’accesso ai dati deve essere protetto
* Si deve poter estrarre i dati in base alle esigenze
* Più persone devono poter lavorare ai dati contemporaneamente
* I dati devono essere conservati per lunghi periodi
* I dati da gestire sono vari: tabelle, spaziali, temporali, …
* La gestione dei dati deve richiedere risorse ragionevoli
* La gestione dei dati deve essere fatta con risorse disponibili
* I dati sono usati con tool diversi: stats, report, GIS, …

### Opzioni per l'archiviazione dei dati

* CSV file e documentazione (e.g. csv e documenti di testo)
* Fogli di calcolo (e.g. MS Excel, OO Calc)
* Database locali (e.g. MS Access)
* Archivi web condivisi (e.g. GBIF, Ornitho, Movebank)
* Database centralizzati (e.g. PostgreSQL, Oracle, MySQL)

Quando i dati vengono archiviati in un database centralizzato, ci sono varie opzioni per inserire i dati nel database, ad esempio:  
* Tablet con applicazioni apposite collegate al DB (dati da validare)  
* Adattamento di applicazioni esistenti (e.g. Ornitho, ODK)  
* Tablet, scaricamento dei file, validazione e upload nel DB  
* Utilizzo di interfacce web per l’inserimento dei dati dalle schede  
* Fogli di calcolo non strutturati per inserimento da schede e upload nel DB  
* Fogli di calcolo strutturati per inserimento da schede e poi upload nel DB  

In ogni caso la definizione dei protocolli di raccolta, la registrazione dei dati, la struttura dati nel database e la procedura di informatizzazione devono essere coordinate e pensate come fasi di uno stesso processo.

Per i dati storici, tipicamente archiviati in fogli di calcolo non strutturati, le fasi di lavoro per integrarli in un database sono:  
1. Validazione dei dati (fatta dal curatore dei dati per il controllo della qualità assieme a chi ha raccolto i dati)  
2. Creazione della struttura dati nel database (fatta dal gestore del database)  
3. L’importazione dei dati nel database (fatta dal curatore dei dati)  
4. La documentazione dei dati (campi, struttura, protocollo di raccolta)  

### Piano per la gestione dei dati: cos’è

* Un Data Management Plan è un documento da preparare all’inizio di un progetto (e poi da aggiornare) che descrive come devono essere gestiti i dati durante e dopo la loro raccolta.
* Fornisce una descrizione dei dati, dei protocolli di raccolta, degli standard usati, delle politiche di accesso, della conservazione di dati, e dei responsabili delle varie attività.
* Garantisce che i dati siano nel formato corretto, ben organizzati e meglio annotati.
* I dati potranno essere compresi anche da chi non li ha raccolti e che possano essere utilizzati anche in futuro.
* I dati possono essere formattati durante la raccolta per rendere più facile l’inserimento in un database.

### Piano per la gestione dei dati: elementi chiave

* Per garantire che i dati siano di qualità, preservati e utilizzabili, la gestione dei dati deve essere una delle attività integranti delle attività dei Parchi
* Acquisizione dei dati
* Controllo di qualità dei dati
* Archiviazione dei dati
* Aggiornamento dei dati
* Accessibilità dei dati
* Responsabile dei dati
* Costo per la gestione dei dati

### Piano per la gestione dei dati: ciclo di vita dei dati

* Pianificare: definizione dei dati che saranno raccolti e come saranno gestiti
* Raccogliere: osservazioni di operatori o sensori e digitalizzazione
* QA/QC: la qualità dei dati assicurata con controlli e strutturata nel formato/strumento adeguato
* Analizzare: i dati vengono analizzati/utilizzati (output scientifici o gestionali)
* Descrivere: i dati sono documentati utilizzando gli appropriati standard per metadati
* Conservare: i dati sono presentati a un appropriato archivio a lungo termine
* Scoprire: i dati vengono trovati e acquisiti, insieme alle informazioni rilevanti sui dati (metadati)
* Integrare: i dati provenienti da fonti diverse sono combinati per essere facilmente analizzati
* Analizzare: i dati vengono analizzati/utilizzati

### Principali caratteristiche dei database relazionali

1. I tipi di dato sono definiti in modo esplicito
2. I valori ammessi dei dati possono essere ristretti
3. Gli oggetti e le loro relazioni sono formalizzati
4. Non c’è ridondanza dei dati (normalizzazione)
5. Si può accedere al database da remoto
6. Viene gestita la concorrenza di accessi
7. Si possono assegnare livelli di accesso diversi
8. Si possono fare backup automatici
9. Si possono archiviare enormi quantità di dati
10. Possibilità di interrogare i dati con SQL
11. I dati vengono processati velocemente
12. Il sistema è centralizzato (richiede un server)
13. Ha una struttura server/client (piattaforma modulare)
14. Si possono utilizzare i dati con qualsiasi software
15. I dati sono archiviati in modo sicuro
16. Standard industriali per i formati dei dati
17. Interoperabilità tra diversi database
18. Tecnologia consolidata
19. Gestione dei dati spaziali
20. Gestione dei dati temporali
21. Necessarie conoscenze di base per utilizzo
22. Necessarie conoscenze avanzate per gestione
23. Garantisce l’integrità dei dati
24. Si prevengono errori di inserimento
25. Si hanno solo informazioni "pulite"
26. Si formalizza l'informazione
27. Aumenta i livelli di sicurezza
28. I dati non vengono corrotti per errori degli operatori
29. I dati non possono essere usati da non autorizzati
30. Permette il riutilizzo dei dati
31. Uso dei dati per diverse applicazioni
32. Uso dei dati sul lungo termine
33. Integrazione dei dati con altri data sets
34. Documentazione dei dati
35. Previene la duplicazione dei dati e migliora accesso
36. I dati possono essere usati da più persone insieme
37. I dati possono essere usati da dovunque
38. I dati possono essere facilmente condivisi
39. Si possono gestire dataset grandi e complessi
40. Grande capienza di storage
41. Modelli di dati con relazioni formali complesse
42. Performance nell'uso dei dati

### Perché i Parchi stanno creando un database

* Rendere fruibili i dati raccolti (adesso e in passato)
* Preservare i dati sul lungo periodo
* Verificare e migliorare la qualità dei dati
* Armonizzare i dati raccolti all’interno del parco
* Conservati in un unico archivio
* Rendere più efficiente la gestione dei dati
* Facilitare la connessione con altri progetti/network/istituzioni
* Semplificare l’eventuale integrazione con dati di altre fonti (remote sensing, modelli climatici, etc)
* PERCHÉ DI FATTO NON CI SONO ALTERNATIVE (sul medio e lungo periodo) compatibili con il mandato dei Parchi

### Progetto Biodiversità

* I dati del Progetto Biodiversità sono stati tutti integrati nei database dei rispettivi Parchi
* La struttura dei data set nei vari database è consistente
* I dati sono stati armonizzati
* I dati hanno avuto simili processi di verifica della qualità
* La coerenza dei protocolli di raccolta dati fra Parchi è stata verificata
* Ottimizzazione delle procedure di registrazione dei dati di campo
* I database dei parchi sono separati ma interoperabili
* I dati sono condivisi in un database “virtuale” comune legato ai database dei singoli Parchi
* Esperienza pilota per future collaborazioni anche con altri parchi  

---
[**Lezione 2.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_02.md) Introduzione ai database - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_02.html)</ins>] [<ins>[**Link presentazione**](https://docs.google.com/presentation/d/1c5SVeZIgyzI1XVzP-DYiVm4xGygObjy3FZR4bRpEIQY/edit?usp=sharing)</ins>]
