
# BigData_CHU: Scripts pour la création et le chargement de données dans les tables

Tout d’abord, depuis Hive (avec l'interface Hue) qui se situe dans la VM Cloudera, on crée la base de données « bigdata_project ». 

## Création des tables externes

Ensuite, on crée les tables externes, on définit les colonnes ainsi que le délimiteur. À partir des jobs réalisés sur « TALEND », on peut importer le contenu du HDFS dans la base de données.

<p align="center">
 <img width="70%" src="../images/dim_consultation.png">
 <br> <em> Création de la table dim_consultation </em>
</p>

Exemples de script pour créer d'autre tables:

### Exemple avec la dimension satisfaction

    CREATE EXTERNAL TABLE dim_satisfaction(sas_id_finess int, sas_score_all_ajust float)
    PARTITIONED BY (sas_region string)
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY ",";
    STORED AS TEXTFILE
    LOCATION "/user/hive/data";

### Utilisation d'une view

Enfin, on a décidé de réaliser une vue pour notre table « fact_resultat » car cela nous permet de gagner du temps sur l’écriture de requêtes, mais aussi de centraliser toutes les informations importantes dans une simple table virtuelle.

    CREATE VIEW vfait_resultat AS

    SELECT
    dim_patient_part.pat_id_patient AS id_patient,
    dim_professionel_sante.ps_id as id_professionnel_sante
    dim_etablissement_sante.eta_id_organisation as id_etablissement_sante,
    dim_consultation.con_id as id_consultation,
    dim_diagnostic.dia_code as id_diagnostic,
    dim_hospitalisation.hos_id as d_hospitalisation

    FROM dim_patient_part, dim_consultation, dim_etablissement_sante, dim_diagnostic, dim_hospitalisation, dim_professionel_sante, dim_deces_part, dim_satisfaction

    WHERE
    
    dim_patient_part.pat_id_patient = dim_consultation.con_1d_patient
    and dim_patient_part.pat_id_patient = dim_hospitalisation.hos_id_patient

    and dim_diagnostic.dia_code = dim_consultation.con_code_diag

    and dim_diagnostic.dia_code = dim_hospitalisation.hos_code_diagnostic

    and dim_professionel_sante.ps_id = dim_consultation.con_id_prof_sante

    and dim_etablissement_sante.eta_id_organisation = dim_professionel_sante.ps_id_org

    and dim_ satisfaction.sas_id_ finess = and dim_etablissement_sante.eta_finesse_etablissement_juridique;

<p align="center">
 <img width="70%" src="../images/Visualisation_vue.png">
 <br> <em> Aperçu de la vue créée </em>
</p>
