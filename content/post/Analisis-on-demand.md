---
title: "Analisis on Demand"
date: 2022-08-01T12:10:00-03:00
draft: false
---
¡Hola! un gusto poder compartir mi primer análisis de datos, me presento soy Victor, y trato de incursionar en esto lo que es el manejo de data. Ahora si vamos a los que nos interesa.
Los datos a considerar son proporcionados por CAMMESA (Compañía Administradora del Mercado Mayorista Eléctrico S.A.), para simplificar es la empresa estatal encargada de administrar el mercado mayorista eléctrico.

![CAMMESA](https://github.com/VtorCJuarez/TheSite/blob/main/static/An%C3%A1lisis%20On%20Demand/CAMESA.png)

[DATOS](http://datos.energia.gob.ar/dataset/publicaciones-cammesa/archivo/30e1c42d-44a7-428f-a55a-12c81dc14186)

A la hora de programar usare **[Jupyter Notebook](https://jupyter.org/) y Visual Studio Code.** 
Todo el código usado estará puesto según vayamos avanzando en el análisis.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

Como todo análisis debemos ver como se compone lo que vamos a analizar.

```python
data = pd.read_csv('demanda-histrica.csv')
```



```python
data.head()
data.tail()
```

![HEAD](https://github.com/VtorCJuarez/TheSite/blob/main/static/An%C3%A1lisis%20On%20Demand/HEAD.png)

![TAIL](https://github.com/VtorCJuarez/TheSite/blob/main/static/An%C3%A1lisis%20On%20Demand/TAIL.png)

Podemos ver que está compuesto de varias categorías, el rango de tiempo va desde el 2012 al 2020, y también vemos que es un archivo bastante extenso siendo que posee más de 122 mil filas, ya teniendo una idea a lo que nos enfrentamos sigamos.
Nos encontramos con una dimensión de (122263, 13), lo mejor de todo es que no poseemos datos nulos lo que nos facilita el análisis.

```python
data.columns
Index(['anio', 'mes', 'agente_nemo', 'agente_descripcion', 'tipo_agente',
       'region', 'provincia', 'categoria_area', 'categoria_demanda', 'tarifa',
       'categoria_tarifa', 'demanda_MWh', 'indice_tiempo'],
      dtype='object')

data.shape
(122263, 13)

data.info()
Data columns (total 13 columns):
 #   Column              Non-Null Count   Dtype  
---  ------              --------------   -----  
 0   anio                122263 non-null  int64  
 1   mes                 122263 non-null  int64  
 2   agente_nemo         122263 non-null  object 
 3   agente_descripcion  122263 non-null  object 
 4   tipo_agente         122263 non-null  object 
 5   region              122263 non-null  object 
 6   provincia           122263 non-null  object 
 7   categoria_area      122263 non-null  object 
 8   categoria_demanda   122263 non-null  object 
 9   tarifa              122263 non-null  object 
 10  categoria_tarifa    122263 non-null  object 
 11  demanda_MWh         122263 non-null  float64
 12  indice_tiempo       122263 non-null  object 
dtypes: float64(1), int64(2), object(10)
```

Al analizar los datos numéricos podemos ver valores interesantes, como las demandas máximas y mínimas.

```python
data.describe()
```

![DESCRIPCION NUMERICA](https://github.com/VtorCJuarez/TheSite/blob/main/static/An%C3%A1lisis%20On%20Demand/DESCRIBE.png)



```python
data['demanda_MWh'].max()
1572308.055

data['demanda_MWh'].min()
-25377.715
```

Antes de seguir avanzando, estudiaremos las variables no numéricas.

```python
data.describe(include=['O'])
```

![DESCRIPCION NO NUMERICA](https://github.com/VtorCJuarez/TheSite/blob/main/static/An%C3%A1lisis%20On%20Demand/NONUM.png)

Las variables que presentan una mayor frecuencia, se pueden destacar las siguientes, como agente con más entradas es ‘EMP DE ENERGIA DE RIO NEGRO SA’, que, si googleamos un poco, podemos averiguar que es EdERSA, la empresa que suministra el servicio público de distribución, comercialización, generación aislada y transporte de energía eléctrica en todo el territorio de la Provincia de Río Negro. Otro dato es que la tarifa más usada es el de GUMAS/AUTOGENERADORES, estos son los Grandes Usuarios Mayores. Tanto la región como la provincia que posee mayor frecuencia es Buenos Aires.
Volviendo a los datos de demanda máxima y mínima, podemos filtrarlos y ver en más detalle en qué periodo de tiempo se produjeron. 

```python
condicion=data.demanda_MWh.isin([1572308.055,-25377.715])
data[condicion]
```

![DEMANDA](https://github.com/VtorCJuarez/TheSite/blob/main/static/An%C3%A1lisis%20On%20Demand/DEMANDA.png)

Luego de haber filtrado, podemos ver que la demanda mínima, se produjo en el mes de abril del 2016, el agente fue el DPEC Corrientes (Dirección Provincial de Energía de Corrientes). La mayor demanda se produjo en julio del 2019 bajo el agente distribuidor de Edenor en la región del Gran Bs. As., una tentativa de idea para explicar esta gran demanda seria que debido a las bajas temperaturas, esta produjo un aumento el uso de elementos calefactores, si relacionamos esta idea con los datos históricos de temperaturas, podemos ver que en ese mes, la media fue entre 15 y 6 grados, siendo este rango menor que en los meses anteriores, otra causa probable es el aumento del precio del gas realizado en abril de ese año. 
Al analizar tanto las provincias como las regiones, podemos apreciar que Buenos Aires se destaca por poseer más del 50% de la demanda total y la región posee el 37%.

```python
provincias_demanda=data['provincia'].value_counts(normalize=True)
data['provincia'].value_counts(normalize=True)
BUENOS AIRES      0.557953
CHUBUT            0.060517
SANTA FE          0.044347
MENDOZA           0.041722
ENTRE RIOS        0.034352
NEUQUEN           0.028291
RIO NEGRO         0.024284
SAN JUAN          0.022820
SANTA CRUZ        0.021666
CORDOBA           0.020988
TUCUMAN           0.018256
SAN LUIS          0.014551
LA RIOJA          0.012465
CORRIENTES        0.012244
FORMOSA           0.011704
MISIONES          0.011451
SALTA             0.011132
JUJUY             0.010935
SGO.DEL ESTERO    0.010829
CATAMARCA         0.010821
CHACO             0.010036
LA PAMPA          0.008637
Name: provincia, dtype: float64

region_demanda=data['region'].value_counts(normalize=True)
data['region'].value_counts(normalize=True)
region_demanda
BUENOS AIRES    0.375543
GRAN BS.AS.     0.182107
PATAGONICA      0.083059
LITORAL         0.078699
NOROESTE        0.074438
CUYO            0.064541
COMAHUE         0.060640
NORESTE         0.045435
CENTRO          0.035538
Name: region, dtype: float64
```

Estos datos no son de sorprender ya que en esta región se encuentra un gran porcentaje de la industria. Si vamos al mapa actual del SADI podemos apreciar la diferencia de potencias instaladas de las distintas regiones, donde destaca la región Buenos Aires.  

![SADI](https://github.com/VtorCJuarez/TheSite/blob/main/static/An%C3%A1lisis%20On%20Demand/SADI.png)

Analizando las demandas máximas y mínimas de los periodos 2012 al 2020 se pudo llegar al siguiente cuadro. Como dato aclaratorio donde no aparece el valor de demanda mínima, es que los datos obtenidos son 0 y se repiten en distintos agentes. 

| Año | Mes demanda max | Mes demanda min | Region demanda max | Region demanda min | Provincia demanda max | Provincia demanda min | Agente demanda max | Agente demanda min | Demanda Max MWh | Demanda Min MWh |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2012 | 6 | 4 | GRAN BS.AS. | NOROESTE | BUENOS AIRES | SALTA | EDENOR DISTRIBUIDOR | EMP.DIST.ENERGIA DE SALTA | 844351.215 | -363.831 |
| 2013 | 12 | - | GRAN BS.AS. | - | BUENOS AIRES | - | EDENOR DISTRIBUIDOR | - | 983326.506 | - |
| 2014 | 6 | - | GRAN BS.AS. | - | BUENOS AIRES | - | EDENOR DISTRIBUIDOR | - | 949827.703 | - |
| 2015 | 6 | - | GRAN BS.AS. | - | BUENOS AIRES | - | EDENOR DISTRIBUIDOR | - | 1006740.649 | - |
| 2016 | 6 | 4 | GRAN BS.AS. | NORESTE | BUENOS AIRES | CORRIENTES | EDENOR DISTRIBUIDOR | DPE CORRIENTES | 1028096.078 | -25377.715 |
| 2017 | 7 | 2 | GRAN BS.AS. | CUYO | BUENOS AIRES | SAN JUAN | EDENOR DISTRIBUIDOR | ENERGIA SAN JUAN SA EX-EDESSA | 780499.483 | -6554.332 |
| 2018 | 7 | 9 | GRAN BS.AS. | CENTRO | BUENOS AIRES | SAN LUIS | EDENOR DISTRIBUIDOR | EDESAL DISTRIBUIDOR | 1046149.598 | -989.002 |
| 2019 | 7 | - | GRAN BS.AS. | - | BUENOS AIRES | - | EDENOR DISTRIBUIDOR | - | 1572308.055 | - |
| 2020 | 1 | - | GRAN BS.AS. | - | BUENOS AIRES | - | EDENOR DISTRIBUIDOR | - | 1200124.891 | - |

Con los datos obtenidos, si representaríamos las demandas máximas respecto al tiempo

![DEMANDA CURVA](https://github.com/VtorCJuarez/TheSite/blob/main/static/An%C3%A1lisis%20On%20Demand/CURVA.png)

Como ultima conclusión podemos observar que si hay un gigante en demanda es Buenos Aires, siendo el que destaca todos los años analizados, el otro dato interesante es que con mayor frecuencia aparecen los meses de junio y julio como meses de gran demanda, los cuales son meses de bajas temperaturas, que la idea popular es pensar que el mayor consumo siempre es en verano. 
Este y otros análisis podrán encontrarlo en https://github.com/VtorCJuarez/DataAnalisis
