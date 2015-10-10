## Mapas de puntos por municipio

Es posible utilizar los datos puntuales directamente, tienen la ventaja sobre la agregación que informan sobre la distribución de la población y obviamente dan más detalle, aunque también pueden ser difíciles de leer.

### Mapa de partido más votado

SQL:

```sql
SELECT * FROM resultados_elecciones
```

CartoCSS:

```
/** category visualization */

#resultados_elecciones {
   marker-fill-opacity: 0.9;
   marker-line-color: #FFF;
   marker-line-width: 0;
   marker-line-opacity: 1;
   marker-placement: point;
   marker-type: ellipse;
   marker-width: 4.5;
   marker-allow-overlap: true;
}

#resultados_elecciones[primer_partido_cat="BNG"] {
   marker-fill: #A6CEE3;
}
#resultados_elecciones[primer_partido_cat="CiU"] {
   marker-fill: #00008b;
}
#resultados_elecciones[primer_partido_cat="ERC"] {
   marker-fill: #B2DF8A;
}
#resultados_elecciones[primer_partido_cat="IU"] {
   marker-fill: #b22222;
}
#resultados_elecciones[primer_partido_cat="Otros"] {
   marker-fill: #FB9A99;
}
#resultados_elecciones[primer_partido_cat="PNV"] {
   marker-fill: #E31A1C;
}
#resultados_elecciones[primer_partido_cat="PP"] {
   marker-fill: #1e90ff;
}
#resultados_elecciones[primer_partido_cat="PSOE"] {
   marker-fill: #ff0000;
}
#resultados_elecciones[primer_partido_cat="UPyD"] {
   marker-fill: #CAB2D6;
}
#resultados_elecciones[primer_partido_cat="Amaiur"] {
   marker-fill: #008080;
}



#provincias::labels[zoom>9] {
  text-name: [municipality_name];
  text-face-name: 'DejaVu Sans Book';
  text-size: 7;
  text-label-position-tolerance: 0;
  text-fill: #000;
  text-halo-fill: #FFF;
  text-halo-radius: 1.5;
  text-dy: -10;
  text-allow-overlap: false;
  text-placement: point;
  text-placement-type: dummy;
  
  
  [zoom>10]{text-size:12}
  [zoom>11]{text-size:14}
  [zoom>12]{text-size:22}
}
```
Resultado:


![](./imgs/01_mas_votado.png)