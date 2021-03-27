<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

#### Lezione 7
## Dati spaziali

Autore: Ferdinando Urbano  

---

### Oggetti spaziali in PostGIS
Fino a qualche anno fa, le informazioni spaziali venivano gestite e analizzate esclusivamente con software dedicati (GIS) in formati dati basati su file (es. shapefile). Oggi, gli approcci più avanzati nella gestione dei dati considerano la componente spaziale degli oggetti (ad esempio le coordinate di un plot, i confini di un parco, la linea di un transetto) come uno dei suoi tanti attributi. Dal punto di vista della gestione dei dati, le informazioni spaziali non sono diverse da una data o da una misura grazie alle estensioni per i dati spaziali che ormai tutti i database hanno, ed in particolare PostgreSQL. Queste estensione aggiungono i tipi di dati spaziali (vettoriali come punti, linee e poligoni, e raster) ai tipi di dati standard che memorizzano altri attributi associati (non spaziali) degli oggetti.  
I tipi di dati spaziali possono essere manipolati con il linguaggio SQL attraverso comandi aggiuntivi specifici. Questo essenzialmente permette di costruire le funzionalità di GIS usando le capacità di un database relazionale. Un database con estensione spaziale non rimpiazza un software GIS, in particolare per la visualizzazione, la creazione di mappe e alcune funzioni avanzate, ma facilita l'integrazione e la gestione dei dati spaziali con le altre informazioni a disposizione.  
Inoltre, i dati spaziali sono generalmente basati su <ins>[standard](http://www.opengeospatial.org/)</ins> condivisi molto utilizzati, e questo rende lo scambio di dati tra diverse piattaforme semplice e lineare e permette una perfetta integrazione fra database spaziali e software GIS, che possono essere utilizzati come client del database.  

L'estensione spaziale di PostgreSQL si chiama PostGIS.  

Un database spaziale aggiunge quindi ai tipi di dati già visti (ad esempio stringhe di testo, numeri, data e ora, booleani) ulteriori tipi di dati (spaziali) che servono per rappresentare le caratteristiche geografiche. I tipi di dati spaziali possono essere vettoriali (punti, linee, poligoni, curve, in 2 o 3 dimensioni) oppure <ins>[raster](http://postgis.net/docs/manual-dev/RT_reference.html)</ins>. In questa lezione faremo riferimento esclusivamente ai dati vettoriali.

Per manipolare i dati durante una query, un database fornisce funzioni specifiche per ogni tipo di dato, ad esempio concatenare stringhe, moltiplicare numeri, calcolare la differenza in giorni fra due date. Un database con estensione spaziale aggiunge un set completo di funzioni per analizzare le componenti geografiche (ad esempio, calcolare l'area), determinare le relazioni spaziali (ad esempio, intersezione) e modificare le geometrie (ad esempio, creare la linea di un transetto a partire dai singoli plot).  
L'elenco delle funzioni disponibili in un database spaziale è molto ampio. Un insieme di funzioni molto comuni è definito dal <ins>[OGC SFSQL](http://www.opengeospatial.org/standards/sfs)</ins>.


### Visualizzazione dei dati spaziali archiviati in un database
Per visualizzare i dati spaziali, come tutti gli altri dati di un database, è necessaria una applicazione client che richiede i dati al server e li mostra all'utente nel formato richiesto. Per i dati spaziali, il client migliore è QGIS. PgAdmin fornisce un metodo molto rapido per visualizzare gli oggetti spaziali di una tabella, anche se non offre tutte le funzionalità di un vero e proprio GIS. Nella [<ins>[Lezione 3](https://feurbano.github.io/corsoparchi/lezioni/lezione_03.html)</ins>] è già stato spiegato come visualizzare i dati spaziali con questi due tool e come esportali come shapefile. In PgAdmin, se i dati spaziali sono riferiti alle coordinate geografiche (latitudine e longitudine), viene automaticamente aggiunto come sfondo il layer di <ins>[OpenStreetMap](https://www.openstreetmap.org/)</ins>.  
Un modo comune per esplorare il contenuto di una query con una componente spaziale è di creare una view che può poi essere visualizzata in ambiente GIS.  
Se una tabella con un campo spaziale è visualizzata in un GIS, la geometria può essere editata direttamente nel GIS come se fosse uno shapefile.  

In questo esempio si usa la funzione `ST_BUFFER` per generare un'area circolare con raggio 100 metri intorno a ogni plot. Per rendere disponibile il risultato a QGIS viene creata una view. Questa view può essere poi caricata nel GIS. The column with the spatial information is *geom*

```sql
CREATE VIEW test.miavista1 AS
SELECT
  trappola_code
  parco_code,
  plot_code,
  ST_BUFFER(geom, 100) as geom
FROM biodiversita.trappole;
```

#### Esercizio
> Scrivere una query per generare una tabella con un buffer di 500 metri per ogni plot e salvarla come una vista nello schema *test* e visualizzarla poi in QGIS. Si consiglia di mettere *plot_code* come prima colonna della tabella così che può essere riconosciuta più facilmente da QGIS come chiave univoca.

### Creare un punto a partire dalle coordinate
Se si visualizza come le coordinate di un oggetto spaziale sono memorizzate nel database, si vede che la rappresentazione del database non è fatta per essere leggibile:
```sql
SELECT
  geom
FROM
  biodiversita.trappole
LIMIT 5;
```

In realtà, anche se all'interno del campo il dato è organizzato in modo da ottimizzarne l'archiviazione, la geometria è solo una sequenza di coordinate con specificato il tipo di dato e il sistema di riferimento usato. Per visualizzare un formato leggibile del campo geometria si può funzione `ST_ASTEXT` o `ST_ASEWKT`:  

```sql
SELECT
  ST_ASTEXT(geom) geometria_coordinate,
  ST_ASEWKT(geom) geometria_completa
FROM
  biodiversita.plot
LIMIT 5;
```

Una geometria di punti può essere creata direttamente da una coppia di coordinate con `ST_MakePoint`:

```sql
SELECT ST_MakePoint(11.001,46.001) AS punto;
```

Qui si crea l'oggetto punto e poi lo si visualizza in una rappresentazione testuale:

```sql
SELECT ST_AsText(ST_MakePoint(11.001,46.001)) AS punto;
```  

Ci sono molte funzioni spaziali disponibili in PostGIS: consultate la <ins>**[documentazione ufficiale di PostGIS](http://postgis.net/docs/manual-dev/reference.html)**</ins> per farvi un'idea del tipo di strumenti che PostGIS offre.  

PostGIS offre un modo per visualizzare tutte le tabelle con attributo spaziale che sono presenti nel database:

```sql
SELECT * FROM geometry_columns;
```

La query precedente ci informa che all'interno del nostro database ci sono un certo numero di tabelle con una colonna geometria, e che ognuna di queste colonne si chiama *geom* e contiene dati bidimensionali e accetta solo un tipo di dati specifico. Inoltre, portano anche informazioni sul sistema di coordinate di riferimento in uso, tramite il parametro <ins>[SRID](https://en.wikipedia.org/wiki/SRID)</ins>.

### Sistemi di riferimento e coordinate

La terra è (molto) approssimativamente sferica, mentre le mappe sono bidimensionali. Le proiezioni sono usate per dare una rappresentazione bidimensionale della terra in modo che la superficie sferica della terra possa essere rappresentata su un piano. Esistono molti sistemi di proiezione diversi, ognuno dei quali utilizza diverse formule matematiche per "ricondurre" la superficie della terra a una superficie piana. Alcune proiezioni comunemente usate sono la [(Transverse) Mercator projection](https://map-projections.net/compare.php?p1=mercator-84&p2=miller&w=0) e la [Lambert Conformal Conic](https://map-projections.net/compare.php?p1=lambert-conformal-conic&p2=mercator-84&w=0). Le proiezioni danno sempre una certa distorsione della forma, dimensione, distanza e/o angolo degli oggetti della superficie terrestre. Per esempio la proiezione di Mercator, di cui una variante (proiezione sferica normale equatoriale di Mercatore) è usata in google maps, mostra una grande sovrastima della superficie verso i poli. In questo sito ([The True Size](https://thetruesize.com)) si possono confrontare le dimensioni di un paese a diverse latitudini usando la proiezione Mercator. Qui puoi trovare altri link interessanti per esplorare le proiezioni delle mappe e le loro distorsioni.

* [Distorsione delle mappe](http://www.gis.osu.edu/misc/map-projections/)
* [Caratteristiche delle mappe](http://bl.ocks.org/syntagmatic/raw/ba569633d51ebec6ec6e/)
* [Confronta le proiezioni](https://map-projections.net/imglist.php)

Le coordinate da sole non permettono di capire dove si trovano sulla terra gli oggetti spaziali (punti, linee, poligoni). Bisogna inoltre identificare un **sistema di riferimento geografico** corrispondente. Un sistema di riferimento geografico utilizza un ellissoide e un datum che include un asse X e Y di riferimento zero, per assegnare le coordinate a certe località. Alcuni sistemi di riferimento geografico comunemente usati sono il [World Geodetic System](http://spatialreference.org/ref/epsg/4326/) (WGS84, EPSG:4326), [il sistema di coordinate proiettato per l'Europa](http://spatialreference.org/ref/epsg/3035/) (ETRS89, EPSG:3035) e il [sistema di coordinate Universal Transverse Mercator](https://gisgeography.com/utm-universal-transverse-mercator-projection/) (UTM).
Ci sono due tipi di sistemi di riferimento geografici, **sistemi di riferimento globali o sferici** e **sistemi di riferimento proiettati** spesso definiti più localmente (ad esempio, paese, continente). Un esempio tipico di un sistema di riferimento globale è EPSG:4326, che usa come datum geodetico ed ellissoide WGS84 e come assi di riferimento zero il primo meridiano di Greenwich (longitudine) e l'equatore (latitudine). Trattandosi di un sistema di riferimento sferico l'unità di misura è in gradi (ad esempio, [questo punto](https://goo.gl/maps/7WyJ7bYBp892): longitudine = 11°08'10.7 "E, latitudine = 46°11'30.5 "N). Un esempio di sistema di riferimento proiettato è il sistema di coordinate Universal Transverse Mercator (UTM). In UTM la terra è divisa in una griglia, dove ogni cella della griglia è proiettata usando una serie standard di proiezioni di mappe con un meridiano centrale per ogni zona UTM di sei gradi. In UTM l'unità di misura è in metri.

La lista dei sistemi di riferimento spaziale disponibili nel database può essere visualizzata usando:

```sql
SELECT * FROM spatial_ref_sys;
```

Ogni sistema di riferimento ha uno specifico identificatore di riferimento spaziale (SRID). Per esempio, il World Geodetic System (SRID = 4326), il Projected coordinate system for Europe (SRID = 3035), UTM per il Nord-Italia zona 32 (SRID = 32632). Quest'ultimo è il sistema usando nella maggior parte dei casi dai parchi, anche se alcuni dati provenienti ad esempio da sensori, possono essere archiviati nel sistema SRID = 4326 (latitudine e longitudine riferite a WGS84).

```sql
SELECT * FROM spatial_ref_sys WHERE srid in (4326, 3035, 32632);
```

Il sistema di riferimento di un oggetto spaziale può essere impostato quando viene creato attraverso il comando `ST_SetSRID` come un questo esempio:

```sql
SELECT ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326);
```

Quando si usano dati spaziali del mondo reale ottenuti da varie fonti, è probabile che si incontrino diversi sistemi di coordinate. Una funzionalità che è spesso utile è la riproiezione dei dati verso un sistema di riferimento (SRID) comune.

Se si prova a comparare gemetrie con coordinate riferite a sistemi diversi si ottiene un errore, come in questo caso dove vengono confrontati due punti definiti in sistemi diversi tramite l'operatore `ST_Equals` (tutte le funzioni spaziali in PostGIS cominciano con *ST_*):
```sql
SELECT
  ST_Equals(
   ST_SetSRID(ST_MakePoint(0,0),4326),
   ST_SetSRID(ST_MakePoint(0,0),32632));
```

Il modo corretto di procedere è riproiettare uno dei due punti in modo da avere entrambi nello stesso sistema di riferimento. Il comando `ST_Transform` serve per riproiettare una geometria e ha la sintassi `ST_Transform(geometria, nuovo_srid)`. In questo esempio si crea un punto in coordinate geografiche e poi si riproietta in UTM32.
```sql
SELECT
  ST_Transform(
   ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326),
  32632);
```

Se si confrontano i punti nei due sistemi di riferimento, le unità sono chiaramente diverse (WGS84 = gradi; UTM = metri):
```sql
SELECT
  ST_AsText(ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326)) wgs84,
	ST_AsText(ST_Transform(ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326),32632)) utm32;
```

Le coordinate non identificano solo una posizione sulla terra e la stessa posizione ha valori diversi a seconda del sistema di riferimento.  
Quando si eseguono operazioni spaziali (come intersezione, unione, distanza tra punti di due livelli) bisogna assicurarsi sempre che i sistemi di riferimento delle coordinate siano gli stessi.

#### Esercizio
> Visualizzare le coordinare dei plot nel sistema 4326 (longitudine, latitudine). Per vedere le coordinate si può usare la funzione ST_AsText oppure le funzioni ST_X e ST_Y che estraggono le due componenti delle coordinate di un punto.


### Calcolare l'area di un poligono
Ci sono molte proprietà di un elemento spaziale che sono implicite nella geometria dell'oggetto e che quindi non è necessario salvare come colonne aggiuntive in una tabella. Un esempio è l'area di un poligono. Si può calcolare l'area di un poligono usando la funzione `ST_Area`. Analogamente si può calcolare il perimetro usando la funzione `ST_Perimeter`. Un esempio applicato al layer dei parchi è:  

```sql
SELECT
  parco_code,
  ST_Area(geom)::integer area,
  ST_Perimeter(geom)::integer perimetro
FROM
  basedata.parchi
ORDER BY
  ST_Area(geom)::integer;
```

Nella query si è utilizzato l'operatore CAST `::` per trasformare il numero in intero e non visualizzare i decimali che in questo caso sono ininfluenti.  

L'area e il perimetro vengono calcolati in base al sistema di riferimento della geometria. Se la geometria è in coordinate geografiche il risultato non avrà senso:

```sql
SELECT
  nome_com,
  ST_Area(geom) area,
  ST_Perimeter(geom) perimetro
FROM basedata.comuni_istat_confini;
```

Per calcolare un valore corretto è quindi necessario prima riproiettare la geometria in UTM32, come nell'esempio seguente:

```sql
SELECT
  nome_com,
  ST_Area(
   ST_Transform(geom,32632)) area,
  ST_Perimeter(
   ST_Transform(geom,32632)) perimetro
FROM
  basedata.comuni_istat_confini;
```

#### Esercizio
> Calcolare l'area e il perimetro dei buffer di 100 metri calcolati intorno ad ogni plot. In questo caso bisogna calcolare l'area a partire dall'oggetto creato con il buffer con una struttura come ST_Area(ST_Buffer(...)).

### Trovare in quale comune ricade un punto
Un ultimo esempio dell'utilizzo delle funzioni spaziali dentro il database è quello di `ST_Intersects`. Questa funzione restituisce `TRUE` se due geometrie si intersecano. Usiamo ora questo comando per vedere in quale comune ricade ogni plot, utilizzando le tabelle *basedata.plot* e *basedata.comuni_istat_confini*. In questo caso la condizione di join (`WHERE`) non è l'equivalenza di un campo, ma l'intersezione dei due strati geografici (cioè quando il punto del plot è dentro il poligono del comune). La geometria dei comuni è però in coordinate geografiche, mentre quella dei plot è in UTM32. Per poterli confrontare devo quindi prima riproiettare uno dei due. Dentro il comando `ST_Intersects` avrò quindi anche un comando `ST_Transform`:

```sql
SELECT
  plot.parco_code,
  plot.plot_code,
  comuni_istat_confini.pro_com,
  comuni_istat_confini.nome_com
FROM
  biodiversita.plot,
  basedata.comuni_istat_confini
WHERE
  ST_Intersects(
   comuni_istat_confini.geom,
   ST_Transform(plot.parchi,4326))
ORDER BY
  plot_code;
```

#### Esercizio
> Trovare in quali comuni ricadono i Parchi. In questo caso il poligono di ogni Parco potrà intersecare più comuni, quindi mi posso aspettare una tabella con più righe per ogni Parco.

---
[**Lezione 8.**](https://github.com/feurbano/corsoparchi/blob/master/lezioni/lezione_08.md) Comandi SQL per aggiornare il database - [<ins>[**Link pagina web**](https://feurbano.github.io/corsoparchi/lezioni/lezione_08.html)</ins>]
