# Cápsula endoscópica

## Descripción del projecto:

En este projecto se plantea poder, dado un video de una exploración endoscópica, detectar todos los frames donde se encuentra una anomalía (pólipos, sangre, úlceras, ...). Para ello se utilizará la información proporciona por _"Kvasir-Capsule Dataset"_, el cual contiene 44 exploraciones de pacientes diferentes con un total de 47,238 frames clasificados en 14 clases diferentes. Cada una de las patologías detectadas contienen un conjunto pequeño de datos aumentando la complejidad del problema.

Para resolver este reto se ha elegido usar dos tipos de Redes Neuronales, las Densas y las Convolucionales. E implementarlas sobre el conjunto de datos previamente preprocesado. 

Las **Redes Neuronales Densas (DNN)** son redes en las que todas las neuronas de una capa están conectadas a todas las neuronas de la siguiente. Son eficaces en cuanto al tratamiento de información porque cada neurona dispone de más datos para tratar, pero a su vez, es más lenta a la hora de procesar los datos. Para nuestro caso, al no implementar convoluciones no es capaz de definir formas y colores, con lo que no es aplicable al objetivo planteado. Sin embargo, creemos que es interesante presentarlo. Aplicaremos 2 tipos de DNN, D1 y D2.


Las **Redes Neuronales Convolucionales (CNN)**, modelos multicapa, utilizan la operación de convolución como base para el procesamiento de los datos. Por ello, son útiles para trabajar con imágenes. Generalmente, toman las imágenes como input, asignándole importancia a ciertos elementos y, así poder diferenciar unas de otras. Se probarán 4 tipos de CNN: CNN1, CNN2, CNN3 y CNN4. Estas se diferencian en el número de repeticiones de las capas de _convolución_ y _pooling_. 

Estos algoritmos se implementarán sobre un conjunto de datos preprocesado. Cabe decir que se realizan 3 prepocessado diferentes obteniendo 3 sets de datos. Estos estan organizados en 2, 10 y 14 categorías.

Con lo que, en terminos generales, se aplican 6 algoritmos a cada set de datos preprocesados. Para evaluar y comparar estos modelos se usará el termino de validación TwoFold, método que separa los datos en 2 subsets, de manera aleatória. En nuestro caso, el conjunto de datos de trabajo es divido en sano, _00Sano_, y anomalias, _01Anomalias. Estos a su vez en _Train_ y _Test_. La idea es entrenar con ______???__________

( los datos seleccionados se han separado en dos subsets, _Split0_ y _Split1_, de manera aleatória. Se entrena con el _Split0_ y se valida con _Split1_ y viceversa.)

Finalmente se resultados generados con cada algoritmo se comparán mediante matrices de confusión.

<img width="460" alt="2022-06-23 15_17_06-Image classification using CNN" src="https://user-images.githubusercontent.com/87124850/175827498-a19bc99f-7d95-4a5b-abd9-f77617f72624.png">

Fases communes de toda Red Neuronal Convolucional

## Orígen del dataset

Kvasir-Capsule es el conjunto de datos original para el desarrollo de este proyecto. El cual es el conjunto de datos PillCAM más grande pubicado públicamente y proviene del repositorio oficial Open Science Framework (OSF). En total, contiene 47,238 imágenes etiquetadas y 117 videos, donde captura puntos de referencia anatómicos y hallazgos patológicos y normales. Generando más de 4,741,621 frames entre imágenes y videos. 

Puede ser descargado desde :

            https://osf.io/dv2ag/wiki/home/
            https://drive.google.com/drive/u/0/folders/18vEHN1CG7oNFKdT2NmhtJjrFhb3tLG1Z
            https://datasets.simula.no/kvasir-capsule/

El conjunto de datos Kvasir-Capsule esta dividido en tres partes: imágenes etiquetadas, vídeos etiquetados y imágenes sin etiquetar. Cada parte se describe a continuación:

- **Imágenes etiquetadas**, este grupo contiene 47,238 imágenes. En su carpeta, las imágenes se encuentran clasificadas en subcarpetas por tipo de hallazgo. El número de imágenes por clase no esta balanceado.Furthermore, the labeled image data includes bounding box coordinates, which can be found in the metadata.csv file. 

- **Vídeos etiquetados**, este otro grupo contiene 43 videos, los cuales contienes diferentes puntos de referencia anatómicos y hallazgos patológicos y normales. Esto corresponde a aproximadamente 19 horas de video, 1,955,675 vframes, que pueden ser convertidos en imágenes si es necesario. Cada video ha sido manualmente evaluado por un profesional médico del campo de la gastroenterología resultando en 47,238 frames anotados.

- **Vídeos sin etiquetar**, grupo que contiene 74 videos sin etiquetar, lo que son aproximadamente 25 horas de video y 2,785,829 frames de video.

Del dataset orginal proporcionado, tal y como se describe anteriormente, el grupo _"videos_etiquetados"_ contiene 47,238 frames categorizados por un profesional, que son los mismos encontrados en el grupo _"imágenes_etiquetadas"_. Con lo que se concluye que el resto de datos de _"videos_etiquetados"_ contiene tejido sin anomalías (mucosas, estructuras anatómicas, ...) y, por ello, "imágenes_etiquetadas" será nuestro conjunto de datos a trabajar. A continuación, esta carpeta ha sido dividida en dos subsets según si los frames presentaban anomalias o no, _00_Sano_ y 01_Anomalías. Para cada subset los frames estan categorizados por hallazgos.
Para poder aplicar el método de validación TwoFold los subsets de las carpetas _00_Sano_ y 01_Anomalías, se han divido al 50% en las carpetas _Train_ y _Test_.

**FALTA LO DE ENTRENO SPLIT 0 Y VALIDO CON SPLIT 1 Y VICEVERSA**

La carpeta _"videos_sin_etiquetar"_ se descarta debido a que la información que contiene es poco útil para métodos de aprendizage supervisado, que es el que usamos en nuestro caso.

![image](https://user-images.githubusercontent.com/87124850/175823080-f8b023b2-8046-4d15-927c-a5a26c49dfbe.png)
Muestras de las imágenes que contiene el dataset

## Preprocesado del dataset

Una vez determinado el conjunto de datos inicial a trabajar ("imágenes_etiquetadas") se procede al preprocesado de imágenes antes de aplicar un modelo directamente.
Este prepocesado consta de los siguientes pasos:

- **noramlización de valores**, mediante la clase _ImageDataGenerator_ propia de Tensorflow Keras y la función de preproceso, _vgg16_. Este paso se encarga de cambiar el orden de bandas de color y hacer un zero-centered a todos los píxeles.  **---DESARROLLAR---**
```
preprocessing_function = tf.keras.application.vgg16.preprocess_input
```
- **Cambio de directorio**
- **Reescalado**, se define el rescalado de las imágenes a 28x28 píxeles. 
- **Categorización**, en este paso se han asociado etiquetas en función de las categorías. Cabe decir en este punto que nosotros hacemos tres categorias generales. Una binomial que define la imagen en normal o anomalía (vector [normal, anomalia]). Las demás son multicategóricas, de 10 y 14 categorías.
- **Batch size**, se define el empaquetamiento de las imágenes salientes en 128 (2^7)

Con esto se genera un array que contiene las imágenes preprocesadas con sus etiquetas asociadas, obteniendo un conjunto de datos listo para aplicar los modelos de interés.

![resultado_pretratamiento_imagenes](https://user-images.githubusercontent.com/87124850/176286445-9585ad32-22c8-4728-bc8d-c0a8b4d42326.PNG)

Ejemplo del resultado de preprocesar una imagen mediantes los pasos indicados.


## Arquitectura de los modelos a aplicar

Se emplean dos modelos principales:

- **D**. Modelo sencillo de Red Neuronal Densa.

- **CNN**. Red Neuronal Nonvolucional (CNN). Trabajaremos con 4 modelos de CNN (CNN1, CNN2, CNN3 y CNN4), los cuales difieren entre ellos unicamente en el número de veces que se aplican las _capas de convolución_ y _pooling_. 


![02_network_flowchart original](https://user-images.githubusercontent.com/87124850/175817555-0e47f2f5-55a9-4157-ac28-86008541ebb7.png)
            Descripción arquitectura de una red neuronal convolucional

Éstos modelos se aplicarán sobre los conjuntos de datos, previamente preprocesados y definidos en 2 (2C), 10 (10C) y 14 (14C) categorías. Con lo que se obtendrá la compracación de 6 modelos por cada conjunto de datos preprocesado.


El modelo general de la CNN aplicada esta compuesta por varias capas, en nuestro caso las hemos defininido como:

### 1. Input Layer (defición de los píxeles)
### 2. Hidden Layers:
  - *Convolutional and Pooling Layer* 
  La capa de convolución (CONV) analiza las imágenes proporcionadas en la entrada y detecta la presencia de un conjunto de caracteristícas obteniendo un mapas de     éstas.
  La capa de Pooling (POOL) es una operación que recibe el mapa de características proviniente de la capa convolucional y lo reduce en dimensionalidad conservando     las características esenciales. MaxPooling
  - *Activation layer ReLu (Rectified Linear Units)*
  Capa que sustituye todos los valores negativos recibidos en la entrada por ceros, haciendo que el modelo sea no lineal y por tanto, más complejo.
  - *Flatten Layer (la matriz genereda es tranformada a vector)*
 
### 3. Output Layer (Softmax layer)

En definitiva vamos a implentar sobre dos modelos de redes neuronales, uno basado 

## Implementación de los módelos para conjunto de datos 2C.
### Paso 0. Upload Dataset (librerías, importación imágenes preprocesadas, ejemplo imagen ) 

Cargamos las librerías necesarías para el proceso de implementación: 

```markdown
import numpy as np
import pandas as pd
import tensorflow as tf
from sklearn.metrics import confusion_matrix
import itertools
import os

```
Seguidamente, importamos las imágenes previamente preprocesadas 

```

```

Y aplicamos el modelo CNN
### Paso 1. Aplicamos los modelos




### Entrenamiento y Validación del modelo

El modelo definido ahora se entrena con la función **fit()**. Para  su evaluación se usa la validación *Two- Fold*, la cual a partir de los datos originales crea dos subsets con el mismo peso. Estos subsets, `split_0` y `split_1`, son usados para para entrenar y validar. Es decir, se entrena con split_0


### Resultados y conclusiones
Para visualizar el desempeño del algoritmo que se emplea en los modelos comparados se emplean matrices de confusión. 
 Para valorar cómo de bueno son los modelos implementados y poder compararlos entre ellos se aplican matrices de confusión.

![cm_CNN3_m](https://user-images.githubusercontent.com/87124850/176322215-bc6f241f-753f-41ec-a78a-c56ac07f9b82.png)
Ejemplo de matriz de confusión del modelo CNN3.

