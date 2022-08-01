---
title: "Analisis on Demand"
date: 2022-08-01T12:10:00-03:00
draft: false
---
Hola! un gusto poder compartir mi primer análisis de datos, relacionado a un campo que me apasiona mucho que es el mercado eléctrico, antes que nada me presento soy Victor, y trato de incursionar en esto lo que es el análisis de datos. Ahora si vamos a los que nos interesa.

Los datos a considerar son proporcionados por CAMMESA (Compañía Administradora del Mercado Mayorista Eléctrico S.A.), para simplificar es la empresa estatal encargada de administrar el mercado mayorista eléctrico.

![CAMMESA](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6bcf5f6b-9fc8-499a-8f9a-5743871b9dd7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220801%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220801T152459Z&X-Amz-Expires=86400&X-Amz-Signature=6e5971d988084342a88936621ff29b3a61e5e6afef18f974d949d3fce3265b39&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

[DATOS](http://datos.energia.gob.ar/dataset/publicaciones-cammesa/archivo/30e1c42d-44a7-428f-a55a-12c81dc14186)

Para simplificarnos la vida a la hora de programar usare **[Jupyter Notebook](https://jupyter.org/) y Visual Studio Code.** 

Todo el código usado estará puesto según vayamos avanzando en el análisis, como primeros pasos se importan las librerías a usar.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

Como todo análisis debemos ver como se compone el dataset antes proporcionado por CAMMESA, el primer paso es crear nuestra variable data, con esto ya podes empezar a ver la composición.

```python
data = pd.read_csv('demanda-histrica.csv')
```

Primero veremos como esta compuesto el encabezado y el final de nuestro csv.

```python
data.head()
data.tail()
```

![HEAD](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3aff723f-e192-4774-9210-b5a861ffba6a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220801%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220801T152519Z&X-Amz-Expires=86400&X-Amz-Signature=7bd023ed2a5eb66f959f9cce5509e29aeddcf7ab6c10f917c0fba8f68a3dec3b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

![TAIL](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/633dd984-989f-4be8-90a6-cd4d81850f87/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220801%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220801T152523Z&X-Amz-Expires=86400&X-Amz-Signature=3aad13e9b05a31ca4dbc55d3197862707ad3838197bec981f2d4763c8016bc0b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Podemos ver que esta compuesto de varias categorías, las cuales vemos que el año va desde el 2012 al 2020, y también vemos que es un archivo bastante extenso siendo que posee mas de 122 mil filas, ya teniendo una idea a lo que nos enfrentamos sigamos.

Al ver la estructura de la tabla podemos apreciar, como dijimos antes poseemos una dimensión de (122263, 13), y lo mejor de todo es que no poseemos datos nulos lo que nos facilita aun mas el análisis.

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

Al analizar los datos numéricos podemos sacar valores interesantes, mas relacionado a las demandas máximas y mínimas.

```python
data.describe()
```

![DESCRIPCION NUMERICA](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/85b06971-fa71-44f7-b000-5212dde142d1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220801%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220801T152620Z&X-Amz-Expires=86400&X-Amz-Signature=35456dd57da6616fab4763074dd3fa277537d270b5c75bc0fe0687ef732ad6bc&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Separando estos dos datos antes mencionados nos quedarían en los siguientes valores.

```python
data['demanda_MWh'].max()
1572308.055

data['demanda_MWh'].min()
-25377.715
```

Antes de seguir avanzando con estos valores, estudiaremos las variables no numéricas.

```python
data.describe(include=['O'])
```

![DESCRIPCION NO NUMERICA](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7c60c881-1c66-4281-9fe0-9e8904e44231/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220801%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220801T152647Z&X-Amz-Expires=86400&X-Amz-Signature=efdccc04d862bb70d96496aea5dba91d8d34eb037e636ae6929242951f80ba10&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Podemos ver que las variables que poseen mas frecuencia, se pueden destacar las siguientes, como agente la que mas aparece es la EMP DE ENERGIA DE RIO NEGRO SA, que si googleamos un poco, podemos averiguar que no es mas que EdERSA, es la empresa que suministra el servicio público de distribución, comercialización, generación aislada y transporte de energía eléctrica en todo el territorio de la Provincia de Río Negro, exceptuando San Carlos de Bariloche y el Departamento de Pichi Mahuida. Otra dato es que la tarifa mas usada es el de GUMAS/AUTOGENERADORES, estos son los Grandes Usuarios Mayores. Para no hacerlo extenso el ultimo dato que podemos destacar es que tanto la region como provincia que mas frecuencia posee es Buenos Aires que históricamente la energía siempre se concentro en ese sector del pais.

Volviendo a los datos numéricos antes guardados, que eran tanto la demanda máxima como mínima, podemos filtrar esos datos y ver en mas detalle en que periodo de tiempo entre otras cosas fue esta demanda. 

```python
condicion=data.demanda_MWh.isin([1572308.055,-25377.715])
data[condicion]
```

![DEMANDA](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1a576977-5015-4f20-b622-a35ae75ed68a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220801%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220801T152743Z&X-Amz-Expires=86400&X-Amz-Signature=918da7c490da303c2d32978b2ab4fb003e1e86a6059e50f38cea8256aab552b0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Comenzando por nuestro menor valor de demanda, podemos ver que se produjo en el mes de abril en el 2016, siendo la misma negativa, el agente fue el DPEC Corrientes, que no es mas que la Dirección Provincial de Energía de Corrientes. Analizando la mayor demanda podemos ver que se produjo en julio del 2019 bajo el agente distribuidor de Edenor en la region del Gran Bs. As., una tentativa de idea para explicar esta gran demanda seria que debido a las bajas temperaturas, aumento el uso de elementos calefactores, si relacionamos esta idea con los datos históricos de temperaturas, podemos ver que en ese mes, la media fue entre 15 y 6, siendo este rango menor que en los meses anteriores, otra causa probable es el aumento del precio del gas realizado en abril de ese año, dadas estas dos condiciones podría explicarse el récord de demanda. 

Al analizar tanto las provincias como las regiones, podemos apreciar que Buenos Aires se destaca por mas del 50% de la demanda total y analizando por region posee el 37%. 

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

Estos datos no son de sorprender ya que en esta region se encuentra el aglomerado industrial, si vamos al mapa actual del SADI podemos apreciar la diferencia de potencias instaladas de las distintas regiones. 

![SADI](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2becf760-1553-4409-b642-a214bf474d2e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220801%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220801T152755Z&X-Amz-Expires=86400&X-Amz-Signature=bc6fea8c2aef8e99bda68f8ece967ed619bcb9594aeb3d68036c58982512136a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Analizando las demandas máximas y mínimas de los periodos 2012 al 2020 se pudo llegar al siguiente cuadro, como dato aclaratorio donde no aparece demanda mínima, es que los datos obtenidos son 0 y se repiten en distintos agentes. 

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

Con los datos obtenidos, si representaríamos las demandas máximas nos quedaría el siguiente grafico

![DEMANDA CURVA](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/77f459f8-cb5d-4220-89c6-379e1ffa317e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220801%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220801T152758Z&X-Amz-Expires=86400&X-Amz-Signature=2ff8a00f6f4c1e2cee8a45e507383d0a7c656f5fc1bf99579bb339a5dec5692c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

Como ultima conclusión podemos observar que si hay un gigante en demanda es Buenos Aires, siendo el que destaco por tener la demanda máxima en todos los años analizados, el otro dato interesante es que con mayor frecuencia aparecen los meses de junio y julio, los cuales son meses de bajas temperaturas, distinta a la idea popular que el mayor consumo siempre es en verano.

El proyecto podrán encontrarlo en https://github.com/VtorCJuarez/DataAnalisis
