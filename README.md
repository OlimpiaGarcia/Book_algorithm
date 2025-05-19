# Book Recomendation Algorithm

## Descripción del problema:
Goodreads es una red social para lectores, que busca ayudarlos a descubrir más libros, mantener el progreso de sus lecturas y obtener recomendaciones.[2]

Sin embargo, como lector quieres obtener una recomendación basada en un libro que te encantó o en varios que te recomendaron, y ahí reside nuetra problematica a resolver:

Dado un input de 3 libros, dar un output de 5 libros recomendados, estas recomendaciones pueden ser por Género o por Rating


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
Se utilizó **multi-hot encoding:** 

Cuando una fila puede tener más de una clase al mismo tiempo (como múltiples géneros por libro)[3], se usa multi-hot encoding. Cada género se convierte entonces en una columna binaria (1 si el libro pertenece a ese género, 0 si no)

1. **Limpieza de datos (clean_data):**

    Se utilizó _ast.literal_eval_ para convertir las cadenas de texto que representaban listas en listas reales de Python. Luego, se eliminaron los espacios adicionales al inicio y al final de cada género, asegurando así una limpieza adecuada. Todos los géneros únicos fueron almacenados en un conjunto (set), lo que garantizó que no hubiera repeticiones.

2. **Ordenamiento de géneros:**

    Una vez identificados los géneros únicos, estos se ordenaron alfabéticamente. Aunque no es un paso obligatorio, esta organización se decidió para facilitar la estructura del dataset y su posterior análisis.

3. **Codificación (hot_encoder):**

    Para cada género único, se creó una nueva columna en el DataFrame con el mismo nombre del género. En cada una de estas columnas, se asignó un 1 si ese género estaba presente en la lista de géneros del libro correspondiente, o un 0 si no lo estaba. Este enfoque permitió representar múltiples géneros para un mismo libro de forma binaria.

4. **Eliminación de la columna original genres:**

    Finalmente, se eliminó la columna original genres, ya que toda su información había sido distribuida en las nuevas columnas binarias, una por cada género.

## Scalation:

Se utilizó **normalización Z-Score:**

Tambien conocida como estandarización. Este método transforma cada valor numérico restando la media de su columna y dividiendo entre su desviación estándar. Como resultado, cada característica numérica queda centrada en una media de 0 y una desviación estándar de 1. Esto ayuda a evitar que las variables con escalas mayores dominen el modelo.[4]

1. **Identificación de columnas numéricas**

   Se analizaron los tipos de datos del DataFrame y se seleccionaron todas las columnas numéricas. Las columnas no numéricas se excluyeron del proceso de escalado para evitar errores

2. **Se estandarizaron los datos**

    Se usó la lbreria de SkLearn de _StandardScaler_ para realizar la estandarización de los datos

3. **Reconstrucción del DataFrame**

   Una vez estandarizadas las columnas numéricas, se reconstruyó el DataFrame combinando estos datos con las columnas no numéricas originales, como los títulos de libros, autores y descripciones. Esto permite conservar toda la información.


Esta fue elegida porque se planea usar un **modelo de clasificación**, aunque aún no se ha definido cuál. Dado que muchos algoritmos de clasificación son sensibles a la escala (por ejemplo, SVM, KNN, regresión logística), la estandarización asegura que todas las características contribuyan de manera equitativa al proceso de aprendizaje.[5]

# Biibliografia:
[1]Grimm, “Books_Dataset_GoodReads(May 2024),” Kaggle.com, 2024. https://www.kaggle.com/datasets/dk123891/books-dataset-goodreadsmay-2024?select=Book_Details.csv (accessed May 19, 2025).

[2]“About Goodreads,” Goodreads.com, 2025. https://www.goodreads.com/about/us (accessed May 19, 2025).

[3]GeeksforGeeks, “An introduction to MultiLabel classification,” GeeksforGeeks, Jul. 15, 2020. https://www.geeksforgeeks.org/an-introduction-to-multilabel-classification/ (accessed May 19, 2025).

[4]GeeksforGeeks, “ZScore Normalization: Definition and Examples,” GeeksforGeeks, Jul. 26, 2024. https://www.geeksforgeeks.org/z-score-normalization-definition-and-examples/ (accessed May 19, 2025).

[5]I. Belcic, “Classification in Machine Learning,” Ibm.com, Oct. 15, 2024. https://www.ibm.com/think/topics/classification-machine-learning (accessed May 19, 2025).
