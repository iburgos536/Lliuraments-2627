# BrawlML: Brawler Markup Language

**Un lenguaje de marcas para definir personajes (brawlers) del juego Brawl Stars**

> Práctica 1 de LM: Características de los lenguajes de marcas

---

## 1. Introducción a los lenguajes de marcas

### 1.1 ¿Qué es un lenguaje de marcas?

Un **lenguaje de marcas** (*markup language*) mezcla el **contenido** de un documento con **marcas** (etiquetas o anotaciones) que dicen cómo está organizado, qué significa o cómo se tiene que ver. Las marcas van escritas dentro del propio texto y así un programa sabe cómo interpretar cada parte.

No es un lenguaje de programación: no ejecuta órdenes ni tiene lógica (nada de bucles ni condiciones). Solo **describe** información.

### 1.2 Orígenes

| Año | Qué pasó |
| :---- | :---- |
| Años 60 | La palabra *markup* viene del mundo de la edición: los editores marcaban los manuscritos con anotaciones para decirle a la imprenta cómo componerlos. |
| 1969 | **GML** (Generalized Markup Language), creado por Charles Goldfarb, Edward Mosher y Raymond Lorie en IBM. |
| 1986 | **SGML** (Standard Generalized Markup Language), estándar ISO 8879. Es el "abuelo" de los lenguajes de marcas modernos. |
| 1991 | **HTML**, creado por Tim Berners-Lee en el CERN para compartir documentos por la web. |
| 1998 | **XML** (eXtensible Markup Language), recomendación del W3C: una versión simplificada de SGML. |
| Hoy | Hay muchos derivados y formatos parecidos: XHTML, SVG, RSS, KML, Markdown, YAML (en un sentido más amplio), etc. |

### 1.3 ¿Para qué se usan?

- **Organizar la información** (documentos, datos, configuraciones).
- **Separar contenido y presentación**: una cosa son los datos y otra cómo se muestran.
- **Intercambiar datos** entre aplicaciones y sistemas distintos.
- **Facilitar el procesamiento automático** por parte de programas.
- **Representar los datos para que los entiendan tanto personas como máquinas**.

### 1.4 Características comunes

1. Están basados en **texto plano**, así que se pueden editar con cualquier editor de texto.
2. Usan **metadatos**: información sobre los datos (*datos sobre los datos*).
3. Facilitan el **procesamiento automático** de los datos.
4. Facilitan la **representación o interpretación** de los datos guardados.

---

## 2. Introducción a BrawlML y las reglas que hemos definido

### 2.1 ¿Qué es BrawlML?

**BrawlML** (*Brawler Markup Language*) es un lenguaje de marcas que nos hemos inventado para **describir un brawler** (personaje) de *Brawl Stars*: sus estadísticas, su ataque, su súper, los gadgets, las star powers, etc.

Para que se parezca al juego, **todas las etiquetas, atributos y valores están en inglés**, igual que los términos que usa Brawl Stars (*brawler*, *super*, *gadget*, *star power*, *hypercharge*…).

Un archivo BrawlML es un archivo de **texto plano** que un programa puede leer para:

- comprobar que el personaje está bien definido,
- generar una **ficha visual** (carta, página web, PDF),
- comparar brawlers entre ellos,
- guardarlos en una base de datos.

### 2.2 Estructura general

```
<brawler atributos>
    <stats> ... </stats>
    <attack .../>
    <super .../>
    <gadget .../>          (opcional)
    <starpower .../>       (opcional)
    <hypercharge .../>     (opcional)
    <balance .../>         (opcional)
</brawler>
```

Cada archivo describe **un solo brawler**. El elemento principal (la raíz) es siempre `<brawler>`.

### 2.3 Reglas de sintaxis (cómo se escribe)

| # | Regla |
| :---- | :---- |
| 1 | El archivo es **texto plano** en codificación UTF-8. |
| 2 | Todo se escribe con **etiquetas** entre `<` y `>`. |
| 3 | Toda etiqueta que se abre (`<health>`) hay que cerrarla (`</health>`), menos las **autocerradas**, que terminan en `/>` (por ejemplo `<attack .../>`). |
| 4 | Las etiquetas tienen que **abrirse y cerrarse en orden**: `<a><b></b></a>` está bien; `<a><b></a></b>` está mal. |
| 5 | Los nombres de etiquetas y atributos van **en inglés y en minúsculas**, sin tildes ni espacios. Distingue mayúsculas de minúsculas (*case sensitive*). |
| 6 | Los **atributos** se escriben como `name="value"` y el valor siempre va entre **comillas dobles**. |
| 7 | Los **comentarios** son líneas que empiezan por `#`. El programa las ignora. |
| 8 | Solo hay **un** elemento `<brawler>` por archivo. |
| 9 | Los espacios y saltos de línea entre etiquetas no significan nada (la sangría es solo para que se lea mejor). |

### 2.4 Elementos del lenguaje

#### `<brawler>`: elemento raíz (obligatorio)

Contiene todo el personaje. **Sus atributos son los metadatos del documento.**

| Atributo | ¿Obligatorio? | Qué es | Valores |
| :---- | :---: | :---- | :---- |
| `name` | Sí | Nombre del brawler | Texto, de 2 a 20 caracteres |
| `rarity` | Sí | Rareza | `Starter`, `Rare`, `Super Rare`, `Epic`, `Mythic`, `Legendary` |
| `class` | Sí | Rol en el juego | `Damage Dealer`, `Tank`, `Marksman`, `Artillery`, `Controller`, `Assassin`, `Support` |
| `author` | No | Quién lo ha creado | Texto |
| `version` | No | Versión de la definición | Formato `número.número` (ej. `1.0`) |

#### `<stats>`: obligatorio

Agrupa las características básicas. Sus elementos llevan **texto dentro**.

| Elemento | ¿Obligatorio? | Qué es | Rango |
| :---- | :---: | :---- | :---- |
| `<health>` | Sí | Puntos de vida | 1 – 20000 |
| `<speed>` | Sí | Velocidad de movimiento | 1 – 1000 |
| `<reload>` | No | Tiempo de recarga de munición, en ms | 100 – 10000 |

```
<stats>
    <health>3200</health>
    <speed>720</speed>
</stats>
```

#### `<attack>`: obligatorio (autocerrado)

Define el ataque básico.

| Atributo | ¿Obligatorio? | Qué es | Valores |
| :---- | :---: | :---- | :---- |
| `name` | Sí | Nombre del ataque | Texto |
| `damage` | Sí | Daño por impacto | Entero, 1 – 10000 |
| `range` | Sí | Distancia en casillas | Entero, 1 – 20 |
| `type` | Sí | Forma del ataque | `projectile`, `burst`, `melee`, `thrown` |

#### `<super>`: obligatorio (autocerrado)

Define la habilidad especial.

| Atributo | ¿Obligatorio? | Qué es | Valores |
| :---- | :---: | :---- | :---- |
| `name` | Sí | Nombre de la súper | Texto |
| `damage` | Sí | Daño de la súper | Entero, 0 – 20000 (0 si no hace daño) |
| `radius` | No | Radio de la súper | Entero, 1 – 10 |

#### `<gadget>`: opcional (máximo 2)

| Atributo | ¿Obligatorio? | Qué es |
| :---- | :---: | :---- |
| `name` | Sí | Nombre del gadget |
| `effect` | Sí | Descripción de lo que hace |

#### `<starpower>`: opcional (máximo 2)

Mismos atributos que `<gadget>` (`name`, `effect`).

#### `<hypercharge>`: opcional (máximo 1)

Mismos atributos que `<gadget>` (`name`, `effect`).

#### `<balance>`: opcional (autocerrado)

Puntuación orientativa del 1 al 10 para valorar el personaje.

| Atributo | ¿Obligatorio? | Qué es | Valores |
| :---- | :---: | :---- | :---- |
| `damage` | Sí | Ataque | Entero, 1 – 10 |
| `defense` | Sí | Resistencia | Entero, 1 – 10 |
| `mobility` | Sí | Lo fácil que es moverse | Entero, 1 – 10 |

### 2.5 Reglas semánticas (de contenido)

1. Todos los valores numéricos tienen que ser **enteros positivos**, sin decimales ni signo.
2. Cada valor numérico tiene que estar dentro de su **rango**.
3. Los atributos `rarity`, `class` y `type` solo aceptan los **valores de la lista**.
4. Las secciones obligatorias (`stats`, `attack`, `super`) tienen que aparecer **exactamente una vez**.
5. No se pueden repetir elementos que no lo permiten (por ejemplo, tres `<gadget>`).
6. El orden de los elementos dentro de `<brawler>` es libre.

### 2.6 Tabla de traducción (por si nos preguntan)

| Concepto | Etiqueta / atributo |
| :---- | :---- |
| Personaje | `brawler` |
| Estadísticas | `stats` |
| Vida | `health` |
| Velocidad | `speed` |
| Recarga | `reload` |
| Ataque | `attack` |
| Daño | `damage` |
| Alcance | `range` |
| Radio | `radius` |
| Hipercarga | `hypercharge` |
| Equilibrio | `balance` |
| Defensa / Movilidad | `defense` / `mobility` |
| Rareza / Clase | `rarity` / `class` |
| Autor / Versión | `author` / `version` |

---

## 3. Clasificación de BrawlML

Los lenguajes de marcas se pueden clasificar según lo que expresan sus marcas:

| Tipo | Qué describe | Ejemplo |
| :---- | :---- | :---- |
| **Procedimental / de presentación** | *Cómo* se tiene que ver o tratar el contenido (negrita, tamaño, color…) | RTF, LaTeX básico, HTML antiguo (`<b>`, `<font>`) |
| **Descriptivo / semántico** | *Qué* es cada parte del contenido, sin decir cómo se muestra | XML, HTML5 semántico (`<article>`, `<nav>`) |

### Nuestra clasificación: **descriptivo / semántico**

#### Por qué

- Las etiquetas dicen **qué es** cada dato: `<health>` es la vida, `<attack>` es el ataque, `<super>` es la súper. No dicen si se tiene que ver en rojo, en negrita o a tamaño 14.
- El archivo **no tiene ninguna instrucción de presentación**. El mismo archivo `.bml` se puede convertir en una carta de juego, una página web, un PDF o una fila de una base de datos, según el programa que lo lea.
- **Cómo se muestran** los datos es cosa del programa (o de una plantilla), no del documento.
- Así se pueden **reutilizar** los mismos datos en muchos sitios distintos.

---

## 4. Caso de uso donde BrawlML encaja bien

### Editor de brawlers de la comunidad (hechos por fans)

Imagina una web donde los jugadores pueden crear sus propios brawlers y compartirlos.

**Cómo ayuda BrawlML:**

1. El usuario escribe o genera el archivo `.bml` de su personaje.
2. El programa **valida** el archivo: comprueba que tiene las secciones obligatorias y que los valores están dentro de los rangos.
3. El programa genera automáticamente una **carta visual** con el nombre, las estadísticas y las habilidades.
4. Los personajes se guardan en una base de datos y se pueden **comparar** u ordenar por rareza, vida, daño, etc.

**Por qué encaja:**

- Los datos están **organizados y son predecibles**, así que un programa los procesa fácil.
- Todos los brawlers siguen el mismo formato, así que es fácil hacer **listas, rankings y filtros**.
- Es **texto plano**: se puede editar con el Bloc de notas y compartir por chat o por correo.
- Separa los datos del diseño: se puede cambiar el aspecto de las cartas sin tocar los archivos.
- Los nombres en inglés coinciden con los del juego, así que cualquier jugador entiende el archivo.

Otros usos que encajan: documentar un juego propio, equilibrar personajes de un mod, generar automáticamente una guía o una wiki.

---

## 5. Ejemplo de uso

### 5.1 Archivo `amber.bml`

```
# Definition of the brawler Amber
# Created by Group 3

<brawler name="Ember" rarity="Legendary" class="Asassin" author="Group3" version="1.0">

    <stats>
        <health>3200</health>
        <speed>720</speed>
        <reload>1400</reload>
    </stats>

    <attack name="Cinder" damage="900" range="8" type="projectile"/>

    <super name="Eruption" damage="1500" radius="3"/>

    <gadget name="Thick Smoke" effect="Hides the brawler for 3 seconds"/>

    <starpower name="Eternal Flame" effect="Burns enemies for 200 damage per second"/>

    <balance damage="8" defense="4" mobility="6"/>

</brawler>
```

### 5.2 Explicación línea por línea

| Fragmento | Qué significa |
| :---- | :---- |
| `# Definition...` | Comentarios. El programa los ignora. |
| `<brawler name="Amber" rarity="Legendary" ...>` | Abre el elemento raíz. Los atributos son los **metadatos**: nombre, rareza, clase, autor y versión. |
| `<stats>...</stats>` | Sección con tres elementos de texto: health 3200, speed 720 y reload 1400 ms. |
| `<attack name="Cinder" .../>` | Ataque básico autocerrado: hace 900 de daño, tiene 8 casillas de alcance y es de tipo proyectil. |
| `<super name="Eruption" .../>` | Súper que hace 1500 de daño en un radio de 3 casillas. |
| `<gadget .../>` | Gadget opcional con su nombre y efecto. |
| `<starpower .../>` | Star power opcional. |
| `<balance .../>` | Valoración opcional de 1 a 10 en daño, defensa y movilidad. |
| `</brawler>` | Cierra el elemento raíz. |

### 5.3 Ejemplos de errores

| Archivo incorrecto | Qué falla |
| :---- | :---- |
| `<health>3200` (sin cerrar) | Etiqueta sin cerrar (rompe la regla 3). |
| `<attack name=Cinder damage=900 .../>` | Atributos sin comillas dobles (regla 6). |
| `<brawler name="Amber" class="Mage" ...>` | `Mage` no es un valor permitido para `class` (regla semántica 3). |
| `<health>-500</health>` | Valor negativo y fuera de rango (reglas semánticas 1 y 2). |
| Archivo sin `<super>` | Falta una sección obligatoria (regla semántica 4). |
| `<Health>3200</Health>` | Mayúsculas en el nombre de la etiqueta (regla 5). |

---

## 6. Caso de uso donde BrawlML NO sería lo mejor

### Describir animaciones, modelos 3D o comportamiento en tiempo real

BrawlML **no sirve** para definir:

- **Animaciones** del personaje (mover los brazos, efectos de partículas…).
- **Modelos 3D, texturas o sprites**.
- **La lógica del juego**: cómo se comporta la IA, qué pasa cuando hay una colisión, cómo se calcula el daño en situaciones especiales.
- **Comportamiento dinámico** (por ejemplo, "si la vida baja del 30 %, sube la velocidad").

**Por qué no sirve:**

- BrawlML solo **describe datos fijos**: no tiene condiciones, bucles ni variables.
- No puede expresar **lógica ni cálculos**. Para eso hace falta un lenguaje de programación (Python, C#, Lua…) o un motor de juego (Unity, Godot).
- No puede guardar **datos binarios** (imágenes, modelos, sonidos) de forma eficiente, porque es texto plano.
- Para **muchísimos datos** (miles de brawlers con muchas estadísticas), un formato como JSON o una base de datos sería más eficiente.

---

## 7. Por qué BrawlML es un lenguaje de marcas

Cumple los **cuatro requisitos** del enunciado:

| Requisito | Cómo lo cumple BrawlML |
| :---- | :---- |
| **Está basado en texto plano** | Los archivos `.bml` son texto plano UTF-8 que se abre y se edita con cualquier editor (Bloc de notas, VS Code, etc.). No hay ningún formato binario. |
| **Usa metadatos** (no es metalenguaje) | Las etiquetas y los atributos son **datos sobre los datos**: `rarity="Epic"`, `class="Marksman"`, `author="Group3"` o `version="1.0"` explican qué es el contenido y de dónde viene. Eso son metadatos, no un metalenguaje (BrawlML no se usa para definir otros lenguajes). |
| **Facilita el procesamiento automático** | La sintaxis es estricta y predecible (etiquetas cerradas, valores entre comillas, rangos definidos), así que un programa puede leer, validar y sacar los datos sin dudas. |
| **Facilita la representación / interpretación** | Las etiquetas tienen nombres fáciles de entender (`<health>`, `<attack>`, `<super>`), así que una persona las entiende de un vistazo. Un programa puede convertir los mismos datos en una carta, una página web o un PDF. |

Además, es un lenguaje **descriptivo**: marca el significado de cada parte del contenido y separa claramente **los datos** de **la presentación**, que es lo esencial de los lenguajes de marcas.
