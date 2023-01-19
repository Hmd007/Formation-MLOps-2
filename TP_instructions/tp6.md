summary: TP6
id: tp6
categories: tp, api
tags: api, flask
status: Published
authors: OCTO Technology
Feedback Link: https://github.com/octo-technology/Formation-MLOps-2/issues/new/choose

# TP6 - Model registry

## Vue d'ensemble
Durée : 30 min


### A l'issue de cette section, vous aurez découvert

- L'interface MLFlow tracking,
- Comment stocker vos expérimentations dans MLFlow,
- Le système de dossier de MLFlow,
- Le versioning de modèle avec MLFlow.

### Mise en place du TP

Pour ce TP, utilisez la branch 6_starting_mlflow

`git checkout 6_starting_mlflow`

Depuis l'interface de Jupyterhub vous pouvez cliquer sur l'icône MLFLow pour lancer MLFlow qui va 
s'ouvrir dans un nouvel onglet.

![mlflow-ui](./docs/tp6/mlflowui.png)

## Intégrer MLFlow dans le code de training

Pour logger le résultat des expérimentations dans MLFlow tracking il faut ajouter un peu de code sur le code d'entraînement.


```python
import mlflow
...
with mlflow.start_run() as run:
    mlflow.sklearn.autolog()
    model = ...
    model.fit(X, y)
```

Une fois que vous avez intégré ce code, vous pouvez retourner dans l'interface Airflow et déclencher un entraînement.

## Explorer le run créé dans MLFlow
Actualiser la page de MLFlow pour voir les runs apparaître

![MLFLOW-run](./docs/tp6/one_experiment.png)

Vous pouvez voir l'ensemble des paramètres et métriques stockées.

Ensuite en cliquant sur le run, vous pouvez aller voir plus de détails et en descendant voir l'artefact généré

![MLFLOW-artefact](./docs/tp6/artifact.png)

## Explorer le système de dossier de MLFlow

En fait MLFlow est basé sur un système de dossier / fichiers plats qui contiennent tout ce que l'on vient de voir.
En plus de cela, MLFlow se sert d'une base de donnée locale pour stocker les métadonnées liés aux runs

Vous pouvez parcourir les métadonnées en explorant le fichier mlflow.db à la racine
```shell
sqlite3 mlflow.db
```

Listez les tables avec commande
```shell
.tables
```

ou faire une requête SQL qui liste toutes vos expérimentations
```shell
SELECT * FROM experiments;
```

ou encore, lister toutes vos métriques
```shell
SELECT * FROM metrics;
```




## Pour aller plus loin

- Ajoutez le log du modèle avec `mlflow.sklearn.log_model` pour que celui-ci apparaisse dans la Model Registry
- Lancez l'entraînement plusieurs fois et regardez la version du modèle s'incrémenter dans la Model Registry
- Parcourez le dossier `/home/jovyan/mlruns/0` pour voir vos artefacts organisés par run
- Essayer de configurer une expérimentation