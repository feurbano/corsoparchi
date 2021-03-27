<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 8
## Comandi SQL per aggiornare il database

Autore: Ferdinando Urbano  

---

### Inserimento di nuovi dati nel database
Ci sono varie opzioni per inserire nuovi dati nella tabella di un database.  
Se in ogni caso bisogna fare data entry, cioè portare le informazioni dalle schede cartacee di campo al formato digitale, **la prima opzione** è quella dell'inserimento manuale record per record direttamente nella tabella finale. Questo avviene sempre tramite interfacce. La più immediata è PgAdmin. In questo caso, come visto nella [lezione 3](https://feurbano.github.io/corsoparchi/lezioni/lezione_03.html), si può aprire la tabella, andare nell'ultima riga, cliccare campo per campo e aggiungere riga per riga tutti i nuovi dati (dando conferma alla fine dell'operazione). Il database farà un controllo dei dati in base alle regole impostate (chiavi primarie, chiavi esterne, check sui campi, tipi di dato) restituendo un errore se queste regole non sono rispettate. L'inserimento dei dati in questo modo non è particolarmente agevole. Un modo per facilitarlo è creare delle maschere di inserimento utilizzando MS Access, LibreOffice Calc o una applicazione web-based (ad esempio sviluppata in linguaggio PhP) come client per creare delle interfacce grafiche dedicate con funzioni specifiche come menù a tendina o verifica immeditata dei valori inseriti (prima che vengano inviati al database). L'inserimento riga per riga può essere fatta anche attraverso il comando `INSERT INTO` con la sintassi `INSERT INTO tabella_destinazione (campo1, campo2, ...) VALUES (valore1, valore2, ...);`.  

**La seconda opzione**, quando i dati vengono raccolti tramite palmare, è di inviare i dati direttamente al database dove possono poi essere verificati da un operatore esperto prima di essere formalmente integrati ai dati "ufficiali" (ad esempio attraverso un campo booleano *validato* che verrà marcato come TRUE dopo verifica, oppure passando attraverso una tabella intermedia prima di venire caricati nella tabella finale). Questa opzione deve essere sviluppata quando si crea l'applicazione su tablet per registrare i dati.  

**La terza opzione** riguarda i dati che sono già disponibili in un foglio di calcolo o in formato .csv (ad esempio prodotto dall'applicazione per la raccolta dati via tablet, se questa non è connessa direttamente al database). In questo caso, si può fare una verifica preliminare della validità dei dati direttamente nei foglio di calcolo (per quanto possibile). I dati devono essere organizzati in una struttura analoga a quella creata nel database (un file .csv per ogni tabella, con stessi campi e stesso formato). A questo punto si può utilizzare l'interfaccia di PgAdmin che offre una funzione di import/export dei dati (selezionare la tabella nel menù ad albero nel pannello di sinistra e cliccare il pulsante destro). L'interfaccia permette di specificare se è una operazione di importazione o di esportazione, il nome del file di destinazione/provenienza, se c'è o menu un header (la prima riga con il nome delle colonne), il separatore usato (virgola, punto e virgola, tabulazione), l'encoding del file di origine/destinazione (importante quando ci sono caratteri accentati) e, infine, la lista dei campi che verranno importati (nel caso dell'importazione, non tutti i campi della tabella del database possono essere presenti nel .csv, in particolare i campi *serial*). Invece di utilizzare l'interfaccia grafica, si possono usare anche il comando SQL `COPY` se il file è sullo stesso server del database, o il comando `/COPY` da lanciare dall'interfaccia *psql* che permette di specificare un file da un computer diverso dal server che ospita il database (ad esempio il computer locale del tecnico che effettua l'operazione di importazione). In questo processo, la parte problematica è la correttezza dei dati. Inserendo i dati nella tabella finale, questi devono essere corretti altrimenti l'operazione non si completerà e il database segnalerà il primo errore trovato che potrà poi essere corretto nel file .csv originale. Se ci sono tanti errori (che possono essere di vario tipo, ad esempio l'uso della virgola invece del punto per i decimali, la presenza di valori non coerenti con il tipo di dato come ad esempio '>5' invece di un intero, valori non presenti nella lista di quelli ammessi per quel campo, coerenza con dati di altre tabelle come la data di una determinazione rispetto alla data del controllo della trappola), le iterazioni diventano molte e il processo diventa lento e complesso da gestire.  

Per ovviare all'inconveniente menzionato sopra, **la quarta opzione** prevede che i dati vengano inseriti in una tabella del database temporanea dove non sono implementati controlli (nessuna chiave esterna o vincoli sui campi, tipi di dato generici come testo anche per valori che dovrebbero essere specifici come date o numeri) e che ha esattamente la stessa struttura del file di origine. Questo garantisce che l'operazione di importazione avvenga senza problemi. I dati possono poi essere controllati e corretti nel database in modo più semplice, specie se le correzione coinvolgono molte righe (ad esempio, correggere il formato di una data, modificare valore scorretti ripetuti, togliere spazi all'inizio o alla fine di una stringa di testo, sostituire nomi di specie scorretti, numero di individui totali non coerente con la somma del numero di maschi + femmine + piccoli + indeterminati, e la lista potrebbe continuare per ore...) utilizzando il comando SQL `UPDATE`. Una volta ripuliti i dati nella tabella temporanea di importazione, questi possono essere caricati nella tabella finale usando il comando `INSERT INTO` in una variante che prevede, invece dell'uso di VALUES più la lista dei valori da inserire, una query di `SELECT` che genera i valori da importare: `INSERT INTO tabella_destinazione (campo1, campo2, ...) SELECT (campo1, campo2, ...) FROM tabella_temporanea;`. Questa quarta opzione è quella che in generale è stata utilizzata, nella maggior parte dei casi, per importare i dati dei Parchi nei rispettivi database.  

Nelle sezioni seguenti vengono mostrati esempi dell'uso dei comandi citati, mentre nella [lezione 9](https://feurbano.github.io/corsoparchi/lezioni/lezione_09.html), in particolare durante la dimostrazione online, verranno mostrati esempi concreti dell'applicazione di queste procedure ai dati del Progetto Biodiversità.  
Alla fine di questa sezione vengono velocemente introdotti

### Il comando INSERT INTO

Inserire utente: Aieie Brazu, nato il 3 dicembre 1987, lavora per la società ACME, pesa 66 chili e non è sposato.
Popolare una tabella con SQL (INSERT INTO and VALUES)

INSERT INTO data.dipendenti(nome, cognome, data_nascita, societa_nome, peso, sposato)
  VALUES ('Gino', 'Pasticcino', '01-05-1978', 'ACME', 73, TRUE);

INSERT INTO data.dipendenti(nome, cognome, data_nascita, societa_nome, peso, sposato)
  VALUES ('Felice', 'Caccamo', '05-09-1951', 'ACME', 93, TRUE);

INSERT INTO data.dipendenti(nome, cognome, data_nascita, societa_nome, peso, sposato)
  VALUES ('Ciccio', 'Pasticcio', '01-04-1972', 'IQ s.p.a.', '79,4', FALSE);

INSERT INTO data.dipendenti(nome, cognome, data_nascita, societa_nome, peso, sposato)
  VALUES ('Grunnio', 'Corocotta', '21-01-1982', 'IQ spa', '65,4', TRUE);

INSERT INTO data.dipendenti(nome, cognome, data_nascita, societa_nome, peso, sposato)
  VALUES ('Ottone', 'Bugusciusciu', '11-12-1994', 'ACME', 67, FALSE);

Popolare una tabella da un file csv

    Esempio con PgAdmin (file dipendenti.csv)

Codice (il percorso del file deve essere adattato):

COPY data.dipendenti (nome, cognome, data_nascita, societa_nome, peso, sposato)
  FROM 'C:\corso_postgres\dipendenti.csv' WITH CSV DELIMITER ';';

    Esempio con SQL (file societa.csv)

Codice (il percorso del file deve essere adattato):

COPY data.societa (societa_nome,settore,num_dipendenti)
  FROM 'C:\corso_postgres\societa.csv' WITH CSV DELIMITER ';';

Esportare i dati in un file esterno

    Con PgAdmin (tabella dipendenti
    Usando SQL

Codice (il percorso del file deve essere adattato):

COPY data.dipendenti
  TO 'C:\corso_postgres\dipendenti_completo.csv' WITH CSV DELIMITER ';' HEADER;

### Inserimento di nuovi dati: COPY
### Inserimento di nuovi dati: /COPY
### Aggiornamento di dati: UPDATE

Se avete molti record da cambiare, è meglio usare una query UPDATE invece di modificare manualmente tutti i record interessati. Per esempio se nel set di dati originale i record NULL sono contrassegnati con "N/A":

UPDATE temp.import_datagroup_dataset_raw
SET età = NULL
WHERE age = 'N/A';


### Cancellazione di dati: DELETE
### Subquery con FROM
### Subquery con WHERE
### Funzioni WINDOW



Per importare una nuova tabella con dati grezzi nel database, si deve creare una tabella temporanea vuota nel database PostgreSQL nello schema temp, dando un nome significativo (ad esempio import_studyarea_dataset_version) e possibilmente aggiungendo un breve commento in modo che queste tabelle possano essere rimosse in seguito. Se preferisci (per esempio perché vuoi lavorare senza connessione Internet), puoi elaborare i dati grezzi in un database locale sul tuo computer, poi eseguire un backup delle tabelle e ripristinarle nello schema temporaneo del database euro*_db (crea uno schema temporaneo sul tuo PostgreSQL locale che corrisponda al nome dello schema). Puoi usare l'utilità di backup/ripristino in PgAdmin, o creare prima la struttura e poi importare i dati (in questo caso, puoi esportare prima come .csv dal tuo db locale, poi importare il csv nel database principale OPPURE puoi fare il backup dei tuoi dati con PgAdmin usando formato Plain e Use Column Insert e Use Insert Commands come opzioni, così puoi poi copiare il backup come SQL standard che popolerà la nuova tabella).
Per importare i dati grezzi in un database (euro*_db o locale), devi creare un file csv (se i proprietari dei dati hanno fornito i dati in un formato diverso, per esempio Excel o MS Access). Se usi "," o ";" come separatore, controlla che nessun testo usi questi caratteri. Se questo è il caso, includi il testo tra virgolette per importarlo correttamente (questa opzione è offerta dagli strumenti di importazione/esportazione). Puoi pulire i valori NULL (se sono contrassegnati da un testo speciale come N/A) una volta che i dati sono importati nel database.
Nel primo caso, l'opzione migliore è quella di replicare la struttura del file originale nel database. Per farlo rapidamente, puoi ottenere l'elenco dei nomi delle colonne dalla prima riga del file csv e usarli per generare l'sql della tabella. Per esempio, è possibile utilizzare il layout

CREATE TABLE temp.import_datagroup_dataset_raw
(
  id seriale,
 [...]
  CONSTRAINT yourtable_pkey PRIMARY KEY (id)
  );


Aggiungere tutte le colonne. Per esempio, se la lista dei nomi delle colonne è (assicuratevi di rimuovere tutti gli spazi nei nomi delle colonne, se presenti):

nome_animale, età, sesso, nota


Nell'editor di testo, puoi sostituire "," con " carattere che varia, \n" (/n è solitamente usato per la linea di ritorno). Poi si può copiare e incollare nel codice SQL per ottenere:

CREATE TABLE temp.import_datagroup_dataset_raw
(
  id seriale,
  nome_animale carattere variabile,
  età carattere variabile,
  sesso carattere variabile,
  nota carattere variabile,
  CONSTRAINT yourtable_pkey PRIMARY KEY (id)
  );


Ora potete eseguire il codice nel database e creare la tabella che siamo pronti a memorizzare i vostri dati grezzi. Nel codice, c'è una colonna aggiuntiva "id". Si tratta di un numero di serie usato come chiave primaria. È possibile che nel dataset originale ci sia una chiave primaria, ma il seriale "id" garantisce che ne esista una. Senza una chiave primaria non è possibile aggiornare una tabella utilizzando un'interfaccia grafica.
Come puoi vedere, il tipo di dati di tutte le colonne è impostato su carattere variabile, anche quando dovrebbe essere qualcos'altro (intero, timestamp e così via). Questo perché con carattere variabile puoi importare tutto, mentre se il tipo di dati è intero e hai alcuni record con testo (per esempio "NULL" o "?"), con intero l'intera importazione si interrompe. È meglio importare tutto e poi controllare nel database se tutti i valori sono corretti.
Prima di tutto, puoi controllare se il tipo di dati è corretto usando un'espressione di cast ("::").
Per esempio:

SELECT date_capture::date FROM temp.yourtable;


Se ci sono record con una data non valida, riceverete un errore da Postgres. Potete quindi elencare tutti i valori esistenti per cercare i potenziali problemi. Per esempio:

SELECT DISTINCT date_capture FROM temp.import_datagroup_dataset_raw ORDER BY date_capture;



Il comando "LIKE" nell'istruzione WHERE è spesso utile per cercare le stringhe (vedere il manuale di postgres).
Quando tutte le colonne sono formattate correttamente puoi caricare nelle tabelle principali fondendole nel tipo di dati corretto (altrimenti, se cerchi di inserire un carattere che varia in un campo numerico o di data, otterrai un messaggio di errore e l'importazione fallirà).

Per importare dati nel database, puoi usare lo strumento di importazione/esportazione in PGAdmin (3 o 4). Qui puoi specificare che il campo "id" non è incluso.
Non puoi usare il comando SQL "copy" perché il percorso del file sarà relativo al server, non alla tua macchina locale. Puoi usarlo se carichi i dati su Gdrive che è specchiato sul server (se conosci il percorso locale, puoi chiedere al supporto). Questa è una buona soluzione anche per file molto grandi (ad esempio i dati dell'accelerometro).
Un'altra opzione è quella di usare il comando "/copy" dall'interfaccia psql (linea di comando, puoi aprirla per esempio da pgadmin/plugins):

\COPY schema.table(list_of columns) FROM 'C:\your_local_folder\file.csv' with CSV HEADER delimiter ';' NULL'';


È simile al comando SQL "copy" ma il file sorgente è sul tuo computer locale. Un'altra opzione è quella di inviare il file e il comando "copia" da eseguire all'amministratore del database e lui lo eseguirà dall'interno del server.

Nota per importare usando psql.exe
Per maggiori informazioni fare riferimento a quanto segue: https://www.postgresql.org/docs/9.1/app-psql.html
In breve, per usare psql nella linea di comando, individuate "psql.exe" e navigate verso di esso in (per esempio) cmd. Di seguito, basato sulle impostazioni predefinite per l'installazione di pgadmin4:
cd C:\Programmi\pgAdmin 4\v4\runtime

Poi lancia, inserendo il tuo nome utente:
psql -h eurodeer2.fmach.it -p 5432 -d eurolynx_db -U <username>
Inserite la password quando richiesto.

Un comando per importare i dati in una data tabella potrebbe essere il seguente (basato sulle colonne di vhf_data_animali):
\COPY temp.vhf_data_animals_UPLOAD (animals_id, vhf_sensors_id, acquisition_time, latitude, longitude, vhf_validity_code, notes) FROM 'C:/path/to/file/to_import.csv' WITH DELIMITER',' NULL'NA' CSV HEADER



Se i dati generati dai sensori (ad esempio i dati GPS) sono forniti in file multipli separati, cioè uno per sensore/animale, potresti voler trovare un modo veloce per importarli nella stessa tabella dove possono essere elaborati insieme. Prima di tutto, devi controllare che tutti i file abbiano le stesse colonne. Se non è così, dovresti provare ad armonizzarli. Poi puoi unirli usando un file batch che li importi nella stessa tabella usando il comando "copy". In alcuni casi, poiché "copy" può essere usato solo sulla macchina locale, è conveniente creare una tabella sulla tua installazione locale di postgres e poi spostarla sul server (per esempio con un dump/restore o con un'esportazione in .csv). Se il file dei dati del sensore non contiene il codice del sensore stesso ed è memorizzato solo nel nome del file, si può recuperare l'elenco dei file .csv usando un comando dos o uno strumento come "Directory Lister", poi si può usare un foglio di calcolo per generare l'insieme delle istruzioni "copy" seguite (una per una) da un aggiornamento che imposta il valore di una colonna aggiuntiva "sensor_code" al nome del sensore ricavato dal nome del file. Questo è solo uno dei tanti trucchi. Si può fare qualcosa di simile usando le funzionalità di R. L'obiettivo è semplicemente quello di accelerare il processo.

Una volta nel database, è possibile eseguire tutti i controlli possibili. Qui sotto ci sono alcune liste di controllo, una per set di dati. Una volta scoperti nuovi problemi, aggiungeteli alla lista. I codici del database per le aree di studio, gli animali e i sensori sono assegnati solo quando i dati sono importati nelle tabelle principali (sono assegnati automaticamente dal database). Durante il processo di importazione e di controllo dei dati, il nome o il codice originale può essere usato e, una volta che tutto è pronto per essere importato, il codice originale può essere sostituito dal codice del database usando il codice originale come chiave di collegamento al codice del database. Questo verrà illustrato nella sezione "Caricamento dei dati nelle tabelle principali".
Si prega di notare che mentre alcuni (limitati) codici SQL sono riportati qui, in generale il repository SQL con il codice per l'elaborazione dei dati è memorizzato nell'account GitHub del progetto (github.com/feurbano/eurodeer_db/tree/master/data_management). Ogni curatore di dati è invitato a contribuire quando viene creato ulteriore codice.




---
[**Lezione 9.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_09.md) Controllo e importazione di un nuovo dataset: dimostrazione pratica - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_09.html)</ins>]
