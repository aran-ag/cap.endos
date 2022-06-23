
## Puntos necesarios en el blog

1.Recopilación de datos 
2.Características descubiertas y implementadas  _¿a qué se refiere?_
3. Visualización de datos
4. Pruebas de distintos modelos _¿aquí se refiere a probar el código con los distintos tipos de resampling metod que hay? twofold validation, percentatge split, cross-validation, bootstrap?_
5. Interpretación de resultados.


### 1.Recopilación de datos 
explicación elección CCN para el porcesado de las imagenes. Explicar en que consiste. 

### 2.Características descubiertas y implementadas
¿a qué se refiere?
### 3.Visualización de datos
### 4.Pruebas de distintos modelos
¿aquí se refiere a probar el código con los distintos tipos de resampling metod que hay? twofold validation, percentatge split, cross-validation, bootstrap?
### 5.Interpretación de resultados



## Descripción del project:

Dado un video de una exploración endoscópica, detectar todos los frames donde se encuentra una anomalía (pólipos, sangre, úlceras, ...). 
Cada una de las patologías detectadas contienen un conjunto pequeño de datos, lo que incrementa la complejidad del problema. 

Kvasir-Capsule Dataset
44 exploraciones de pacientes diferents. Un total de 47,238 frames clasificados en 14 clases diferentes

Empleo de CNN

## Fases:

1.Preprocesado de datos:
  a. obtenció de los datos
  b. Prepocesado:
2. Fase CNN:
  a. construcción modelo
  b. entrenamiento del modelo
  c. validación del modelo
  
  
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











You can use the [editor on GitHub](https://github.com/aran-ag/tutorialgithubpages/edit/main/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

## Objetivos

## Metodología

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/aran-ag/tutorialgithubpages/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
