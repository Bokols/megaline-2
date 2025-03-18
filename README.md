# Descripción del Proyecto: Modelo de Recomendación de Planes de Clientes de Megaline

## Introducción

Megaline, una compañía de telefonía móvil, ha identificado un desafío significativo: muchos de sus clientes siguen utilizando planes móviles heredados a pesar de que existen opciones más nuevas y completas. Para abordar este problema, Megaline tiene como objetivo desarrollar un modelo de aprendizaje automático que pueda predecir a cuál de los dos planes disponibles (Smart o Ultra) es probable que un cliente cambie, en función de su comportamiento de uso. La compañía tiene acceso a datos de clientes que ya han hecho la transición a uno de los nuevos planes, y el objetivo es crear un modelo con una precisión de al menos 0.75 (75%) que pueda recomendar el plan adecuado a los clientes potenciales.

## Resumen del Proyecto

El proyecto involucra los siguientes pasos:

1. **Exploración de los Datos**: Analizar los datos de comportamiento de los clientes para comprender su estructura y características.
2. **División de los Datos**: Dividir los datos en conjuntos de entrenamiento, validación y prueba para garantizar una evaluación adecuada del modelo.
3. **Desarrollo del Modelo**: Entrenar tres modelos de aprendizaje automático: Regresión Logística, Árbol de Decisión y Bosque Aleatorio para predecir el plan (Smart o Ultra) basado en el comportamiento del cliente.
4. **Evaluación del Modelo**: Evaluar los modelos utilizando las puntuaciones de precisión en el conjunto de validación.
5. **Normalización de los Datos**: Normalizar los datos y volver a evaluar los modelos para determinar si la precisión mejora.
6. **Ajuste de Hiperparámetros**: Optimizar los hiperparámetros del modelo de Bosque Aleatorio para maximizar su rendimiento.
7. **Evaluación Final**: Probar el mejor modelo en el conjunto de prueba para evaluar su capacidad de generalización.
8. **Prueba de Realidad**: Realizar una prueba de realidad creando un modelo base basado en las proporciones de cada plan en el conjunto de datos.

El objetivo general es crear un modelo que no solo cumpla con la precisión objetivo, sino que también proporcione información valiosa para que Megaline pueda orientar mejor a sus clientes y mejorar sus estrategias de marketing.

## Exploración de los Datos

El conjunto de datos consta de 3,214 observaciones y 5 características:

- **calls**: Número de llamadas realizadas por el cliente en un período determinado.
- **minutes**: Duración total de las llamadas en minutos.
- **messages**: Número de mensajes de texto enviados por el cliente.
- **mb_used**: Uso de datos en megabytes.
- **is_ultra**: Variable objetivo que indica el plan actual del cliente (0 para Smart, 1 para Ultra).

Al revisar los datos, la distribución muestra que aproximadamente el 69% de los clientes están en el plan Smart y el 31% están en el plan Ultra. Las características son numéricas y no hay valores faltantes en el conjunto de datos. Las estadísticas descriptivas básicas confirman que los datos van de 0 a un número alto de llamadas, minutos, mensajes y uso de datos.

## División de los Datos

Para evaluar adecuadamente el rendimiento del modelo, el conjunto de datos se divide en tres subconjuntos:

- **Conjunto de Entrenamiento (60%)**: Utilizado para entrenar los modelos.
- **Conjunto de Validación (20%)**: Utilizado para evaluar el rendimiento del modelo y ajustar los hiperparámetros.
- **Conjunto de Prueba (20%)**: Utilizado para evaluar la capacidad de generalización del modelo.

Esta división asegura que los modelos se entrenen con una parte de los datos, se validen con otra y se prueben con un conjunto de datos completamente nuevo.

## Desarrollo y Evaluación del Modelo

Se eligieron tres modelos de aprendizaje automático para la tarea de clasificación:

1. **Regresión Logística**: Un modelo fundamental adecuado para problemas de clasificación binaria.
2. **Árbol de Decisión**: Un modelo versátil que puede manejar relaciones no lineales.
3. **Bosque Aleatorio**: Un método de ensamblaje que combina múltiples árboles de decisión para mejorar el rendimiento.

Cada modelo fue entrenado en el conjunto de entrenamiento y evaluado en el conjunto de validación. Las puntuaciones de precisión para cada modelo fueron las siguientes:

- **Regresión Logística**: 70.45% (No cumple con el umbral del 75%).
- **Árbol de Decisión**: 74.65% (Cumple con el umbral).
- **Bosque Aleatorio**: 80.09% (Supera el umbral y es el mejor desempeño).

## Normalización de los Datos

Para mejorar el rendimiento de los modelos sensibles a la escala de los datos, como la Regresión Logística, se normalizaron las características utilizando el StandardScaler. Los resultados después de la normalización fueron los siguientes:

- **Regresión Logística**: 74.96% (Mejora).
- **Árbol de Decisión**: 74.65% (Sin cambios).
- **Bosque Aleatorio**: 80.09% (Sin cambios).

La normalización ayudó a mejorar el rendimiento del modelo de Regresión Logística, pero no afectó significativamente a los modelos de Árbol de Decisión ni de Bosque Aleatorio, que ya estaban rindiendo bien.

## Ajuste de Hiperparámetros

Dado el excelente rendimiento del modelo de Bosque Aleatorio, el ajuste de hiperparámetros se centró en este modelo para mejorar su rendimiento. Utilizando **RandomizedSearchCV**, se optimizaron los siguientes hiperparámetros:

- **n_estimators**: Número de árboles en el bosque.
- **max_depth**: Profundidad máxima de cada árbol.
- **min_samples_split**: Número mínimo de muestras requeridas para dividir un nodo interno.
- **min_samples_leaf**: Número mínimo de muestras requeridas para ser un nodo hoja.

Los hiperparámetros optimizados fueron:

- **n_estimators**: 100
- **max_depth**: 10
- **min_samples_split**: 5
- **min_samples_leaf**: 1

Después de aplicar los hiperparámetros óptimos, el modelo de Bosque Aleatorio logró una precisión de 80.09% en el conjunto de validación.

## Evaluación Final

El modelo de Bosque Aleatorio con los hiperparámetros optimizados fue evaluado en el conjunto de prueba para evaluar su capacidad de generalización. La precisión en el conjunto de prueba fue del 79.63%, lo que está muy cerca del rendimiento en el conjunto de validación, lo que indica que el modelo se generaliza bien a datos no vistos.

## Prueba de Realidad

Para validar el rendimiento del modelo, se realizó una prueba de realidad comparando la precisión del modelo con un modelo base que predice la clase mayoritaria (plan Smart) según la distribución de planes en el conjunto de datos. El modelo base predijo el plan Smart para todos los usuarios y logró una precisión del 69.35%. En contraste, el modelo de Bosque Aleatorio entrenado tuvo un rendimiento significativamente mejor con una precisión del 79.63%, demostrando que el modelo captura patrones significativos en los datos y supera las estrategias simples.

## Hallazgos

- **Regresión Logística**: Aunque es adecuado para la clasificación binaria, este modelo no cumplió con el umbral de precisión del 75%.
- **Árbol de Decisión**: Un modelo razonablemente bueno, logrando una precisión del 74.65%, pero ligeramente por debajo del umbral deseado.
- **Bosque Aleatorio**: El modelo más efectivo, alcanzando una precisión del 80.09%, tanto antes como después del ajuste de hiperparámetros. Este modelo fue claramente el mejor.
- **Normalización de Datos**: Mejoró el rendimiento del modelo de Regresión Logística, pero no afectó a los modelos de Árbol de Decisión ni de Bosque Aleatorio.
- **Ajuste de Hiperparámetros**: Optimizar el modelo de Bosque Aleatorio llevó a una ligera mejora en el rendimiento.
- **Prueba de Realidad**: El modelo superó significativamente al modelo base, demostrando su capacidad para aprender patrones valiosos.

## Conclusión

El proyecto logró desarrollar un modelo de aprendizaje automático capaz de predecir qué plan de Megaline (Smart o Ultra) es probable que un cliente elija según su comportamiento de uso. El modelo de Bosque Aleatorio, después de ajustarse, alcanzó una precisión del 79.63%, superando el umbral deseado del 75%. El modelo demostró la capacidad de generalizar bien a datos no vistos, lo que lo convierte en una herramienta confiable para que Megaline recomiende planes a sus clientes. Además, el modelo superó a las estrategias base simples, lo que confirma su utilidad práctica.

En general, el rendimiento del modelo de Bosque Aleatorio es satisfactorio para su implementación, y puede ayudar a Megaline a personalizar sus estrategias de marketing y mejorar la satisfacción del cliente al ofrecer recomendaciones de planes personalizadas.
