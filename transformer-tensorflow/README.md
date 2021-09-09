# MODELO TRANSFORMER (tensorflow)

Esta implementación del modelo _Baseline Transformer_ se apoya en el uso de la librería de _Keras_ de _Tensorflow_.  

Su arquitectura consta de dos partes: el decodificador y el codificador, de manera que el codificador capta la atención de las características de las imágenes y el decodificador capta la atención enmascarada en los tokens de los subtítulos. 

## Environment

Para crear el environment de conda adecuado se proporciona el archivo [environment.yml](environment.yml) :  `conda env create -f environment.yml`


## Notebook

En el notebook [Image_captioning_Transformer_tf.ipynb](Image_captioning_Transformer_tf.ipynb) se detalla el pre-procesado de los datos, la construcción del modelo _Transformer_, el entrenamiento y la generación de subtítulos para el conjunto de test. Además, cabe destacar que para obtener buenos subtítulos es necesario disponer de un volumen de información suficientemente grande y de calidad, como el de _MS COCO_. 

No obstante, en ocasiones disponer de volúmenes demasiado grandes, puede extender la duración del entrenamiento, por tanto, se han realizado dos entrenamientos distintos. En el primero de ellos se entrena el modelo implementado con todo el conjunto de datos de train, teniendo en cuenta la división de _Karpathy_. En cambio, en el segundo de los entrenamientos se limita el número de imágenes, con subtítulos de referencia del conjunto de entrenamiento, a 100 mil, para lo que se define la función **_“data_limiter”_**.

## Resultados

La evaluación de los subtítulos resultantes se obtiene con el notebook [viewer-pred&cocoEval.ipynb](../score/viewer-pred&cocoEval.ipynb). En las siguiente tabla se muestras los resultados de las métricas:

|       Métricas   del modelo Baseline (tf)     |     Bleu_1    |     Bleu_2    |     Bleu_3    |     Bleu_4    |     METEOR    |     ROUGE_L    |     CIDEr    |     SPICE    |
|:---------------------------------------------:|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|:--------------:|:------------:|:------------:|
|         SIN LIMITADOR (entrenamiento 1)       |      0,684    |      0,506    |      0,352    |      0,261    |      0,217    |      0,497     |     0,796    |     0,130    |
|     CON   LIMITADOR      (entrenamiento 2)    |      0,640    |      0,446    |      0,303    |      0,208    |      0,198    |      0,462     |     0,637    |     0,117    |
