<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

#### Lezione 8
## COMANDI SQL PER AGGIORNARE IL DATABASE

Autore: Ferdinando Urbano  

---

### Inserimento di nuovi dati nel database
Ci sono varie opzioni per inserire nuovi dati nella tabella di un database.  

Se in ogni caso bisogna fare data entry, cioè portare le informazioni dalle schede cartacee di campo al formato digitale, **la prima opzione** è quella dell'inserimento manuale record per record direttamente nella tabella finale. Questo avviene sempre tramite interfacce. La più immediata è PgAdmin. In questo caso, come visto nella <ins>[lezione 3](https://feurbano.github.io/corsoparchi/lezioni/lezione_03.html)</ins>, si può aprire la tabella, andare nell'ultima riga, cliccare campo per campo e aggiungere riga per riga tutti i nuovi dati (dando conferma alla fine dell'operazione). Il database farà un controllo dei dati in base alle regole impostate (chiavi primarie, chiavi esterne, check sui campi, tipi di dato) restituendo un errore se queste regole non sono rispettate. L'inserimento dei dati in questo modo non è particolarmente agevole. Un modo per facilitarlo è creare delle maschere di inserimento utilizzando MS Access, LibreOffice Calc o una applicazione web-based (ad esempio sviluppata in linguaggio PhP) come client per creare delle interfacce grafiche dedicate con funzioni specifiche come menù a tendina o verifica immeditata dei valori inseriti (prima che vengano inviati al database). L'inserimento riga per riga può essere fatta anche attraverso il comando `INSERT INTO` con la sintassi `INSERT INTO tabella_destinazione (campo1, campo2, ...) VALUES (valore1, valore2, ...);`.  

**La seconda opzione**, quando i dati vengono raccolti tramite palmare, è di inviare i dati direttamente al database dove possono poi essere verificati da un operatore esperto prima di essere formalmente integrati ai dati "ufficiali" (ad esempio attraverso un campo booleano *validato* che verrà marcato come TRUE dopo verifica, oppure passando attraverso una tabella intermedia prima di venire caricati nella tabella finale). Questa opzione deve essere sviluppata quando si crea l'applicazione su tablet per registrare i dati.  

**La terza opzione** riguarda i dati che sono già disponibili in un foglio di calcolo o in formato .csv (ad esempio prodotto dall'applicazione per la raccolta dati via tablet, se questa non è connessa direttamente al database). In questo caso, si può fare una verifica preliminare della validità dei dati direttamente nei foglio di calcolo (per quanto possibile). I dati devono essere organizzati in una struttura analoga a quella creata nel database (un file .csv per ogni tabella, con stessi campi e stesso formato). A questo punto si può utilizzare l'interfaccia di PgAdmin che offre una funzione di import/export dei dati (selezionare la tabella nel menù ad albero nel pannello di sinistra e cliccare il pulsante destro). L'interfaccia permette di specificare se è una operazione di importazione o di esportazione, il nome del file di destinazione/provenienza, se c'è o menu un header (la prima riga con il nome delle colonne), il separatore usato (virgola, punto e virgola, tabulazione), l'encoding del file di origine/destinazione (importante quando ci sono caratteri accentati) e, infine, la lista dei campi che verranno importati (nel caso dell'importazione, non tutti i campi della tabella del database possono essere presenti nel .csv, in particolare i campi *serial*). Invece di utilizzare l'interfaccia grafica, si possono usare anche il comando SQL `COPY` se il file è sullo stesso server del database, o il comando `/COPY` da lanciare dall'interfaccia *psql* che permette di specificare un file da un computer diverso dal server che ospita il database (ad esempio il computer locale del tecnico che effettua l'operazione di importazione). In questo processo, la parte problematica è la correttezza dei dati. Inserendo i dati nella tabella finale, questi devono essere corretti altrimenti l'operazione non si completerà e il database segnalerà il primo errore trovato che potrà poi essere corretto nel file .csv originale. Se ci sono tanti errori (che possono essere di vario tipo, ad esempio l'uso della virgola invece del punto per i decimali, la presenza di valori non coerenti con il tipo di dato come ad esempio '>5' invece di un intero, valori non presenti nella lista di quelli ammessi per quel campo, coerenza con dati di altre tabelle come la data di una determinazione rispetto alla data del controllo della trappola), le iterazioni diventano molte e il processo diventa lento e complesso da gestire.  

Per ovviare all'inconveniente menzionato sopra, **la quarta opzione** prevede che i dati vengano inseriti in una tabella del database temporanea dove non sono implementati controlli (nessuna chiave esterna o vincoli sui campi, tipi di dato generici come testo anche per valori che dovrebbero essere specifici come date o numeri) e che ha esattamente la stessa struttura del file di origine. Questo garantisce che l'operazione di importazione avvenga senza problemi. I dati possono poi essere controllati e corretti nel database in modo più semplice, specie se le correzioni coinvolgono molte righe (ad esempio, correggere il formato di una data, modificare valore scorretti ripetuti, togliere spazi all'inizio o alla fine di una stringa di testo, sostituire nomi di specie scorretti, numero di individui totali non coerente con la somma del numero di maschi + femmine + piccoli + indeterminati, e la lista potrebbe continuare per ore...) utilizzando il comando SQL `UPDATE`. Una volta ripuliti i dati nella tabella temporanea di importazione, questi possono essere caricati nella tabella finale usando il comando `INSERT INTO` in una variante che prevede, invece dell'uso di VALUES più la lista dei valori da inserire, una query di `SELECT` che genera i valori da importare: `INSERT INTO tabella_destinazione (campo1, campo2, ...) SELECT (campo1, campo2, ...) FROM tabella_temporanea;`. Questa quarta opzione è quella che in generale è stata utilizzata, nella maggior parte dei casi, per importare i dati dei Parchi nei rispettivi database.  

Nelle sezioni seguenti vengono mostrati esempi dell'uso dei comandi citati, mentre nella <ins>[lezione 9](https://feurbano.github.io/corsoparchi/lezioni/lezione_09.html)</ins>, in particolare durante la dimostrazione online, verranno mostrati esempi concreti dell'applicazione di queste procedure ai dati del Progetto Biodiversità.  
Alla fine di questa sezione vengono velocemente introdotti

### INSERT INTO
Il comando SQL `INSERT` inserisce nuove righe in una tabella. Si possono inserire una o più righe specificando i valori da inserire oppure utilizzare il risultato di un'altra query. Nella struttura del comando `INSERT INTO tabella_destinazione (campo1, campo2, ...) VALUES (valore1, valore2, ...);` che inserisce i valori specificati dalla query e del suo equivalente `INSERT INTO tabella_destinazione (campo1, campo2, ...) SELECT (campo1, campo2, ...) FROM tabella_temporanea;` che utizza un'altra query per generare i valori da inserire, i nomi delle colonne di destinazione dell'inserimento possono essere elencati in qualsiasi ordine, basta che lo stesso ordine sia rispettato dai valori che vengono inseriti (i nomi dei campi di origine e destinazione non devono essere uguali: i nomi sono ininfluenti, l'unico elemento rilevante è l'ordine ed è in base a quello che il database associa il valore da inserire alla colonna relativa nella tabella di destinazione). Se non viene fornita alcuna lista di nomi di colonna, il default è che si considerano tutte le colonne della tabella nell'ordine in cui compaiono nella tabella di destinazione.  
Non è necessario che venga fornito un valore per tutti i campi della tabella di destinazione: si possono includere solo i campi desiderati (obbligatori sono solo i campi che costituiscono la chiave primaria, a meno che questa non sia un campo `SERIAL` perché in questo caso il database assegnerà un valore autonomamente).  
Se l'espressione per qualsiasi colonna non è del tipo di dati corretto (ad esempio viene fornito un numero invece di un testo), verrà tentata una conversione automatica del tipo che può avere o meno successo (si può sempre convertire un numero a un testo, ma non sempre si può convertire un testo a un numero).  
È necessario avere il privilegio `INSERT` su una tabella per poter inserire dei dati.

Per mostrare un esempio di come funzioni il comando `INSERT` creiamo una tabella vuota che popoleremo poi con i nomi degli animali e indicheremo specie e genere (se volete creare una vostra tabella di test, dovete cambiare il nome della tabella e della chiave primaria, ad esempio sostituendo il vostro nome a *ferdi*, altrimenti otterrete un errore perché c'è già nel database un oggetto con quel nome). Oltre al nome, alla species e al genus, aggiungo un campo *bellezza*. Dopo la dichiarazione della lista dei campi e del loro tipo di dato, aggiungo la chiave primaria (*nome_animale*) e un vincolo sul campo *bellezza* (che deve essere compreso fra 0 e 10).  

```sql
CREATE TABLE test.tabella_di_ferdi
(
    nome_animale character varying ,
    species_nome character varying NOT NULL,
    genus_nome character varying,
    bellezza integer,
    CONSTRAINT tabella_di_ferdi_chiaveprimaria PRIMARY KEY (nome_animale),
    CONSTRAINT bellezza_controllo CHECK (bellezza >= 0 and bellezza <= 10)
);
```

Se necessario posso anche specificare altri utenti che hanno il permesso di vedere e modificare questa tabella:

```sql
GRANT INSERT, SELECT, UPDATE
ON TABLE test.tabella_di_ferdi
TO furbano;
```

Ora posso inserire dati nella tabella vuota. Comincio con un record inserito tramite comando `VALUES`

```sql
INSERT INTO test.tabella_di_ferdi
  (nome_animale, species_nome, genus_nome, bellezza)
VALUES
  ('Uomo', 'Homo sapiens', 'Homo', '7');
```

Inserisco ora due record con il comando `VALUES` ma in questo caso non inserisco il campo *bellezza* e inverto l'ordine di specie e genus:

```sql
INSERT INTO test.tabella_di_ferdi
  (nome_animale, genus_nome, species_nome)
VALUES
  ('Cimice', 'Halyomorpha', 'Halyomorpha halys'),
  ('Beluga', 'Delphinapterus', 'Delphinapterus leucas');
```

Inserisco ora dei record prendendoli dalla tabelle *biodiversita.biodiversita_animali*. Uso la funzione SQL `RANDOM()` che genera un numero casuale fra 0 e 1 per popolare il campo *bellezza*. Per ottenere un numero intero fra 0 e 10, moltiplico per 10 e trasformo in intero, per esempio: `SELECT (RANDOM()*10)::integer;`.  
In questo esempio inserisco nella nuova tabella tutte le specie dalla tabella *biodiversita.biodiversita_animali* che appartengono alla famiglia *Strigidae*.  
Per verificare i dati che verranno inseriti, è sempre utile prima visualizzare il risultato della query di `SELECT` prima di lanciare il comando con `INSERT`.

```sql
INSERT INTO test.tabella_di_ferdi
  (nome_animale, species_nome ,genus_nome, bellezza)  

SELECT
  animale_code, species_name, genus_name, (RANDOM()*10)::integer
FROM
  biodiversita.biodiversita_animali
where
  livello_tassonomico = 'species' AND
  family_name = 'Strigidae';
```

#### Esercizio
> Nella vostra tabella inserire manualmente l'aquila reale, poi con il comando VALUES il cinghiale e infine con il comando SELECT tutti i carabidi importandoli dalla tabella *biodiversita.biodiversita_animali*.

### COPY
Il modo più immediato, ma non sempre più veloce ed efficiente, per importare dati da un file esterno nel database è utilizzare l'interfaccia grafica di PgAdmin che offre questa funzionalità, come mostrato in precedenza. Una alternativa per chi ha dimestichezza con la gestione di PostrgeSQL è l'uso dei comandi `COPY` e `/COPY`.  

Il comando `COPY` serve per trasferire i dati tra le tabelle PostgreSQL e file esterni. In particolare, può essere usato per inserire dati di un file .csv in una tabella di PostgreSQL o per esportare il contenuto di una tabella di Postgres in un file .csv.  
`COPY TO` esporta il contenuto di una tabella (non funziona con le viste) in un file, mentre `COPY FROM` importa i dati da un file a una tabella (aggiungendo i dati a quelli già presenti nella tabella).  
`COPY TO` può anche copiare i risultati di una query SELECT. Se viene specificato un elenco di colonne, `COPY TO` copia solo i dati nelle colonne specificate nel file. Per `COPY FROM`, ogni campo del file viene inserito, in ordine, nella colonna specificata.  
`COPY` con un nome di file indica al server PostgreSQL di leggere o scrivere direttamente su un file. Il file deve essere accessibile dall'utente PostgreSQL e il file deve essere fisicamente archiviato sullo stesso del server del database, quindi di fatto un utente può importare/esportare da/verso un file sul proprio computer solo se il database è installato in locale sullo stesso computer. Questo ne limita molto l'uso pratico.  

### /COPY
Il comando `COPY` non deve essere confuso con l'istruzione psql `\COPY`. `\COPY` invoca *COPY FROM STDIN* o *COPY TO STDOUT*, e poi recupera/memorizza i dati in un file accessibile al client psql. Così, l'accessibilità del file e i diritti di accesso dipendono dal client invece che dal server. In sostanza, con `\COPY` (che NON è un comando SQL e quindi non può essere eseguito da un editor SQL) si possono salvare tabelle dal database al proprio computer o caricare nel database un file .csv che si trova sul proprio computer.  
Quando si importano/esportano dati con l'interfaccia grafica di PgAdmin, il client traduce la richiesta in un comando `\COPY`.  

Per lanciare direttamente il comando `\COPY` è necessario aprire una console <ins>[psql](https://www.postgresql.org/docs/devel/app-psql.html)</ins>. Questo tipo di operazioni verrà presumibilmente effettuata solo dai curatori dei dati con una certa esperienza o dal gestore di database. Per aprire la console psql bisogna individuare sul computer il file "psql.exe" ed eseguirlo. Ad esempio, su Windows e utilizzando il path standard dell'installazione di PgAdmin4 si può digitare in una console DOS:  
```
"C:\Program Files\pgAdmin 4\v4\runtime\psql.exe"
-U corso_user -h db.parco.gran-paradiso.g3wsuite.it
-p 2345 -d corso_parchi
```  
e inserire poi la password. A questo punto si possono esportare i dati a un file locale, come in questo esempio:  

```
\COPY test.tabella_di_ferdi
TO 'C:\temp\copia_tabella.csv'
WITH CSV DELIMITER ';' HEADER;
```

In modo simile è possibile importare una tabella usando il comando:

```
\COPY test.tabella_di_ferdi
  (nome_animale, species_nome ,genus_nome, bellezza)
FROM
  'C:\temp\tabella_da_importare.csv' WITH DELIMITER ',' CSV HEADER
```

In ogni manuale online sarà poi possibile verificare le diverse opzioni di importazione/esportazione da/per un file .csv, in particolare in relazione al simbolo usato come separatore, al valore utilizzato per indicare NULL, alla presenza dell'header (riga di intestazione) e all'encoding del file.

### UPDATE
Se c'è la necessità di cambiare molti record ed è possibile usare una regola (ad esempio eliminare gli spazi all'inizio e alla fine di una stringa o settare come maiuscola solo la prima lettera di una parola) è meglio usare una query `UPDATE` invece di modificare manualmente i record interessati. In un comando `UPDATE` bisogna indicare la tabella di origine e poi tramite il comando `SET` specificare quali campi vanno modificati e con quale valore. Si possono indicare delle condizioni `WHERE` per limitare l'aggiornamento solo ad alcune righe.  

Per fare un esempio prima inseriamo alcuni record nella nostra tabella con evidenti errori (in particolare, gli spazi prima e dopo il nome dell'animale, non sempre evidenti quando si visualizza una tabella):

```sql
INSERT INTO test.tabella_di_ferdi
  (nome_animale, species_nome, genus_nome, bellezza)
VALUES
  (' Giraffa ', 'Giraffa camelopardalisss', 'Giraffa', 10),
  ('Zebra ', 'Equus quagga', 'Equus', 9);
```

Se si prova a visualizzare il record corrispondente al nome *Giraffa* il risultato è nullo perché la stringa è scritta in modo diverso e quindi non corrisponde a quanto richiesto:
```sql
SELECT * FROM test.tabella_di_ferdi
WHERE nome_animale = 'Giraffa';
```

Utilizzando la funzione SQL `TRIM()` si possono togliere tutti gli spazi all'inizio e alla fine di una stringa (ma non al suo interno) grazie al comando `UPDATE`:

```sql
UPDATE test.tabella_di_ferdi
SET nome_animale = TRIM(nome_animale);
```

A questo punto la query preedente dovrebbe restituire la riga corretta.  
Per correggere invece la parola *camelopardalisss* scritta in modo scorretto si può ancora utilizzare `UPDATE` specificando con `WHERE` le righe a cui si vuole applicare il cambio, come illustrato nel codice:  

```sql
UPDATE test.tabella_di_ferdi
SET nome_animale = 'Giraffa camelopardalis'
WHERE nome_animale = 'Giraffa camelopardalisss';
```

### Cancellazione di dati: DELETE
Il comando `DELETE` di PostgreSQL permette di cancellare una o più righe da una tabella (per eliminare completamente la tabella dal database bisogna invece usare il comando `DROP TABLE`).  
La seguente mostra la sintassi di base dell'istruzione DELETE: `DELETE FROM table_name WHERE condizione;`.  
Per prima cosa, si specifica il nome della tabella da cui volete cancellare i dati dopo le parole chiave `DELETE FROM`. Poi si usa una condizione nella sezione `WHERE` per specificare quali righe della tabella eliminare. La clausola `WHERE` è opzionale. Se si omette la clausola `WHERE`, l'istruzione `DELETE` cancella tutte le righe della tabella.  
In questo esempio vengono eliminate tutte le righe che come genere hanno 'Equus':

```sql
DELETE FROM test.tabella_di_ferdi
WHERE genus_nome = 'Equus';
```

---
[**Lezione 9.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_09.md) Controllo e importazione di un nuovo dataset: dimostrazione pratica - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_09.html)</ins>]
