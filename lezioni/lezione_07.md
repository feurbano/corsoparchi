<p align="center"> <img src="materiale/loghi.png" width="315" height="100" /></p>

# WORK IN PROGRESS

#### Lezione 7
## Dati spaziali

Autore: Ferdinando Urbano  

---

### Oggetti spaziali in PostGIS
Fino a qualche anno fa, le informazioni spaziali venivano gestite e analizzate esclusivamente con software dedicati (GIS) in formati dati basati su file (es. shapefile). Oggi, gli approcci più avanzati nella gestione dei dati considerano la componente spaziale degli oggetti (ad esempio le coordinate di un plot, i confini di un parco, la linea di un transetto) come uno dei suoi tanti attributi. Dal punto di vista della gestione dei dati, le informazioni spaziali non sono diverse da una data o da una misura grazie alle estensioni per i dati spaziali che ormai tutti i database hanno, ed in particolare PostgreSQL. Queste estensione aggiungono i tipi di dati spaziali (vettoriali come punti, linee e poligoni, e raster) ai tipi di dati standard che memorizzano altri attributi associati (non spaziali) degli oggetti.  
I tipi di dati spaziali possono essere manipolati con il linguaggio SQL attraverso comandi aggiuntivi specifici. Questo essenzialmente permette di costruire le funzionalità di GIS usando le capacità di un database relazionale. Un database con estensione spaziale non rimpiazza un software GIS, in particolare per la visualizzazione, la creazione di mappe e alcune funzioni avanzate, ma facilita l'integrazione e la gestione dei dati spaziali con le altre informazioni a disposizione.  
Inoltre, i dati spaziali sono generalmente basati su <ins>[standard](http://www.opengeospatial.org/)</ins> condividi molto utilizzati, e questo rende lo scambio di dati tra diverse piattaforme semplice e lineare e permette una perfetta integrazione fra database spaziali e software GIS, che possono essere utilizzati come client del database.  

L'estensione spaziale di PostgreSQL si chiama PostGIS.  

Un database spaziale aggiunge quindi ai tipi di dati già visti (ad esempio stringhe di testo, numeri, data e ora, booleani) ulteriori tipi di dati (spaziali) che servono per rappresentare le caratteristiche geografiche. I tipi di dati spaziali possono essere vettoriali (punti, linee, poligoni, curve, in 2 o 3 dimensioni) oppure <ins>[raster](http://postgis.net/docs/manual-dev/RT_reference.html)</ins>. In questa lezione faremo riferimento esclusivamente ai dati vettoriali.

Per manipolare i dati durante una query, un database fornisce funzioni specifiche per ogni tipo di dato, ad esempio concatenare stringhe, moltiplicare numeri, calcolare la differenza in giorni fra due date. Un database con estensione spaziale aggiunge un set completo di funzioni per analizzare le componenti geografiche (ad esempio, calcolare l'area), determinare le relazioni spaziali (ad esempio, intersezione) e modificare le geometrie (ad esempio, creare la linea di un transetto a partire dai singoli plot).  
L'elenco delle funzioni disponibili in un database spaziale è molto ampio. Un insieme di funzioni molto comuni è definito dal <ins>[OGC SFSQL](http://www.opengeospatial.org/standards/sfs)</ins>.


### Visualizzazione dei dati spaziali archiviati in un database
Per visualizzare i dati spaziali, come tutti gli altri dati di un database, è necessaria una applicazione client che richiede i dati al server e li mostra all'utente nel formato richiesto. Per i dati spaziali, il client migliore è QGIS. PgAdmin fornisce un metodo molto rapido per visualizzare gli oggetti spaziali di una tabella, anche se non offre tutte le funzionalità di un vero e proprio GIS. Nella [<ins>[Lezione 3](https://feurbano.github.io/corsoparchi/lezioni/lezione_03.html)</ins>] è già stato spiegato come visualizzare i dati spaziali con questi due tool e come esportali come shapefile. In PgAdmin, se i dati spaziali sono riferiti alle coordinate geografiche (latitudine e longitudine), viene automaticamente aggiunto come sfondo il layer di <ins>[OpenStreetMap](https://www.openstreetmap.org/)</ins>.  
Un modo comune per esplorare il contenuto di una query con una componente spaziale è di creare una view che può poi essere visualizzata in ambiente GIS.  
Se una tabella con un campo spaziale è visualizzata in un GIS, la geometria può essere editata direttamente nel GIS come se fosse uno shapefile.  

In questo esempio si usa la funzione `ST_BUFFER` per generare un'area circolare con raggio 50 metri intorno a ogni plot. Per rendere disponibile il risultato a QGIS viene creata una view. Questa view può essere poi caricata nel GIS. The column with the spatial information is *geom*

```sql
CREATE VIEW test.miavista1 AS
SELECT
  trappola_code
  parco_code,
  plot_code,
  ST_BUFFER(geom, 50) as geom
FROM biodiversita.trappole;
```

##### Esercizio
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

The earth is approximately spheric whereas maps are two-dimensional. Projections are used to give such a two-dimensional representation of the earth. Many different projection systems exist, each using different mathematical formulas to estimate the earth on a flat surface. Some commonly used projections are the [(Transverse) Mercator projection](https://map-projections.net/compare.php?p1=mercator-84&p2=miller&w=0), the [Robinson projection](https://map-projections.net/compare.php?p1=robinson&p2=schjerning-1&w=0) and the [Lambert Conformal Conic](https://map-projections.net/compare.php?p1=lambert-conformal-conic&p2=mercator-84&w=0). Projections always give a certain distortion of the shape, size, distance and/or angle between different features on the earth's surface. For instance the Mercator projection, from which a variant (Spherical Normal equatorial Mercator projection) is used in google maps, shows large overestimation of the surface area towards the poles. At this website ([The True Size](https://thetruesize.com)) you can compare the size of a country at different latitudes using the Mercator projection. Here you can find some more interesting links to explore map projections and map distortion.

* [Map distortion](http://www.gis.osu.edu/misc/map-projections/)
* [Map characteristics](http://bl.ocks.org/syntagmatic/raw/ba569633d51ebec6ec6e/)
* [Compare projections](https://map-projections.net/imglist.php)

Coordinates alone do not allow to understand where on earth spatial objects (points, lines, polygons) are located. In addition a corresponding **geographical reference system** needs to be identified. A geographical reference system uses an ellipsoid and a datum including a reference zero X and Y axis, in order to assign coordinates to certain locations. Some commonly used geographical reference systems are the [World Geodetic System](http://spatialreference.org/ref/epsg/4326/) (WGS84, EPSG:4326), [the Projected coordinate system for Europe](http://spatialreference.org/ref/epsg/3035/) (ETRS89, EPSG:3035) and the [Universal Transverse Mercator coordinate system](https://gisgeography.com/utm-universal-transverse-mercator-projection/) (UTM).
There are two types of geographical reference systems, **global or spherical reference systems** and **projected reference systems** often defined more locally (e.g., country, continental). A typical example of a global reference system is EPSG:4326, which uses as geodetic datum and ellipsoid WGS84 and as zero reference axes the prime meridian at Greenwich (longitude) and the equator (latitude). Since this is a spherical reference system the measurement unit is in degrees (e.g., [we are here](https://goo.gl/maps/7WyJ7bYBp892): longitude = 11°08'10.7"E, latitude = 46°11'30.5"N). An example of a projected reference system is the Universal Transverse Mercator (UTM) coordinate system. In UTM the earth is devided into a grid, where each grid cell is projected using a standard set of map projections with a central meridian for each six-degree wide UTM zone. In UTM the measurement unit is in meters.

All spatial reference systems available in a postgresql-postgis spatial database can be called using:

```sql
SELECT * FROM spatial_ref_sys;
```

Each reference system has a specific spatial reference identifier (SRID). For instance, the World Geodetic System (SRID = 4326), the Projected coordinate system for Europe (SRID = 3035), UTM for North-Italy (SRID = 32632).

```sql
SELECT * FROM spatial_ref_sys WHERE srid in (4326, 3035, 32632);
```

The reference system of a spatial objects can be set as follows:
```sql
SELECT ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326);
```

When using real world spatial data obtained from various sources, you will likely encounter different coordinate systems. One of the tasks that you will need to accomplish will be to re-project the data into a common SRID, in order to be able to do any useful work.

If you feed in geometries with differing SRIDs you will just get an error:
```sql
SELECT ST_Equals(
        ST_GeomFromText('POINT(0 0)', 4326),
        ST_GeomFromText('POINT(0 0)', 32632)
);
```

Once the SRID code is set you can transform it into another reference system:
```sql
SELECT ST_Transform(ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326),32632);
```

If you compare, the units are clearly different (WGS84 = degrees; UTM = meters):
```sql
SELECT  ST_AsText(ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326)) wgs84,
	ST_AsText(ST_Transform(ST_SetSRID(ST_MakePoint(11.136293,46.191794),4326),32632)) utm32;
```

**Take home message:**
Coordinates only do not identify a position on earth and the same position has different values according to the reference system.
When performing spatial operations (such as intersection, union, distance between points from two layers) always make sure the coordinate reference systems are the same.

In our database, we are not storing planar Euclidean coordinates, but use latitude and longitude to identify a point on the ellipsoid expressed by the geodetic datum WGS\_1984 - the one used globally by GPS systems.

##### EXERCISE
* Visualize the coordinates of your points in both WGS84 and UTM32 (SRID 32632)


### Calcolare l'area di un poligono
Ci sono molte proprietà di un elemento spaziale che sono implicite nella geometria dell'oggetto e che quindi non è necessario salvare come colonne aggiuntive in una tabella

### Riproiettare le coordinate di un punto
### Trovare in quale comune ricade un punto
