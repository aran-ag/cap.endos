# Cápsula endoscópica
## Descripción del projecto:

En este projecto se plantea poder, dado un video de una exploración endoscópica, detectar todos los frames donde se encuentra una anomalía (pólipos, sangre, úlceras, ...). Para ello se utilizará la información proporciona por _"Kvasir-Capsule Dataset"_, el cual contiene 44 exploraciones de pacientes diferentes con un total de 47,238 frames clasificados en 14 clases diferentes. Cada una de las patologías detectadas contienen un conjunto pequeño de datos aumentando la complejidad del problema.

Dado que ciertas patologías son menos probables de ser encontradas en exploraciones, la cantidad de los ejemplos de algunas anomalías esta desbalanceada respecto de otras. Por ejemplo, se tienen 55 imágenes de pólipos en comparación con las 12 imágenes para Blood Hematin. 


Para resolver este reto se ha elegido un método de aprendizaje supervisado conocido como Redes Neuronales. Éstas estan consideradas como muy buenos clasificadores. De entre los diferentes tipos que hay se ha escogido trabajar con Redes Neuronales Densas  y Redes Neuronales Convolucionales. Cabe resaltar que en _Kvasir-Capsule_ utilizaron un modelo ResNet(Residual Network) y, por ello, hemos querido probar otros tipos de redes neuronales.

Las **Redes Neuronales Densas (DNN)** son redes en las que todas las neuronas de una capa están conectadas a todas las neuronas de la siguiente. Son eficaces en cuanto al tratamiento de información porque cada neurona dispone de más datos para tratar, pero a su vez, es más lenta a la hora de procesar los datos. En este caso, al no implementar convoluciones no es capaz de definir formas y colores, con lo que no es aplicable al objetivo planteado. Sin embargo, se cree que es interesante presentar esta estructura para hacer una comparativa con las CNN. Se aplican dos estructuras de DNN, las cuales se etiquetan como D1 y D2.

Las **Redes Neuronales Convolucionales (CNN)**, son modelos multicapa, que utilizan una operación matemática llamada convolución para el procesamiento de los datos. Por ello, son útiles para trabajar con imágenes ya que si que detectan formas y colores. Generalmente las CNN, toman las imágenes como input, asignándole importancia a ciertos elementos y así, poder diferenciar unas de otras. Se probarán 4 estructuras distintas de CNN: CNN1, CNN2, CNN3 y CNN4. Éstas se diferencian en el número de repeticiones de las capas de _convolución_ y _pooling_ que presentan en la estructura.

Estas estructuras se testearán en los 3 escenarios distintos que se han generados. Estos escenarios contemplan las imágenes categorizadas en grupos de 14, 10 y 2.

Al igual que _Kvasir-Capsule_ nos planteamos aplicar una metodología de **2Fold cross-validation**, que consiste en coger el conjunto  de datos y dividirlo en 2 partes iguales. Para este caso hemos decidido que esta partición sea en _Split1_ y _Split0_, y que contengan cada una contenga el 50% del total del dataset. Estas agrupaciones serán usadas para entrenar y validar las estructuras de redes neuronales propuestas. Se entrenará con _Split 0_ y se validará con _Split1_ y, viceversa.
 
 ___DETALLE DE LAS CATEGORÍAS___


3 CODIFGOS DISTINOTS PARA CADA ESCENARIO, CADA CÓDIGO CONTIENE 

En terminos generales, se aplican 6 estructuras distintas de redes neuronales. Para evaluar y comparar estos modelos se usará el termino de _validación cruzada TwoFold_, método que separa los datos en 2 subsets, de manera aleatória EN PORPOR A LA. En nuestro caso, el conjunto de datos de trabajo es divido en sano, _00Sano_, y anomalias, _01Anomalias. Éstos a su vez son separados en _Train_ y _Test_, en cada una de estas subcarpetas las imágenes se encuentran categorizadas por el tipo de hallazgo. La separación en _Train_ y _Test_ ha sido del mismo peso para cada uno.


Finalmente, los resultados generados se comparán mediante matrices de confusión.




<p align="center">
<img width="460" alt="2022-06-23 15_17_06-Image classification using CNN" src="https://user-images.githubusercontent.com/87124850/175827498-a19bc99f-7d95-4a5b-abd9-f77617f72624.png">
</p>
<p align="center">
Fases communes de toda Red Neuronal Convolucional
</p>

Pasos seguidos durante el proceso:

## Orígen del dataset

Kvasir-Capsule es el conjunto de datos original para el desarrollo de este proyecto. Es el conjunto de datos PillCAM más grande de acceso libre pubLicado y proviene del repositorio oficial Open Science Framework (OSF). En total, contiene 47,238 imágenes etiquetadas y 117 videos, donde captura puntos de referencia anatómicos y hallazgos patológicos y normales. Generando más de 4,741,621 frames entre imágenes y vídeos. 

Puede ser descargado desde :

[https://osf.io/dv2ag/wiki/home/](url)

[https://drive.google.com/drive/u/0/folders/18vEHN1CG7oNFKdT2NmhtJjrFhb3tLG1Z](url)

[https://datasets.simula.no/kvasir-capsule/](url)


El conjunto de datos _Kvasir-Capsule_ esta dividido en tres partes: imágenes etiquetadas, vídeos etiquetados y vídeos sin etiquetar. Cada parte se describe a continuación:

- **Imágenes etiquetadas**, este grupo contiene 47,238 imágenes de 336x336 píxeles. En su carpeta, las imágenes se encuentran clasificadas en subcarpetas por tipo de hallazgo. El número de imágenes por clase no esta balanceado. Además, las imágenes etiquetadas incluyen las coordenadas de un rectangulo perimetral sobre las anomalías detectadas, las cuales se pueden encontrar en el _metadata.csv_

- **Vídeos etiquetados**, este otro grupo contiene 43 vídeos, los cuales contienen diferentes puntos de referencia anatómicos y hallazgos patológicos y normales. Esto corresponde a aproximadamente 19 horas de video, 1,955,675 frames, que pueden ser convertidos en imágenes si es necesario. Cada vídeo ha sido evaluado personalmente por un profesional médico del campo de la gastroenterología. Esta evaluación tiene como resutado el _metadata.csv_ donde se puede encontrar las 47,238 anotaciones que corresponden a los 47,238 frames anotados.

- **Vídeos sin etiquetar**, grupo que contiene 74 vídeos sin etiquetar, no revisados por un profesiona, lo que son aproximadamente 25 horas de vídeo y 2,785,829 frames de vídeo.

Del dataset orginal proporcionado, tal y como se describe anteriormente, el grupo _"videos etiquetados"_ contiene 47,238 frames categorizados por un profesional, que son los mismos encontrados en el grupo _"imágenes etiquetadas"_. Con lo que se concluye que el resto de datos de _"videos etiquetados"_ contiene información de tejido sin anomalías (mucosas, estructuras anatómicas, ...) y, por ello, _"imágenes etiquetadas"_ será nuestro conjunto de datos a trabajar. 


<p align="center">
![image](https://user-images.githubusercontent.com/87124850/175823080-f8b023b2-8046-4d15-927c-a5a26c49dfbe.png)
</p>
<p align="center">
Muestras de las imágenes que contiene el dataset
</p>

La carpeta _"videos_sin_etiquetar"_ se descarta debido a que la información que contiene es poco útil para métodos de aprendizage supervisado, que es el que usamos en este caso.

COOREGIR A continuación, la carpeta _"imágenes etiquetadas"_ sido dividida en dos subsets según si los frames presentaban anomalias o no, _00_Sano_ y 01_Anomalías. Éstos a su vez son separados en _Train_ y _Test_, en cada una de estas subcarpetas las imágenes se encuentran categorizadas por el tipo de hallazgo. La separación en _Train_ y _VALIDATION_ ha sido del mismo peso para cada uno.

TRAIN I VALIDATION 



## Preprocesado del dataset

El preproceso se aplicará mediante un generador de imágenes, _ImageDataGenerator_, del propio módulo de Tensorflow, que es con el que vamos a trabajar. Dentro de este generador se define la función de preproceso _vgg16_ que ya ha sido utilizada.
```
preprocessing_function = tf.keras.application.vgg16.preprocess_input
```
Los pasos a realizar por este pocesador de iméges es: 
- **Noramlización de valores**,cambio  del orden de bandas de color y zero-centered a todos los píxeles. 
- **Indicación de la ruta de orígen de las imágenes**
- **Reescalado**, se define el rescalado de las imágenes a 28x28 píxeles. 
- **Categorización**, agrupamiento de las imágenes en 14, 10 y 2 categorías.
- - **Batch size**, se define el empaquetamiento de las imágenes salientes en 128 (2^7)

Con este procesado se genera un array que contiene las imágenes con sus etiquetas asociadas, obteniendo un conjunto de datos listo para pasarlo por las distintas estructuras de las redes neuronales.

<p align="center">
![imagenes pretatadas](https://user-images.githubusercontent.com/87124850/176514794-2a360d03-3ad4-45d5-8562-8e73fd5727d7.PNG)
</p>
<p align="center">
Ejemplo del resultado de preprocesar una imágen mediante los pasos indicados.
</p>

## Arquitectura de los modelos a aplicar

Se emplean dos modelos principales:

- **D**. Modelo sencillo de Red Neuronal Densa. Se trabajara con 2 estructuras der DNN, D1 y D2. Los cuales difieren en que, D1 contiene 100 neuronas y una capa ReLu, y D2 contiene 150 neuronas más por cada una de las dos capas ocultas. Estas capas ocultas se someterán a un dropout de 0.5. 

- **CNN**. Red Neuronal convolucional. Se trabajará con 4 estructuras de CNN (CNN1, CNN2, CNN3 y CNN4), los cuales difieren entre ellos únicamente en el número de veces que se aplican las _capas de convolución_ y _pooling_ y el número filtros de cada capa. Se empieza en la CNN1 con 32 filtros.

<p align="center">
![02_network_flowchart original](https://user-images.githubusercontent.com/87124850/175817555-0e47f2f5-55a9-4157-ac28-86008541ebb7.png)
 </p>
<p align="center"> 
Ejemplo de una CNN.
</p>

El modelo general de la CNN aplicada esta compuesta por varias capas, en este caso las hemos defininido como:
1. __Input Layer__, capa que define el número de neuronas que tendrá la primera capa en función de la resolución de cada imagen. 
2. __Convolutional Layer__, capa formada por:
_Convolutional layer_ mediante la aplicación de esta técnica se detecta la presencia de rasgos característicos, tanto geometricos y cromáticos, hacieno un mapa de ellas.
_Polling layer_, peración que recibe el mapa de características proviniente de la capa convolucional y lo reduce en dimensionalidad conservando     las características esenciales.
3. __Output Layer__
       
En todos los casos se ha aplicado la capa de activación _ReLu_(Rectified Linear Units) en todas las capas intermedias y _Softmax_ para todos los Output Layers.
  
### Enumeración de las librerías usadas
- Numpy
- Pandas
- Tensorflow
- sklearn.metrics 
- os


### RESULTADOS

Una vez entrenandos los modelos mediante el método 2Fold cross-validation (hemos pasado train dataset como _split0_ y el validation dataset como _split1_) se le pasa un dataset que será igual al validation dataset pero ordenado y así, evaluar si el modelo hace predicciones correctamente. Una vez obtenidos los resultados de las predicciones se obtiene la matriz de confusión para los resultados para split1-split0 y split0_split1. A continuación, se hace la media de estas matrices para obtenir una matriz que sea la media. Comparando estas últimas matrices de confusión se puede determinar cual es el modelo que mejor efectua su trabajo. 

Los criterios que definirán la mejor opción son:
- mayor número de aciertos, la que tenga la suma más alta de los elementos de la diagonal principal.
- menos falsos negativos, es decir, que la suma de los elementos que quedan a la derecha de la diagonal principal sea mínima.

UNA VEZ ENTRENADOS EL MODELOS  MEDIANTE EL METOD 2FOLD VALIDATIO (HEMOS PASADO TRAIN DATASET COMO SPLIT 0 Y EL VALIDATION DATASET COMO SPLIT 1 ) se le pasa un test dataset que sera igual al validation dataset pero ordenado para evaluar si el modelo hace predicciones correctameent. Una vez obtenidos 
 14
 10
 2

añadir se puede obtener el valor del accuracy a medida del paso de los epchs ygraficando nos da idea la eficiende del modelo y de si existe un posible sobreajuste
3 graficas
####ESCENARION 12C
MATRIZ
COMO OBSERVAMOS LA MATRIZ DA 
LA EVALUACION FINALIZA OBTENIENDO LAS MATRICES DE CONFUSIONS PARA CADA UNA DE LOS MODELOS PROBADOS Y SE COMPARA LA MEDIA DE LAS MATRICES OBTENIDAS PARA EL CASO SPLIT0-SPLIT1 Y SPLIT1-O


<p align="center"> 
![cm_CNN3_m](https://user-images.githubusercontent.com/87124850/176322215-bc6f241f-753f-41ec-a78a-c56ac07f9b82.png)
 </p>
 <p align="center"> 
Ejemplo de matriz de confusión del modelo CNN3 en forma de mapa de calor.
</p>

COMPARACION DE LOS 3 CASOS

![2C_m](https://user-images.githubusercontent.com/87124850/176498083-4e82a43b-ab58-45f8-9b61-16b1067c8458.PNG)


![14C_m](https://user-images.githubusercontent.com/87124850/176560080-3e6b10fe-aaf8-4cd5-895e-a0ed16b038c6.JPG)

<p align="center"> 
</p>
