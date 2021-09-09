[Sept. 2021]
# Decripción automática de imágenes

Este repositorio contiene la implementación del Proyecto de Máster Final **"Modelos de atención para descripción de imágenes basadas en _Transformers_"**, realizado por Ana Galán y que completa la formación del Máster en Ingeniería de Telecomunicación de la Universidad Carlos III.

---

## RESUMEN 
La descripción automática del subtitulado de imágenes combina el análisis de imágenes y la generación de oraciones, en este contexto, el uso de atención permite acotar lo que se debe describir y el orden. Por ello, en este trabajo se evalúa el uso de modelos basados en _Transformers_ sobre el conjunto de datos subtitulado _MS COCO_. 

La arquitectura del modelo denominado _Baseline_, basada en el uso de _Transformers_, consta de dos partes: el decodificador y el codificador, de manera que el codificador capta la atención de las características de las imágenes y el decodificador capta la atención enmascarada en los tokens de los subtítulos. 

Por otro lado, los subtítulos generados por las diferentes implementaciones se evalúan con cinco métricas: _Bleu_, _Rouge_, _Meteor_, _CIDEr_ y _SPICE_. Las cuatro primeras métricas se centran en comparar los grams del subtítulo de referencia y del subtítulo generado, con diferentes modificaciones. En cambio, la métrica _SPICE_ tiene el objetivo de evaluar el contenido semántico, de forma similar a como lo haría el ser humano.

Además, para terminar la evaluación de las implementaciones desarrolladas en este trabajo se implementa un modelo recurrente con el objetivo de comparar y desarrollar las ventajas de las capas de atención frente a las capas recurrentes.


---

## DATA SET
El conjunto de datos seleccionado es MS COCO _“Microsoft Common Objects in Context”_. Este conjunto de datos tiene disponibles más de 200k imágenes enfocadas entre otros usos a clasificación de la imagen, localización de objetos, segmentación semántica o subtitulado de imágenes.

A continuación se detallan los enlaces oficiales para descargar los conjuntos de datos subtitulados disponibles:

- Conjunto de entranamiento: [train2014](http://images.cocodataset.org/zips/train2014.zip)
- Conjunto de validación: [val2014](http://images.cocodataset.org/zips/val2014.zip)
- Anotaciones del conjunto de entrenamiento y validación: [annotations_trainval2014](http://images.cocodataset.org/annotations/annotations_trainval2014.zip)

Se recomienda descargar el conjunto de datos y organizarlo en la siguientes carpetas


<pre>
ms-coco
  annotations
  images
    train2014
    val2014
</pre>

---

## SCORE

El cálculo de las métricas está respaldado por las métricas de _Bleu_, _Meteor_, _Rouge-L_, _CIDEr_ y _SPICE_ descritas en el repositorio [flauted/coco-caption](https://github.com/flauted/coco-caption).

---

## MODELOS TRANSFORMER

A continuación se exponen dos implementaciones del modelo de _Transformers_: la primera se apoya principalmente en las librerías de _Tensorflow_, para el desarrollo de cada una de las capas del modelo y la otra en el conjunto de herramientas de línea de comandos _Fairseq_. El modelo _Transformer_ descrito en la memoria se denomina _Baseline_.

### IMPLEMENTACIÓN 1
La primera de las implementaciones del modelo _Transformer Baseline_, requiere de una fase de pre-procesado de los datos y se apoya en las librerías de la plataforma de _Tensorflow_, para facilitar la creación del modelo y el entrenamiento.

Esta implementación se detalla en el Notebook: [Image_captioning_Transformer_tf.ipynb](transformer-tensorflow/Image_captioning_Transformer_tf.ipynb)

### IMPLEMENTACIÓN 2

La segunda de las implementaciones se apoya en el repositorio [krasserm/fairseq-image-captioning](https://github.com/krasserm/fairseq-image-captioning) que proporciona dos arquitecturas del modelo _Baseline_, con el uso de las herramientas de línea de comandos _fairseq_ escritas en Pytorch. 


---

## MODELO CON REDES RECURRENTES

Por último, se desarrolla la implementación de un modelo recurrente, con el objetivo de realizar una comparación de las implementaciones del modelo _Transformers_ definido, con otros tipos de modelos, como son las redes recurrentes.


