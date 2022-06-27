# Cápsula endoscópica

## Descripción del project:


Dado un video de una exploración endoscópica, detectar todos los frames donde se encuentra una anomalía (pólipos, sangre, úlceras, ...). 
Cada una de las patologías detectadas contienen un conjunto pequeño de datos, lo que incrementa la complejidad del problema. 

Kvasir-Capsule Dataset
44 exploraciones de pacientes diferents. Un total de 47,238 frames clasificados en 14 clases diferentes

Empleo de CNN

<img width="460" alt="2022-06-23 15_17_06-Image classification using CNN" src="https://user-images.githubusercontent.com/87124850/175827498-a19bc99f-7d95-4a5b-abd9-f77617f72624.png">

## Esquema 

1 modelo CNN 
selección de datos
preprceso


## Orígen del dataset

Kvasir-Capsule es el conjunto de datos original para el desarrollo de este proyecto. El cual es el conjunto de datos PillCAM más grande pubicado públicamente y proviene del repositorio oficial Open Science Framework (OSF). En total, contiene 47,238 imágenes etiquetadas y 117 videos, donde captura puntos de referencia anatómicos y hallazgos patológicos y normales. Generando más de 4,741,621 frames entre imágenes y videos. 

Puede ser descargado desde :

            https://osf.io/dv2ag/wiki/home/
            https://drive.google.com/drive/u/0/folders/18vEHN1CG7oNFKdT2NmhtJjrFhb3tLG1Z
            https://datasets.simula.no/kvasir-capsule/

El conjunto de datos Kvasir-Capsule esta dividido en tres partes: imágenes etiquetadas, vídeos etiquetados y imágenes sin etiquetar. Cada parte se describe a continuación:

- **Imágenes etiquetadas**, este grupo contiene 47,238 imágenes guardadas en formato PNG. En su carpeta, las imágenes se encuentran clasificadas en subcarpetas por tipo de hallazgo. El número de imágenes por clase no esta balanceado.Furthermore, the labeled image data includes bounding box coordinates, which can be found in the metadata.csv file. 

- **Vídeos etiquetados**, este otro grupo contiene 43 videos, los cuales contienes diferentes puntos de referencia anatómicos y hallazgos patológicos y normales. Esto corresponde a aproximadamente 19 horas de video, 1,955,675 vframes, que pueden ser convertidos en imágenes si es necesario. Cada video ha sido manualmente evaluado por un profesional médico del campo de la gastroenterología resultando en 47,238 frames anotados.


- **Vídeos sin etiquetar**, grupo que contiene 74 videos sin etiquetar, lo que son aproximadamente 25 horas de video y 2,785,829 frames de video.


Del dataset orginal proporcionado, tal y como se describe, el grupo "videos_etiquetados" contiene 47,238 frames categorizados por un profesional, que son los mismos que encontramos en el grupo "imágenes_etiquetadas". Con lo que se concluye que el resto de datos de "videos_etiquetados" contiene tejido sin anomalías (mucosas, estructuras anatómicas, ...) y por tanto, "imágenes_etiquetadas" sera nuestro conjunto de datos a trabajar

La carpeta "videos_sin_etiquetar" se descarta debido a que la información que contiene es poco útil para métodos de aprendizage supervisado que es el que usaremos en nuestro caso.

![image](https://user-images.githubusercontent.com/87124850/175823080-f8b023b2-8046-4d15-927c-a5a26c49dfbe.png)
Muestras de las imágenes que contiene el dataset

## Preprocesado del dataset

Una vez determinado el conjunto de datos inicial a trabajar ("imágenes_etiquetadas") se procede al preprocesado de imágenes antes de aplicar nuestro modelo directamente.
Este prepocesado consta de los siguientes pasos:

- **noramlización de valores**, mediante la clase _ImageDataGenerator_ propia de Tensorflow Keras y la función de preproceso, _vgg16_. Este paso se encarga de cambiar el orden de bandas de color y hacer un zero-centered a todos los píxeles,  **---DESARROLLAR---**
```
preprocessing_function = tf.keras.application.vgg16.preprocess_input
```
- **Cambio de directorio**,
- **Reescalado**, se define el rescalado de las imágenes a 28x28 píxeles. 
- **Categorización**, en este paso se han asociado etiquetas en función de las categorías. Cabe decir en este punto que nosotros hacemos dos categorias generales. Una binomial que define la imagen en normal o anomalía (vector [normal, anomalia]). Y otra que divide entre las 14 categorías establecidas.
- **Batch size**, se define el empaquetamiento de las imágenes salientes en 128 (2^7)

Con esto se genera un array que contiene las imagenes preprocesadas con sus etiquetas asociadas, conjunto de datos listo para aplicar los modelos a comprobar.

![Uploading resultado_pretratamiento_imagenes.PNG…]()
Ejemplo del resultado de preprocesar una imagen mediantes los pasos indicados.


## Arquitectura de los modelos a aplicar.

Se emplearán dos modelos principales:

- **mD**. Modelo sencillo que no implementa convoluciones y, por tanto, no es capaz de definir formas y colores con lo que no es aplicable al objetivo planteado. Sin embargo, creemos que es interesante presentarlo.  

- **mCNN**. Red neuronal convolucional (CNN). 

![02_network_flowchart original](https://user-images.githubusercontent.com/87124850/175817555-0e47f2f5-55a9-4157-ac28-86008541ebb7.png)
            Descripción arquitectura de una red neuronal convolucional

El modelo de CNN esta compuesto por varias capas, en nuestro caso las hemos defininido como:

### 1. Input Layer (definición de los píxeles)
### 2. Hidden Layers:
  - *Convolutional and Pooling Layer* (n veces)
  La capa de convolución (CONV) analiza las imágenes proporcionadas en la entrada y detecta la presencia de un conjunto de caracteristícas obteniendo un mapas de     éstas.
  La capa de Pooling (POOL) es una operación que recibe el mapa de características proviniente de la capa convolucional y lo reduce en dimensionalidad conservando     las características esenciales. MaxPooling
  - *Activation layer ReLu (Rectified Linear Units)*
  Capa que sustituye todos los valores negativos recibidos en la entrada por ceros, haciendo que el modelo sea no lineal y por tanto, más complejo.
  - *Flatten Layer (la matriz genereda es tranformada a vector)*
 
### 3. Output Layer (Softmax layer)

Esta arquitectura se mantiene para los demás modelos generados, diferenciandose éstos por el número de repeticiones de las capas de CONV y POOL.

## Implementación de la CNN.
### Paso 0. Upload Dataset (librerías, importación imágenes preprocesadas, ejemplo imagen ) 

Cargamos las librerías necesarías para el proceso de implementación: 

```markdown
import numpy as np
import pandas as pd
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras.models import Sequential  ## Building Model
from tensorflow.keras.layers import Activation, Dense, Flatten, BatchNormalization, Conv2D, MaxPool2D  ## Building Model
from tensorflow.keras.optimizers import Adam  ## Training Model
from tensorflow.keras.metrics import categorical_crossentropy  ##Training Model
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from sklearn.metrics import confusion_matrix
import itertools
import os
import csv
import shutil
import random
import glob
import matplotlib.pyplot as plt
import warnings
```
Así como, importamos las imágenes procesadas 

```

```

Después del preprocesado realizado las imágenes obtenidas son las siguientes:

**(ejemplo imágenes preprocesadas)**



### Paso 1. Input layer

```
```

### Paso 2. Hidden Layers + Output layer

```
model = Sequential([
        Conv2D(filters=32, kernel_size=(3,3), activation="relu", padding="same", input_shape=(img_h,img_w,nbands)),
        MaxPool2D(pool_size=(2,2), strides=2),
        Conv2D(filters=64, kernel_size=(3,3), activation="relu",padding="same"),
        MaxPool2D(pool_size=(2,2), strides=2),
        Flatten(),
        Dense(units=ncat, activation="softmax"),
])
```

### Paso 3. Output layer
 aquí iría softmax validation


### Entrenamiento y Validación del modelo
El modelo definido ahora se entrena con la función **fit()**. Para  su evaluación se usa la validación *Two- Fold*, la cual a partir de los datos originales crea dos subsets con el mismo peso. Estos subsets, `split_0` y `split_1`, son usados para para entrenar y validar. Es decir, se entrena con split_0


### Resultados y conclusiones
matrices de confusiones



