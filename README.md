# Mix-Energie : analyse production & consommation énergie en France

## Structure des sources du projet

```
mix_energy
├── Makefile
├── README.md
├── airflow
├── dbt
├── fastapi
├── ingest_dbt
├── notebook
│   └── predict.ipynb
├── predict
```

### Structure module ingest_dbt

Module fournissant des outils pour l'extraction des données sources, la sauvegarde des données brutes dans un bucket avant de les charger dans BigQuery

README [>>](./ingest_dbt/README.md)

```
├── ingest_dbt
│   ├── README.md
│   ├── poetry.lock
│   ├── pyproject.toml
│   ├── src
│   │   └── mix_energy
│   │       ├── __init__.py
│   │       ├── air_quality_ingest.py
│   │       ├── base_carbone_ingest.py
│   │       ├── bigquery_loader.py
│   │       ├── bigquery_schema_generator.py
│   │       ├── bucket_to_bigquery.py
│   │       ├── bucket_to_bigquery_airflow.py
│   │       ├── eco2mix_ingest.py
│   │       ├── gcp_utils.py
│   │       ├── meteo_ingest.py
│   │       └── test_thibault.ipynb
│   └── tests
│       ├── __init__.py
│       └── unit
│           ├── test_air_quality_ingest.py
│           ├── test_bigquery_loader.py
│           ├── test_bigquery_schema_generator.py
│           ├── test_bucket_to_bigquery.py
│           ├── test_dag_base_carbone.py
│           ├── test_dag_eco2mix_national_cons_def.py
│           ├── test_dag_eco2mix_national_tr.py
│           ├── test_dag_eco2mix_regional_cons_def.py
│           ├── test_dag_eco2mix_regional_tr.py
│           ├── test_eco2mix_ingest.py
│           └── test_meteo_ingest.py
```

### Structure module predict

Module pour la création, l'entrainement d'un modèle de ML afin de réaliser des prédictions sur la consommation d'énergie nationale ou régionales des 15 prochaines minutes

README [>>](./predict/README.md)

```
├── predict
│   ├── README.md
│   ├── poetry.lock
│   ├── pyproject.toml
│   ├── src
│   │   └── predict
│   │       ├── __init__.py
│   │       ├── data.py
│   │       ├── mllogs.py
│   │       ├── model.py
│   │       ├── preproc.py
│   │       └── train.py
│   └── tests
│       └── __init__.py
```

### Structure module fastapi

Application définissant des API REST pour permettre l'accès aux données transformées dans le dataset ou réaliser une prédiction sur la consommation d'énergie à venir

README [>>](./fastapi/README.md)

```
├── fastapi
│   ├── Dockerfile
│   ├── README.md
│   ├── poetry.lock
│   ├── pyproject.toml
│   ├── src
│   │   └── mix_energy_api
│   │       ├── __init__.py
│   │       ├── __main__.py
│   │       ├── bigquery_service.py
│   │       ├── config.py
│   │       ├── main.py
│   │       └── schemas.py
│   └── tests
│       └── unit
│           └── test_bigquery_service.py
```

### Structure pipeline dbt

Projet DBT qui gère la transformation et l'agrégation des données pour produire des tables silver et gold

README [>>](./dbt/README.md)

```
dbt
├── dbt_packages
├── dbt_project.yml
├── logs
├── models
│   ├── gold
│   │   ├── kpi_reg.sql
│   │   ├── kpi.sql
│   │   ├── nat_cons_agre_j.sql
│   │   ├── nat_tr_agre_j.sql
│   │   ├── nat_tr_predi.sql
│   │   ├── reg_cons_agre_j.sql
│   │   ├── reg_tr_agre_j.sql
│   │   └── reg_tr_predi.sql
│   ├── mix_energie_indice_docs.md
│   ├── mix_energie_region_docs.md
│   ├── silver
│   │   ├── airquality_cons_reg_histo.sql
│   │   ├── eco2mix_national_cons_def_histo.sql
│   │   ├── eco2mix_regional_cons_def_histo.sql
│   │   ├── meteo_cons_reg_histo.sql
│   │   ├── schema.yml
│   │   └── _stg_silver_cons_def_histo.yml
│   └── staging
│       ├── air_quality
│       │   ├── schema.yml
│       │   ├── stg_air_quality_bordeaux.sql
│       │   ├── stg_air_quality_caen.sql
│       │   ├── stg_air_quality_dijon.sql
│       │   ├── stg_air_quality_lille.sql
│       │   ├── stg_air_quality_lyon.sql
│       │   ├── stg_air_quality_marseille.sql
│       │   ├── stg_air_quality_nantes.sql
│       │   ├── stg_air_quality_orleans.sql
│       │   ├── stg_air_quality_paris.sql
│       │   ├── stg_air_quality_rennes.sql
│       │   ├── stg_air_quality_strasbourg.sql
│       │   ├── stg_air_quality_toulouse.sql
│       │   └── _stg_air_quality.yml
│       └── meteo
│           ├── schema.yml
│           ├── stg_meteo_bordeaux.sql
│           ├── stg_meteo_caen.sql
│           ├── stg_meteo_dijon.sql
│           ├── stg_meteo_lille.sql
│           ├── stg_meteo_lyon.sql
│           ├── stg_meteo_marseille.sql
│           ├── stg_meteo_nantes.sql
│           ├── stg_meteo_orleans.sql
│           ├── stg_meteo_paris.sql
│           ├── stg_meteo_rennes.sql
│           ├── stg_meteo_strasbourg.sql
│           ├── stg_meteo_toulouse.sql
│           └── _stg_meteo.yml
├── profiles.yml.exemple
├── README.md
├── seeds
│   ├── cities.csv
│   └── properties.yml
└── target
```

### Structure module airflow

Application définissant des tâches programmées pour
- Soit extraire les données sources et les injecter dans BigQuery avant de lancer leur transformation au sein du Datawarehouse
- Soit effectuer l'entrainement des modèles de prédiction de la consommation d'énergie

README [>>](./airflow/README.md)

```
├── airflow
│   ├── Dockerfile
│   ├── README.md
│   ├── config
│   │   └── airflow.cfg
│   ├── dags
│   │   ├── dag_base_carbone.py
│   │   ├── dag_eco2mix_national_cons_def.py
│   │   ├── dag_eco2mix_national_tr.py
│   │   ├── dag_eco2mix_regional_cons_def.py
│   │   ├── dag_eco2mix_regional_tr.py
│   │   └── dag_train_model.py
│   ├── docker-compose.yaml
│   ├── logs
│   └── plugins
```

### Structure application Streamlit

Application Web affichant des dashboard produits à partir des données transformées en utilisant les API REST présentées par l'application FastAPI

README [>>](./front-streamlit/README.md)

```
front-streamlit
├── README.md
├── dashboard
│   ├── background.jpg
│   ├── dashboard_app.py
│   ├── dashboard_share.py
│   ├── data_api_client.py
│   └── pages
│       ├── 1_national_historique.py
│       ├── 2_national_temps_reel.py
│       ├── 3_regional_historique.py
│       └── 4_regional_temps_reel.py
├── poetry.lock
├── pyproject.toml
└── streamlit
    └── config.toml
```


### Structure des scripts Terraform

```
iac
├── auto.tfvars
├── deploy_full.sh
├── deploy_gcp_project.sh
├── main.tf
├── provider.tf
├── reset_and_import_bucket.sh
├── tfvars
│   ├── dev.tfvars
│   └── prod.tfvars
└── variables.tf
```

## Configuration environnement de développment

Créer un environnement virtuel
```
pyenv virtualenv 3.11.8 mix-energie-env
```

Activez le dans le répertoire où les sources ont été clonées
```
pyenv local mix-energie-env
```

## Variables d'environnement

Le fichier **.env.copy** contient la liste des variables d'environnement utilisés par l'ensemble des modules/applications.
L'utilisateur doit effectuer les opérations suivantes:
```
cp .env.copy .env
cp .envrc.copy .envrc
```
Puis éditer le fichier **.env** et renseigner chaque variable

## Commandes Makefile

### Commandes d'installation/setup

> - **firts_init** : Commande à lancer en premier pour installer le minimum nécessaire au développement
> - **setup** : Installe un module poetry (nécessaire d'être dans le répertoire du pyproject.toml)
> - **build_predict**: Génération des binaires pour le module *predict*
> - **clean_predict**: Supprime les binaires générées pour le module *predict*

### Commandes Serveur (développement)

> - **start_fastapi** : lance l'application exposant les API REST sur le port 8888
> - **start_fastapi_dev**: lance l'application exposant les API REST en mode ***"dev"*** sur le port 8888
> - **start_mlflow_server**: démarre le serveur MLFLOW sur le port 5000

### Commandes Docker

> - **build_local_fastapi** : construit en local l'image docker de l'application FastAPI
> - **build_local_airflow** : construit en local l'image docker de l'application AirFlow
> - **build_local_streamlit** : construit en local l'image docker de l'application Streamlit
> - **run_local_fastapi** : démarre le docker de l'application FastAPI en local sur la base des variables d'environnement renseignées
> - **run_local_streamlit** : démarre l'ensemble des éléments nécessaires pour le front end Streamlit
> - **run_local_airflow** : démarre l'ensemble des éléments nécessaires pour Airflow
> - **stop_local_airflow** : arrête l'ensemble des éléments liés à Airflow

### Commandes spéciales
> - **coffee** : construit tous les modules en local
