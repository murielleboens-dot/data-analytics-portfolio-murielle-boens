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

## Enjeux data

Les données présentent plusieurs défis :

- formats variés (CSV, gzip, JSON API)
- structures imbriquées (JSON)
- données déclaratives ou partielles

L’enjeu principal est l’harmonisation et la mise en cohérence de ces sources.

---

## Pipeline de traitement des données

Le projet met en œuvre un pipeline complet :

1. **Collecte**
   - téléchargement de fichiers compressés (INSEE)
   - lecture de fichiers volumineux (gzip)
   - appel API REST (ATMO)

2. **Transformation**
   - nettoyage des données
   - normalisation des formats
   - conversion des types
   - flattening de structures JSON

3. **Structuration**
   - création de tables analytiques homogènes (DataFrames) :
     - `crime`
     - `declared_income`
     - `disposable_income`
     - `air_pollution` (EN COURS)

   - création de tables de correspondance géographique pour gérer les différentes granularités territoriales et les codes INSEE :
     - `insee_to_commune`
     - `iris_to_commune`

4. **Requêtes et filtrage**
   - utilisation de DuckDB pour requêtes SQL ciblées

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

## Résultats (phase exploratoire)

Cette première phase du projet met en place les bases nécessaires à l’analyse :

- datasets nettoyés et exploitables
- structuration des indicateurs
- premières explorations des distributions

Les analyses approfondies et les corrélations feront l’objet d’une phase ultérieure.

---

## Limites

- hétérogénéité des sources
- granularité différente selon les indicateurs
- données parfois incomplètes
- dépendance à la qualité des déclarations

---

## Compétences mobilisées

- Python (pandas, requests, numpy)
- Manipulation de données multi-sources
- Appels API REST
- Traitement de fichiers volumineux (gzip)
- DuckDB (requêtes SQL)
- Nettoyage et structuration de données
- Analyse exploratoire

---

## Structure du projet

- `N1` — Introduction et cadrage
- `N2` — Sources de données et méthodologie
- `N3` — Collecte et transformation des données
- `cleaned_data/` — fichiers nettoyés
- `requirements.txt` — dépendances