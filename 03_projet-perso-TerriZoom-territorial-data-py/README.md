# TerriZoom — Analyse multi-indicateurs territoriaux

## Contexte

Les territoires locaux sont souvent décrits à travers des dimensions qui interagissent fortement entre elles :
niveau de vie, vulnérabilité sociale, dynamiques économiques, sécurité, environnement.

Ces dimensions sont généralement observées à travers des sources hétérogènes, produites à des échelles différentes et difficilement lisibles lorsqu’elles sont prises séparément.

TerriZoom vise à croiser ces données publiques afin de construire une lecture territoriale plus structurée, comparative et exploitable à l’échelle communale.

---

## Problématique

Comment construire une lecture multicritère cohérente des communes à partir de données publiques hétérogènes portant sur le niveau de vie, les fragilités sociales, la sécurité et la pollution ?

---

## Objectif du projet

Construire un cadre d’analyse territoriale multicritère à l’échelle communale afin de :

- harmoniser et croiser plusieurs sources de données publiques
- produire des indicateurs comparables entre communes
- explorer les profils territoriaux selon plusieurs dimensions
- tester des méthodes d’aide à la décision multicritère (TOPSIS, ELECTRE III)
- rendre les résultats lisibles dans un dashboard Power BI

---

## Données

Le projet repose sur une base consolidée au niveau **commune** (`insee_commune_id`), construite à partir de plusieurs sources publiques hétérogènes.

### Référentiel territorial
- **Sources :** INSEE, tables de correspondance **IRIS → commune**
- **Niveau final :** commune
- **Contenu :** identifiant INSEE, nom de commune, population, densité, nombre d’IRIS rattachés
- **Couverture :** référentiel géographique **2021**

### Services, équipements et activité territoriale
- **Sources :** données publiques d’activité et de services agrégées au niveau communal
- **Niveau final :** commune
- **Contenu :** densités pour 1 000 habitants de plusieurs catégories d’activités et de services (`agriculture`, `industry`, `construction`, `commercial_services`, `public_services`, `total`)
- **Couverture :** **2024**

### Niveau de vie
- **Source :** **INSEE**, données fiscales et sociales localisées
- **Niveau source :** IRIS
- **Niveau final :** commune après agrégation
- **Contenu :** revenus, bas revenus et structure des ressources des ménages
- **Couverture :** **2021**

### Sécurité et délinquance
- **Source :** données publiques du **Ministère de l’Intérieur / SSMSI**
- **Niveau final :** commune
- **Contenu :** indicateurs de délinquance pour 1 000 habitants selon plusieurs types d’infractions
- **Couverture :** **2016–2024**

### Pollution de l’air
- **Source :** **API ATMO** / données ouvertes de qualité de l’air
- **Niveau final :** commune
- **Contenu :** niveaux moyens, maxima et parts d’observations dans les niveaux élevés pour plusieurs polluants (`NO2`, `O3`, `PM10`, `PM2.5`, `SO2`)
- **Couverture :** **2025**

**Remarque :** le **TOPSIS**/**ELECTRE** utilisé dans le projet ne mobilise qu’un sous-ensemble de cette base complète, sélectionné pour l’exercice de classement multicritère.

---

## Structure du projet

- `N1` — Introduction et cadrage
- `N2` — Sources de données et méthodologie
- `N3` — Collecte et transformation des données
- `N4` — Agrégation des données
- `N5` — Analyse multicritère
- `N6` — Tableau de bord Power BI
- `cleaned_data/` — fichiers nettoyés
- `requirements.txt` — dépendances

---

## Enjeux data

Les données présentent plusieurs défis :

- formats variés (CSV, gzip, JSON API)
- mailles géographiques différentes (IRIS, commune)
- couvertures temporelles hétérogènes selon les sources
- structures parfois imbriquées ou partielles
- nécessité de normaliser certaines variables par la population
- restriction finale au sous-ensemble de communes disposant d’un recouvrement exploitable entre sources

L’enjeu principal est donc l’harmonisation des identifiants, des échelles géographiques et des périodes d’observation afin de construire une base cohérente pour l’analyse multicritère.

---

## Pipeline de traitement des données

Le projet met en œuvre un pipeline complet allant de la collecte à la structuration analytique des données :

1. **Collecte**
   - téléchargement de fichiers compressés (INSEE)
   - lecture de fichiers volumineux (gzip)
   - appel API REST (ATMO)

2. **Transformation**
   - nettoyage et filtrage des données
   - normalisation des formats
   - conversion des types

3. **Structuration**
   - création de tables analytiques homogènes (DataFrames) exportées dans `cleaned_data/` :
     - `crime`
     - `declared_income_2021`
     - `disposable_income_2021`
     - `economy`
     - `air_pollution`
     - tables spécifiques (ex : arrondissements Marseille)

   - mise en place d’une table pivot géographique permettant les jointures inter-sources :
     - `insee_to_commune` (clé centrale du modèle)

4. **Requêtes et filtrage**
   - utilisation de DuckDB pour requêtes SQL ciblées et validation des transformations

A titre d'exemple, visualisez le code exploratoire de données [ici](https://github.com/murielleboens-dot/data-analytics-portfolio-murielle-boens/blob/main/03_projet-perso-TerriZoom-territorial-data-py/N4%20%E2%80%94%20EXPLORE%20%26%20AGGREGATE%20TerriZoom.ipynb)

---

## Méthodologie

L’approche repose sur une logique d’analyse multi-factorielle :

- mise en relation d’indicateurs de nature différente
- comparaison entre territoires
- analyse exploratoire des distributions

Une attention particulière est portée :

- aux biais liés aux données déclaratives
- aux différences de granularité
- aux limites d’interprétation des corrélations

---

## Résultats (phases N3 et N4)

Les phases **N3 (collecte / nettoyage)** et **N4 (exploration / agrégation)** ont permis de poser la base analytique du projet **TerriZoom**.

À ce stade, le projet a permis de :
- nettoyer et structurer plusieurs sources de données publiques
- harmoniser des jeux de données de granularités et temporalités différentes
- construire une clé géographique commune (`insee_to_commune`)
- agréger les principales sources au niveau communal
- produire une première table analytique unifiée : `terrizoom_communes`

La phase N4 a également permis de :
- consolider les données de revenus à partir des IRIS
- sélectionner et agréger les indicateurs de criminalité les plus exploitables
- agréger les données de pollution de l’air à l’échelle communale
- mener une première exploration des distributions, couvertures et dynamiques territoriales

Un point méthodologique important a été identifié puis corrigé :
les cas particuliers de **Paris, Lyon et Marseille** ont nécessité une harmonisation spécifique des codes d’arrondissements municipaux afin d’être correctement réintégrés au niveau communal.

Le périmètre principal d’analyse repose désormais sur **364 communes disposant de données complètes** sur les sources retenues.

---

## Résultats - phase N5 - ANALYSE MULTICRITERES

La phase N5 (analyse multicritère) a permis de construire un premier cadre de comparaison territoriale à partir de la table communale consolidée produite en N4.

À ce stade, le projet a permis de :

- retenir un sous-ensemble de critères couvrant quatre dimensions : **dynamisme territorial**, **niveau de vie**, **sécurité** et **pollution**
- définir le sens de préférence de chaque critère (**bénéfice** ou **coût**)
- conserver l’échantillon complet par traitement simple des dernières valeurs manquantes
- produire deux scénarios de classement avec **TOPSIS**
- implémenter une lecture complémentaire avec **ELECTRE III**
- comparer les résultats obtenus selon les pondérations et selon la méthode mobilisée

La phase N5 montre que les résultats multicritères sont **sensibles aux choix méthodologiques**, sans être totalement instables : certains profils communaux restent robustes, tandis que d’autres varient selon que l’on privilégie une logique de proximité à un profil idéal (TOPSIS) ou une logique de surclassement pair à pair (ELECTRE III).

Le périmètre principal d’analyse repose sur **364 communes**.

---

## Résultats — phase N6 - TABLEAU DE BORD POWER BI

La phase **N6** transforme les résultats analytiques du projet en un **tableau de bord interactif** organisé en trois pages.

### Page 1 — Vue d’ensemble socio-économique et territoriale
![ tableau de bord TerriZoom — Page 1](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%201.png)

### Page 2 — Croisement de données socio-économiques et criminalité
![ tableau de bord TerriZoom — Page 2](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%202.png)

### Page 3 — Exposition moyenne aux polluants atmosphériques
![ tableau de bord TerriZoom — Page 3](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%203.png)

[Voir la page dédiée au dashboard Power BI](https://github.com/murielleboens-dot/data-analytics-portfolio-murielle-boens/blob/main/03_projet-perso-TerriZoom-territorial-data-py/N6%20%E2%80%94%20POWER%20BI%20DASHBOARD%20-%20TerriZoom%20.md)

---

## Conclusions du projet à ce stade

À ce stade, **TerriZoom** montre qu’une lecture territoriale sérieuse demande :

- un fort travail d’**harmonisation de sources hétérogènes** ;
- une sélection rigoureuse d’un **périmètre comparable** ;
- une lecture **multicritère**, car aucun indicateur isolé ne suffit à décrire un territoire ;
- une attention constante aux **limites d’interprétation**, en particulier lorsque les temporalités et les couvertures diffèrent selon les sources.

Le projet permet également de mettre en évidence plusieurs enseignements :

- les contrastes territoriaux restent visibles dès la phase descriptive ;
- les relations entre niveau de vie, services, sécurité et pollution sont réelles, mais rarement simples ;
- la restitution visuelle dans Power BI améliore fortement la lisibilité des résultats pour un usage exploratoire ou décisionnel ;
- les résultats multicritères dépendent des choix méthodologiques, ce qui justifie une approche prudente et transparente.

En l’état, le projet ne propose **pas une image exhaustive de toutes les communes françaises**, mais un **cadre analytique cohérent** appliqué à **364 communes comparables**, principalement situées dans la zone élargie de Marseille et de Lyon.

---

## Limites

Le projet reste soumis à plusieurs limites structurelles liées aux sources mobilisées :

- couverture géographique inégale, notamment pour la pollution de l’air ;
- temporalités différentes selon les datasets ;
- granularités hétérogènes (IRIS, commune, jour, trimestre, année) ;
- disponibilité partielle de certaines données.

Ces contraintes introduisent :
- des limites de comparabilité ;
- un biais géographique dans le sous-ensemble exploitable ;
- une prudence nécessaire dans l’interprétation.

Le périmètre retenu ne constitue donc **pas une image nationale**, mais un sous-ensemble cohérent de communes comparables au regard des données disponibles.

---

## Choix méthodologiques

Afin de garantir la cohérence de l’analyse, le projet repose sur les choix suivants :

- utiliser **`insee_to_commune`** comme table de référence géographique ;
- harmoniser les identifiants communaux, y compris pour Paris, Lyon et Marseille ;
- agréger les différentes sources au **niveau communal** ;
- restreindre l’analyse principale aux **364 communes complètes** ;
- privilégier la qualité, la cohérence et la comparabilité des données à l’exhaustivité.

Pour la criminalité, seuls les indicateurs les plus robustes ont été retenus pour le noyau principal d’analyse.  
Pour la pollution, les variables ont été traitées comme des **niveaux ordonnés** et agrégées avec des indicateurs adaptés.

---

## Compétences mobilisées

- Python (`pandas`, `numpy`, `requests`, `matplotlib`, `seeborn`) ;
- intégration et croisement de données multi-sources ;
- traitement de fichiers hétérogènes (CSV, gzip, JSON API) ;
- requêtes analytiques avec DuckDB ;
- nettoyage, structuration et normalisation de données ;
- harmonisation de sources de granularités et temporalités différentes ;
- construction de référentiels et de correspondances géographiques ;
- analyse exploratoire et agrégation d’indicateurs ;
- préparation de données pour l’analyse multicritère ;
- mise en œuvre de méthodes MCDA : TOPSIS et ELECTRE III ;
- analyse de sensibilité via scénarios de pondération ;
- Power BI ;
- data visualization ;