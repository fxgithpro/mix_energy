## Module pour l'ingestion des sources dans Google Storage et Google BigQuery

- **gcp_utils** : Utilitaires pour se connecter et écrire dans un bucket de Google Storage
- ***_ingest** : script dédié à l'ingestion d'une source précise
- **bigquery_schema_generator** : Génération de schéma de tables dans Google BigQuery à partir de la structure des données sources
- **bigquery_loader** : Charge un fichier CSV pour l'insérer dans une table BigQuery avec génération dynamique de schéma
- **bucket_to_bigquery** : charge les données d'une source stockée dans Google Storage dans Bigquery
- **bucket_to_bigquery_airflow** : Fonction pour réaliser les opérations de transfert des données de Google Storage dans Google BigQuery au sein d'une tâche Airflow
