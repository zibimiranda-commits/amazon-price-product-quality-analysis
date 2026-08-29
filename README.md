# 🛒 Amazon Price & Product Quality Analysis

## Analyse de la relation entre remises, satisfaction client et qualité perçue

---

## 🎯 Besoin métier et problématique

> **Une promotion ne doit pas masquer une éventuelle faiblesse de la qualité d'un produit. Lorsqu'un produit semble s'appuyer fortement sur les remises pour attirer les clients, il est essentiel d'analyser si ces réductions s'accompagnent également d'une satisfaction réelle en matière de qualité et de performance. Dans le cas contraire, l'analyse devra permettre d'identifier les éventuels problèmes sous-jacents susceptibles d'expliquer cette perception, et d'y apporter des recommandations.**
>
> **Notre analyse visera ainsi à étudier les évaluations et les avis clients afin de déterminer si une note élevée semble davantage refléter un véritable rapport qualité-prix et une satisfaction liée aux qualités intrinsèques du produit, ou si elle pourrait être associée à un simple « effet d'aubaine » lié à la baisse du prix.**

---

## 🎯 Objectif du projet

L'objectif est d'analyser les données produits et les avis clients afin d'étudier la relation entre :

- 💰 le prix et les remises ;
- ⭐ les évaluations des produits ;
- 💬 le contenu des avis clients ;
- 📊 le volume d'avis.

L'analyse cherchera notamment à déterminer si les produits fortement remisés présentent une satisfaction comparable à celle des produits moins remisés, et à identifier les éventuels signaux de faiblesse liés à la qualité ou à la performance.

---

# 🔎 Axes d'analyse

### 1. Prix et satisfaction

Étudier la relation entre le niveau de remise et les évaluations obtenues par les produits.

### 2. Effet d'aubaine vs qualité intrinsèque

Analyser les avis clients afin de rechercher si les évaluations positives semblent davantage associées :

- au prix et au rapport qualité-prix ;
- ou aux qualités intrinsèques du produit : performance, fiabilité, durabilité, facilité d'utilisation, etc.

### 3. Remises et engagement client

Examiner la relation entre les remises, les évaluations et le volume d'avis afin d'identifier d'éventuelles tendances dans le comportement des clients.

### 4. Identification des signaux de faiblesse

Explorer le contenu des avis afin d'identifier les problèmes récurrents pouvant concerner :

- la qualité ;
- la durabilité ;
- la performance ;
- la fiabilité ;
- les défauts techniques ;
- la conformité du produit ;
- ou d'autres éléments susceptibles d'influencer la satisfaction.

### 5. Recommandations

À partir des résultats obtenus, formuler des recommandations permettant d'identifier les leviers d'amélioration potentiels du produit et de mieux comprendre le rôle des promotions dans la perception de la valeur.

---

# 🛠️ 1. Préparation et nettoyage des données — Power Query

## Typage et normalisation

- Conversion de `actual_price` et `discounted_price` en **nombre décimal fixe** après suppression des symboles monétaires.
- Conversion de `discount_percentage` en **nombre décimal**.
- Conservation de `rating` en **nombre décimal**.
- Conservation de `rating_count` en **nombre entier**.
- Suppression des lignes présentant des erreurs ou des valeurs manquantes problématiques.

## 🔍 Profilage et contrôle de la qualité

Utilisation des fonctionnalités de profilage de Power Query afin d'identifier les valeurs valides, les valeurs vides et les erreurs.

- Identification des valeurs manquantes dans `rating_count`.
- Identification et suppression de la ligne contenant une erreur dans `rating`.
- Analyse de la distribution de `product_id`.
- Identification des doublons à partir de `product_id`.
- Vérification des lignes dupliquées avant leur suppression.
- Suppression des doublons lorsque les informations produit étaient identiques.

## 🗂️ Restructuration des catégories

La colonne `category` contenait plusieurs niveaux hiérarchiques concaténés et séparés par le délimiteur `|`.

Une séparation de la colonne a permis d'identifier les différents niveaux de classification.

Les quatre niveaux pertinents ont été conservés et renommés :

- `main_category`
- `sub_category`
- `product_family`
- `product_category`

La cinquième colonne, présentant une proportion importante de valeurs vides, a été supprimée.

Les noms des catégories ont également été harmonisés afin de garantir une nomenclature cohérente.

## 💬 Restructuration des avis clients

Les informations relatives aux utilisateurs et aux avis étaient regroupées dans une même ligne et pouvaient contenir plusieurs valeurs.

Une table dédiée `reviews` a donc été créée afin de restructurer ces informations.

Les étapes réalisées comprennent :

- Séparation des informations `user_id`, `user_name`, `review_id`, `review_title` et `review_content`.
- Fractionnement des chaînes de caractères à l'aide de délimiteurs.
- Traitement des colonnes supplémentaires générées lors du fractionnement.
- Fusion des éléments nécessaires afin de conserver les informations complètes des avis.
- **Dépivotage** des données avec `product_id` comme colonne d'ancrage.
- Fractionnement de la colonne `Attribute`.
- **Pivotage** de la colonne `Attribute.1` afin de reconstruire les attributs sous forme de colonnes.
- Reconstruction des champs :
  - `user_id`
  - `user_name`
  - `review_id`
  - `review_title`
  - `review_content`
- Suppression des colonnes devenues inutiles après le pivotage.
- Suppression des lignes vides ou ne contenant pas d'informations d'avis.

### 📌 Granularité de la table `reviews`

La restructuration permet d'obtenir le niveau de détail suivant :

> **1 ligne = 1 avis d'un utilisateur sur 1 produit.**

Un même produit peut donc être associé à plusieurs utilisateurs et plusieurs avis.

## 🧹 Nettoyage final

- Suppression des espaces inutiles à l'aide de la fonction `TRIM`.
- Vérification de la cohérence des données textuelles.
- Contrôle de la structure finale des tables `Products` et `reviews`.
- Vérification de la cohérence et de l'intégrité globale du jeu de données.

---

# 📊 2. Modélisation des données — Power BI

*Cette section sera complétée lors de la phase de modélisation.*

Les éléments abordés seront notamment :

- création du modèle de données ;
- définition des relations entre les tables ;
- construction du modèle en étoile ;
- définition de la hiérarchie des catégories ;
- préparation des données nécessaires à l'analyse.

---

# 📈 3. Analyse — Power BI

*Section à compléter après la modélisation.*

L'analyse portera notamment sur :

- la relation entre prix, remises et évaluations ;
- le rapport entre niveau de remise et volume d'avis ;
- l'analyse des avis clients ;
- l'identification des thèmes associés à la satisfaction et aux éventuelles faiblesses produit ;
- la comparaison entre différents segments de produits.

---

# 💡 4. Recommandations

*Section à compléter après l'analyse des résultats.*

Les recommandations seront formulées à partir des tendances et relations réellement observées dans les données.

---

# 🧰 Outils utilisés

- **Power Query** — préparation, transformation et nettoyage des données
- **Power BI** — modélisation, analyse et visualisation
- **DAX** — mesures et indicateurs analytiques
- **NLP / Text Analysis** — analyse exploratoire du contenu des avis clients

---

# 📁 Structure du projet

```text
amazon-price-product-quality-analysis/
│
├── data/
│   └── dataset
│
├── powerbi/
│   └── amazon_analysis.pbix
│
├── README.md
│
└── screenshots/
    └── portfolio/
