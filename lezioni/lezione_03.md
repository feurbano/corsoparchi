<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

#### Lezione 3
## USARE E GESTIRE UN DATABASE
---
### Installare PgAdmin
Per interagire con un database PostgreSQL non è necessario installare PostgreSQL, ma è sufficiente installare un client che si collega al database, fisicamente installato su un server del Parco (o nel cloud). PostgreSQL è necessario solo se dovete creare un nuovo database nel vostro computer.

PgAdmin è la più comune interfaccia grafica per l'interrogazione dei dati e la gestione di PostgreSQL. È possibile visualizzare le tabelle, interrogare e scaricare i dati, creare nuove tabelle e view, modificare e inserire dati, gestire gli utenti, fare backup.  
Le versioni fino a PgAdmin4 v4.50 sono basate su browser (il tool viene aperto in Google Chrome, Firefox, Safari o altro). Dalla versione PgAdmin4 v5.0 il tool è stand alone (si apre come programma a se). Le due versioni sono comunque equivalenti.  
PgAdmin è un software open source e ha aggiornamenti molto frequenti. Se si installa PostgreSQL, PgAdmin4 viene installato automaticamente.  
PgAdmin non è l'unico client che si può usare per manipolare i dati e gli oggetti nel database. Alcuni tool per finalità specifiche sono presentati di seguito, invece una alternativa a PgAdmin4 è dBeaver (https://dbeaver.io/download/, versione Community), molto utile anche per generare i diagrammi ER (è il tool utilizzato per creare le immagini riportate in queste pagine).  

Si può scaricare PgAdmin4 da qui:  
[<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_03.html)</ins>]


* <ins>[**Download PgAdmin for WINDOWS**](https://www.pgadmin.org/download/pgadmin-4-windows/)</ins>  
* <ins>[**Download PgAdmin for MAC**](https://www.pgadmin.org/download/pgadmin-4-macos/)</ins>  
Versione consigliata: ...

Quando viene aperto la prima volta, PgAdmin chiede di creare una password. Questa non è la password di accesso ai database, ma solo la password di accesso a PgAdmin (visto che poi PgAdmin salva al suo interno tutte le password di accesso ai database). Potete mettere qualsiasi password facile da ricordare (ad esempio 'postgres').



### Struttura di PgAdmin

L'interfaccia di PgAdmin è organizzata in 5 sezioni principali (vedi immagine). La visualizzazione degli elementi può essere ottimizzata attraverso le opzioni di personalizzazione.  

Le tre parti principali del client pgAdmin4 sono la barra dei menu pgAdmin, il controllo della struttura pgAdmin e il controllo del browser a schede . Ogni parte è utilizzata per eseguire diversi tipi di attività di gestione. L’utente può facilmente creare un nuovo database utente o ruolo e postgres utilizzando il controllo ad albero pgAdmin. Come è possibile creare un nuovo utente e un database con le tabelle sotto quell’utente è mostrato in questo tutorial.

Quando pgAdmin si apre, l'interfaccia presenta una barra dei menu e una finestra divisa in due riquadri: il controllo dell'albero del Browser nel pannello di sinistra e un browser a schede nel pannello di destra.

<p align="center"> <img src="materiale/l03_pgadmin_panels_.png" /></p>

* Descrizione dei pannelli
Il primo pannello (1)
La barra dei menu di pgAdmin fornisce menu a discesa per l'accesso a opzioni, comandi e utilità. La barra dei menu mostra le seguenti selezioni: File, Object, Tools* e Help. Le selezioni possono essere grigie, il che indica che sono disabilitate per l'oggetto attualmente selezionato nel controllo ad albero di pgAdmin.
Usa le opzioni della finestra di dialogo Preferenze per personalizzare il comportamento del client. Per aprire la finestra di dialogo Preferenze, selezionare Preferenze dal menu File. Il pannello sinistro della finestra di dialogo Preferenze mostra un controllo ad albero; ogni nodo del controllo ad albero fornisce l'accesso alle opzioni che sono relative al nodo sotto il quale sono visualizzate.
Usa i campi del pannello Nodi per selezionare i tipi di oggetti che verranno visualizzati nel controllo ad albero Browser:
Il pannello visualizza un elenco di oggetti del database; fai scorrere l'interruttore situato accanto a ciascun oggetto per mostrare o nascondere l'oggetto del database. Quando si interrogano i cataloghi di sistema, è possibile ridurre il numero di tipi di oggetti visualizzati per aumentare la velocità.

Il secondo pannello (2)
La barra degli strumenti di pgAdmin fornisce i pulsanti di scelta rapida per le funzioni utilizzate più di frequente come View Data e lo strumento Query che sono utilizzati più frequentemente in pgAdmin. Questa barra degli strumenti è visibile nel pannello Browser. I pulsanti vengono abilitati/disabilitati in base al nodo del browser selezionato.
Barra degli strumenti pgAdmin
    Usare il pulsante Query Tool per aprire il Query Tool nel contesto del database corrente.
    Usa il pulsante Visualizza dati per visualizzare/modificare i dati memorizzati in una tabella selezionata.
    Usate il pulsante Righe filtrate per accedere al popup Filtro dati per applicare un filtro a un insieme di dati da visualizzare/modificare.
    Usa il pulsante Cerca oggetti per accedere alla finestra di dialogo degli oggetti di ricerca. Ti aiuta a cercare qualsiasi oggetto del database.

Il terzo pannello (3)
Usa il campo sul pannello delle impostazioni della scheda per specificare le proprietà relative alla scheda.
La scheda Properties visualizza le informazioni sull'oggetto selezionato.
La scheda SQL visualizza lo script SQL che ha creato l'oggetto evidenziato e, quando applicabile, un'istruzione SQL (commentata) che elimina l'oggetto selezionato. Puoi copiare le istruzioni SQL nell'editor di tua scelta usando le scorciatoie taglia e incolla.
I grafici sulla scheda Dashboard forniscono un'analisi attiva delle statistiche di utilizzo per il server o il database selezionato.
La scheda Statistics visualizza le statistiche raccolte per ogni oggetto sul controllo ad albero;
La scheda Dipendenze visualizza gli oggetti da cui dipende l'oggetto attualmente selezionato.
La scheda Dipendenze visualizza una tabella di oggetti che dipendono dall'oggetto attualmente selezionato nel browser pgAdmin.
Altre schede si aprono quando si accede alle funzionalità estese offerte dagli strumenti di pgAdmin (come lo strumento Query, il Debugger o l'editor SQL). Usa l'icona di chiusura (X) situata nell'angolo superiore destro di ogni scheda per chiudere la scheda quando hai finito di usare lo strumento. Come le schede permanenti, queste schede possono essere riposizionate nella finestra client di pgAdmin.

Il quarto pannello (4)
The left pane of the main window displays a tree control (the pgAdmin tree control) that provides access to the objects that reside on a server.
Il controllo ad albero fornisce un'elegante panoramica dei server gestiti e degli oggetti che risiedono su ogni server. Fai clic con il tasto destro del mouse su un nodo all'interno del controllo ad albero per accedere a menu sensibili al contesto che forniscono un rapido accesso alle attività di gestione per l'oggetto selezionato.
Il pannello sinistro della finestra principale mostra un controllo ad albero (il controllo ad albero di pgAdmin) che fornisce l'accesso agli oggetti che risiedono su un server.
Puoi espandere i nodi nel controllo ad albero per visualizzare gli oggetti del database che risiedono su un server selezionato.

Il quinto pannello (5)
Il browser a schede fornisce un rapido accesso alle informazioni statistiche su ogni oggetto nel controllo ad albero e agli strumenti e alle utilità di pgAdmin (come lo strumento Query e il debugger). pgAdmin apre ulteriori schede di funzionalità ogni volta che si accede alle funzionalità estese offerte dagli strumenti di pgAdmin; è possibile aprire, chiudere e riorganizzare le schede di funzionalità secondo necessità.

* Opzioni di visualizzazione
Usa il dialogo Preferenze per personalizzare il contenuto e il comportamento del display di pgAdmin. Per aprire la finestra delle Preferenze, seleziona Preferenze dal menu File.
I pulsanti di aiuto nell'angolo in basso a sinistra di ogni finestra di dialogo apriranno la guida in linea per la finestra stessa. Puoi accedere ad un aiuto aggiuntivo di Postgres navigando attraverso il menu Help, e selezionando il nome della risorsa che vuoi aprire.

Per impostazione predefinita, ogni volta che si apre uno strumento, pgAdmin aprirà una nuova scheda del browser. Puoi controllare questo comportamento modificando il nodo Display della finestra di dialogo Preferenze per ogni strumento. Per aprire la finestra di dialogo delle preferenze, selezionare Preferenze dal menu File.
    Utilizzare il campo Segnaposto del titolo della scheda Debugger per personalizzare il titolo della scheda Debugger.
    Quando la dimensione dinamica della scheda è impostata su True, le schede assumeranno la dimensione completa come da titolo, ciò sarà applicabile anche alle schede già aperte
    Quando l'opzione Apri in una nuova scheda del browser è selezionata per Query tool, Schema Diff o Debugger, si aprirà in una nuova scheda del browser quando richiamata.
    Usa il campo Segnaposto del titolo della scheda dello strumento Query per personalizzare il titolo della scheda dello strumento Query.
    Usa il campo segnaposto del titolo della scheda Visualizza/Modifica per personalizzare il titolo della scheda Visualizza/Modifica dati.

* Aggiunta di un nuovo server  
  Nome: database test  
  Host: localhost  
  Porta: 5432  
  Utente: postgres  
  Password: -  
* Descrizione dei principali elementi visualizzati all'interno di un database (server, database, schema, tabelle)  
  schema  
  tabelle  
  campi  
  vincoli  
  viste
  sequenze
  utenti e gruppi di utenti  
* Interagire con una tabella
  visualizzare tabella
  modificare un valore
  cancellare un valore
  selezionare i dati
  esportare i dati
  visualizzare i dati spaziali
  visualizzare ed esportare una view
* Interfaccia SQL  
  semplice query
  aprire più tab
  chiudere un tab




---
[**Lezione 4.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_04.md) Comandi SQL base - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_04.html)</ins>]
