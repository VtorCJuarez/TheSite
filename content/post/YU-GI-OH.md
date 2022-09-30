---
title: "¡Que empiece el Duelo!"
date: 2022-09-25T17:27:32-03:00
draft: false
---
**Yu-Gi-Oh!** es un manga creado por Kazuki Takahashi, que ha dado lugar a toda una franquicia, además de múltiples series de anime, películas, juegos de cartas y numerosos videojuegos. La publicación comenzó el 30 de octubre de 1996 y finalizó el 8 de marzo de 2004 con treinta y ocho volúmenes.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/1200px-Yu-Gi-Oh!_(Logo).jpg?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/1200px-Yu-Gi-Oh!_(Logo).jpg?raw=true)

Como siempre, un gusto de nuevo encontrarme en otro análisis. Esta vez les traigo un dataset relacionado al mundo de las cartas de Yu-Gi-Oh!, esto les debe despertar el niño interno a muchos. Los datos nos lo proporciona Kaggle, nuestra pagina de confianza. 

[Datos Yu-Gi-Oh!](https://www.kaggle.com/datasets/archanghosh/yugioh-database?select=Yugi_images)

A la hora de elegir nuestras herramientas, nos volvemos a encontrar con un viejo conocido, que es el uso de Python con sus diversas librerías, para el análisis de datos, siempre muy útil y versátil a la hora de un análisis completo.

## Tipo de cartas

Como algunos recordaran, este juego consistía en tener un mazo con distintos tipo de cartas, que luego durante la partida uno iba colocando en el campo de batalla, estas se dividían en tres:

- **Monster    5339**
- **Spell      1653**
- **Trap       1381**

Dentro de las reglas del juego, que varían según donde se las lea, se puede tener un Deck Principal de entre 40 a 60 cartas en total, un Deck Extra de hasta 15 cartas y un Side Deck (o Deck de Soporte) de hasta 15 cartas también. Si tomamos estos datos, podríamos calcular la cantidad de mazos distintos posibles. Tomando el caso mas fácil de solo poseer un Deck de 40 cartas, y que las mismas se puedan repetir, nos encontraríamos que son posibles, con nuestros datos actuales, unos 8.2270407477684396088358510287882e+156 Decks posibles.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/Tipo%20de%20cartas.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/Tipo%20de%20cartas.png?raw=true)

Introduciéndonos aun mas en como se clasificaban estas cartas, no solo se quedaban ahí, estos tipos además poseían su rareza, desde las comunes hasta las extremadamente raras:

- **Common: 3873**
- **Rare: 1501**
- **Super Rare: 1136**
- **Ultra Rare: 968**
- **Secret Rare: 552**
- **Short Print: 255**
- **Premium Gold Rare: 61**
- **Super Short Print: 12**
- **Ghost Rare: 10**
- **Starlight Rare: 5**

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/rareza.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/rareza.png?raw=true)

Como curiosidad, busque una de estas cartas **Starlight Rare**, que solo hay 5, y todas son del tipo monstruo.

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/PHRA-EN100%20StR.png?raw=true" alt="PHRA-EN100 StR.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/PHRA-EN100%20StR.png?raw=true)

Si nos adentramos aun mas, el tipo monstruo posee, un dato adicional y es su atributo: 

- **DARK: 1446**
- **EARTH: 1296**
- **LIGHT: 1076**
- **WIND: 534**
- **WATER: 523**
- **FIRE: 463**
- **DIVINE: 1**

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/atributo.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/atributo.png?raw=true) 

Lo que llama la atención es el atributo DIVINE, que posee nada mas que 1 solo monstruo, no es mas que el famoso “The Winged Dragon of Ra” pero en modo esfera. 

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/GFP2-EN180%20GR.png?raw=true" alt="GFP2-EN180 GR.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/GFP2-EN180%20GR.png?raw=true)

De lo que no hablamos es que estas cartas, salen a la venta en sets, de los cuales según nuestros datos son 86 diferentes sets publicados, de los que podemos destacar, por poseer una mayor cantidad de tipo de cartas, son los siguientes:

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/Set_cartas_5.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/Set_cartas_5.png?raw=true)

Ya que nos respondimos varias preguntas, respecto a las características que poseen las cartas, en líneas generales, podemos pasar a analizar en especial al tipo monstruo. 

## Monster

Para iniciar este análisis, centrado únicamente en los monstruos, tuvimos que aplicar un poco de limpieza y ordenamiento de datos. Ya realizado el lado pesado (como siempre todo el código estará publico en su correspondiente GitHub), pasemos a contar con que nos encontramos.

Para saber donde nos encontramos parados, se realizo un grafico de dispersión, de los distintos monstruos, con los datos de ataque y defensa como variables, adicional a esto, la suma de ambas, tanto ataque como defensa, nos da su respectiva escala de color, donde podremos clasificarlos según su poder total. 

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/ataq_def.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/ataq_def.png?raw=true)

Si le damos un vistazo rápido, podemos ver como las mayoría de los datos están concentrados en las zonas de 0 a 3000 de ambas variables, y los puntos mas alejados los veremos en mas detalle, dentro de un momento. 

Estos monstruos, un dato que los caracteriza, es que poseen un nivel, que es conocido como la cantidad de estrellas, y dentro del juego proporcionan un aproximado de cuán poderoso es el monstruo en cuestión.

La distribución de nivel es la siguiente:

- **4☆: 1538**
- **3☆: 780**
- **2☆: 413**
- **1☆: 411**
- **8☆: 395**
- **6☆: 380**
- **5☆: 365**
- **7☆: 289**
- **10☆: 114**
- **9☆: 80**
- **11☆: 21**
- **12☆: 19**

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/estrellas.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/estrellas.png?raw=true)

Siendo de sorprender , que un tercio de las cartas son monstruos de 4☆, seguidos por 3☆ y 2☆, lo que no es sorpresa, es que las cartas con un nivel de 11☆ y 12☆, sean las que menor cantidad posean. 

Volviendo al tema del ataque y defensa, si buscamos en cada caso, el top 5, nos encontraríamos con la siguiente comparación.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/cartas%20mas%20ataque.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/cartas%20mas%20ataque.png?raw=true)

Podemos destacar que son 3 los monstruos con mas ataque del juego (siendo 5000 el máximo), como dato aclaratorio se dejo aparte cartas que no mostraban sus datos, y también cartas que en este juego son muy comunes, que poseen **?** en su ataque como defensa.

Los 3 que mas ataque poseen son:

- **Superdimensional Robot Galaxy Destroyer**
- **Rocket Arrow Expres**
- **Flower Cardian Lightflare**

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/REDU-EN044%20UR.png?raw=true" alt="REDU-EN044 UR.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/REDU-EN044%20UR.png?raw=true)

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/GAOV-EN016%20R.png?raw=true" alt="GAOV-EN016 R.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/GAOV-EN016%20R.png?raw=true)

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/RATE-EN045%20R.png?raw=true" alt="RATE-EN045 R.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/RATE-EN045%20R.png?raw=true)

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/BODE-EN028%20UR.png?raw=true" alt="BODE-EN028 UR.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/BODE-EN028%20UR.png?raw=true)

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/DANE-EN012%20R.png?raw=true" alt="DANE-EN012 R.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/DANE-EN012%20R.png?raw=true)

Si aplicamos el mismo razonamiento para los monstruos con mas defensa nos encontraremos con lo siguiente.

![https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/cartas%20mas%20defensa.png?raw=true](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/cartas%20mas%20defensa.png?raw=true)

En este caso el valor máximo es de 4500 y solo lo poseen dos monstruos, siendo uno de estos el que podríamos apodar como la carta mas poderosa (si sumamos ataque y defensa).

Las 2 que mas defensa poseen son:

- **T.G. Halberd Cannon/Assault Mode**
- **Quintet Magician**

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/DANE-EN012%20R.png?raw=true" alt="DANE-EN012 R.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/DANE-EN012%20R.png?raw=true)

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/GFP2-EN127%20UR.png?raw=true" alt="GFP2-EN127 UR.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/GFP2-EN127%20UR.png?raw=true)

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/BODE-EN028%20UR.png?raw=true" alt="BODE-EN028 UR.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/BODE-EN028%20UR.png?raw=true)

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/EXVC-EN043%20UR.png?raw=true" alt="EXVC-EN043 UR.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/EXVC-EN043%20UR.png?raw=true)

![<img src="https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/SOI-EN003%20UR.png?raw=true" alt="SOI-EN003 UR.png" width="200" height="250">](https://github.com/VtorCJuarez/TheSite/blob/main/static/yu%20gi%20img/SOI-EN003%20UR.png?raw=true)

Para invocar nuestra carta mas poderosa (**Quintet Magician**), es necesario:

> *Debe ser Invocada por Fusión (5 monstruos Lanzador de Conjuros). Si esta carta es Invocada por Fusión usando 5 monstruos Lanzador de Conjuros con nombres diferentes: puedes destruir todas las cartas que controle tu adversario. Esta carta boca arriba en el Campo no puede ser Sacrificada, ni usada como Material de Fusión, y además no puede ser destruida por efectos de cartas.*
> 

La verdad una carta que no debería faltar en ningún Deck.

Espero que hayan disfrutado este análisis, tanto como yo de elaborarlo. Como siempre este y otros proyectos, lo encontraran en mi [GitHub](https://github.com/VtorCJuarez/Datanalyst).

Hasta el siguiente análisis, espero esta vez traérselos mas pronto que tarde.
