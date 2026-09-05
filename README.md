# 🛒 Amazon Price & Product Quality Analysis

## Analyse de la relation entre remises, satisfaction client et qualité perçue

---

# 🎯 Besoin métier et problématique

Les promotions constituent un levier commercial important, mais une réduction de prix ne permet pas à elle seule d'évaluer la qualité réelle d'un produit.

Lorsqu'un produit bénéficie d'une remise importante, il est donc intéressant d'examiner si les données disponibles suggèrent également :

- une satisfaction client élevée ;
- une perception positive de la qualité ;
- de bonnes performances ;
- une fiabilité satisfaisante ;
- ou, au contraire, la présence de problèmes récurrents dans les avis.

L'analyse cherche ainsi à étudier conjointement les prix, les remises, les évaluations et le contenu des avis clients afin de mieux comprendre si les produits fortement remisés présentent une satisfaction comparable aux autres produits et quels éléments semblent contribuer à la perception de leur valeur.

L'objectif n'est pas d'établir une relation causale entre remise et satisfaction, mais d'identifier des **associations, tendances et signaux** observables dans le jeu de données.

---

# 🎯 Objectif du projet

L'objectif du projet est d'analyser les données produits et les avis clients afin d'étudier la relation entre :

- 💰 le prix et les remises ;
- ⭐ les évaluations moyennes des produits ;
- 💬 le contenu des avis clients ;
- 📊 le volume d'évaluations associé aux produits ;
- 📝 le volume d'avis textuels disponibles.

L'analyse cherchera notamment à déterminer si les données suggèrent que les produits fortement remisés présentent une satisfaction comparable à celle des produits moins remisés, tout en identifiant les éventuels signaux de faiblesse liés à la qualité, à la performance, à la durabilité ou à la fiabilité.

---

# 🔎 Axes d'analyse

## 1. 💰 Prix et satisfaction

Étudier la relation entre le niveau de remise, les prix et les évaluations moyennes obtenues par les produits.

L'objectif est notamment d'observer si les produits fortement remisés semblent présenter des niveaux de satisfaction différents de ceux des produits moins remisés.

---

## 2. ⭐ Effet d'aubaine vs qualité intrinsèque

Analyser le contenu des avis clients afin d'examiner si les commentaires positifs semblent davantage associés :

- au prix ;
- au rapport qualité-prix ;
- aux promotions ;

ou aux qualités intrinsèques du produit telles que :

- la performance ;
- la fiabilité ;
- la durabilité ;
- la facilité d'utilisation ;
- la qualité de fabrication.

Cette analyse reste exploratoire et ne cherche pas à établir que la remise cause directement une meilleure ou une moins bonne satisfaction.

---

## 3. 📊 Remises et engagement client

Examiner les relations entre :

- le niveau de remise ;
- les évaluations moyennes ;
- le nombre d'évaluations associé aux produits ;
- le volume d'avis textuels disponibles.

L'objectif est d'identifier d'éventuelles tendances dans l'engagement associé aux différents produits.

---

## 4. 💬 Identification des signaux de faiblesse

Explorer le contenu des avis afin d'identifier les problèmes récurrents pouvant concerner :

- la qualité ;
- la durabilité ;
- la performance ;
- la fiabilité ;
- les défauts techniques ;
- la conformité du produit ;
- la facilité d'utilisation ;
- ou d'autres éléments susceptibles d'influencer la satisfaction.

---

## 5. 💡 Recommandations

À partir des tendances réellement observées dans les données, formuler des recommandations permettant :

- d'identifier les principaux leviers d'amélioration ;
- de détecter les catégories de produits présentant des signaux de faiblesse ;
- de mieux comprendre la place du prix dans la perception de la valeur ;
- d'identifier les caractéristiques intrinsèques les plus souvent associées à la satisfaction.

Les recommandations finales seront formulées uniquement après l'analyse complète des résultats.

---

# 🛠️ 1. Préparation et nettoyage des données — Power Query

## 📌 Source et structure initiale des données

Le jeu de données utilisé dans ce projet provient de **Kaggle**.

**Nom du dataset :** `Amazon Sales Dataset`  
**Format :** fichier CSV

### Structure initiale

- **1 465 lignes**
- **16 colonnes**

Les colonnes initiales sont :

- `product_id`
- `product_name`
- `category`
- `discounted_price`
- `actual_price`
- `discount_percentage`
- `rating`
- `rating_count`
- `about_product`
- `user_id`
- `user_name`
- `review_id`
- `review_title`
- `review_content`
- `img_link`
- `product_link`

Le jeu de données brut regroupe dans une même structure des informations relatives :

- aux produits ;
- aux prix ;
- aux catégories ;
- aux évaluations ;
- aux utilisateurs ;
- aux avis clients.

La structure initiale présentait notamment plusieurs informations d'avis regroupées dans une même ligne.

Plusieurs utilisateurs, identifiants d'avis, titres et contenus pouvaient ainsi être associés à un même produit au sein de colonnes concaténées.

Cette organisation nécessitait une phase de préparation et de restructuration avant la modélisation.

### 📸 Structure initiale du jeu de données

![Structure initiale des données](Screenshot%20(102).png)

---

## 🔢 Typage et normalisation des données

La première étape a consisté à vérifier et adapter les types de données dans Power Query afin de rendre les champs exploitables pour les calculs et les visualisations.

### `actual_price` et `discounted_price`

Les valeurs monétaires étaient initialement stockées sous forme textuelle.

Les transformations suivantes ont été appliquées :

- suppression des symboles monétaires ;
- nettoyage des caractères empêchant la conversion ;
- conversion vers un type numérique adapté aux données monétaires.

### `discount_percentage`

La colonne a été convertie en valeur numérique décimale afin de permettre son utilisation dans les calculs.

Une valeur telle que :


0.47
correspond ainsi à :

47 %

lorsque le champ est formaté en pourcentage dans Power BI.

### rating

La colonne rating a été configurée comme nombre décimal afin de permettre les calculs de moyenne et les comparaisons entre produits.

### rating_count

La colonne rating_count a été configurée comme nombre entier.

Elle représente le nombre d'évaluations associé au produit dans le dataset.

Elle ne doit pas être confondue avec le nombre d'avis textuels présents dans la table Reviews.

## 🔍 Profilage et contrôle de la qualité des données

Les fonctionnalités de profilage de Power Query ont été utilisées afin d'identifier :

les valeurs valides ;
les valeurs vides ;
les erreurs ;
les éventuels doublons ;
la distribution des principales colonnes.

### 📸 Contrôle de la qualité des données

![Data Quality](Data_Quality.png)

### rating_count

Le profilage a identifié 2 valeurs vides dans la colonne rating_count.

Les lignes concernées ont été examinées puis supprimées afin d'éviter d'introduire des valeurs manquantes dans les analyses basées sur le volume d'évaluations.

### rating

Le profilage a également identifié 1 valeur en erreur dans la colonne rating.

La ligne concernée a été vérifiée puis supprimée.

## 🔎 Identification et traitement des doublons

La distribution de product_id a été analysée afin de vérifier si chaque identifiant correspondait bien à une seule ligne produit.

Au cours des différentes étapes de préparation, le profilage a notamment montré :

1 351 valeurs distinctes à une étape intermédiaire ;
1 259 valeurs apparaissant une seule fois à cette même étape.

La fonctionnalité Keep Duplicates de Power Query a été utilisée afin d'isoler les lignes concernées et de comprendre l'origine des répétitions.

### 📸 Identification des doublons via Keep Duplicates

![Duplicate Identification](Duplicate_identification.png)

L'analyse a montré que certains product_id apparaissaient plusieurs fois avec des informations produit identiques ou très similaires, notamment concernant :

le nom du produit ;
le prix avant remise ;
le prix après remise ;
le niveau de remise ;
les catégories.

Les différences observées concernaient principalement les informations relatives aux utilisateurs et aux avis.

Cela a confirmé la nécessité de distinguer deux niveaux de granularité :

Products
1 ligne = 1 produit

et :

Reviews
plusieurs occurrences d'avis peuvent être associées au même produit

Après séparation des informations relatives aux avis, les répétitions ont été traitées au niveau de la table Products afin de conserver une seule ligne par product_id.

Résultat final de la table Products
1 348 produits
1 348 product_id distincts
1 348 product_id uniques

La colonne product_id constitue ainsi l'identifiant unique de la table Products.

La différence entre les valeurs observées à certaines étapes intermédiaires du nettoyage et les 1 348 produits finaux résulte de l'ensemble du processus de nettoyage, et ne doit pas être attribuée uniquement à la suppression des doublons.

# 🗂️ Restructuration des catégories

La colonne initiale category contenait plusieurs niveaux de classification concaténés et séparés par le caractère |.

Elle a été divisée afin de créer plusieurs axes d'analyse distincts et réutilisables dans Power BI.

Exemple de structure initiale :

Main category | Sub category | Product family | Product category | ...

### 📸 Traitement et séparation des catégories

![Category Split](category_split.png)

Les quatre niveaux retenus ont été renommés :

main_category
sub_category
product_family
product_category

La cinquième colonne générée par le fractionnement présentait une proportion importante de valeurs vides et n'apportait pas de niveau de classification suffisamment exploitable.

Elle a donc été supprimée.

Les libellés des catégories ont également été harmonisés afin d'améliorer la cohérence de la nomenclature utilisée dans le rapport.

# 💬 Restructuration des avis clients

Les informations relatives aux utilisateurs et aux avis étaient initialement regroupées dans plusieurs colonnes pouvant contenir plusieurs valeurs pour un même produit.

Une table dédiée Reviews a donc été créée afin de rapprocher les données de la granularité nécessaire pour l'analyse textuelle.

Les étapes réalisées comprennent notamment :

séparation des informations user_id, user_name, review_id, review_title et review_content ;
fractionnement des chaînes de caractères à l'aide de délimiteurs ;
traitement des colonnes supplémentaires générées lors du fractionnement ;
fusion des éléments nécessaires afin de conserver les informations complètes ;
dépivotage des données avec product_id comme colonne d'ancrage ;
fractionnement de la colonne Attribute ;
pivotage des attributs afin de reconstruire les différentes informations sous forme de colonnes ;
suppression des colonnes intermédiaires devenues inutiles ;
suppression des lignes vides ou ne contenant pas d'informations exploitables.

### 📸 Focus technique — Unpivot & Pivot

Les principaux champs reconstruits comprennent :

product_id
user_id
user_name
review_id
review_title
review_content

📌 Granularité de la table Reviews

La restructuration permet d'obtenir le niveau de détail suivant :

1 ligne = 1 occurrence d'avis normalisée, associée à un produit et à un utilisateur.

Un même produit peut donc apparaître plusieurs fois dans la table lorsqu'il est associé à plusieurs avis.

Cette formulation est volontairement distincte de :

1 ligne = 1 avis unique

car le nombre total de lignes et le nombre d'identifiants review_id distincts ne sont pas exactement identiques.

Cette différence est contrôlée ultérieurement à l'aide des mesures :

Reviews Count = COUNTROWS(Reviews)

et :

Distinct Review Count = DISTINCTCOUNT(Reviews[review_id])

### 📸 Résultat final de la normalisation

🧠 Classification exploratoire des avis dans Power Query

Afin d'enrichir l'analyse textuelle avant l'utilisation de Python, deux classifications exploratoires ont été créées dans Power Query :

review_theme
review_sentiment

Ces classifications reposent sur des règles lexicales et des mots-clés.

Elles ne constituent donc pas un modèle de machine learning ou un modèle NLP entraîné.

Elles servent de première grille d'analyse qui pourra ensuite être comparée à une méthode NLP réalisée avec Python.

🏷️ review_theme

La colonne review_theme cherche à identifier le thème principal évoqué dans chaque avis à partir du texte combiné de :

review_title + review_content

Le texte est normalisé en minuscules avant l'application des règles.

Les principales catégories retenues sont :

Defect / Problem
Durability
Price / Value
Ease of Use
Performance
Reliability / Functionality
Quality
General / Other

La classification est actuellement mono-thème :

un avis reçoit un thème principal.

L'ordre des règles est donc important lorsqu'un avis contient des expressions appartenant à plusieurs catégories.

Par exemple, un avis mentionnant simultanément un problème technique et le prix pourra être classé dans la première catégorie correspondant aux règles appliquées.

Cette méthode constitue une première approximation et possède plusieurs limites :

certains mots peuvent avoir des significations différentes selon le contexte ;
un même avis peut réellement aborder plusieurs thèmes ;
les règles ne détectent pas toujours l'ironie ou les formulations complexes ;
certains termes génériques peuvent produire des faux positifs.

🙂 review_sentiment

Une colonne review_sentiment a également été créée afin d'obtenir une première classification du sentiment exprimé dans chaque avis.

Les catégories utilisées sont :

Positive
Negative
Neutral / Mixed
No Review

La classification analyse également la combinaison :

review_title + review_content

Les règles négatives sont évaluées avant les règles positives afin d'éviter que certaines phrases contenant à la fois des termes positifs et une plainte claire soient classées automatiquement comme positives.

Exemple conceptuel :

"Good product but stopped working after two weeks"

contient le terme positif good, mais également une indication claire de dysfonctionnement.

Une classification purement fondée sur la présence du mot good serait donc insuffisante.

Des exceptions ont également été ajoutées pour réduire certaines erreurs de contexte.

Par exemple, le terme :

issue

ne doit pas entraîner automatiquement un sentiment négatif lorsqu'il apparaît dans une expression telle que :

without any issues
⚠️ Limites de la classification Power Query

review_theme et review_sentiment constituent des classifications heuristiques basées sur des règles.

Elles permettent :

une première exploration des avis ;
l'identification de mots-clés fréquents ;
la construction rapide d'axes d'analyse dans Power BI.

Cependant, elles ne doivent pas être interprétées comme une vérité absolue.

La prochaine phase du projet utilisera Python et des techniques NLP afin de :

produire une seconde classification indépendante ;
comparer les résultats ;
analyser les cas de désaccord ;
identifier les limites de la méthode lexicale ;
préparer une validation plus rigoureuse.
🧹 Nettoyage final

Après la restructuration des tables, plusieurs contrôles finaux ont été réalisés :

suppression des espaces inutiles à l'aide de fonctions telles que TRIM ;
vérification de la cohérence des données textuelles ;
contrôle des types de données ;
vérification de la structure finale des tables ;
contrôle de l'unicité de product_id dans Products ;
vérification de la cohérence des relations entre produits et avis.
🕒 Limite temporelle du dataset

Le dataset ne contient pas de véritable colonne de date associée aux produits, aux évaluations ou aux avis.

Il n'est donc pas possible de réaliser de manière fiable :

une évolution des notes dans le temps ;
une analyse mensuelle ou annuelle ;
une évolution historique des prix ;
une analyse avant/après promotion ;
une véritable analyse temporelle des avis.

Les informations de publication ou de mise à jour de la page Kaggle ne doivent pas être utilisées comme date individuelle des observations.

Aucune dimension Date artificielle n'a donc été créée dans le modèle.

📊 2. Modélisation des données — Power BI

Après la préparation et le nettoyage réalisés dans Power Query, les données ont été structurées dans Power BI Desktop.

L'objectif est de disposer d'un modèle permettant d'analyser les produits, les prix, les remises, les évaluations et les avis à des niveaux de granularité cohérents.

🗂️ Structure du modèle

Le modèle repose principalement sur deux tables :

Products
Reviews
📦 Products

La table Products constitue la table de référence des produits.

Granularité

1 ligne = 1 produit

La colonne :

product_id

constitue l'identifiant unique de cette table.

Après nettoyage :

1 348 produits

sont présents dans la table.

Elle contient notamment les informations relatives :

aux produits ;
aux catégories ;
aux prix ;
aux remises ;
à la note moyenne du produit ;
au volume d'évaluations associé au produit.
💬 Reviews

La table Reviews contient les informations restructurées relatives aux avis clients.

Granularité

1 ligne = 1 occurrence d'avis normalisée associée à 1 produit et à 1 utilisateur.

Un même product_id peut donc apparaître plusieurs fois dans cette table.

Les principaux champs comprennent notamment :

product_id
user_id
user_name
review_id
review_title
review_content

Cette table constitue la base des analyses portant sur :

les utilisateurs ;
les avis textuels ;
les thèmes ;
le sentiment.
🧠 Table préparée pour l'analyse NLP

Une structure dédiée à l'analyse NLP a également été préparée.

La table Reviews_NLP contient actuellement :

product_id
review_id
review_title
review_theme
review_content
review_sentiment

Elle contient :

10 734 lignes

Cette structure sera utilisée par Python afin que l'analyse NLP soit effectuée au niveau des avis normalisés plutôt que directement sur les colonnes concaténées du fichier CSV brut.

🌳 Hiérarchie des catégories

Une hiérarchie a été créée dans la table Products afin de permettre une navigation progressive dans les catégories.

La hiérarchie utilisée est :

Main Category
      ↓
Sub Category
      ↓
Product Family
      ↓
Product Category

Cette structure permettra d'utiliser le drill-down dans Power BI afin de passer progressivement d'une catégorie générale à un niveau plus détaillé.

⚙️ Configuration des propriétés des colonnes

Après le chargement dans Power BI, les propriétés des colonnes ont été contrôlées afin de garantir leur utilisation correcte dans les mesures et les visualisations.

Les principaux contrôles ont porté sur :

le type de données ;
le format d'affichage ;
la catégorie de données ;
le comportement des champs numériques et textuels.
💰 Champs monétaires

Les colonnes :

actual_price
discounted_price

ont été configurées comme valeurs numériques monétaires.

Les montants du dataset sont exprimés en roupies indiennes (₹ / INR).

📊 Champs numériques

Les principaux champs numériques ont été configurés comme suit :

rating              → nombre décimal
rating_count        → nombre entier
discount_percentage → nombre décimal / pourcentage
🔑 Identifiants

Les champs :

product_id
user_id
review_id

sont conservés comme champs textuels.

Ils représentent des identifiants et ne doivent donc pas être additionnés ou moyennés.

🔗 Relation entre les tables

Une relation a été créée entre Products et Reviews à partir de :

product_id

La cardinalité est :

Products (1) ─────────── (*) Reviews

Ainsi :

un produit apparaît une seule fois dans Products ;
un produit peut être associé à plusieurs lignes dans Reviews.
📸 Vérification du filtrage

Un test a été réalisé afin de vérifier que la sélection d'un produit dans Products filtre correctement les avis associés dans Reviews.

Ce contrôle a permis de confirmer le fonctionnement attendu de la relation.

🧮 3. Création des mesures DAX de référence

Après la mise en place du modèle et la vérification des relations, une première série de mesures DAX a été créée.

Ces mesures constituent les indicateurs descriptifs de référence du projet.

Elles permettront ensuite de construire des analyses plus avancées autour des cinq axes métier.

⭐ 1. Satisfaction produit
Average Rating
Average Rating =
AVERAGE(Products[rating])

Valeur observée :

4,09

Cette mesure calcule la moyenne des notes stockées dans la table Products.

Il est important de préciser que :

chaque ligne de Products représente un produit.

Average Rating correspond donc à la moyenne des notes moyennes des produits, chaque produit ayant le même poids dans ce calcul.

Cette mesure ne correspond pas à une moyenne calculée directement à partir de toutes les évaluations individuelles Amazon.

Une moyenne pondérée par rating_count pourra être étudiée ultérieurement dans les mesures analytiques.

💰 2. Prix et remises
Average Actual Price
Average Actual Price =
AVERAGE(Products[actual_price])

Valeur observée :

≈ ₹5,70 K

Cette mesure calcule le prix moyen des produits avant remise.

Average Discounted Price
Average Discounted Price =
AVERAGE(Products[discounted_price])

Valeur observée :

≈ ₹3,31 K

Cette mesure calcule le prix moyen des produits après remise.

Elle permettra notamment de comparer le positionnement tarifaire avant et après réduction.

Average Discount
Average Discount =
AVERAGE(Products[discount_percentage])

Valeur observée :

0,47

soit :

47 %

lorsque la mesure est formatée en pourcentage dans Power BI.

Cette mesure servira de référence pour étudier les relations entre remise, satisfaction et engagement.

📦 3. Volume de produits
Product Count
Product Count =
COUNTROWS(Products)

Valeur exacte :

1 348

Power BI peut afficher cette valeur sous forme abrégée :

1,35 K

Cette mesure permet de contrôler le nombre de produits présents dans le contexte de filtre utilisé.

📊 4. Volume d'évaluations et engagement
Total Rating Count
Total Rating Count =
SUM(Products[rating_count])

Cette mesure additionne le nombre d'évaluations indiqué pour les produits.

Elle représente donc un volume d'évaluations agrégé au niveau produit.

Elle ne correspond pas au nombre de lignes présentes dans Reviews.

Cette distinction est essentielle :

rating_count
≠
nombre d'avis textuels disponibles
Average Rating Count
Average Rating Count =
AVERAGE(Products[rating_count])

Valeur observée :

≈ 17,66 K

Cette mesure calcule le nombre moyen d'évaluations associé à un produit.

Elle permettra de comparer l'engagement moyen entre différentes catégories ou différents niveaux de remise.

💬 5. Avis clients

Les mesures suivantes utilisent la table Reviews.

Reviews Count
Reviews Count =
COUNTROWS(Reviews)

Cette mesure compte le nombre total de lignes de la table normalisée.

La structure normalisée contient actuellement :

10 734 lignes

Power BI peut afficher cette valeur sous forme abrégée :

≈ 11 K

Cette mesure doit être interprétée comme un nombre d'occurrences d'avis normalisées, et non automatiquement comme le nombre d'identifiants d'avis uniques.

Distinct Review Count
Distinct Review Count =
DISTINCTCOUNT(Reviews[review_id])

Valeur affichée dans Power BI :

≈ 9 K

Cette mesure compte le nombre d'identifiants review_id distincts.

Elle permet de distinguer :

nombre de lignes

de :

nombre d'identifiants d'avis uniques
Distinct User Count
Distinct User Count =
DISTINCTCOUNT(Reviews[user_id])

Valeur affichée dans Power BI :

≈ 9 K

Cette mesure compte les utilisateurs distincts présents dans la table d'avis.

Elle permettra notamment d'étudier la diversité des utilisateurs associés aux avis disponibles.

📌 Synthèse des mesures de référence
Axe	Mesure	Table	Rôle
⭐ Satisfaction	Average Rating	Products	Mesurer la note moyenne des produits
💰 Prix	Average Actual Price	Products	Mesurer le prix moyen avant remise
💰 Prix	Average Discounted Price	Products	Mesurer le prix moyen après remise
💰 Remise	Average Discount	Products	Mesurer le niveau moyen de remise
📦 Produits	Product Count	Products	Compter les produits analysés
📊 Évaluations	Total Rating Count	Products	Mesurer le volume total d'évaluations indiqué au niveau produit
📊 Évaluations	Average Rating Count	Products	Mesurer le nombre moyen d'évaluations associé à un produit
💬 Avis	Reviews Count	Reviews	Compter les lignes d'avis normalisées
💬 Avis	Distinct Review Count	Reviews	Compter les identifiants d'avis distincts
👤 Utilisateurs	Distinct User Count	Reviews	Compter les utilisateurs distincts
🐍 4. Prochaine étape — Python et NLP

La prochaine étape consiste à poursuivre l'analyse des avis clients avec Python et des techniques de Natural Language Processing (NLP).

Cette phase doit compléter, et non simplement reproduire, la classification réalisée dans Power Query.

L'objectif est notamment de comparer deux approches :

Power Query
classification lexicale basée sur des règles

et :

Python / NLP
analyse indépendante du sentiment
🎯 Objectifs de l'analyse NLP

L'analyse Python permettra notamment de :

combiner review_title et review_content ;
analyser le sentiment avec VADER ;
créer un score de sentiment numérique ;
créer une classification NLP indépendante ;
comparer le sentiment Python avec review_sentiment ;
identifier les avis pour lesquels les deux méthodes sont en désaccord ;
examiner manuellement les cas ambigus ;
mesurer ultérieurement les performances sur un échantillon annoté manuellement.
🔄 Logique prévue
Avis clients normalisés
        │
        ├───────────────┐
        ↓               ↓
Power Query          Python NLP
règles lexicales       VADER
        │               │
        ↓               ↓
review_sentiment   vader_sentiment
review_theme       vader_compound
        │               │
        └───────┬───────┘
                ↓
          Comparaison
                ↓
      Analyse des désaccords
                ↓
       Validation manuelle
                ↓
             Power BI
📊 Validation

La qualité de la classification ne sera pas évaluée uniquement à partir de quelques observations visuelles.

Une validation plus rigoureuse pourra être réalisée en constituant un échantillon d'avis annotés manuellement.

Cela permettra ensuite de calculer des métriques telles que :

accuracy ;
precision ;
recall ;
F1-score ;
matrice de confusion.

Aucune valeur de précision ne sera annoncée avant la réalisation de cette validation.

🔎 Étapes analytiques suivantes

Une fois l'analyse NLP terminée et les résultats réintégrés dans le modèle Power BI, le projet pourra passer aux mesures analytiques permettant de répondre directement aux cinq axes métier.

Les analyses pourront notamment inclure :

comparaison des notes selon le niveau de remise ;
moyenne pondérée des notes par rating_count ;
segmentation des remises ;
comparaison prix / satisfaction ;
comparaison remise / engagement ;
analyse des thèmes des avis ;
part des avis positifs et négatifs ;
analyse des catégories concentrant les problèmes ;
comparaison sentiment Power Query / sentiment Python ;
analyse de la dispersion des prix et des notes ;
utilisation de médianes et d'écarts-types lorsque cela apporte une information pertinente.

Ces mesures seront ajoutées uniquement après la finalisation de la phase Python/NLP.

💡 Recommandations — À venir

Les recommandations finales ne sont pas encore formulées.

Elles seront basées exclusivement sur les tendances réellement observées après :

la préparation des données ;
la modélisation ;
l'analyse NLP ;
la création des mesures analytiques ;
la construction des visualisations finales.

L'objectif sera notamment d'identifier :

les produits ou catégories présentant des signaux de faiblesse ;
les facteurs les plus souvent associés aux avis positifs ;
les problèmes les plus fréquemment évoqués ;
les situations dans lesquelles une remise importante ne semble pas s'accompagner d'une satisfaction élevée ;
les leviers d'amélioration potentiels.
⚠️ Limites de l'analyse

Plusieurs limites doivent être prises en compte dans l'interprétation des résultats.

Absence de données temporelles

Le dataset ne fournit pas de dates individuelles pour les produits ou les avis.

Aucune tendance temporelle fiable ne peut donc être calculée.

Absence de données de ventes

Le dataset ne fournit pas directement :

les quantités vendues ;
le chiffre d'affaires ;
le taux de conversion.

Le niveau de remise ne peut donc pas être utilisé pour mesurer directement son effet sur les ventes.

rating au niveau produit

La colonne rating représente une note agrégée associée au produit.

Elle n'est pas une note individuelle liée à chaque avis textuel.

rating_count différent du volume d'avis textuels

rating_count représente le nombre d'évaluations indiqué pour chaque produit.

Le nombre de lignes de Reviews représente uniquement les avis textuels disponibles et normalisés dans le dataset.

Ces deux mesures ne doivent donc pas être interprétées comme équivalentes.

Classification lexicale

Les classifications review_theme et review_sentiment créées dans Power Query reposent sur des règles.

Elles peuvent produire des erreurs de contexte et seront donc confrontées à une approche NLP indépendante.

Interprétation des relations

Les analyses permettront d'identifier des associations entre variables.

Elles ne permettront pas, à elles seules, d'établir une relation causale entre :

promotion
→
satisfaction

ou :

promotion
→
engagement
🧰 Outils utilisés
Power Query — préparation, transformation, nettoyage et normalisation des données
Power BI — modélisation, analyse et visualisation
DAX — création des indicateurs et mesures
Python — analyse complémentaire des avis clients
Pandas — manipulation et préparation des données en Python
NumPy — opérations numériques
VADER Sentiment Analysis — analyse NLP du sentiment
Git / GitHub — documentation et présentation du projet

La phase Python / NLP est actuellement en cours. Les résultats NLP ne seront documentés comme résultats définitifs qu'une fois l'analyse réellement réalisée.

📁 Structure du projet
amazon-price-product-quality-analysis/
│
├── data/
│   └── amazon.csv
│
├── powerbi/
│   └── Amazon portfolio.pbix
│
├── python/
│   └── amazon_nlp.py
│
├── screenshots/
│   ├── Screenshot (102).png
│   ├── Data_Quality.png
│   ├── Duplicate_identification.png
│   ├── category_split.png
│   ├── unpivot_pivot_process.png
│   ├── reviews_transformation.png
│   └── test_filtre.png
│
└── README.md

La structure ci-dessus doit rester alignée avec les noms réels des fichiers présents dans le repository. Si un fichier est renommé dans GitHub, son nom devra également être mis à jour dans cette section et dans les liens correspondants du README.

🚧 Statut actuel du projet

Les étapes actuellement terminées sont :

préparation du dataset brut ;
contrôle de la qualité des données ;
traitement des valeurs manquantes et erreurs ;
restructuration des catégories ;
normalisation des avis ;
création des tables Products et Reviews ;
création de la relation entre les tables ;
configuration du modèle Power BI ;
création des mesures DAX descriptives de référence ;
création d'une première classification lexicale des thèmes ;
création d'une première classification lexicale du sentiment ;
préparation de la table destinée à l'analyse NLP ;
configuration de l'environnement Python ;
validation du fonctionnement de Pandas, NumPy et VADER.
Étape en cours
Python / NLP

La prochaine phase consiste à appliquer l'analyse de sentiment Python aux avis normalisés, comparer ses résultats aux règles Power Query puis préparer leur intégration dans Power BI.

📈 Objectif final

À terme, ce projet doit permettre de construire un tableau de bord Power BI combinant :

Prix
+
Remises
+
Satisfaction
+
Engagement
+
Thèmes des avis
+
Sentiment

afin d'obtenir une vision plus complète de la relation entre le positionnement tarifaire des produits et la qualité perçue par les clients.


### Les corrections les plus importantes à comprendre

Il y en a quatre sur lesquelles je veux attirer ton attention, parce qu'un recruteur pourrait justement t'interroger dessus.

**Premièrement, les 1 351 → 1 348 produits.** Ton ancien README faisait croire que la simple suppression des doublons expliquait cette variation. Or le même passage annonce d'abord 1 351 valeurs distinctes puis 1 348 après nettoyage. :contentReference[oaicite:3]{index=3} J'ai donc corrigé la causalité : les 1 348 constituent **l'état final après l'ensemble du nettoyage**, sans prétendre que la déduplication seule a supprimé trois identifiants distincts.

**Deuxièmement, `rating_count` n'est pas `Reviews Count`.** Ton ancien README qualifiait les deux d'indicateurs d'engagement sans suffisamment distinguer les populations mesurées. :contentReference[oaicite:4]{index=4} Désormais, le README explique explicitement qu'un chiffre provient du compteur d'évaluations Amazon au niveau produit, alors que l'autre compte les lignes textuelles normalisées disponibles dans notre échantillon.

**Troisièmement, nous ne prétendons plus que chaque ligne de `Reviews` est nécessairement un avis unique.** Ton ancien README l'affirmait explicitement. :contentReference[oaicite:5]{index=5} Comme `COUNTROWS(Reviews)` est supérieur à `DISTINCTCOUNT(review_id)`, la formulation « occurrence d'avis normalisée » est méthodologiquement plus solide.

**Quatrièmement, j'ai corrigé le statut du projet.** L'ancien README annonçait « Passage aux mesures analytiques » et répétait `Average Rating`, alors que notre vraie prochaine étape est Python/NLP. :contentReference[oaicite:6]{index=6} La nouvelle version reflète maintenant exactement notre progression : **DAX de référence terminé → Python/NLP en cours → DAX analytique ensuite → recommandations finales**.

Avec cette version, ton README ne fait plus seulement « portfolio joli » : il explique aussi **les hypothèses, la granularité, les limites et les choix méthodologiques**. C'est précisément ce qui le rend défendable en entretien.
