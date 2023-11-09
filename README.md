# Reporte - Laboratorio #6 y #7

Universidad del Valle de Guatemala <br>
Facultad de Ingeniería <br>
Departamento de Ciencias de la Computación <br>
Deep Learning y Sistemas Inteligentes - Sección 20

Jeyner Arango - 201106 <br> Cristian Laynez - 201281

## EDA del dataset
En ambos data set se analizo y se pudo ver que la data esta limpia, no existe data repetida ni redundante. Se encargo de llevar a cabo pasar valores de NA a -1 para una mejor administración de datos. Se encargo de guardar todos los datos ya procesados en el data set "_AllData.csv". Lo que si se llevo a cabo fue la verificacion de tipos de variables en todas las columnas para que coincidan y no existan combinaciones extrañas.

También se puede observar que la nota de calificación es de rango de 0 a 10.

EL data set de libros implementados suele estar distribuido para niños generalmente.

Se opto por tener un csv en donde se filtrara los libros con las mejores ratings para poder dar mejores recomendaciones.

## Estructura de las redes y funcionamiento
En la Red Neuronal por medio de filtros colaborativos se utilizaron 2 modelos
El primero consistía en 10 capas: 2 de ingreso de los usuarios y libros. 2 de incrustación 1 de concatenación 2 capas densas ocultas y una capa de salida.

1. Creación de la ruta de incrustación para libros y usuarios:
- Se define una entrada (input) para los libros y otra entrada para los usuarios. Esto se hace utilizando `Input` de Keras. Cada entrada tiene una forma de (1,), ya que se espera un solo valor (por ejemplo, un ID de libro o un ID de usuario) en cada paso de tiempo.
- Luego, se crea una capa de incrustación (`Embedding`) para representar los libros y los usuarios en un espacio vectorial de menor dimensión. En este caso, se utilizan incrustaciones de 5 dimensiones para ambos libros y usuarios. Estas incrustaciones se utilizan para capturar las características latentes de los libros y usuarios.

2. Concatenación de características:
- Las incrustaciones de libros y usuarios se aplanan (`Flatten`) para convertirlas en vectores unidimensionales. Esto es necesario para combinar estas características en una sola entrada para el modelo.

3. Adición de capas totalmente conectadas:
- Después de aplanar las incrustaciones, se concatenan para combinar las características de libros y usuarios en un solo vector.
- Luego, se agregan capas densas (`Dense`) para procesar estas características concatenadas. En este caso, se tienen dos capas densas con 128 y 32 unidades ocultas respectivamente, y se utiliza la función de activación ReLU (Rectified Linear Unit) en ambas capas. Estas capas aprenden patrones y relaciones entre libros y usuarios.

4. Salida del modelo:
- Finalmente, se agrega una capa densa de salida sin función de activación específica. Esta capa produce una única predicción numérica, que representa la calificación o preferencia estimada de un usuario por un libro.

Este modelo aunque en principio bien planteado y similar al segundo denota que en la fase de Test resultó peor que una distribución aleatoria de recomendaciones y denoto también una tendencia a overfitting.

El segundo modelo se basó en el desarrollo de la red basada en el Paper Neural Collaborative Filtering publicado en Agosto del 2017. Este modelo resultó bastante mejor aunque en principio la estructura es muy parecida la misma en la práctica por las capas ocultas y la forma en que fue construida tuvo una función de pérdida mucho menor.

## ¿Qué modelo funciona mejor y por qué? (Discusión)

Los modelos basados en contenido y los modelos de filtros colaborativos son dos enfoques diferentes en sistemas de recomendación, y cada uno tiene sus ventajas y desventajas. La superioridad de uno sobre el otro depende en gran medida del contexto y de los requisitos específicos de la aplicación.

### Modelos Basados en Contenido:

Ventajas
1. Explicabilidad: Los modelos basados en contenido son más transparentes y explicables en comparación con los modelos colaborativos. Puedes entender por qué un artículo se recomienda porque se basa en atributos y características específicas del contenido.
2. Fácil inicio: Estos modelos son más fáciles de implementar, especialmente si tienes información detallada sobre los elementos a recomendar.
3. Recomendaciones personalizadas: Los modelos basados en contenido son buenos para proporcionar recomendaciones altamente personalizadas. Pueden tener en cuenta las preferencias del usuario basadas en su historial.
4. Bueno en problemas de arranque en frío: Son menos propensos a sufrir problemas de arranque en frío, ya que pueden realizar recomendaciones basadas en el contenido, incluso cuando no hay suficiente información de interacción de usuario.

Desventajas:
1. Falta de coincidencias: Pueden tener dificultades para descubrir nuevos elementos que no estén directamente relacionados con las preferencias pasadas del usuario.
2. Dependencia de metadatos de alta calidad: Los modelos basados en contenido dependen de metadatos precisos y relevantes sobre los elementos.

### Modelos de Filtros Colaborativos

Ventajas

1. Descubrimiento de elementos no explorados: Los modelos colaborativos pueden sugerir elementos que son populares entre usuarios con preferencias similares, lo que puede llevar a descubrimientos inesperados.
2. No requieren metadatos detallados: No dependen de metadatos detallados sobre los elementos, lo que los hace útiles cuando la información sobre los elementos es limitada.

Desventajas

1. Problemas de arranque en frío: Son vulnerables a problemas de arranque en frío, ya que necesitan suficientes datos de interacción de usuario para hacer recomendaciones efectivas.
2. Falta de explicabilidad: A menudo, los modelos colaborativos no proporcionan una explicación clara de por qué se realiza una recomendación.
3.  Problemas de escalabilidad: Los modelos colaborativos pueden enfrentar problemas de escalabilidad cuando se tienen grandes conjuntos de datos y usuarios.

Aunque no hay un enfoque superior en general la elección entre modelos basados en contenido y modelos de filtros colaborativos depende de factores como la disponibilidad de datos, el nivel de personalización deseado, la importancia de la explicabilidad y otros factores específicos de la aplicación. En el caso de estas bases de datos más extensas y con poca metadata el modelo colaborativo resultó de mejor calidad ya que la información de los libros resultó limitada al no conocer siquiera el género del libro aunque pueda que haya una correlación fuerte entre el autor y el género.
