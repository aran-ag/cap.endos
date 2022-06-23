
## Puntos necesarios en el blog

1. Recopilación de datos 
2. Características descubiertas y implementadas  _¿a qué se refiere?_
3. Visualización de datos
4. Pruebas de distintos modelos _¿aquí se refiere a probar el código con los distintos tipos de resampling metod que hay? twofold validation, percentatge split, cross-validation, bootstrap?_
5. Interpretación de resultados.


# Posible estructura
(guía https://www.guru99.com/convnet-tensorflow-image-classification.html)

## Descripción del project:

Dado un video de una exploración endoscópica, detectar todos los frames donde se encuentra una anomalía (pólipos, sangre, úlceras, ...). 
Cada una de las patologías detectadas contienen un conjunto pequeño de datos, lo que incrementa la complejidad del problema. 

Kvasir-Capsule Dataset
44 exploraciones de pacientes diferents. Un total de 47,238 frames clasificados en 14 clases diferentes

Empleo de CNN

## Fases:

### 1. Preprocesado de datos:
  - obtenció de los datos
  - Prepocesado
### 2. Fase CNN:
  - construcción modelo
  - entrenamiento del modelo
  - validación del modelo
  
  
### Preprocesado de datos:
 - división videos en frames
 - resize downscale images
 - One hot encoded
 - randomize the data

### CNN. Building Blocks
  - convolutional layer
  - pooling layer
  - ReLu layer
  - fully connected layer
  - loss layer
  - 
 <img width="628" alt="2022-06-23 15_03_41-Deep Convolutional Neural Networks" src="https://user-images.githubusercontent.com/87124850/175305452-91d84800-67b2-433d-8c25-ab8bc8d803cc.png">
 
### CNN. Construccion modelo
  - input
  - convolution and max pooling layer
  - flatten layer
  - fully connected layer
  - output layer
 

 ### CNN. Entrenando el modelo
 división del data set en 70 (train) y 30 (validation)
 
 ### CNN. Validación del modelo








