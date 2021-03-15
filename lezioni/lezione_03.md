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

Il client di pgAdmin 4 è dotato di un display altamente personalizzabile con pannelli drag-and-drop che puoi organizzare per fare il miglior uso del tuo ambiente desktop.

Il controllo ad albero fornisce un'elegante panoramica dei server gestiti e degli oggetti che risiedono su ogni server. Fai clic con il tasto destro del mouse su un nodo all'interno del controllo ad albero per accedere a menu sensibili al contesto che forniscono un rapido accesso alle attività di gestione per l'oggetto selezionato.

Il browser a schede fornisce un rapido accesso alle informazioni statistiche su ogni oggetto nel controllo ad albero e agli strumenti e alle utilità di pgAdmin (come lo strumento Query e il debugger). pgAdmin apre ulteriori schede di funzionalità ogni volta che si accede alle funzionalità estese offerte dagli strumenti di pgAdmin; è possibile aprire, chiudere e riorganizzare le schede di funzionalità secondo necessità.

Usa il dialogo Preferenze per personalizzare il contenuto e il comportamento del display di pgAdmin. Per aprire la finestra delle Preferenze, seleziona Preferenze dal menu File.

I pulsanti di aiuto nell'angolo in basso a sinistra di ogni finestra di dialogo apriranno la guida in linea per la finestra stessa. Puoi accedere ad un aiuto aggiuntivo di Postgres navigando attraverso il menu Help, e selezionando il nome della risorsa che vuoi aprire.

Tradotto con www.DeepL.com/Translator (versione gratuita)

Le tre parti principali del client pgAdmin4 sono la barra dei menu pgAdmin, il controllo della struttura pgAdmin e il controllo del browser a schede . Ogni parte è utilizzata per eseguire diversi tipi di attività di gestione. L’utente può facilmente creare un nuovo database utente o ruolo e postgres utilizzando il controllo ad albero pgAdmin. Come è possibile creare un nuovo utente e un database con le tabelle sotto quell’utente è mostrato in questo tutorial.

Prima di iniziare questo tutorial, è necessario verificare che pgAdmin4 sia installato e funzioni correttamente nel sistema operativo Ubuntu. Se pgAdmin4 non è installato nel sistema, è possibile seguire i passaggi del seguente tutorial per installare prima pgAdmin4 e avviare questo tutorial.

https://linuxiano.altervista.org/installa-pgadmin4-su-ubuntu/

Dopo aver completato correttamente l’installazione di pgAdmin4, aprite il seguente collegamento da qualsiasi browser per aprire il client pgAdmin4 .

http: // localhost: 5050



* Finestra principale  
* Descrizione dei pannelli  
* Descrizione dei principali elementi visualizzati all'interno di un database (server, database, schema, tabelle)  
* Opzioni di visualizzazione  
* Aggiunta di un nuovo server  
Nome: database test  
Host: localhost  
Porta: 5432  
Utente: postgres  
Password: -  
* Interfaccia SQL    


---
[**Lezione 4.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_04.md) Comandi SQL base - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_04.html)</ins>]
