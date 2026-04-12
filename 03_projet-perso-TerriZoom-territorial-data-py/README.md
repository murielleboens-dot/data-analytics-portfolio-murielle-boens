# TerriZoom — Analyse multi-indicateurs territoriaux

## Contexte

Les débats publics locaux reposent souvent sur des perceptions : sentiment d’insécurité, vision de la pauvreté, perception de niveaux de pollution (ou souvent absence de perception).

Ces perceptions peuvent être partielles, biaisées ou difficilement comparables entre territoires.

Ce projet vise à confronter ces perceptions à des indicateurs objectifs issus de données publiques.

---

## Problématique

Les indicateurs objectifs confirment-ils ou contredisent-ils les perceptions locales concernant la sécurité, le niveau de vie et les niveaux de pollution ?

---

## Objectif du projet

Construire une analyse territoriale basée sur des données multi-sources afin de :

- comparer des indicateurs à différentes échelles géographiques
- analyser leur évolution et leur distribution
- explorer des corrélations entre phénomènes (criminalité, niveau de vie, pollution)

---

## Données

Le projet repose sur plusieurs sources publiques hétérogènes :

### Criminalité
- Source : données publiques (CSV) - Ministère de l'intérieur
- Niveau : communes (INSEE Communes codes)
- Contenu : nombre de faits, types d’infractions

### Niveau de vie
- Source : INSEE (déclarations fiscales)
- Niveau : quartiers dans les communes (IRIS codes)
- Contenu : revenu médian, distribution des revenus

### Qualité de l’air
- Source : API ATMO (JSON)
- Niveau : communes (INSEE Communes codes)
- Contenu : indice ATMO et polluants

---

## Structure du projet

- `N1` — Introduction et cadrage
- `N2` — Sources de données et méthodologie
- `N3` — Collecte et transformation des données
- `N4` — Agrégation des données
- `N5` — Analyse multi-critères
- `cleaned_data/` — fichiers nettoyés
- `requirements.txt` — dépendances

---

## Enjeux data

Les données présentent plusieurs défis :

- formats variés (CSV, gzip, JSON API)
- structures imbriquées (JSON)
- données déclaratives ou partielles

L’enjeu principal est l’harmonisation et la mise en cohérence de ces sources.

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

## Limites

Le projet reste soumis à plusieurs limites structurelles liées aux sources mobilisées :

- couverture géographique inégale, notamment pour la pollution de l’air
- temporalités différentes selon les datasets
- granularités hétérogènes (IRIS, commune, jour, trimestre, année)
- disponibilité partielle de certaines données

Ces contraintes introduisent :
- des limites de comparabilité
- un biais géographique dans le sous-ensemble exploitable
- une prudence nécessaire dans l’interprétation

Le périmètre retenu ne constitue donc **pas une image nationale**, mais un sous-ensemble cohérent de communes comparables au regard des données disponibles.

---

## Choix méthodologiques

Afin de garantir la cohérence de l’analyse, le projet repose sur les choix suivants :

- utiliser **`insee_to_commune`** comme table de référence géographique
- harmoniser les identifiants communaux, y compris pour Paris, Lyon et Marseille
- agréger les différentes sources au **niveau communal**
- restreindre l’analyse principale aux **364 communes complètes**
- privilégier la qualité, la cohérence et la comparabilité des données à l’exhaustivité

Pour la criminalité, seuls les indicateurs les plus robustes ont été retenus pour le noyau principal d’analyse.  
Pour la pollution, les variables ont été traitées comme des **niveaux ordonnés** et agrégées avec des indicateurs adaptés.

Ces choix permettent de construire un cadre analytique solide pour la suite du projet, notamment en vue d’une future analyse multi-critères.

---

## Prochaines étapes

- **N5 — Analyse multi-critères**
  - construction d’indicateurs composites
  - exploration des corrélations
  - aide à la décision territoriale

- **N6 — Dashboard PowerBI**
  - construction d'un premier dashboard test sur PowerBI

---

## Compétences mobilisées à ce stade

- Python (`pandas`, `numpy`, `requests`, `matplotlib`, `seaborn`)
- Intégration et croisement de données multi-sources
- Appels API REST et traitement de fichiers volumineux
- Requêtes SQL avec DuckDB
- Nettoyage, structuration et normalisation de données
- Harmonisation de données hétérogènes (formats, granularités, temporalités)
- Construction de tables de référence géographique
- Analyse exploratoire et agrégation d’indicateurs
- Préparation de données pour l’analyse multi-critères
