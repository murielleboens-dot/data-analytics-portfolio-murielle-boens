# Fact News Territoires — Analyse multi-indicateurs territoriaux

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
- Niveau : communes
- Contenu : nombre de faits, types d’infractions

### Niveau de vie
- Source : INSEE (déclarations fiscales)
- Niveau : communes
- Contenu : revenu médian, distribution des revenus

### Qualité de l’air
- Source : API ATMO (JSON)
- Niveau : communes
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

## Résultats (phase de préparation des données)

La phase N3 finalisée constitue la couche de préparation analytique du projet.

Elle aboutit à :
- des datasets nettoyés, structurés et exploitables
- une harmonisation multi-sources (formats, temporalité, granularité)
- la mise en place d’une clé géographique commune (`insee_to_commune`)
- un socle de données prêt pour l’agrégation et l’analyse multi-critères

Un point structurant identifié à ce stade :
seules **362 communes disposent de données complètes sur l’ensemble des indicateurs retenus**

Ce sous-ensemble constitue le périmètre analytique de la phase suivante (encore en cours)

---

## Limites

Les principales limites du projet sont liées à l’hétérogénéité des sources :

- couverture géographique inégale (pollution limitée à certaines zones)
- temporalités différentes entre les datasets (2021 à 2025)
- données partielles ou contraintes (notamment pour la criminalité)

Ces éléments introduisent :
- des limites de comparabilité
- un biais géographique dans le sous-ensemble exploitable
- des contraintes d’interprétation des résultats

---

## Choix méthodologiques

Afin de garantir la cohérence de l’analyse, le projet repose sur les choix suivants :

- utilisation d’une clé géographique commune (`insee_to_commune`) comme pivot de jointure
- restriction de l’analyse aux 362 communes disposant de données complètes
- priorité donnée à la qualité et à la comparabilité des données plutôt qu’à la couverture exhaustive

Ce choix améliore la robustesse des analyses, mais introduit un biais géographique qu’il convient de prendre en compte.

---

## Prochaines étapes

- **N4 — Agrégation des données**
  - construction d’une table analytique unique au niveau commune
  - consolidation des indicateurs multi-sources

- **N5 — Analyse multi-critères**
  - construction d’indicateurs composites
  - exploration des corrélations
  - aide à la décision territoriale

---

## Compétences mobilisées à ce stade

- Python (pandas, requests, numpy)
- Manipulation de données multi-sources
- Appels API REST
- Traitement de fichiers volumineux (gzip)
- DuckDB (requêtes SQL)
- Nettoyage et structuration de données
- Analyse exploratoire
