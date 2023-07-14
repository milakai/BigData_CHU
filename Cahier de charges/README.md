
# BigData_CHU cahier de charges

Dans le cadre de notre projet Big Data, les exigences spécifiques de notre storytelling nous ont été données via un cahier des charges, mettant en lumière les analyses souhaitées par les utilisateurs.
Suite à cela, le développement d'une série de requêtes SQL et création des vues pour faciliter l'extraction et la manipulation des données nécessaires à ces analyses depuis la base de données ont été estimés. Ces requêtes permettront d'implémenter efficacement le storytelling en transformant les données brutes en informations significatives et en histoires compréhensibles.

Une première consultation a permis de mettre en évidence le type d'analyses souhaitées par les utilisateurs :

- Taux de consultation des patients dans un établissement X sur une période de temps Y
    - Patients (Patient)
    - Nbr consultation (Consultation)
    - Etablissement (Etablissement-Santé)
    - Période de temps (Consultation)

- Taux de consultation des patients par rapport à un diagnostic X sur une période de temps Y
    - Patients (Patient)
    - Nbr consultation (Consultation)
    - Diagnostic (Diagnostic)
    - Période de temps (Consultation)

```SQL
    CREATE VIEW vtaux_consultation_patients_diagnostic_periode AS

    SELECT 

    COUNT(dim_patient.pat_nom)/COUNT(dim_consultation.con_id_patient), dim_diagnostic.dia_label, dim_consultation.con_date FROM dim_patient, dim_consultation, dim_diagnostic

    GROUP BY dim_diagnostic.dia_label, dim_consultation.con_date

    WHERE dim_patient.pat_id_pk=dim_consultation.con_id_patient AND dim_consultation.con_code_diag=dim_diagnostic.dia_code ;
```

- Taux global d'hospitalisation des patients dans une période donnée Y (f)
    - Patients (Patient)
    - Hospitalisation (Hospitalisation)
    - Période de temps (Hospitalisation)

```SQL
    SELECT COUNT(dim_patient.pat_nom)/COUNT(dim_hospitalisation.hos_id) , dim_hospitalisation.hos_date FROM dim_patient, dim_hospitalisation

    INNER JOIN dim_hospitalisation ON dim_patient.pat_id_pk = dim_hospitalisation.hos_id_patient

    GROUP BY dim_hospitalisation.hos_date HAVING dim_hospitalisation.hos_date= « 2019 (Y) » ;
```

- Taux d'hospitalisation des patients par rapport à des diagnostics sur une période donnée
    - Patients (Patient)
    - Hospitalisation (Hospitalisation)
    - Diagnostics (Diagnostics)
    - Période donnée (Hospitalisation)
```SQL
    SELECT dim_diagnostic.dia_label, COUNT(dim_patient.pat_nom)/COUNT(dim_hospitalisation.hos_id), dim_hospitalisation.hos_date FROM dim_patient, dim_hospitalisation

    INNER JOIN dim_hospitalisation ON dim_patient.pat_id_pk=dim_hospitalisation.hos_id_patient

    INNER JOIN dim_diagnostic ON dim.diagnostic.dia_code=dim_hospitalisation.hos_id

    GROUP BY dim_hospitalisation.hos_date;
```

- Taux d'hospitalisation/consultation par sexe, par âge
    - Hospitalisation (Hospitalisation)
    - Consultation (Consultation)
    - Sexe (Patient)
    - Age (Patient)

- Taux de consultation par professionnel (f)
    - Consultation (Consultation)
    - Professionnel (Professionnel de santé)

```SQL
    CREATE VIEW vtaux_consultation_professionnel AS

    SELECT COUNT(dim_consultation.con_date)/COUNT(dim_professionnel_sante.ps_nom) FROM dim_consultation, dim_professionnel_sante

    INNER JOIN dim_consultation ON dim_consultation.con_id_pk=dim_professionnel_sante.ps_id_pk;
```

- Nombre de décès par localisation (région) et sur l'année 2019
    - Décès (Décès)
    - Localisation (Décès)
    - année 2019 (Décès)

```SQL
    SELECT dec_nom_region_deces, COUNT(dec_nom) AS Nombre_décès FROM dim_deces 

    WHERE date=2019 GROUP BY dec_nom_region_deces
```

- Taux global de satisfaction par région sur l'année 2020
    -  Satisfaction (satisfaction 2020)
    
```SQL
    SELECT date, sas_region, SUM(sas_score_all_ajust)

    AS Taux_satisfaction_global

    FROM dim_satisfaction WHERE date="2020" GROUP BY date, sas_region;
```
