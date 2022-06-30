# Cápsula endoscópica
## Descripción del proyecto:

En este proyecto se plantea, dado un video de una exploración endoscópica, poder detectar todos los frames donde se encuentra una anomalía (pólipos, sangre, úlceras, etc.). Para ello se utilizará la información proporciona por _"Kvasir-Capsule Dataset"_, el cual contiene 44 exploraciones de pacientes diferentes con un total de 47.238 frames clasificados en 14 clases diferentes. Cada una de las patologías detectadas contienen un conjunto distinto de datos, aumentando de esta manera la complejidad del problema.

Dado que ciertas patologías son menos probables de ser encontradas durante la exploración, la cantidad de los ejemplos de algunas anomalías esta desbalanceada respecto de otras. Por ejemplo, se tienen 55 imágenes de pólipos y 12 imágenes para Blood Hematin en comparación con las 866 de angiectasia.

Para resolver este reto se ha elegido un método de aprendizaje supervisado conocido como Redes Neuronales. Éstas están consideradas como muy buenas clasificadoras. De entre los diferentes tipos que hay, se ha escogido trabajar con Redes Neuronales Densas y Redes Neuronales Convolucionales. Cabe resaltar que en _Kvasir-Capsule_ utilizaron un modelo ResNet (Residual Network) y, por ello, se ha decidido probar otros tipos de redes neuronales.

Las **Redes Neuronales Densas (DNN)** son redes en las que todas las neuronas de una capa están conectadas a todas las neuronas de la siguiente. Son eficaces en cuanto al tratamiento de información porque cada neurona dispone de más datos para tratar, pero a su vez, es más lenta a la hora de procesar los datos. En este caso, al no implementar convoluciones no es capaz de definir formas y colores complejas, con lo que no es aplicable al objetivo planteado. Sin embargo, se cree que es interesante presentar esta estructura para hacer una comparativa con las CNN. Se aplican dos estructuras de DNN, las cuales se denominan D1 y D2.

Las **Redes Neuronales Convolucionales (CNN)**, son modelos multicapa, que utilizan una técnica llamada convolución para el procesamiento de los datos. Por ello, son útiles para trabajar con imágenes ya que sí que detectan rasgos geométricos y cromáticos más complejos. Generalmente las CNN, toman las imágenes como input, asignándole importancia a ciertos elementos y, así, poder diferenciar unas de otras. Se probarán 4 estructuras distintas de CNN: CNN1, CNN2, CNN3 y CNN4. Éstas se diferencian en el número de repeticiones de las capas de _convolución_ y _pooling_ que presentan en la estructura. En el caso particular de la CNN3 y CNN4, además, se les ha añadido una capa oculta de 250 neuronas después de la capa de aplastamiento dimensional.

Al igual que _Kvasir-Capsule_ nos planteamos aplicar una metodología de **2-Fold cross-validation**, la cual consiste en coger el conjunto  de datos y dividirlo en 2 partes iguales. Para este caso se ha decidido que esta partición sea en _Split 0_ y _Split 1_, y que contengan cada una el 50% del total del dataset, proporcionalmente al contenido de los 14 subdirectorios preestablecidos. Estas agrupaciones serán usadas para entrenar y validar las estructuras de redes neuronales propuestas. En concreto, se entrenará con _Split 0_ y se validará con _Split 1_ y, posteriormente, a la inversa ejecutando después la media aritmética de los resultados de ambos para obtener el resultado final.

En definitiva, se generan 3 escenarios diferentes: 14C, 10C y 2C. A cada uno de ellos se aplican las 6 estructuras de redes neuronales comentadas.


Finalmente, los resultados generados se comparan mediante matrices de confusión.

<p align="center">
<img width="460" alt="2022-06-23 15_17_06-Image classification using CNN" src="https://user-images.githubusercontent.com/87124850/175827498-a19bc99f-7d95-4a5b-abd9-f77617f72624.png">
</p>
<p align="center">
Figura 1. Fases comunes de toda Red Neuronal Convolucional
</p>

Pasos seguidos durante el proceso:

## Origen del dataset

Kvasir-Capsule es el conjunto de datos original para el desarrollo de este proyecto. Es el conjunto de datos PillCAM más grande de acceso libre publicado y proviene del repositorio oficial Open Science Framework (OSF). En total, contiene 47.238 imágenes etiquetadas y 117 vídeos, donde captura puntos de referencia anatómicos y hallazgos patológicos y normales. Generando más de 4.741.621 frames entre imágenes y vídeos. 

Puede ser descargado desde:

[https://osf.io/dv2ag/wiki/home/](url)

[https://drive.google.com/drive/u/0/folders/18vEHN1CG7oNFKdT2NmhtJjrFhb3tLG1Z](url)

[https://datasets.simula.no/kvasir-capsule/](url)


El conjunto de datos _Kvasir-Capsule_ está dividido en tres partes: imágenes etiquetadas, vídeos etiquetados y vídeos sin etiquetar. Cada parte se describe a continuación:

- **Imágenes etiquetadas**, este grupo contiene 47.238 imágenes de resolución 336x336 píxeles. En su carpeta, las imágenes se encuentran clasificadas en subdirectorios por tipo de hallazgo. El número de imágenes por clase no está balanceado. Además, las imágenes etiquetadas incluyen las coordenadas de un rectángulo perimetral sobre las anomalías detectadas, las cuales se pueden encontrar en el archivo _metadata.csv_

- **Vídeos etiquetados**, este otro grupo contiene 43 vídeos, los cuales contienen diferentes puntos de referencia anatómicos y hallazgos patológicos y normales. Esto corresponde a aproximadamente 19 horas de video, 1.955.675 frames, que pueden ser convertidos en imágenes si es necesario. Cada vídeo ha sido evaluado personalmente por un profesional médico del campo de la gastroenterología. Esta evaluación tiene como resultado el archivo _metadata.csv_ donde se pueden encontrar las 47.238 anotaciones que corresponden a los 47.238 frames facilitados.

- **Vídeos sin etiquetar**, grupo que contiene 74 vídeos sin etiquetar, no revisados por un profesional, lo que son aproximadamente 25 horas de vídeo y 2.785.829 frames de vídeo.

Del dataset orginal proporcionado, tal y como se describe anteriormente, el grupo _"videos etiquetados"_ contiene 47.238 frames categorizados por un profesional, que son los mismos encontrados en el grupo _"imágenes etiquetadas"_. Con lo que se concluye que el resto de datos de _"videos etiquetados"_ contiene información de tejido sin anomalías (mucosas, estructuras anatómicas, etc.) y, por ello, _"imágenes etiquetadas"_ será nuestro conjunto de datos a trabajar.

![dataset_prepro](https://user-images.githubusercontent.com/87124850/176562248-7714a921-009f-401c-ac61-1a62eaba7cc4.png)
<p align="center">
Figura 2. Muestras de las imágenes que contiene el dataset
</p>

La carpeta _"videos_sin_etiquetar"_ se descarta debido a que la información que contiene es poco útil para métodos de aprendizaje supervisado, que es el que se ha propuesto para este este caso.

## Preprocesado del dataset

El procesado del dataset previo a pasarlo por las estructuras de redes neuronales, consiste en un generador de imágenes, _ImageDataGenerator_, del propio módulo de Tensorflow. Dentro de este generador se define la función de preproceso _vgg16_ proviniente de otro modelo ya ajustado y probado en miles de objetos:
```
preprocessing_function = tf.keras.application.vgg16.preprocess_input
```
 _ImageDataGenerator_ ejecuta las siguientes modificaciones al input: 
- **Normalización de valores**: cambio del orden de bandas de color y zero-centered a todos los píxeles. 
- **Indicación de la ruta de origen de las imágenes**
- **Reescalado**, definición del reescalado de las imágenes a una resolución de 28x28 píxeles. 
- **Categorización**, agrupamiento de las imágenes en 14, 10 y 2 categorías.
- **Batch size**, definición del empaquetamiento o lote de las imágenes salientes en 128 (2^7).

Con este procesado se genera un array que contiene las imágenes con sus etiquetas asociadas, obteniendo un conjunto de datos listo para pasarlo por las distintas estructuras de redes neuronales.

![imagenes pretatadas](https://user-images.githubusercontent.com/87124850/176514794-2a360d03-3ad4-45d5-8562-8e73fd5727d7.PNG)
<p align="center">
Figura 3. Ejemplo del resultado de preprocesar una imágen mediante los pasos indicados.
</p>

## Arquitectura de los modelos a aplicar

Se emplean dos modelos principales:

- **D**. Modelo sencillo de Red Neuronal Densa. Se trabajara con 2 estructuras der DNN (D1 y D2). Los cuales difieren en que, D1 contiene dos capas ocultas de 100 neuronas cada una y D2 contiene 2 capas con 250 neuronas cada una (150 más por capa) además de ser sometidas a un dropout del 50%. Ambas capas ocultas constan de función de activación de tipo Relu y sus capas de salida de función de activación Softmax.

- **CNN**. Red Neuronal convolucional. Se trabajará con 4 estructuras de CNN (CNN1, CNN2, CNN3 y CNN4), los cuales difieren entre ellos únicamente en el número de veces que se aplican las _capas de convolución_ y _pooling_ y el número filtros de cada capa. Se empieza en la CNN1 con 32 filtros y acaba con 256 a la CNN4. Además, CNN3 y CNN4 constan de una capa oculta con 250 neuronas después de la capa de aplastamiento dimensional.


![02_network_flowchart original](https://user-images.githubusercontent.com/87124850/175817555-0e47f2f5-55a9-4157-ac28-86008541ebb7.png)
<p align="center"> 
Figura 4. Ejemplo de una CNN.
</p>

El modelo general de la CNN aplicada esta compuesta por varias capas, en este caso las hemos defininido como:
1. __Input Layer__, capa que define el número de neuronas que tendrá la primera capa en función de la resolución de cada imagen. 
2. __Convolutional Layer__, capa formada por:
- _Convolutional layer_ mediante la aplicación de esta técnica se detecta la presencia de rasgos característicos, tanto geometricos como cromáticos, generando un mapa de ellas.
- _Pooling layer_, recibe el mapa de características proviniente de la capa convolucional y lo reduce en tamaño conservando las características esenciales.
3. __Output Layer__, capa que define el número de neuronas de salida en función del número de categorías que se desea asignar al caso de estudio.
       
En todos los casos se ha aplicado la capa de activación _ReLu_(Rectified Linear Units) en todas las capas intermedias y _Softmax_ para todos los Output Layers.
  
### Enumeración de las librerías usadas
- Numpy
- Tensorflow
- Sklearn
- Os
- Itertools
- Matplotlib

### RESULTADOS

Una vez entrenandos los modelos mediante el método 2-fold cross-validation (se ha introducido _split0_ como train dataset y _split1_ como validation dataset y viceversa) se le introduce un dataset que será igual al validation dataset pero ordenado (al cual se le denominará test dataset) y, de esta manera, evaluar si el modelo hace predicciones correctamente. Una vez obtenidos los resultados de las predicciones se obtiene la matriz de confusión para los resultados de _split 0 - split 1_ y _split 1 - split 1_. A continuación, se hace la media aritmética de estas matrices para obtenir una matriz que sea el resultado de esta operación. Comparando estas últimas matrices de confusión se puede determinar cuál es el modelo que mejor efectua su trabajo.

Los criterios que definirán la mejor opción son:
- El máximo valor de aciertos.
- El mínimo valor de falsos negativos.
- La cantida de falsos positivos podrá ser evaluada para tomar una decisión en caso de incertidumbre, aunque siempre será preferible que tengan valor mínimo.


Gráficando el valor del _accuracy_ a medida del paso de los epochs se puede saber si el modelo es eficiente y si existe algun posible sobreajuste.




## Comparación de los 3 casos

![14C_m](https://user-images.githubusercontent.com/87124850/176560080-3e6b10fe-aaf8-4cd5-895e-a0ed16b038c6.JPG)
<p align="center"> 
Figura 5. 14C
</p>

![10C_m](https://user-images.githubusercontent.com/87124850/176570824-9e034ed7-5d19-4f37-a25b-43de02c9f152.PNG)
<p align="center"> 
Figura 6. 10C
</p>
 
![2C_m (1)](https://user-images.githubusercontent.com/87124850/176571019-24ded0a6-48a2-452c-bdde-aea39c26898d.PNG)
<p align="center"> 
Figura 7. 2C
</p>

### Matrices de confusión para:

**Escenario 14C:**

![Matriz_M_CNN3_14C](https://user-images.githubusercontent.com/87124850/176566104-45841298-6aa5-4b0f-b51b-f99da30af02b.png)
<p align="center"> 
Figura 8. Ejemplo de matriz de confusión del modelo CNN3 para el escenario 14C.
</p>


**Escenario 10C:**

![Matriz_M_CNN3_10C](https://user-images.githubusercontent.com/87124850/176566112-361f8838-7c8e-46c3-ac4b-535b2407c1a1.png)
<p align="center"> 
Figura 9. Ejemplo de matriz de confusión del modelo CNN3 para el escenario 10C.
</p>


**Escenario 2C:**

![Matriz_M_CNN3_2C](https://user-images.githubusercontent.com/87124850/176566121-78469cdf-c4a5-494b-9c54-379b01615bb2.png)

<p align="center"> 
Figura 10. Ejemplo de matriz de confusión del modelo CNN3 para el escenario 2C.
</p>







<p align="center"> 
</p>




