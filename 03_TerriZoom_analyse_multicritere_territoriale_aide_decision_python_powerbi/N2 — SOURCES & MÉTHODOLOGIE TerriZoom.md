# TerriZoom — Sources de données et méthodologie

## Données

Le projet repose sur une base communale consolidée construite à partir de plusieurs sources publiques hétérogènes.  
Toutes les données ont été harmonisées dans une table unique au niveau **commune** (`insee_commune_id`), afin de permettre des croisements entre dimensions socio-économiques, territoriales, sécuritaires et environnementales.

La base complète mobilisée dans le projet est plus large que le sous-ensemble utilisé ensuite pour l’exercice **TOPSIS** ou **ELECTRE**. Ce dernier ne constitue qu’une sélection de variables jugées représentatives pour un premier classement multicritère.

### 1. Référentiel territorial et variables de contexte

**Niveau final utilisé :** commune

**Contenu :**
- identifiant INSEE de la commune
- nom de la commune
- population totale
- classe de densité
- libellé de densité

**Sources :**
- référentiels géographiques INSEE

**Couverture temporelle :**
- géographie harmonisée à partir des correspondances territoriales **2021**

### 2. Services, équipements et activité territoriale

**Niveau final utilisé :** commune

**Contenu :**
- `count_agriculture_per_1000`
- `count_industry_per_1000`
- `count_construction_per_1000`
- `count_commercial_services_per_1000`
- `count_public_services_per_1000`
- `total_count_per_1000`

Ces variables décrivent la densité de différentes catégories d’activités et de services pour 1 000 habitants.

**Source :**
- données publiques d’activité économique et de services agrégées au niveau communal dans le pipeline

**Couverture temporelle :**
- **2024**

### 3. Niveau de vie et structure des revenus

**Niveau source :** IRIS  
**Niveau final utilisé :** commune après agrégation

**Contenu :**
- `revenu_q1_eur`
- `revenu_median_eur`
- `revenu_q3_eur`
- `revenu_d1_eur`
- `revenu_d9_eur`
- `taux_bas_revenus_pct`
- `part_revenus_activite_pct`
- `part_pensions_pct`
- `part_revenus_patrimoine_pct`
- `part_prestations_sociales_pct`
- `part_prestations_familiales_pct`
- `part_minima_sociaux_pct`
- `part_prestations_logement_pct`
- `part_impots_pct`

La base contient aussi plusieurs variables dérivées utiles pour l’exploration :
- `revenu_median_eur_std`
- `taux_bas_revenus_pct_std`
- `part_prestations_sociales_pct_std`
- `revenu_median_eur_iqr`

**Source :**
- **INSEE**, données fiscales et sociales localisées

**Couverture temporelle :**
- **2021**

### 4. Sécurité et délinquance

**Niveau final utilisé :** commune

**Contenu :**
- `cambriolages_de_logement_per_1000`
- `destructions_et_degradations_volontaires_per_1000`
- `escroqueries_et_fraudes_aux_moyens_de_paiement_per_1000`
- `usage_de_stupefiants_per_1000`
- `violences_physiques_hors_cadre_familial_per_1000`
- `violences_physiques_intrafamiliales_per_1000`
- `vols_dans_les_vehicules_per_1000`
- `vols_de_vehicule_per_1000`
- `vols_sans_violence_contre_des_personnes_per_1000`

Les indicateurs sont exprimés pour 1 000 habitants afin de rendre les communes comparables malgré les écarts de population.

La table finale conserve également :
- `n_years_available` : nombre d’années disponibles pour la commune

**Source :**
- données publiques du **Ministère de l’Intérieur / SSMSI**

**Couverture temporelle :**
- **2016 à 2024**

### 5. Pollution de l’air

**Niveau final utilisé :** commune

**Contenu :**
- nombre d’observations : `air_pollution_n_obs`
- niveaux moyens et maximums par polluant :
  - `no2_level_mean`, `no2_level_max`
  - `o3_level_mean`, `o3_level_max`
  - `pm10_level_mean`, `pm10_level_max`
  - `pm25_level_mean`, `pm25_level_max`
  - `so2_level_mean`, `so2_level_max`
- parts d’observations dans les niveaux élevés :
  - `no2_level_share_ge4`, `o3_level_share_ge4`, `pm10_level_share_ge4`, `pm25_level_share_ge4`, `so2_level_share_ge4`
  - `no2_level_share_ge5`, `o3_level_share_ge5`, `pm10_level_share_ge5`, `pm25_level_share_ge5`, `so2_level_share_ge5`

La base actuelle repose donc sur des variables fines par polluant, et non sur le seul indice ATMO global.

**Source :**
- **API ATMO** / données ouvertes de qualité de l’air

**Couverture temporelle :**
- **2025**

### 6. Logique de consolidation

La table finale n’est pas une copie brute de chaque source nationale. Elle résulte d’un travail de :
- standardisation des formats
- harmonisation des identifiants géographiques
- agrégation des données IRIS au niveau communal lorsque nécessaire
- normalisation de certaines variables par population
- restriction à l’intersection des communes disposant d’un recouvrement exploitable entre plusieurs sources

Cette étape est importante, car toutes les sources ne couvrent ni les mêmes territoires, ni les mêmes années, ni la même maille géographique.

### 7. Place du sous-ensemble TOPSIS

Le **TOPSIS** ou **ELECTRE** utilisé dans le projet repose uniquement sur un **sous-ensemble de variables** choisi pour sa représentativité dans le cadre de l’exercice.

Il combine quatre dimensions :
- **dynamisme territorial**
- **niveau de vie**
- **sécurité**
- **pollution**

Ce sous-ensemble sert à illustrer une méthode de classement multicritère. Il ne résume donc pas à lui seul toute la base consolidée.