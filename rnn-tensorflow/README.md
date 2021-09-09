# MODELO RECURRENTE

Los modelos de traducción automática se suelen adaptar bien a la generación de subtítulos de imágenes, por ello, se adapta uno de estos modelos, de manera que el decodificador _RNN_ permite generar subtítulos de imágenes.

Este modelo consta también de dos partes diferenciables: codificador/decodificador. Por un lado, el codificador CNN (_“Convolutional neural network”_) recibe las características extraídas durante el pre-procesado de las imágenes y el decoficador devuelve las predicciones utilizando capas recurrentes, de forma similar que en [Show, Attend and Tell: Neural Image Caption Generation with Visual Attention](https://arxiv.org/abs/1502.03044)

Por otro lado, la implementación esta basada en [Implementation of Attention Mechanism for Caption Generation on Transformers using TensorFlow
](https://www.tensorflow.org/tutorials/text/image_captioning)

## Environment

Para crear el enviroment de conda adecuado se proporciona el archivo [environment.yml](environment.yml) :  `conda env create -f environment.yml`


## Notebook

En el notebook [Image_captioning_RNN.ipynb](Image_captioning_RNN.ipynb) se detalla el pre-procesado de los datos, la construcción del modelo recurrente, el entrenamiento y la predicción de subtítulos para el conjunto de test. 

## Resultados

La evaluación de los subtítulos resultantes se evalúan con el notebook [viewer-pred&cocoEval.ipynb](../score/viewer-pred&cocoEval.ipynb), obteniendo los siguientes resultados de la siguiente tabla:

|     Métricas   del modelo recurrente    |     Bleu_1    |     Bleu_2    |     Bleu_3    |     Bleu_4    |     METEOR    |     ROUGE_L    |     CIDEr    |     SPICE    |
|:---------------------------------------:|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|:--------------:|:------------:|:------------:|
|      Implementación con      CNN-RNN    |      0,469    |      0,282    |      0,165    |      0,096    |      0,161    |      0,353     |     0,336    |     0,105    |





