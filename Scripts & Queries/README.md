
# BigData_CHU: Scripts pour la création et le chargement de données dans les tables

Tout d’abord, depuis Hive (avec l'interface Hue) qui se situe dans la VM Cloudera, on crée la base de données « bigdata_project ». 

## Création tables externes

Ensuite, on crée les tables externes, on définit les colonnes ainsi que le délimiteur. À partir des jobs réalisés sur « TALEND », on peut importer le contenu du HDFS dans la base de données.



<p align="center">
 <img width="33%" src="images/dim_consultation.png">
 <br> <em> Création de la table dim_consultation </em>
</p>

Exemples de script pour créer d'autre tables:

### Partition2

    CREATE EXTERNAL TABLE dim_satisfaction(sas_id_finess int, sas_score_all_ajust float)
    PARTITIONED BY (sas_region string)
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY ",";
    STORED AS TEXTFILE
    LOCATION "/user/hive/data";
