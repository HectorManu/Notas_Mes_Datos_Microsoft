# Introducción

Los modelos de aprendizaje automático son algoritmos informáticos que usan datos para realizar estimacinose (suposiciones fundamentadas) o tomar decisiones. 

# ¿Qué son los modelos de aprendizaje automático? 
- Compoennte principal del aprendizaje automático y,  en definitiva, los que estmoas intentando crear. Un modelo podría calcular la edad de una persona a partid de una foto, predecir lo que le gustaría ver en las redes sociales o decidir hacia dónde se debe mover un brazo robótico. En nuestro escenario, queremos crear un modelo que pueda calcular la talla de botas más adecuada para un perro en funciín de a talla de arnés.

Los modelos se pueden crear de muchas manera. Por ejemplo, son las personas la que crean un modelo tradicional que simula el vuelo de un avión, con conocimientos de física e ingenería. Los modelos de aprendizaje automático son especiales, ya que en lugar de editarlos las persona para que sea eficaes, son los datos lo que les dan forma, es decir, aprende de la experiencia.

## Planteamintos sobre los modelos

Un modelo puede plantearse ocmo una función que acepta datos como entrada y genra una salida. En concreto, un modelo usa los datos de entrada para cacular otra cosa. Por ejemplo, en nuestro escenario, queremos crear un mdelo al que se le proporciona una talla de arnés y calcula la tall ad unas botas:



# Mejora y prueba de modelos de MACHIN LEARNING
El siguiente material de aprendizaje le proporcion aalguas razones sencilla por las que se reliza el ajuste deficiente y sobreajuste, así como lo que se puede hacer al respecto..

## Normalización y estandarizción

El *escaldo de características* es una técnica que cambia el intervalo de valores que tiene una característica. Esto permite a los modelos aprender de forma más rápida y sólida.

### Normalización frente a estadnrización

La *normalización significa modificar la escala de los valores para que todos se ajusten a un intervalo determinado* (**normalmente de 0 a 1**). Por ejemplo, si tuviera una lista de edades de persona de 0, 500 y 100 añoz, podría normalzarlas diviendo las edades entre 100, de modo que los **los valores fueran 0, 0,5 y 1**. | La *Estandrización es similar, pero, en su lugar, se resta la media de valores y después, se divide por la desviación estándar.*. Significa que, **después de la estandarización, el valor medio es 0 y aproximadamente el 95% de los valores se encuentra entre -2 y 2**.


### ¿Por qué es necesario modificar la escala?

### La estandarízación permite que los parámetros se entrenen a la misma velocidad 

### El escalado ayuda con 

1. ¿Cuál es la ventaja de normalizar los datos?

Tiempos de entrenamiento más rápidos
Correcto. El tiempo de entrenamiento suele ser más rápido cuando se normalizan los datos.


Eliminación más precisa de los valores que faltan

Identificación de algoritmos preferentes para el entrenamiento
2. Un modelo que está entrenando funciona bien en su conjunto de entrenamiento, pero mal en su conjunto de prueba. ¿Qué es lo más probable que ocurra?

Se ha producido un ajuste deficiente y el modelo no es lo suficientemente preciso: debe seguir entrenándolo.

Se ha producido un sobreajuste y el modelo no funciona bien con nuevos datos fuera del entrenamiento: podría detener el entrenamiento antes o recopilar datos más diversos.
Correcto. Es probable que se haya producido un sobreajuste y puede ajustar el entrenamiento para mejorar el rendimiento en el conjunto de pruebas. Debe tener en cuenta si necesita datos de entrenamiento más diversos o si realiza el entrenamiento durante demasiado tiempo.


El modelo está bien; en su lugar, debe usar los datos de entrenamiento para probarlo.
3. El modelo se va a usar en una aplicación compleja, en la que se le exige un rendimiento muy fiable. ¿Cuál es el método adecuado para probar la confiabilidad de sus modelos en situaciones difíciles?

Creación de un conjunto de entrenamiento mayor

Utilice el enfoque de exclusión y cree un tercer conjunto de datos especial que incluya ejemplos en los que el resultado de sus modelos deba cumplir con los umbrales de rendimiento.
Correcto. Puede probar mejor la confiabilidad de sus modelos en situaciones difíciles con un tercer conjunto de datos, que se ha creado específicamente para medir la fiabilidad de su aplicación.


Controle el costo mientras realiza el entrenamiento (cualquier variabilidad) y, si quiere, puede detener el entrenamiento.
Incorrecto. Aunque mantenerse al tanto de los costos durante el entrenamiento puede ayudar a evitar el sobreajuste, debe buscar otras maneras de probar la confiabilidad de los modelos



# Entrenamiento y evaluación de modelos de regresión 

**Los modelos usarn la *regresión* para predecir un  número**.

Responda a las siguientes preguntas para comprobar lo que ha aprendido.

1.Está usando Scikit-learn para entrenar un modelo de regresión a partir de un conjunto de datos de ventas. Quiere poder evaluar el modelo para asegurar que predecirá con precisión con datos nuevos. ¿Qué tiene que hacer?

Usar todos los datos para entrenar el modelo. Luego usar todos los datos para evaluarlo.

Entrenar el modelo usando solo las columnas de características y, luego, evaluarlo usando solo la columna de etiquetas.

Dividir los datos de forma aleatoria en dos subconjuntos. Usar un subconjunto para entrenar el modelo y el otro para evaluarlo.
Correcto. Una forma habitual de entrenar y evaluar modelos es retener un conjunto de datos de evaluación al entrenar.

2.Ha creado un objeto de modelo mediante la clase LinearRegression de Scikit-learn. ¿Qué debe hacer para entrenar el modelo?

Llamar al método predict() del objeto model, especificando la característica de entrenamiento y las matrices de etiquetas.
Eso es incorrecto. Para entrenar un modelo, use el método fit().


Llamar al método fit() del objeto model, especificando la característica de entrenamiento y las matrices de etiquetas.
Correcto. Para entrenar un modelo, use el método fit().


Llamar al método score() del objeto model, especificando la característica de entrenamiento y las matrices de características de prueba.
3.Entrena un modelo de regresión mediante Scikit-learn. Al evaluarlo con datos de prueba, calcula que el modelo logra una métrica de R al cuadrado de 0,95. ¿Qué le indica esta métrica sobre el modelo?

El modelo explica la mayor parte de la varianza entre los valores predichos y los reales.
Correcto. La métrica de R al cuadrado es una medida de cuánto de la varianza se puede explicar por el modelo.


El modelo tiene una precisión del 95 %.
Eso es incorrecto. La métrica de R al cuadrado es una medida de cuánto de la varianza se puede explicar por el modelo.


En promedio, las predicciones son 0,95 mayores que los valores reales.








# Creación y derecripción de modelos de clasificación en el apredizaje automático

## Intruducción 
- Las salidas de los modelos de clasificacón son categóricas, lo que significa que se pueen usar para etiquetar entrada o tomar decisiones. Por ejemplo, un automóvvil sin conductor usa la clasificación para decidir si girara a la izquirda  o a la drecha en una bifurcación de la carretera. Un modelo de clasificación difiere de los mdoelso de regresión clásica en que las salidads son contiuas, como el tamaño de un zapato o la velocidad de un tren. Los modelos de clasificación son diversos en su forma de funcnionar para empezar nos centramos en la regresión logística, que es un tipo de modelo más sencillo y popular que se usa ampleamente en muchos sectores de la ciencia y de la industria.

### Escenario: Predicción de aludes con aprendizaje automático
