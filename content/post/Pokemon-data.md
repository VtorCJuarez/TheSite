---
title: "Pokémon, ¡Yo te elijo!"
date: 2022-08-11T12:55:17-03:00
draft: false
---
Cuando hablamos de un análisis de datos, podemos pensar que deben ser temas serios, pero para adquirir practica en el campo no es necesario que todos los temas deban ser de una índole económica o científica, pero si tratados con la seriedad que ello confiere. Como lo anuncia el titulo, trataremos con un dataset muy interesante relacionado al mundo Pokémon, el anime que todos conocemos.

![https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/International_Pok%C3%A9mon_logo.svg.png](https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/International_Pok%C3%A9mon_logo.svg.png)

A diferencia del anterior análisis, este será mucho mas visual, todo el código lo podrán encontrar en su correspondiente [GitHub](https://github.com/VtorCJuarez/Datanalyst/tree/main/Pokemon%20data).

Para ser un poco mas ordenados separaremos este estudio en tres etapas, ahora si, vamos por la primera etapa.

## Limpieza y manipulación de datos

Como desconocemos nuestro dataset, procederemos a ver como se compone.

![https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/HEAD.PNG](https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/HEAD.PNG)

Vemos que posee mas columnas que las que podemos visualizar, separaremos en dos los datos.

```python
df[['defense', 'sp_attack',
       'sp_defense', 'speed', 'catch_rate', 'percentage_male',
       'against_normal', 'against_fire', 'against_water', 'against_electric',
       'against_grass', 'against_ice', 'against_fight', 'against_poison']].head()
```

![https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/RESTO.PNG](https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/RESTO.PNG)

Y crearemos un dataframe que llamaremos **weakness**, que usaremos luego para realizar un estudio relacionado a los fuertes de cada tipo.

```python
weakness = df[['against_normal', 'against_fire', 'against_water', 'against_electric',
       'against_grass', 'against_ice', 'against_fight', 'against_poison',
       'against_ground', 'against_flying', 'against_psychic', 'against_bug',
       'against_rock', 'against_ghost', 'against_dragon', 'against_dark',
       'against_steel', 'against_fairy']]
```

Dado el dataframe **df** podemos ver que tendremos que manipularlo y limpiarlo un poco, así nos sea mas sencillo poder trabajar con el, vemos que la columna **type_1** esta todo en mayus, procederemos a pasar todo a minúsculas, pasaremos **weight_pounds** a kilogramos, **percentage_male** lo transformaremos a float, y vemos que los nombres de los Pokémons poseen un problema los que llevan Mega en su nombre, además trataremos todos los Nan (valores vacíos) para su eliminación o cambios por otro valor y por ultimo verificaremos nuestra información. 

![https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/TABLA1.PNG](https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/TABLA1.PNG)

Realizados los cambios podemos verificar y visualizar como se vera la tabla a analizar. 

![head.PNG](Poke%CC%81mon,%20%C2%A1Yo%20te%20elijo!%20027f82aaa73e4226b3c01ecb19e80416/head.png)

```python
df.info()

Output exceeds the size limit. Open the full output data in a text editor
<class 'pandas.core.frame.DataFrame'>
Int64Index: 1045 entries, 0 to 1044
Data columns (total 16 columns):
 #   Column           Non-Null Count  Dtype  
---  ------           --------------  -----  
...
 14  percentage_male  1045 non-null   float64
 15  weight_kg        1045 non-null   float64
dtypes: float64(4), int64(8), object(4)
memory usage: 138.8+ KB
```

Verificada nuestra información podemos pasar a lo realmente interesante.

## EDA

Una de las primeras preguntas que me realice es cuantos de cada tipo hay. Y esto es muy sencillo gracias a contar los valores que tenemos en la columna de tipos. 

![https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/PLOT1.png](https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/PLOT1.png)

Interesante ver que los Pokémons que mas predominan son los del tipo agua, pero hay un dato adicional que puede cambiarnos este análisis, y es que pueden tener un segundo tipo.

Para hacerlo mas interesante comparemos todos los tipos y veremos cual combinación es la que predomina.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/TABLA2.PNG?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/TABLA2.PNG?raw=true)

Para visualizar mejor usaremos un mapa de calor para ver como se compone nuestra tabla.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/MAPACALOR1.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/MAPACALOR1.png?raw=true)

Ahora si podemos afirmar que el tipo con mas cantidad es el tipo agua, seguido del tipo normal, todos ellos sin un segundo tipo, pero si incluimos que posean un segundo tipo, el que mas posee son los normales tipo volador. 
Si alguna vez vieron la serie, sabrán que existen distintas generaciones, y en todas ellas incluyen distintos tipos, pero lo que mas me interesa saber es cuantos legendarios fueron apareciendo por generación.

![https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/BARRAS1.png](https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/BARRAS1.png)

Podemos ver que según avanzaron las generaciones la inclusión de nuevos tipos, se vio acrecentando en parte. Siendo la séptima generación donde mas Pokémons tipo sub legendario se incluyeron. Y la octava donde mas legendarios aparecieron. 

Hablando de legendarios, los mismos poseen mucho poder, pero la pregunta es cual es el Pokémon mas poderoso?, siendo que existen distintos tipos de stats, para ello procederemos a realizar una suma de los mismos y comparar para encontrar a los 5 mas fuertes.

  

![https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/MASPODER.PNG](https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/MASPODER.PNG)

![eternamax.jpg](Poke%CC%81mon,%20%C2%A1Yo%20te%20elijo!%20027f82aaa73e4226b3c01ecb19e80416/eternamax.jpg)

Con una diferencia significativa nos encontramos con **Eternatus** un Pokémon legendario del tipo dragón/veneno, introducido en la octava temporada.

Si generalizamos un poco podemos ver la diferencia de poder entre las distintas generaciones en el siguiente boxplot o diagrama de caja.

![https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/MEDIAPORGENRATION.png](https://raw.githubusercontent.com/VtorCJuarez/TheSite/main/static/img_pokemon/MEDIAPORGENRATION.png)

Podemos ver que la mediana de los datos se encuentran casi todas las generaciones parejas, y en la octava generación hay un dato que se sale del resto que no es mas que el Pokémon antes mencionado. 

Si pasamos este análisis a cada tipo de Pokémon nos encontramos con lo siguiente.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/MEDIAPORTIPO.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/MEDIAPORTIPO.png?raw=true)

Aquí si podemos encontrarnos con una resaltada diferencia, y es que los Pokémon tipo dragón su mediana se encuentra por encima del resto de tipos. 

![16140823976294.jpg](Poke%CC%81mon,%20%C2%A1Yo%20te%20elijo!%20027f82aaa73e4226b3c01ecb19e80416/16140823976294.jpg)

Sigamos comparando datos, y esta vez lo haremos con los tipos que todos conocemos, estos son fuego, agua y planta. Para ellos usaremos los datos medios de estos tres tipos y los visualizaremos en un grafico polar. 

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/PLOT2.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/PLOT2.png?raw=true)

Como lo supimos, el mejor siempre fue **Charmander**, los datos nos dan la razón.

Dejando de lado los stats, vayamos a otras características como son el peso y la altura. Donde teniendo estos datos podríamos sacar un BMI y ver cuales son los Pokémons que mas índice de masa poseen y cuales menos.  

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/INDICES.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/INDICES.png?raw=true)

Lo sorprendente es la altura máxima con la que nos encontramos, que es de 100 metros, y otro dato es que **Cosmog** que es uno de los mas livianos, al evolucionar en **Cosmoem** se convierte en el Pokémon mas denso. 

Si hablamos de sexos el mundo Pokémon no queda atrás y es que si visualizamos la proporción de machos y hembras llegamos a este grafico de torta. 

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/PLOT3.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/PLOT3.png?raw=true)

Dejando de lado los géneros, y volviendo a un dato que es importante en el mundo Pokémon, que es tanto el ataque como la defensa, podemos visualizar esta vez el total de datos de cada Pokémon existente y trazar una recta para separa de los que tienen mas ataque que defensa y viceversa.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/PLOT4.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/PLOT4.png?raw=true)

Dados todos estos puntos podemos denotar los siguientes tres, mayor defensa, mayor ataque, y algo que no vimos hasta el momento, el Pokémon mas débil.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/mas_defensa.PNG?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/mas_defensa.PNG?raw=true)

Si vemos a nuestro Pokémon con mas defensa podemos ver que es el que anteriormente habíamos nombrado como el mas poderoso, **Eternatus Eternamax.**

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/mas_ataque.PNG?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/mas_ataque.PNG?raw=true)

Si vemos a nuestro Pokémon con mas ataque, es uno de los mas conocidos pero en su versión Mega.  **Mewtwo** es un Pokémon legendario y artificial de tipo psíquico introducido en la primera generación que a partir de la sexta generación puede megaevolucionar en **Mega Mewtwo X.**

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/mas_debil.PNG?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/mas_debil.PNG?raw=true)

Y nuestro Pokémon con el ataque y defensa mas bajo nos encontramos con **Happiny**, un Pokémon bebé de tipo normal introducido en la cuarta generación.

## **Weakness**

Como ultimo análisis usaremos nuestro dataframe antes generado y separado de nuestros datos principal. Y procederemos a analizar, que tipo es mejor contra que tipo de Pokémon. Si procedemos igual que en la tabla que generemos con los distintos tipos. 

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/TABLAweak.PNG?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/TABLAweak.PNG?raw=true)

Y a esto mismo lo pasamos a una visualización que podamos entender mejor. 

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/mapa_calor2.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/mapa_calor2.png?raw=true)

En primera instancia podemos denotar que un valor esta fuera de lo normal, así que veamos cual es el problema. Vayamos a ver cual son los valores máximos en la columna de ***ice*** que es donde encontramos nuestro problema. 

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/ice_error.PNG?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/ice_error.PNG?raw=true)

Sabiendo cual es nuestro problema, podemos corregirlo y así obtener los datos que estamos buscando. Obteniendo el siguiente mapa de calor.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/Mpa_calor3.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/img_pokemon/Mpa_calor3.png?raw=true)

Ahora sabiendo que tipos nos sirve para luchar contra que tipo, podemos dar por finalizado este análisis. 
Como siempre fue un gusto poder compartirles todo este trabajo que llevo su tiempo, hasta el siguiente análisis.