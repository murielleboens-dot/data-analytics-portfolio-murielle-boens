## N6 — POWER BI DASHBOARD - TerriZoom

## Rôle de la phase N6

La phase **N6** transforme la table analytique communale produite dans les phases **N3**, **N4** et **N5** en un **dashboard Power BI interactif** destiné à l’exploration, à la comparaison et à la restitution des résultats.

L’objectif n’est pas seulement de visualiser quelques indicateurs, mais de proposer une **lecture territoriale synthétique et navigable** à partir d’un sous-ensemble cohérent de communes disposant d’un socle commun de données comparables.

---

## Rappel de la problématique

**Comment construire une lecture multicritère cohérente des communes à partir de données publiques hétérogènes portant sur le niveau de vie, les dimensions socio-économiques, la criminalité et la qualité de l’air ?**

Le dashboard répond à cette problématique en rendant plus lisibles les arbitrages entre plusieurs dimensions territoriales, sans masquer les limites de couverture des sources mobilisées.

---

## Périmètre d’analyse

Le dashboard repose sur un **sous-ensemble de 364 communes** pour lesquelles un socle commun de données a pu être consolidé.

Ce périmètre couvre principalement une **zone élargie de Marseille et de Lyon**, car ce sont les communes pour lesquelles le croisement des sources socio-économiques, de sécurité et de qualité de l’air était le plus exploitable dans le cadre du projet.

Le tableau de bord ne constitue donc **pas une vision nationale exhaustive**, mais un **périmètre comparatif cohérent**.

---

## Objectifs du dashboard

La phase N6 vise à :

1. transformer la table finale au niveau **commune** en dataset prêt pour la visualisation ;
2. construire des vues interactives pour comparer les communes selon plusieurs dimensions ;
3. mettre en évidence des profils territoriaux multi-critères ;
4. faciliter l’interprétation d’indicateurs socio-économiques, de sécurité et d’environnement ;
5. proposer une restitution plus lisible des résultats issus des phases analytiques précédentes.

---

## Structure du dashboard

Le dashboard est organisé en **trois pages complémentaires**.

### 1. Vue d’ensemble socio-économique et territoriale

Cette première page propose une entrée synthétique sur le périmètre retenu :
- nombre de communes ;
- population totale ;
- revenu médian moyen ;
- services publics pour 1 000 habitants ;
- cambriolages pour 1 000 habitants ;
- part moyenne d’exposition aux PM2,5 en niveau ATMO ≥ 4 ;
- comparaison du revenu médian selon le type de commune ;
- top 15 des communes par revenu médian ;
- table de détail communale.

![Dashboard TerriZoom — Page 1](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%201.png)

#### Résultats clés — page 1

- Les **communes urbaines intermédiaires et urbaines denses** présentent en moyenne les revenus médians les plus élevés du périmètre. Les **communes rurales**, en particulier hors espaces périurbains, apparaissent globalement avec des revenus médians plus bas.
- La hiérarchie des communes les plus favorisées fait ressortir plusieurs communes de l’aire lémanique ou de zones à fort niveau de vie.
- Les indicateurs de synthèse montrent immédiatement qu’une lecture territoriale pertinente suppose de croiser **revenu, services, sécurité et environnement**, et non de raisonner sur un seul critère.

---

### 2. Croisement de données socio-économiques et criminalité

Cette page explore les relations entre plusieurs variables :
- revenu médian et cambriolages ;
- services publics pour 1 000 habitants et revenu médian ;
- top 15 des communes par taux de violences intrafamiliales.

La taille des bulles est utilisée pour représenter la **taille des communes**.

![Dashboard TerriZoom — Page 2](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%202.png)

#### Résultats clés — page 2

- Le nuage **revenu médian / cambriolages** montre une dispersion réelle : il n’existe pas de relation simple et mécanique entre niveau de vie et taux de cambriolages.
- Certaines communes plus favorisées affichent malgré tout des niveaux de cambriolages élevés, ce qui invite à éviter toute lecture trop linéaire.
- Le nuage **services publics / revenu médian** montre qu’un niveau plus élevé de services publics pour 1 000 habitants ne correspond pas automatiquement à un revenu médian plus élevé.
- Le classement par **violences intrafamiliales** rappelle qu’un diagnostic territorial crédible doit intégrer des indicateurs de sécurité différenciés, et non seulement des indicateurs agrégés de prospérité.

---

### 3. Exposition moyenne aux polluants atmosphériques

Cette page est dédiée à la qualité de l’air. Elle présente :
- la part moyenne d’exposition en niveau **ATMO ≥ 4** pour l’ozone, les PM2,5 et les PM10 ;
- les communes les plus exposées pour l’ozone ;
- les communes les plus exposées pour les PM2,5 ;
- une table de consultation des niveaux moyens par polluant.

![Dashboard TerriZoom — Page 3](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%203.png)

#### Résultats clés — page 3

- Parmi les polluants visualisés, **l’ozone** présente la part moyenne d’exposition élevée la plus importante sur le périmètre retenu.
- Les **PM2,5** restent présentes à un niveau non négligeable, mais inférieur à celui observé pour l’ozone.
- Les **PM10** apparaissent en moyenne moins présents dans les niveaux élevés au sein du sous-ensemble comparé.
- La ventilation par commune met en évidence des profils locaux distincts, ce qui renforce l’intérêt d’une lecture environnementale à l’échelle communale.

---

## Ce que la phase N6 apporte au projet

La phase N6 joue un rôle de **restitution analytique**. Elle permet de :

- passer d’une logique de tables et de notebooks à une logique d’**exploration visuelle** ;
- rendre les résultats du projet plus accessibles à un décideur ou un lecteur non technique ;
- montrer la cohérence entre la préparation des données, l’analyse multicritère et la communication des résultats ;
- mettre en évidence des contrastes territoriaux qui restent plus difficiles à saisir dans un notebook seul.

---

## Conclusions à ce stade

Le dashboard confirme plusieurs points importants du projet **TerriZoom** :

- la **comparabilité territoriale** exige un fort travail préalable d’harmonisation des sources ;
- les communes ne se laissent pas décrire de manière pertinente par un indicateur unique ;
- les relations entre dimensions socio-économiques, sécurité et environnement sont **réelles mais non mécaniques** ;
- la restitution visuelle permet de mieux faire émerger des profils contrastés et des communes atypiques ;
- les résultats doivent toujours être interprétés à la lumière du **périmètre réel d’analyse** : 364 communes comparables, principalement situées dans la zone élargie de Marseille et de Lyon.

Le dashboard constitue donc une **brique de communication et d’aide à la décision**, adossée à un travail complet de collecte, nettoyage, agrégation et analyse multicritère.

---

## Livrables associés

- `TerriZoom Dashboard - Power BI.pbix`
- `TerriZoom Dashboard - Power BI - Page 1.png`
- `TerriZoom Dashboard - Power BI - Page 2.png`
- `TerriZoom Dashboard - Power BI - Page 3.png`

---

## Compétences mobilisées

- Power BI
- data visualization
- structuration d’un dashboard décisionnel
- traduction d’une analyse notebook en interface visuelle
- sélection et hiérarchisation d’indicateurs
- restitution de résultats multi-sources et multi-critères
