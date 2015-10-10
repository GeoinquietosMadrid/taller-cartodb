# Importar los datos

## Provincias

* Nuevo dataset sincronizado cada semana desde [esta dirección](https://docs.google.com/uc?id=0B392y-77KML9NWtma3VFc2IwajQ&export=download)

* Juntar provincias de Canarias y de Peninsula y Baleares:

```sql
SELECT * FROM provincias_peninbal 
UNION ALL
SELECT * FROM provincias_canarias
```

* Salvar como un nuevo dataset y renombrarlo como `provincias`

* En este mapa vamos a mover las Canarias para tener un mapa que se puede presentar más fácilmente, aunque perdemos el basemap claro

```sql
UPDATE provincias
SET the_geom = ST_Translate(the_geom,5.0,7.4)
WHERE
ST_Intersects(
 the_geom,
 ST_MakeEnvelope(-18.748169,27.571591,-13.342896,29.463514,4326)
)
```

* Otra opción es importar directamente mi dataset desde [esta url](https://jsanzacademy1.cartodb.com/api/v2/sql?q=select+*+from+provincias&format=geojson)

* Editamos los metadatos para dar la atribución correcta al mapa.

 * Despcripción: Provincias de España
 * Source: http://centrodedescargas.cnig.es/CentroDescargas/inicio.do
 * Attributions: IGN/CNIG
 * Tags: provinces, spain, ign



## Resultados electorales

Desde [este repo](https://github.com/javisantana/globe-elecciones) es posible descargar un csv con buena parte de los municipios geocodificados con resultados de elecciones. He geocodificado el resto de municipios y he mejorado el dataset normalizando los nombres de los partidos en tres nuevas columnas utilizando esta sentencia SQL:

```sql
UPDATE resultados_elecciones r SET primer_partido_cat = r2.partido_cat
FROM (
 SELECT cartodb_id,
  CASE 
  WHEN primer_partido_nombre ILIKE '%psoe%' OR primer_partido_nombre ILIKE '%p.s.o.e%' THEN 'PSOE'
  WHEN primer_partido_nombre ILIKE '%upyd%' THEN 'UPyD'
  WHEN primer_partido_nombre like '%IU%' THEN 'IU'
  WHEN primer_partido_nombre ILIKE '%PP%' OR primer_partido_nombre ILIKE '%p.p.%' THEN 'PP'
  WHEN primer_partido_nombre ILIKE '%ciu%' THEN 'CiU'
  WHEN primer_partido_nombre ILIKE '%amaiur%' THEN 'Amaiur'
  WHEN primer_partido_nombre ILIKE '%pnv%' THEN 'PNV'
  WHEN primer_partido_nombre ILIKE '%erc%' THEN 'ERC'
  WHEN primer_partido_nombre ILIKE '%bng%' THEN 'BNG'
  ELSE 'Otros' 
  END AS partido_cat
FROM resultados_elecciones
) r2
WHERE r.cartodb_id = r2.cartodb_id
```

* Importamos un dataset de puntos con los resultados de las elecciones de 2011 por municipio desde [esta url](https://jsanzacademy1.cartodb.com/api/v2/sql?q=select+*+from+resultados_elecciones&format=geojson).

* Añadimos los metadatos adecuados para dar atribución al origen de los mismos

 * Description: Resultados elecciones 2011
 * Source https://github.com/javisantana/globe-elecciones
 * Attribution: Javi Santana
