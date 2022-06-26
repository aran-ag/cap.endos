# Capsula endoscópica

## Descripción del project:


Dado un video de una exploración endoscópica, detectar todos los frames donde se encuentra una anomalía (pólipos, sangre, úlceras, ...). 
Cada una de las patologías detectadas contienen un conjunto pequeño de datos, lo que incrementa la complejidad del problema. 

Kvasir-Capsule Dataset
44 exploraciones de pacientes diferents. Un total de 47,238 frames clasificados en 14 clases diferentes

Empleo de CNN

<img width="460" alt="2022-06-23 15_17_06-Image classification using CNN" src="https://user-images.githubusercontent.com/87124850/175827498-a19bc99f-7d95-4a5b-abd9-f77617f72624.png">


## Orígen del dataset
de dónde se coge y de qué consta
fotos

_This is the official repository for the Kvasir-Capsule dataset, which is the largest publicly released PillCAM dataset. In total, the dataset contains 47,238 labeled images and 117 videos, where it captures anatomical landmarks and pathological and normal findings. The results is more than 4,741,621 images and video frames all together._

_The full dataset can be dowloaded via: https://osf.io/dv2ag/_

_The dataset can be split into three distinct parts; Labeled image data, labeled video data, and unlabaled video data. Each part is further described below._

Labeled images In total, the dataset contains 47,238 labeled images stored using the PNG format. The images can be found in the images folder. The classes that each of the images belongs correspond to the folder they are stored. For example, the ’polyp’ folder contains all polyp images, and the ’Angiectasia’ folder contains all images of Angiectasia. The number of images per class is not balanced, which is a common challenge in the medical field because some findings occur more often than others. This adds an additional challenge for researchers since methods applied to the data should also be able to learn from a small amount of training data. The labeled images represent 14 different classes of findings. Furthermore, the labeled image data includes bounding box coordinates, which can be found in the metadata.csv file.

Labeled videos The dataset contains a total of 43 labeled videos containing different findings and landmarks. This corresponds to approximately 19 hours of video and 1,955,675 video frames that can be converted to images if needed. Each video has been manually assessed by a medical professional working in the field of gastroenterology and resulted in a total of 47,238 annotated frames.

Unlabeled videos In total, the dataset contains 74 unlabeled videos, which is equal to approximatley 25 hours of video and 2,785,829 video frames.

Image Labels
Kvasir-Capsule includes the follow image labels for the labeled part of the dataset:

ID	Label
0	Ampulla of Vater
1	Angiectasia
2	Blood - fresh
3	Blood - hematin
4	Erosion
5	Erythema
6	Foreign body
7	Ileocecal valve
8	Lymphangiectasia
9	Normal clean mucosa
10	Polyp
11	Pylorus
12	Reduced mucosal view
13	Ulcer_

## Preprocesado del dataset

Del dataset proporcionando solo se trabaja con "labelled images", el cual será nuestro set de datos base. Esta elección recae en que estos datos son los que aparecen en el metadata.csv indicando, frame por frame, su categorización y problemática por un profesional. 

_Labelled videos_ se descarta por no presentar imágenes de anomalias y _unlabelled images_ es descartada también porque la información que proporciona es poco útil para métodos de aprendizage supervisado.

![image](https://user-images.githubusercontent.com/87124850/175823080-f8b023b2-8046-4d15-927c-a5a26c49dfbe.png)
Muestras de las imágenes que contiene el dataset

El dataset sera preprocesado de dos maneras. En una se 
categorias binomial (Normal y Anomalía) y multicategórica (14 categorias)
La CNN se aplicará en el dataset preprocesado binomial y multicategórico. En cada apartado se 


el preproceso se hace dentro de un imagedatagenerator propio de Tensorflow keras. se le añade la función de preporceso vgg16, la cual cambia el ordena de las bandas de color y hace un zero-centered a todos los píxeles.
a parte de este preproceso, se reescala las imágenes a 28 x 28 píxeles. Luego se han asociado etíquetas en función de las categorías (vector [normal, anomalia])
finalmente se define un batc_size en el que queremos que las imagenes entren al modelo.

- noramlización de valores,vgg16
- cambio de directorio
- definimos reescalado 
- en funcion de la subcarpeta dentro del directorio madre (antes definido, 2 categorias , 14 categoras)
bacth size 2^7 (128), empaquetado 

Con esto se genera un array que contienen sus imagenes  preprcesaads con sus etiquetas asociadas

## Arquitectura de los modelos a aplicar.

Se emplearán dos modelos principales:

- **Modelo 0**. Modelo sencillo que no implementa convoluciones y, por tanto, no es capaz de definir formas y colores con lo que no es aplicable al objetivo planteado. Sin embargo, creemos que es interesante presentarlo.  

- **Modelo A**. Red neuronal convolucional (CNN). 

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



