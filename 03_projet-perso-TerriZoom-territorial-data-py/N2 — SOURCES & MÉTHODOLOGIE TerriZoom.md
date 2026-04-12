# TerriZoom — Sources de données et méthodologie

## 1. Criminalité

Source : données publiques Ministère de l'Intérieur (CSV)

- Niveau : communes
- Variables :
  - nombre de faits
  - types d’infractions

Limites :
- dépend des déclarations et du type d'infraction: certaines infractions restent confidentielles

---

## 2. Niveau de vie

Source : Données publiques INSEE (déclarations de revenus)

- Niveau : quartiers de communes
- Variables :
  - revenu médian, Q1, Q3, D1, D9 
  - distribution
  - patrimoine, activité
  - prestations sociales

Limites :
- temporalité restreinte

---

## 3. Qualité de l’air

Source : API ATMO

- Indicateur : indice ATMO
- Niveau : commune

Avantages :
- indicateur standardisé
- multi-polluants

Limites :
- dépend des stations de mesure
- disponibilité variable selon les secteurs géographiques
- difficulté d'accès: seule de petites quantités de données peuvent être remontées par l'API.

---

## Méthodologie

1. Collecte des données
2. Harmonisation des formats
3. Création de tables analytiques
4. Analyse exploratoire
5. Visualisation