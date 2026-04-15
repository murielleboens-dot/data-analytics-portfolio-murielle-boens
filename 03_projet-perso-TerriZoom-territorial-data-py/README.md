# TerriZoom — Analyse multi-indicateurs territoriaux

---

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

## Résultats — phases N3 et N4

Les phases N3 (collecte  nettoyage) et N4 (exploration  agrégation) ont permis de poser la base analytique du projet TerriZoom.

À ce stade, le projet a permis de :

 - nettoyer et structurer plusieurs sources de données publiques
 - harmoniser des jeux de données de granularités et temporalités différentes
 - construire une clé géographique commune (`insee_to_commune`)
 - agréger les principales sources au niveau communal
 - produire une première table analytique unifiée  `terrizoom_communes`

La phase N4 a également permis de 

 - consolider les données de revenus à partir des IRIS
 - sélectionner et agréger les indicateurs de criminalité les plus exploitables
 - agréger les données de pollution de l’air à l’échelle communale
 - mener une première exploration des distributions, couvertures et dynamiques territoriales

Un point méthodologique important a été identifié puis corrigé  les cas particuliers de Paris, Lyon et Marseille ont nécessité une harmonisation spécifique des codes d’arrondissements municipaux afin d’être correctement réintégrés au niveau communal.

Le périmètre principal d’analyse repose désormais sur 364 communes disposant de données complètes sur les sources retenues.

### Validation du sous-ensemble retenu  un périmètre cohérent mais non représentatif de la France entière

La constitution du sous-ensemble de 364 communes a permis de disposer d’un périmètre de travail cohérent, exploitable et comparable entre plusieurs sources. En revanche, ce sous-ensemble n’est pas représentatif de l’ensemble des communes françaises du point de vue du profil de densité.

La comparaison entre le subset retenu et l’ensemble des communes françaises montre en effet une surreprésentation très forte des classes de densité 1 et 2, c’est-à-dire des communes denses et urbaines intermédiaires. À l’inverse, les classes 3 et 4, dominantes à l’échelle nationale, donc davantage associées aux espaces périurbains et ruraux, sont très peu présentes dans le sous-ensemble final.

Cela met en évidence un biais de sélection urbain marqué, directement lié à la contrainte de complétude croisée des données. En pratique, les analyses produites par TerriZoom ne doivent donc pas être lues comme une image générale de la France communale, mais comme une lecture appliquée à un périmètre plus urbain, plus dense et plus riche en données exploitables.

![Analyse de représentativité du subset retenu](./images/TerriZoom%20-%20Analyse%20de%20représentativité%20du%20subset%20retenu.pngimagesTerriZoom%20-%20Analyse%20de%20repr%C3%A9sentativit%C3%A9%20du%20subset%20retenu.png)

### Corrélations socio-économiques  le revenu seul ne suffit pas

L’exploration des variables socio-économiques met en évidence une structure cohérente autour de deux grands axes complémentaires.

Le premier axe est un axe de vulnérabilité sociale. Les IRIS présentant une part plus importante de ménages à bas revenus affichent également une part plus forte de prestations sociales. De la même manière, les IRIS où la part des revenus d’activité est élevée tendent à présenter une dépendance plus faible aux minima sociaux. Cette configuration dessine une opposition lisible entre des territoires davantage portés par les revenus du travail et d’autres plus dépendants des transferts.

Le second axe est un axe de composition du revenu. La relation entre revenus élevés et revenus patrimoniaux apparaît positive, mais plus dispersée. La relation entre part des revenus d’activité et revenu médian est elle aussi positive, tout en restant modérée. Autrement dit, le niveau de revenu importe, mais sa structure interne importe tout autant.

Ces résultats confirment qu’un revenu médian seul ne suffit pas à caractériser les profils socio-économiques. Les indicateurs de revenus d’activité, de revenus patrimoniaux, de minima sociaux et de transferts sociaux apportent une information complémentaire décisive pour comprendre la diversité des situations locales.

![Corrélation revenus activité et minima sociaux](./images/TerriZoom%20-%20Correlation%20revenus%20activite%20et%20minima%20sociaux.png)
![Corrélation revenus du patrimoine et salaire median](./images/TerriZoom%20-%20Correlation%20revenus%20du%20patrimoine%20et%20salaire%20median.png)

### Saisonnalité des polluants  des dynamiques différenciées selon les polluants

L’analyse des séries journalières et trimestrielles de pollution met en évidence des dynamiques saisonnières nettement différenciées.

L’ozone (O3) suit un profil de saison chaude très marqué. Son niveau moyen quotidien augmente progressivement de l’hiver vers le printemps puis l’été, atteint ses niveaux les plus élevés en T2 et T3, puis recule en T4. Les distributions trimestrielles confirment cette lecture  les niveaux les plus dégradés se concentrent davantage pendant les périodes chaudes.

À l’inverse, les PM2,5 présentent un profil de saison froide beaucoup plus net. Les niveaux apparaissent plus élevés et plus variables en T1, baissent en T2, atteignent leur point bas en T3, puis remontent à nouveau en T4. Là encore, la distribution trimestrielle des niveaux confirme le déplacement vers des niveaux plus élevés pendant les périodes froides.

Dans la même logique, le NO2 et les PM10 suivent eux aussi plutôt une dynamique de saison froide, tandis que le SO2 reste globalement stable, avec un profil peu informatif comparativement aux autres polluants.

Ces résultats montrent que la phase N4 ne se limite pas à agréger des données  elle fait déjà apparaître des structures temporelles interprétables, utiles pour comprendre les logiques environnementales du périmètre étudié.

![Saisonnalité des polluants - Ozone - vue 1](./images/TerriZoom%20-%20Saisonnalité%20des%20polluants%20-%20Ozone%20-%20vue%202.png)
![Saisonnalité des polluants - Ozone - vue 2](./images/TerriZoom%20-%20Saisonnalité%20des%20polluants%20-%20Ozone.png)
![Saisonnalité des polluants - PM2,5 - vue 1](./images/TerriZoom%20-%20Saisonnalité%20des%20polluants%20-%20PM2,5%20-%20vue%202.png)
![Saisonnalité des polluants - PM2,5 - vue 2](./images/TerriZoom%20-%20Saisonnalité%20des%20polluants%20-%20PM2,5.png)

### Criminalité  des profils territoriaux spécialisés plutôt qu’un bloc homogène

L’exploration des principaux indicateurs de criminalité montre que les communes qui ressortent fréquemment dans les classements élevés ne forment pas un groupe homogène de “forte criminalité”. Elles présentent au contraire des profils différenciés, relativement stables dans le temps selon les types d’infraction observés.

Saint-Tropez se distingue surtout pour les vols sans violence contre les personnes. Saint-Quentin-Fallavier ressort fortement sur l’usage de stupéfiants. Grenoble apparaît davantage sur des atteintes aux biens. Marseille ressort plus nettement sur certains indicateurs comme le vol de véhicules et des niveaux élevés d’usage de stupéfiants. Lyon conserve des niveaux durablement élevés sur les vols sans violence contre les personnes. En parallèle, la fraude aux moyens de paiement et les violences physiques intrafamiliales montrent des tendances plus diffuses à la hausse dans plusieurs communes.

Le “top” des communes observées doit donc être interprété comme un ensemble de profils territoriaux spécialisés selon les formes de délinquance, et non comme un cluster uniforme.

Un point de vigilance méthodologique important apparaît ici  la normalisation pour 1 000 habitants améliore la comparabilité apparente, mais peut devenir plus fragile dans les communes à forte population présente non résidente, notamment touristiques ou saisonnières. Des communes comme Saint-Tropez ou Cannes peuvent ainsi voir leur population réelle largement dépasser leur population municipale officielle pendant une grande partie de l’année, ce qui peut conduire à surestimer certains taux rapportés aux seuls habitants permanents.

![Usage de stupéfiants - Top 10 communes](./images/TerriZoom%20-%20Usage%20de%20stupefiants%20-%20TOP%2010%20Communes.png)
![Violences intrafamiliales - Top 10 communes](./images/TerriZoom%20-%20Violences%20intra%20familiales%20%20-%20TOP%2010%20Communes.png)
![Vols sans violence - Top 10 communes](./images/TerriZoom%20-%20Vols%20sans%20violence%20%20-%20TOP%2010%20Communes.png)

---

## Résultats — phase N5 — Analyse multicritère

La phase N5 a permis de construire un premier cadre de comparaison territoriale à partir de la table communale consolidée produite en N4.

À ce stade, le projet a permis de :

 - retenir un sous-ensemble de critères couvrant quatre dimensions  dynamisme territorial, niveau de vie, sécurité et pollution
 - définir le sens de préférence de chaque critère (bénéfice ou coût)
 - conserver l’échantillon complet par traitement simple des dernières valeurs manquantes
 - produire deux scénarios de classement avec TOPSIS
 - implémenter une lecture complémentaire avec ELECTRE III
 - comparer les résultats obtenus selon les pondérations et selon la méthode mobilisée

### Sensibilité aux pondérations  un classement sensible, mais non instable

Sous le second scénario de pondération, le haut du classement demeure partiellement stable, tout en étant moins dominé par un seul profil très dynamique.

Saint-Tropez conserve la première place, ce qui suggère que son positionnement ne repose pas uniquement sur un critère exceptionnel, mais sur une combinaison plus large d’indicateurs favorables. D’autres communes restent durablement bien classées, comme Valbonne, Chamonix-Mont-Blanc, Bourg-Saint-Maurice, Lavandou, Biot ou Sainte-Maxime, ce qui indique des profils multicritères relativement robustes.

Quelques mouvements apparaissent néanmoins. Saint-Flour entre dans le top 10, tandis qu’Aubière recule légèrement. Cela montre que la hiérarchie est sensible aux choix de pondération, mais sans devenir incohérente ou erratique.

En bas de classement, les communes les moins bien classées restent globalement associées à une combinaison moins favorable de vulnérabilité sociale, de niveau de revenu, et selon les cas d’indicateurs de sécurité ou de pollution plus dégradés. Autrement dit, les hypothèses de pondération modifient la hiérarchie, mais les profils extrêmes conservent une certaine cohérence.

![Analyse multicritère TOPSIS](./images/TerriZoom%20-%20Analyse%20multicritère%20TOPSIS.png)

### TOPSIS et ELECTRE III  deux logiques d’évaluation réellement différentes

La comparaison entre TOPSIS et ELECTRE III montre que les deux méthodes ne récompensent pas exactement les mêmes profils communaux.

TOPSIS valorise les communes les plus proches d’un profil idéal équilibré. La logique est compensatoire  un bon niveau sur certains critères peut compenser des positions plus faibles sur d’autres, tant que l’ensemble reste proche d’une solution considérée comme favorable.

ELECTRE III, au contraire, se montre plus sensible aux relations de surclassement pair à pair. Une commune n’a pas besoin d’être proche d’un optimum global pour bien ressortir. Elle peut être bien classée dès lors qu’elle surclasse fréquemment d’autres communes sans être fortement dominée sur les critères importants.

Cette différence de logique apparaît clairement dans les résultats. Plusieurs communes situées seulement au milieu de la hiérarchie TOPSIS remontent fortement sous ELECTRE III, comme Thônes, Motte-Servolex, Chamalières, Monistrol-sur-Loire ou Yzeure. À l’inverse, certaines communes bien positionnées dans TOPSIS demeurent relativement solides dans les deux approches, comme Chamonix-Mont-Blanc, Bourg-Saint-Maurice, Saint-Jean-de-Maurienne ou Aubière, ce qui suggère qu’une partie du signal multicritère reste robuste d’une méthode à l’autre.

Au final, la comparaison confirme que les résultats multicritères sont sensibles aux choix méthodologiques, non seulement en matière de pondération, mais aussi de méthode de décision. Ce point renforce l’intérêt d’une démarche transparente  un classement n’est jamais “neutre”, il dépend toujours d’une architecture de choix explicites.

![Comparaison ELECTRE III et TOPSIS](./images/TerriZoom%20-%20Analyse%20multicritère%20comparaison%20ELCTRE%20III%20TOPSIS.png)

---

## Résultats — phase N6 — Tableau de bord Power BI

La phase N6 transforme les résultats analytiques du projet en un tableau de bord interactif organisé en trois pages, avec une quatrième page envisagée pour approfondir les liens entre structure économique, pollution et inégalités territoriales.

### Page 1 — Vue d’ensemble socio-économique et territoriale

![Tableau de bord TerriZoom — Page 1](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%201.png)

#### Résultats clés — page 1

 - Les communes urbaines intermédiaires et urbaines denses présentent en moyenne les revenus médians les plus élevés du périmètre étudié. Les communes rurales, lorsqu’elles sont présentes dans le subset, se situent globalement à des niveaux de revenu plus faibles.
 - Les indicateurs de synthèse montrent immédiatement qu’une lecture territoriale crédible demande de croiser plusieurs dimensions  revenu, services, sécurité et environnement.
 
 Cette première page remplit donc une fonction de cadrage  elle permet de situer rapidement le périmètre, mais aussi de rappeler qu’aucun indicateur isolé ne suffit à décrire un territoire.

### Page 2 — Croisement de données socio-économiques et criminalité

![Tableau de bord TerriZoom — Page 2](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%202.png)

#### Résultats clés — page 2

 - Le nuage revenu médian  cambriolages montre une dispersion réelle  il n’existe pas de relation simple et mécanique entre niveau de vie et cambriolages pour 1 000 habitants.
 - Les communes les plus peuplées se concentrent toutefois assez souvent dans une zone de cambriolages relativement soutenue, fréquemment autour de 8 à 12 pour 1 000 habitants.
 - Les niveaux les plus faibles de cambriolages sont davantage observés parmi des communes de plus petite taille, sans qu’il soit possible d’en déduire une loi générale.
 - Le nuage services publics  revenu médian montre qu’une forte densité de services publics pour 1 000 habitants n’est pas associée ici aux revenus médians les plus élevés.
 - Les revenus médians les plus élevés, visibles comme outliers dans la distribution, se situent plutôt dans des communes où la densité de services publics reste faible à modérée.
 - Le classement par violences intrafamiliales rappelle qu’un diagnostic territorial crédible doit intégrer des indicateurs de sécurité différenciés, et non seulement des indicateurs agrégés.

### Page 3 — Exposition moyenne aux polluants atmosphériques

![Tableau de bord TerriZoom — Page 3](./images/TerriZoom%20Dashboard%20-%20Power%20BI%20-%20Page%203.png)

#### Résultats clés — page 3

 - Parmi les polluants visualisés, l’ozone présente la part moyenne d’exposition élevée la plus importante sur le périmètre retenu.
 - Les PM2,5 restent présentes à un niveau non négligeable, mais inférieur à celui observé pour l’ozone.
 - Les PM10 apparaissent en moyenne moins présentes dans les niveaux élevés au sein du sous-ensemble comparé.
 - La ventilation par commune met en évidence des profils locaux distincts, ce qui renforce l’intérêt d’une lecture environnementale à l’échelle communale.
 - L’articulation avec les analyses N4 montre que ces niveaux moyens doivent toujours être replacés dans leur saisonnalité propre.

### Page 4 — Amélioration envisagée  socio-économie, structure territoriale et pollution

Une quatrième page est envisagée pour prolonger le dashboard dans une logique plus directement orientée vers l’aide à la décision. Cette page pourrait explorer les liens entre pollution, structure économique, revenu et vulnérabilité sociale.

Parmi les croisements les plus pertinents à tester 

 - pollution vs densité industrielle ;
 - pollution vs densité de commerces  services marchands ;
 - pollution vs revenu médian ;
 - pollution vs part de bas revenus  prestations sociales ;
 - pollution selon type de commune ou classe de densité.

Une telle extension permettrait de passer d’une lecture descriptive de la pollution à une lecture plus structurée des inégalités environnementales, des profils économiques locaux et des compromis territoriaux.

[Voir la page dédiée au dashboard Power BI](httpsgithub.commurielleboens-dotdata-analytics-portfolio-murielle-boensblobmain03_projet-perso-TerriZoom-territorial-data-pyN6%20%E2%80%94%20POWER%20BI%20DASHBOARD%20-%20TerriZoom%20.md)

---

## Synthèse des résultats clés du projet

À ce stade, TerriZoom fait ressortir plusieurs enseignements structurants.

Le premier est qu’une analyse territoriale sérieuse commence par un travail lourd, souvent invisible, d’harmonisation inter-sources. Sans ce travail préalable, aucune comparaison multicritère crédible n’est possible.

Le deuxième est que la qualité analytique se paie par une restriction du périmètre. Le sous-ensemble retenu gagne en cohérence ce qu’il perd en représentativité nationale. Il permet une comparaison robuste, mais sur un espace plus urbain que la France communale dans son ensemble.

Le troisième est qu’un territoire ne se laisse pas résumer par une seule variable. Les analyses exploratoires montrent déjà que la réalité locale se structure à travers plusieurs dimensions  composition du revenu, vulnérabilité sociale, spécialisation des formes de criminalité, saisonnalité des polluants, différences de densité et de structure urbaine.

Le quatrième est que la phase multicritère ne produit pas une vérité unique, mais une hiérarchie dépendante d’hypothèses explicites. Les pondérations comptent. La méthode compte. La comparaison entre TOPSIS et ELECTRE III montre précisément pourquoi cette transparence est indispensable.

Le cinquième est que la visualisation dans Power BI ne sert pas seulement à “faire joli”. Elle transforme des résultats éclatés en lecture navigable, met en évidence des contrastes plus difficiles à saisir dans le code seul, et ouvre une première passerelle vers des usages plus décisionnels.

---

## Limites

Le projet reste soumis à plusieurs limites structurelles liées aux sources mobilisées :

 - couverture géographique inégale, notamment pour la pollution de l’air
 - temporalités différentes selon les datasets
 - granularités hétérogènes (IRIS, commune, jour, trimestre, année)
 - disponibilité partielle de certaines données
 - biais de représentativité du sous-ensemble final
 - fragilité possible de certains taux rapportés à la population officielle dans les communes touristiques ou à forte population présente

Ces contraintes introduisent 

 - des limites de comparabilité 
 - un biais géographique dans le sous-ensemble exploitable 
 - une prudence nécessaire dans l’interprétation 
 - un besoin de contextualisation avant toute lecture opérationnelle

Le périmètre retenu ne constitue donc pas une image nationale, mais un sous-ensemble cohérent de communes comparables au regard des données disponibles.

---

## Choix méthodologiques

Afin de garantir la cohérence de l’analyse, le projet repose sur les choix suivants 

 - utiliser `insee_to_commune` comme table de référence géographique 
 - harmoniser les identifiants communaux, y compris pour Paris, Lyon et Marseille 
 - agréger les différentes sources au niveau communal 
 - restreindre l’analyse principale aux 364 communes complètes 
 - privilégier la qualité, la cohérence et la comparabilité des données à l’exhaustivité

Pour la criminalité, seuls les indicateurs les plus robustes ont été retenus pour le noyau principal d’analyse.
Pour la pollution, les variables ont été traitées comme des niveaux ordonnés et agrégées avec des indicateurs adaptés.
Pour l’analyse multicritère, les résultats ont été testés avec plusieurs scénarios de pondération et deux méthodes distinctes afin de rendre visibles les effets des hypothèses retenues.

---

## Compétences mobilisées

 - Python (`pandas`, `numpy`, `requests`, `matplotlib`, `seaborn`) 
 - intégration et croisement de données multi-sources 
 - traitement de fichiers hétérogènes (CSV, gzip, JSON API) 
 - requêtes analytiques avec DuckDB 
 - nettoyage, structuration et normalisation de données 
 - harmonisation de sources de granularités et temporalités différentes 
 - construction de référentiels et de correspondances géographiques 
 - analyse exploratoire et agrégation d’indicateurs 
 - préparation de données pour l’analyse multicritère 
 - mise en œuvre de méthodes MCDA  TOPSIS et ELECTRE III 
 - analyse de sensibilité via scénarios de pondération 
 - Power BI 
 - data visualization

---

## Conclusion

TerriZoom montre qu’un projet de data analyse territoriale solide ne consiste pas seulement à agréger quelques fichiers publics pour produire un classement final. Le cœur du travail se situe dans la construction d’un périmètre lisible, dans la mise à plat des biais, dans la capacité à faire parler ensemble des sources hétérogènes, puis dans l’acceptation du fait qu’un territoire ne se résume jamais à un seul chiffre.

À travers ses phases successives, le projet met en évidence une ligne claire  mieux décider demande d’abord de mieux qualifier ce que l’on compare. Cela suppose d’assumer les limites du périmètre, de rendre explicites les choix méthodologiques, et de confronter plusieurs lectures d’un même espace plutôt que de chercher un classement prétendument neutre.

Le projet aboutit ainsi à une proposition utile, à la fois analytique et opérationnelle  un cadre capable de faire émerger des profils territoriaux, d’identifier des points de vigilance, d’ouvrir des comparaisons multicritères transparentes, et de préparer une lecture plus directement mobilisable dans des logiques d’aide à la décision locale.

---

## Ouverture — Aide à la décision

La suite naturelle de TerriZoom consiste à faire évoluer le projet d’une logique d’exploration analytique vers une logique d’aide à la décision territoriale.

Dans cette perspective, plusieurs prolongements sont envisageables :

 - construction de profils-types de communes pour comparer des territoires analogues 
 - ajout de scénarios de pondération adaptés à différents acteurs (collectivités, services publics, investisseurs, acteurs de la transition, aménageurs) 
 - développement d’indicateurs de vulnérabilité cumulée ou de pression territoriale 
 - approfondissement des liens entre pollution, activité économique et fragilité sociale 
 - intégration future d’une logique de priorisation d’actions plutôt que de simple classement

Autrement dit, TerriZoom ne se limite pas à décrire des écarts entre communes. Le projet prépare les bases d’un outil capable d’éclairer des arbitrages territoriaux réels, à condition de toujours conserver la même exigence  rendre visibles les hypothèses, les limites et les compromis qui structurent toute décision.
