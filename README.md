# Aplicación de estrategias de deep-learning para la detección de animales en imágenes de foto trampeo

<img src="https://www.ingebook.com/ib/pimg/Ingebook/00100_0000001391_UHU_transparente.png" align="right" height="190" />

**Alumno:** Carlos García Silva

**Centro:** Universidad de Huelva (UHU)   
**Titulación:** Grado Ingeniería Informática   
**Departamento:** Ingeniería Electrónica, de Sistemas Informáticos y Automática   
**Área de conocimiento:** Ingeniería de Sistemas y Automática

**Tutor 1:** Diego Marín Santos   
**Tutor 2:** Manuel Emilio Gegúndez Arias

___

# Introducción:

La aplicación de estrategias de Deep Learning (DL) está mostrando gran potencialidad en el análisis de imágenes
digitales de distinta naturaleza. En este TFG se aplican a imágenes adquiridas mediante cámaras de foto trampeo para la
detección de la presencia de animales en las imágenes.

# Objetivos:

Aprendizaje de los fundamentos de las redes neuronales convolucionales (CNN) y su aplicación mediante la librería
TensorFlow de Python. Revisión del estado del arte, diseño e implementación de CNN en el problema de experimentación
planteado. Análisis y evaluación de resultados.

# Requisitos previos:

- Python: v3.8
- TensorFlow: v2.10.0 GPU (En nuestro entorno macOS con Apple Silicon)
- Repositorio [CameraTraps](https://github.com/microsoft/CameraTraps) de Microsoft, que hemos incluido en la
  carpeta [repos](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/repos) de nuestro repositorio
- Repositorio [ai4eutils](https://github.com/microsoft/ai4eutils) de Microsoft, que hemos incluido en la
  carpeta [repos](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/repos) de nuestro repositorio
- Modelo
  MegaDetector: [megadetector_v4_1_0.pb](https://lilablobssc.blob.core.windows.net/models/camera_traps/megadetector/md_v4.1.0/md_v4.1.0.pb)

# Librerías de Python utilizadas:

Si al ejecutar los notebooks, sucede algún tipo de error, asegúrese de tener instaladas las siguientes librerías en su
entorno:

| Nombre | Versión |     | Nombre | Versión |     | Nombre | Versión |
|---------|---------|-----|---------|---------|-----|---------|---------|
| humanfriendly | 10.0 |     |jsonpickle | 2.2.0 |     | jupyterlab | 3.4.4 |
| matplotlib | 3.5.2 |     | numpy | 1.23.1 |     | opencv | 4.6.0 |
| pandas | 1.4.4 |     | pillow | 9.2.0 |     | python | 3.8.13 |
| scikit-learn | 1.1.1 |     | tensorflow-macos | 2.10.0 |     | tensorflow-metal | 0.6.0 |

<p align="center">
  <img src="https://ibvm.es/wp-content/uploads/2020/06/babysitter-background-white-1.png" height="20" />
</p>
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Python_logo_and_wordmark.svg/2560px-Python_logo_and_wordmark.svg.png" height="100" align="left" />
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/32/OpenCV_Logo_with_text_svg_version.svg/831px-OpenCV_Logo_with_text_svg_version.svg.png" height="100" />
  <img src="https://matplotlib.org/stable/_images/sphx_glr_logos2_003.png" height="100" align="right" />
</p>
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/NumPy_logo_2020.svg/1280px-NumPy_logo_2020.svg.png" height="100" align="left" />
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Jupyter_logo.svg/1767px-Jupyter_logo.svg.png" height="100" />
  <img src="https://l3c.cloud/wp-content/uploads/2019/02/tensorflow-logo.png" height="100" />
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Pandas_logo.svg/2560px-Pandas_logo.svg.png" height="100" align="right" />
</p>
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Scikit_learn_logo_small.svg/2560px-Scikit_learn_logo_small.svg.png" height="100" align="left" />
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/96/Microsoft_logo_%282012%29.svg/1200px-Microsoft_logo_%282012%29.svg.png" height="100" />
</p>
<p align="center">
  <img src="https://ibvm.es/wp-content/uploads/2020/06/babysitter-background-white-1.png" height="20" />
</p>

Para mayor facilidad hemos creado una carpeta donde guardaremos copias de seguridad de los entornos que vayamos creando
a medida que completemos proyecto. Cada vez que instalemos una nueva librería o actualicemos la versión de las ya
existentes subiremos una copia de seguridad. La carpeta en cuestión
es [TFG-DeteccionFototrampeo/env](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/env).

___

# Ejecutar aplicación

En la creación de la aplicación hemos implementado varios notebooks, cada uno de ellos ejecuta cada una de las fases
que forman el proyecto global. Lo hemos realizado de esta manera para una mejor gestión de las nuevas funcionalidades
que se añadían a medida que se avanzaba el desarrollo.

Para realizar este proyecto, se nos ha facilitado un dataset con imágenes de foto trampeo de diferentes especies de
animales, humanos y vehículos, capturadas en los parajes
del [Parque natural de Doñana](https://es.wikipedia.org/wiki/Parque_nacional_y_natural_de_Doñana). La forma de manejar
los datos es a través de un fichero CSV, donde en una columna se nos indica la ruta del fichero correspondiente a la
imagen y en su segunda columna se nos dice a qué clase pertenece dicha imagen.

Como hemos mencionado anteriormente, para cada funcionalidad se ha intentado crear un notebook que la ejecute, por
algunos de ellos no necesitan ser ejecutados para obtener los resultados. A continuación, vamos a explicar la
funcionalidad de cada uno, así como el orden de ejecución.

___

# Notebooks principales:

## a01_GeneraDataset

En el dataset existen varias especies de animales, en nuestro proyecto, por el momento solo nos
interesa identificar la presencia de animales, por lo que solo nos serán necesarias dos clases _Animal_ o _Vacía_.
Además, en nuestro caso queríamos trabajar con un dataset de valores binarios para las clases, lo que tuvimos que
realizar una conversión previa, en la que:

- Indicamos la nomenclatura de las clases originales (variable _ORIGINAL_CLASSES_).
- Nomenclaturas que formarán la nueva clase _ANIMAL_ (variable _CLASS_ANIMAL_).
- Nomenclaturas que formarán la nueva clase _VACÍA_ (variable _CLASS_EMPTY_).

Para hacer funcionar el notebook de forma correcta, deberemos indicar:

- Localización del fichero CSV de origen, indicada en la variable _dataset_csv_relative_.
- Localización donde se guardarán los ficheros CSV fraccionados, indicada en la variable _custom_csv_relative_.
- Número de muestras de la clase _VACÍA_, indicada en la variable _NUMBER_IMAGES_.
- Porcentaje de muestras de la clase _VACÍA_ para el CSV de entrenamiento, indicada en la variable _TRAIN_PERCENTAGE_.
- Porcentaje de muestras de la clase _VACÍA_ para el CSV de validación, indicada en la variable _VALIDATION_PERCENTAGE_.
- Porcentaje de muestras de la clase _VACÍA_ para el CSV de test, indicada en la variable _TEST_PERCENTAGE_.

Si lo deseamos podemos hacer una partición del número de muestras de la clase vacía, en nuestro caso hemos hecho una de
700 muestras y otra para el tamaño original de 10000 muestras.

El dataset quedará dividido en tres partes, _Train_, _Validation_ y _Test_. Por cada uno generamos un CSV con el mismo
formato que el original. Estos ficheros que guardarán por defecto en la
carpeta [TFG-DeteccionFototrampeo/data](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/data).

<p align="center">
  <img src="./resources/data.png" height="100" />
</p>

## Notebook: a02_GeneraDetecciones

A la hora de procesar las imágenes el primer paso es generar las detecciones. Para ello hemos decidido hacer uso de un
modelo ya entrenado. Se trata de Megadetector, un modelo de detección de objetos desarrollado por Microsoft disponible
en su repositorio [CameraTraps](https://github.com/microsoft/CameraTraps).

Para este notebook, tenemos dos modos de aportarle los datos de entrada.

La primera es indicarle la ruta de un fichero CSV, del que tomará las rutas de las imágenes de un dataset y la ruta raíz
de dicho dataset, en nuestro caso los ficheros CSV, por defecto se encuentran
en [data](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/data).

La segunda forma es indicando un directorio de entrada de donde tomará las imágenes a las que le aplicará el modelo de
detecciones entrenado de MegaDetector. (Por defecto hemos creado la
carpeta [input](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/input) para que tome las imágenes).

Una vez obtenidos los resultados, genera un fichero JSON con los datos de cada detección encontrada en cada una de las
imágenes. Se generan tanto un JSON global que contiene todos los resultados de la ejecución, así como un fichero JSON
por cada imagen de forma individual.

Todos estos resultados son guardados en la
carpeta [output_json](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_json), para los ficheros de
resultados globales, hemos creado una carpeta dentro de la anteriormente mencionada, a modo de historial, se trata
de [registry](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_json/registry).

___
SIN ACTUALIZAR
___

### Notebook: b01_GeneraMascaras

Módulo creado para generar máscaras con las regiones de interés de las imágenes dadas. En este caso tomaremos como
regiones de interés los bounding boxes de las detecciones, por tanto, partiremos de los ficheros JSON generados en el
anterior notebook, que se encuentran en la
carpeta [output_json](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_json), por defecto.

Como salida tendremos imágenes binarias, las cuales han sido guardadas con valores 0 y 255. Estas imágenes las usaremos
como máscara, por defecto hemos asignado que se guarden en la
carpeta [output_mask](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_mask), con el nombre de la
imagen original añadiendo la terminación __mask_

#### Notebook: c01_AplicaMascaras

En este módulo combinaremos los resultados de los dos notebooks anteriores. Es decir, aplicaremos las máscaras a las
imágenes originales, con el fin de obtener imágenes con el fondo negro y la zona correspondiente a las detecciones,
manteniendo el valor de los píxeles originales.

Tomaremos como entrada un fichero JSON, por defecto tomamos como origen la
carpeta [output_json](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_json). De estos ficheros
obtenemos tanto la ruta al fichero de la imagen original, para tomar el valor de los píxeles, como los datos de las
coordenadas de los bounding boxes de las detecciones.

También necesitaremos una segunda entrada, que será la ruta al fichero de la máscara generada con el notebook
b01_GeneraMascaras, por lo que antes de ejecutar este módulo se ha debido de lanzar el anterior.

Por último, deberemos indicar la ruta destino donde serán guardados los resultados. En este caso por defecto hemos
asignado la carpeta [output_masked](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_masked), donde
se guardarán ficheros de imagen con las características anteriormente mencionadas.

### Notebook: b02_GeneraRecortes

En este módulo partiremos de los resultados de ejecutar el notebook _a01_GeneraDetecciones_. A partir de los ficheros
JSON, situaremos los bounding boxes en las fotos correspondientes. Generaremos nuevas imágenes con únicamente el
contenido de dichos recuadros, serán creados tantos ficheros como detecciones haya por imagen original.

La carpeta de origen será [output_json](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_json) y
como carpeta destino, por defecto hemos
asignado [output_crop](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_crop)

### Notebook: b03_GeneraBoundingBoxes

Módulo muy similar al anterior. En este caso, una vez calculados los bounding boxes, crearemos una copia de la imagen
original, a la que le hemos renderizado los BBoxes sobre ellas (estas copias han sido previamente redimensionadas).

El resultado final es una imagen con los bounding boxes de las detecciones, dibujados.

Una vez más la carpeta de origen de donde se tomarán los datos,
será [output_json](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_json) y como carpeta
destino [output_img](https://github.com/byLiTTo/TFG-DeteccionFototrampeo/tree/main/output_img)

## Notebook: a03_EntrenaClasificador

En este notebook cargamos los dataset generados hasta ahora, así como las imágenes con las máscaras aplicadas.

Nuestro clasificador de imágenes se tratará de un modelo basado en una Red Neuronal Convolucional (CNN), la cual
diseñaremos y entrenaremos en este fichero.



___
