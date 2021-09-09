
# MODELO TRANSFORMER (fairseq)

Esta segunda implementación del modelo _Baseline Transformer_ se apoya en el uso de las herramientas de línea de comandos _fairseq_ extendidas en el repositorio [krasserm/fairseq-image-captioning](https://github.com/krasserm/fairseq-image-captioning) y se detallan dos arquitecturas: _“default baseline architecture”_ y _“simplistic baseline architecture”_.

Igual que la implementación con _Tensorflow_ la primera de las arquitecturas utiliza el codificador para captar la atención de las características de las imágenes y el decodificador para captar la atención enmascarada en los tokens de los subtítulos. En cambio, la arquitectura _“simplistic baseline architecture”_, no utiliza codificador, si no que proyecta las características directamente en el decodificador


## Environment

Para crear el environment de conda adecuado se proporciona el archivo [environment.yml](environment.yml) :  `conda env create -f environment.yml`


## Pre-procesado 

El pre-procesado consta de dos partes diferenciadas: el pre-procesado de los subtítulos y el pre-procesado de las imágenes.

Nota: Los comandos de pre-procesado que se muestran, a continuación, guardan la información de forma predetermina en el directorio `output`.

Para el pre-procesado de los subtítulos se utiliza el script **preprocess_captions.sh** del repositorio de [krasserm/fairseq-image-captioning](https://github.com/krasserm/fairseq-image-captioning).

<pre>
./preprocess_captions.sh ../ms-coco
</pre>

Por otro lado, el pre-procesado de las imágenes se puede realizar de dos formas, la primera utilizando el modelo _“InceptionV3”_ y la segunda con el modelo _“Resnet101"_, con los scripts **preprocess_images.sh** y **preprocess_features.sh** del repositorio de [krasserm/fairseq-image-captioning](https://github.com/krasserm/fairseq-image-captioning).

Para la segunda forma, dado que el repositorio de GitHub de [peteanderson80/bottom-up-attention](https://github.com/peteanderson80/bottom-up-attention) proporciona las características pre-entrenadas con el modelo _“Resnet101”_ para cada uno de los conjuntos (entrenamiento, validación y prueba), simplemente es necesario descargar los archivos _tsv_, leerlos  y guardar las características en formato _npy_, teniendo en cuenta los siguientes campos: “image_id”, “image_h”, “image_w”, “num_boxes”, “boxes” y “features”.

<pre>
# Extracción de características con el modelo _“InceptionV3”_ 
./preprocess_images.sh ms-coco

# Extracción de características con el modelo _“Resnet101"_ 
./preprocess_features.sh ms-coco/features
</pre>

Se recomienda que se guarden los archivos _tsv_ siguiente la siguiente estructura de directorios:

<pre>
ms-coco
  annotations
  features
    karpathy_test_resnet101_faster_rcnn_genome.tsv
    karpathy_train_resnet101_faster_rcnn_genome.tsv.0
    karpathy_train_resnet101_faster_rcnn_genome.tsv.1
    karpathy_val_resnet101_faster_rcnn_genome.tsv
  images
    train2014
    val2014
</pre>


## Entrenamiento
Durante el entrenamiento es clave el uso de la herramienta de línea de comandos `fairseq-train` de la librería fairseq, dado que facilita dicho entrenamiento y permite la extensión de algunas de sus funcionalidades.

En la memoria se detallan 5 entrenamientos distintos, para los que se especifican diferentes opciones en la parametrización de modelo. Los mejores resultados se obtienen tras una primera ronda con el criterio cross-entropy loss, y después con una segunda ronda se ajusta con _SCST_, para optimizar la métrica _CIDEr_. Además de la parametrización de la siguiente tabla:

|     Ronda 1. Parámetros               |                              |     Ronda 2. Parámetros                     |                                 |
|:---------------------------------------:|:-------------------------------------:|:---------------------------------------:|:----------------------------------------:|
|                                       |                                     |     restore-file                      |     .checkpoints/checkpoint20.pt       |
|     save-dir                          |     .checkpoints                    |     save-dir                          |     .checkpoints-scst                  |
|     user-dir                          |     task                            |     user-dir                          |     task                               |
|     task                              |     captioning                      |     task                              |     captioning                         |
|     arch                              |     default-captioning-arch         |     arch                              |     default-captioning-arch            |
|     encoder-layers                    |     3                               |     encoder-layers                    |     3                                  |
|     decoder-layers                    |     6                               |     decoder-layers                    |     6                                  |
|     features                          |     obj                             |     features                          |     obj                                |
|     feature-spatial-encoding          |     Active                          |     feature-spatial-encoding          |     Active                             |
|     optimizer                         |     adam                            |     optimizer                         |     adam                               |
|     adam-betas                        |     "(0.9,0.999)"                   |     adam-betas                        |     "(0.9,0.999)"                      |
|     lr                                |     0.0003                          |     reset-optimizer                   |     Active                             |
|     lr-scheduler                      |     inverse_sqrt                    |     lr                                |     5,00E-06                           |
|     min-lr                            |     1,00E-09                        |     weight-decay                      |     0.0001                             |
|     encoder-embed-dim                 |     512                             |     encoder-embed-dim                 |     512                                |
|     max-epoch                         |     25                              |     max-epoch                         |     27                                 |
|     dropout                           |     0.3                             |     dropout                           |     0.3                                |
|     criterion                         |     label_smoothed_cross_entropy    |     criterion                         |     self_critical_sequence_training    |
|     label-smoothing                   |     0.1                             |     max-sentences                     |     5                                  |
|     weight-decay                      |     0.0001                          |     max-source-positions              |     100                                |
|     max-tokens                        |     4096                            |     max-target-positions              |     50                                 |
|     max-source-positions              |     100                             |     tokenizer                         |     moses                              |
|     warmup-init-lr                    |     1,00E-08                        |     bpe                               |     subword_nmt                        |
|     warmup-updates                    |     8000                            |     bpe-codes                         |     output/codes.txt                   |
|     num-workers                       |     2                               |     ddp-backend                       |     no_c10d                            |
|                                       |                                     |     scst-beam                         |     5                                  |
|                                       |                                     |     scst-penalty                      |     1.0                                |
|                                       |                                     |     scst-validation-set-size          |     0                                  |
|                                       |                                     |     num-workers                       |     2                                  |

## Generación de subtítulos
Tras el entrenamiento del modelo con el script *generate.py**, del repositorio de [krasserm/fairseq-image-captioning]((https://github.com/krasserm/fairseq-image-captioning)), se generan los subtítulos del conjunto de test. 

## Score
La evaluación de los subtítulos resultantes se obtiene con el notebook [viewer-pred&cocoEval.ipynb](../score/viewer-pred&cocoEval.ipynb). En las siguiente tabla se muestras los resultados de las métricas para ambas métricas:

|     Métricas   del modelo fairseq (entren. 1)    |     Bleu_1    |     Bleu_2    |     Bleu_3    |     Bleu_4    |     METEOR    |     ROUGE_L    |     CIDEr    |     SPICE    |
|:--------------------------------------------------------:|:---------------:|--------------:|:-------------:|:-------------:|:--------------:|:----------------:|-------------:|:------------:|
|     Ronda 1                                            |     0,725     |     0,562     |     0,430     |     0,331     |     0,281     |     0,554      |     1,108    |     0,211    |
|     Ronda   2                                          |     0,785     |     0,640     |     0,501     |     0,385     |     0,279     |     0,580      |     1,235    |     0,218    |



