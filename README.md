
## Puntos necesarios en el blog

1. Recopilación de datos 
2. Características descubiertas y implementadas  _¿a qué se refiere?_
3. Visualización de datos
4. Pruebas de distintos modelos _¿aquí se refiere a probar el código con los distintos tipos de resampling metod que hay? twofold validation, percentatge split, cross-validation, bootstrap?_
5. Interpretación de resultados.


## Descripción del project:

Dado un video de una exploración endoscópica, detectar todos los frames donde se encuentra una anomalía (pólipos, sangre, úlceras, ...). 
Cada una de las patologías detectadas contienen un conjunto pequeño de datos, lo que incrementa la complejidad del problema. 

Kvasir-Capsule Dataset
44 exploraciones de pacientes diferents. Un total de 47,238 frames clasificados en 14 clases diferentes

Empleo de CNN



## Orígen del dataset
de dónde se coge y de qué consta
fotos

## Preprocesado del dataset

Del dataset proporcionando solo se trabaja con "labelled images" la cual será nuestro set de datos. Esta elección se basa en que los datos fiables del ...
Debido a que la única fuente fiable de información es la que prociona el metadata, se seleccionan solo los datos de image labelled, descartando unlabelled images y labelled videos. La primera no es util para métodos supervisados y la segunda ...

fotos iniciales, muestras

categorias binomial (Normal y Anomalía) y multicategórica (14 categorias)
La CNN se aplicará en el dataset preprocesado binomial y multicategórico. En cada apartado se 

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
El modelo definido ahora se entrena con la función **fit()**. Para  su evaluación se usa la validación *Two- Fold*, la cual a partir de los datos originales crea dos subsets con el mismo peso. Estos subsets (split_0 y split_1) son usados para para entrenar y validar. Es decir, se entrena con split_0


### Resultados y conclusiones
matrices de confusiones



