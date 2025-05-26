# Book Recomendation Algorithm

## Descripción del problema:
Goodreads es una red social para lectores, que busca ayudarlos a descubrir más libros, mantener el progreso de sus lecturas y obtener recomendaciones.[2]

Sin embargo, como lector quieres obtener una recomendación basada en un libro que te encantó o en varios que te recomendaron, y ahí reside nuetra problematica a resolver:

Dado un input de 1 libro, se da un output  de 10 libros con caracteristicas similares y se calcula que tan bueno es el libro que se está recomendando, según la categoria de Rating a la que pertenecen:

| Rango rating | Clase | Descripción |
| ------------ | ----- | ----------- |
| <= 2.5       | 0     | Malo        |
| >2.5 y <=2.9 | 1     | Regular     |
| >3 y <=4.0   | 2     | Bueno       |
| >4.0         | 3     | Excelente   |



## DataSet:

**Descripción:**
Este conjunto de datos ofrece una colección completa de información proveniente de GoodReads, una plataforma popular para que los lectores descubran y reseñen libros. Incluye información sobre libros, detalles de los libros y reseñas de usuarios, ofreciendo perspectivas sobre el mundo literario. \[1]

**Columnas:**[1]

1. **book\_id:** Un identificador único para cada libro en el conjunto de datos.

2. **cover\_image\_uri:** La URI (Identificador Uniforme de Recursos) o URL que apunta a la imagen de portada del libro.

3. **book\_title:** El título del libro.

4. **book\_details:** Detalles adicionales sobre el libro, como un resumen, sinopsis del argumento u otra información descriptiva.

5. **format:** El formato del libro, como tapa dura, libro de bolsillo, libro electrónico, audiolibro, etc.

6. **publication\_info:** Información sobre la publicación del libro, incluyendo la editorial, la fecha de publicación y cualquier otro dato relevante.

7. **authorlink:** La URI o URL que dirige a más información sobre el autor, si está disponible.

8. **author:** El nombre del autor o los autores del libro.

9. **num\_pages:** La cantidad de páginas que tiene el libro. 

10. **genres:** El género al que pertenece el libro, puede ser uno o varios

### División del DataSet
Se dividió en 80% para datos de entrenamiento y 20% restante para test

## Encoding:
### Género:
Se utilizó **multi-hot encoding:** 

Cuando una fila puede tener más de una clase al mismo tiempo (como múltiples géneros por libro)[3], se usa multi-hot encoding. Cada género se convierte entonces en una columna binaria (1 si el libro pertenece a ese género, 0 si no)

1. **Limpieza de datos (clean_data):**

    Se utilizó `_ast.literal_eval_` para convertir las cadenas de texto que representaban listas en listas reales de Python. Luego, se eliminaron los espacios adicionales al inicio y al final de cada género, asegurando así una limpieza adecuada. Todos los géneros únicos fueron almacenados en un conjunto (set), lo que garantizó que no hubiera repeticiones.

2. **Ordenamiento de géneros:**

    Una vez identificados los géneros únicos, estos se ordenaron alfabéticamente. Aunque no es un paso obligatorio, esta organización se decidió para facilitar la estructura del dataset y su posterior análisis.

3. **Codificación (hot_encoder):**

    Para cada género único, se creó una nueva columna en el DataFrame con el mismo nombre del género. En cada una de estas columnas, se asignó un 1 si ese género estaba presente en la lista de géneros del libro correspondiente, o un 0 si no lo estaba. Este enfoque permitió representar múltiples géneros para un mismo libro de forma binaria.

4. **Eliminación de la columna original genres:**

    Finalmente, se eliminó la columna original `genres`, ya que toda su información había sido distribuida en las nuevas columnas binarias, una por cada género.

### Autor: 
Se utilizó **multi-hot encoding** para los autores:

1. **Limpieza de datos (clean_authors):**

   Se creó una función llamada `clean_authors` para normalizar los nombres de los autores. Esta función realiza dos tareas principales:
   
    - **Eliminación de espacios innecesarios:** Se usa `.strip()` para quitar los espacios al inicio y al final de cada nombre de autor.
    - **Manejo de errores:** Si un campo de autor está vacío o es NaN, se reemplaza con la etiqueta `'Unknown'`.
   
   Todos los autores únicos se almacenan en un conjunto (`set`) para evitar repeticiones y se devuelven como una lista.

2. **Ordenamiento de autores:**

   Una vez obtenida la lista de autores únicos, se ordenan alfabéticamente usando `sort()`. Esto, se realiza para mantener una estructura clara y ordenada del dataset, especialmente útil para análisis posteriores o visualización.

3. **Codificación (author_hot_encoder):**

   Se creó una nueva columna binaria por cada autor único. La función `author_hot_encoder` evalúa si el nombre del autor en cada filacoincide exactamente con el autor de la columna correspondiente:
   
   - Si coincide, se asigna un 1.
   - Si no coincide se asigna un 0.

4. **Eliminación de la columna original author:**

   Tras la codificación, la columna original `author` se elimina , ya que su información ahora se encuentra distribuida en las nuevas columnas binarias.


## Escalamiento:

Se utilizó **normalización Z-Score:**

Tambien conocida como estandarización. Este método transforma cada valor numérico restando la media de su columna y dividiendo entre su desviación estándar. Como resultado, cada característica numérica queda centrada en una media de 0 y una desviación estándar de 1. Esto ayuda a evitar que las variables con escalas mayores dominen el modelo.[4]

1. **Identificación de columnas numéricas**

   Se analizaron los tipos de datos del DataFrame y se seleccionaron todas las columnas numéricas. Las columnas no numéricas se excluyeron del proceso de escalado para evitar errores

2. **Se estandarizaron los datos**

    Se usó la lbreria de SkLearn de `StandardScaler` para realizar la estandarización de los datos

3. **Reconstrucción del DataFrame**

   Una vez estandarizadas las columnas numéricas, se reconstruyó el DataFrame combinando estos datos con las columnas no numéricas originales, como los títulos de libros, autores y descripciones. Esto permite conservar toda la información.


Esta fue elegida porque se planea usar un **modelo de clasificación**, aunque aún no se ha definido cuál. Dado que muchos algoritmos de clasificación son sensibles a la escala (por ejemplo, SVM, KNN, regresión logística), la estandarización asegura que todas las características contribuyan de manera equitativa al proceso de aprendizaje.[5]


## Implementación:

### KNN 

Se uso KNN para recomendar libros a partir de un `ID` dado, esto posterioremente se cambiará para que lo que se ingrese sea un título y se busque el ID asociado al mismo.

#### Metodología:
Se recibe el `ID` del libro, el DF escalado, el DF que contiene los títulos y descripción indexados por el ID del DF numerico y se calculan los 10 vecinos más cercanos, una vez realziado esto, se buscan los ID's en el DF de texto y se despliegan en pantalla.

## Target:
Posterioremente nos preguntamos quue tan buenos son los libros que se estan recomendando.

Por lo que se procedió a crear la clase objetivo `Rating_class` la cual se muestra a continación:

| Rango rating | Clase | Descripción |
| ------------ | ----- | ----------- |
| <= 2.5       | 0     | Malo        |
| >2.5 y <=2.9 | 1     | Regular     |
| >3 y <=4.0   | 2     | Bueno       |
| >4.0         | 3     | Excelente   |

Esto con el fin de predecir en base al Rango del Rating que tan Excelente o Malo es un libro.

Para esto tuvimos que desescalar la columna `average_rating` y convertirla en la original.
Con estos datos creamos la columna rating class y la llenamos con las clases de las distintas categorias usnado cut para seleccionar los intrvalos, bins son los rangos y labels son las clases que van a quedar en la columna,

Se convirtió la columna `rating_class` a `int` y se verificó la distribución de las clases, dando como resultado un DataSet algo desbalanceado:

| rating_class | Cantidad |
|--------------|----------|
| 0            | 33       |
| 1            | 669      |
| 2            | 5348     |
| 3            | 6930     |


