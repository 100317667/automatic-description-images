# SCORE

La evaluación de los subtítulos resultantes se evalúan con el notebook [viewer-pred&cocoEval.ipynb](viewer-pred&cocoEval.ipynb) que se apoya en el repositorio [flauted/coco-caption](https://github.com/flauted/coco-caption)

## Environment

Para crear el environment de conda adecuado se proporciona el archivo [environment.yml](environment.yml) :  `conda env create -f environment.yml`
Además es necesario descargar las siguientes carpeta y archivos del repositorio [flauted/coco-caption](https://github.com/flauted/coco-caption):
- `pycocoevalcap`
- `pycocotools`
- `get_stanford_models.py`
- `get_stanford_models.sh`
