---
title: "Potenciar Trabajo"
date: 2022-08-22T13:21:17-03:00
draft: false
---
> *El Programa Potenciar Trabajo busca mejorar las posibilidades de empleo y generar nuevas propuestas productivas por medio de la terminalidad educativa, la formación laboral, la certificación de competencias o la creación, promoción y fortalecimiento de unidades productivas gestionadas por personas físicas que se encuentren en situación de alta vulnerabilidad social y económica.
La finalidad del Programa Potenciar Trabajo es promover la inclusión social plena y mejorar los ingresos de las personas físicas que están en situación de alta vulnerabilidad para que alcancen la autonomía económica.*
> 

Así es como se describe este programa en su respectivo sitio web. Pero realmente cual es el impacto que el mismo posee?

Como todo programa posee sus respectivos registros, que por suerte son públicos, los cuales se pueden encontrar en [DATA](https://datosabiertos.desarrollosocial.gob.ar/dataset/potenciar-trabajo/resource/6e9de963-cf95-4555-87af-84f1126e346b), donde nos encontramos con la siguiente descripción para un mejor entendimiento de estos datos.

| Nombre del campo | Tipo de dato | Descripción |
| --- | --- | --- |
| persona_id | integer | ID único |
| periodo | integer | Periodo de pago |
| monto | float | Monto del Salario Social Complementario del periodo |
| provincia | string | Provincia actual del titular |
| municipio |  | Municipio actual del titular |
| sexo | string | Sexo según DNI |
| genero | string | Género autopercibido |
| edad | integer | Edad al 2021-09-30 |
| nacionalidad | string | Nacionalidad |

Antes de ponernos manos a la obra, todo el código usado y archivos se encuentra en su respectiva carpeta en GitHub.

Ahora ya en nuestro notebook, procedemos a da run primer vistazo. Nos encontramos con un dataset de 9 columnas por 18667050 filas, donde podemos denotar que la nacionalidad esta en mayúsculas, poseemos valores Nan en diversas columnas, y dando un vistazo mas detallado nos encontramos que varias categorías poseen mal sus nombres.

Ya limpio y organizado nuestro dataset, podemos validar nuestro datos para ver que todo este en orden.

```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 18667051 entries, 0 to 18667050
Data columns (total 9 columns):
 #   Column        Dtype         
---  ------        -----         
 0   persona_id    int64         
 1   periodo       datetime64[ns]
 2   monto         int64         
 3   provincia     object        
 4   municipio     object        
 5   sexo          object        
 6   genero        object        
 7   edad          float64       
 8   nacionalidad  object        
dtypes: datetime64[ns](1), float64(1), int64(2), object(5)
memory usage: 1.3+ GB
```

Como todo análisis vamos a ir respondiendo preguntas, como cuanto dinero por periodo se dio, que nacionalidades nos podemos encontrar y en que cantidad, provincias con mas titulares, etc.

Para ser mas ordenados empezaremos a analizar en el siguiente orden: Cantidad de titulares, Dinero, Sexo, Edad.

## Cantidad de titulares

Para empezar podemos analizar como fue evolucionando la cantidad de titulares en el tiempo, para ello es tan simple como contar los valores alrededor de los periodos de tiempo.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/barras_cantidad_periodo.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/barras_cantidad_periodo.png?raw=true)

Podemos apreciar como en nuestro periodo de tiempo que va desde el 01/03/2020 hasta 01/12/2021, la cantidad de titulares fue creciendo.

Respondida nuestra primera pregunta, sigamos viendo con que cantidad de titulares podemos encontrarnos al analizar, su nacionalidad, provincia y municipio.

Antes de pasar a los números me gustaría mostrarles lo conectado que estamos con el mundo.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/paises.PNG?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/paises.PNG?raw=true)

Todos los países marcados, son las nacionalidades las cuales los titulares dijeron poseer, donde podemos encontrar todo tipo de naciones de casi todos los continentes. Ya teniendo este dato podemos ver la distribución de titulares por país.

![barras_cantidad_pais.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/barras_cantidad_pais.png?raw=true)

Como podemos ver nos encontramos que las nacionalidades con mas titulares, son por lejos Argentina, seguido del valor ***none*** el cual es un dato vacío, esto es debido que muchos titulares no poseen una nacionalidad en la base de datos, después podemos observar a Bolivia y Paraguay que son países limítrofes, y curiosamente les sigue Perú, esto no es nada de extrañar al ver los valores de inmigrantes en argentina de los distintos países.

| Países | Inmigrantes |
| --- | --- |
| Paraguay | 690.948 |
| Bolivia | 426.394 |
| Chile | 216.855 |
| Perú | 198.744 |

A la hora de avanzar en nuestro análisis, podemos continuar con la cantidad de titulares por provincia. 

![barras_cantidad_provincia.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/barras_cantidad_provincia.png?raw=true)

No es raro que en primer lugar se encuentre Bs As, lo que si es curioso que una provincia como Tucumán se encuentre en segundo lugar, y dejando al ultimo a Tierra del Fuego. Si trasladamos estos datos a otro tipo de grafico nos encontramos con lo siguiente. 

![circular_cantidad_provincia.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/circular_cantidad_provincia.png?raw=true)

Donde podemos apreciar que Bs As posee mas del 50% de los titulares registrados.

Por ultimo veremos cuales son los municipios con mas titulares.

![barras_cantidad_municipio.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/barras_cantidad_municipio.png?raw=true)

Nos encontramos que La Matanza es el municipio con mas titulares registrados, la misma es considerada como el corazón de la provincia de Bs As, según su pagina web, y como sorpresa nos encontramos con la ciudad capital de Tucumán, que ocupa el quinto puesto. Dados estos interesantes resultados, pasemos a hablar de dinero.

## Dinero

Como primera pregunta que me viene a la cabeza es, cuanto dinero por periodo de tiempo se dio en este programa? o Cuanto dinero se va dando en total?

Respondiendo a la pregunta del total de dinero es tan simple como aplicar el siguiente comando.

```python
df['monto'].sum()
223484976433.61008
```

Obvio puede parecer mucho dinero, hablamos de 223 miles de millones, pero es significante si lo comparamos al PBI de Argentina? dado que hablamos de valores de periodos entre 2020 y 2021, usaremos los valores que nos da Google de 383,1 miles de millones de USD (2020), además si calculamos en base al dólar que nos da el Banco Central de 101 $/USD, nos da un resultado aproximado y rápido de un 0.58% del PBI.

Ahora si vemos la evolución del monto de dinero en función de los distintos periodos.

![barras_monto_periodo.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/barras_monto_periodo.png?raw=true)

Al igual que la cantidad de titulares, podemos ver que el monto de dinero total, fue aumentando de manera acrecentada con el tiempo. Además si comparamos la proporción del monto respecto a cada provincia nos vamos a encontrar, una grafica muy similar a la que obtuvimos analizando la cantidad de titulares. 

![circular_monto_provincia.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/circular_monto_provincia.png?raw=true)

## Sexo

Para empezar, lo primero que me viene a la mente, es querer saber la distribución de sexos.

![circular_sexo_total.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/circular_sexo_total.png?raw=true)

Es muy interesante ver que en gran proporción el sexo predominante entre los titulares es el sexo femenino.

Esto se ve reflejado mejor, si vemos como se distribuye en cada provincia

![barras_sexo_provincia.PNG](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/barras_sexo_provincia.PNG?raw=true)

Antes de pasar a la ultima sección de nuestro análisis, ya que hablamos de sexo, hablaremos un poco de genero. 

```python
df['genero'].unique()
array(['none', 'Varón', 'No Binarie', 'Mujer', 'Feminidad trans/travesti',
       'Masculinidad trans', 'Otro'], dtype=object)
```

Nos encontramos con unas pocas categorías, que si analizamos sus números, obtenemos lo siguiente.

![barras_genero_cantidad.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/barras_genero_cantidad.png?raw=true)

## Edad

Para darnos una idea mas exacta de como es la distribución de edades usaremos lo que se conoce como un histograma.

![histogram_edades.png](https://github.com/VtorCJuarez/TheSite/blob/main/static/potenciar_trabajo/histogram_edades.png?raw=true)

Ahora que poseemos una idea mas clara, procedamos a responder preguntas. Para ello vamos a usar unas instrucciones en nuestro notebook.

```python
df['edad'].mean()
35.8280677975545
df['edad'].median()
34.0
df['edad'].max()
71.0
df['edad'].min()
18.0
df['edad'].mode()
24.0
```

Obtenidos nuestros valores, vemos que nuestra media de edad es de 35-36 años, muy cercana a nuestra mediana que es de 34 años, mientras que nuestra moda es de 24 años, y nuestro rango de edades va de 18 a 71 años. 

Espero que este análisis les haya resultado interesante, obvio faltaron muchas preguntas, un dataset como este, da mucho juego a hacer distintos tipos de análisis. 

Como siempre todo el código podrán encontrarlo en su correspondiente [LINK](https://github.com/VtorCJuarez/Datanalyst/tree/main/Potenciar%20trabajo), hasta el próximo análisis.
