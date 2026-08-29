# 🛒 Amazon Price & Product Quality Analysis

## Analyse de la relation entre remises, satisfaction client et qualité perçue

---

## 🎯 Besoin métier et problématique

> **Une promotion ne doit pas masquer une éventuelle faiblesse de la qualité d'un produit. Lorsqu'un produit semble s'appuyer fortement sur les remises pour attirer les clients, il est essentiel d'analyser si ces réductions s'accompagnent également d'une satisfaction réelle en matière de qualité et de performance. Dans le cas contraire, l'analyse devra permettre d'identifier les éventuels problèmes sous-jacents susceptibles d'expliquer cette perception, et d'y apporter des recommandations.**
>
> **Notre analyse visera ainsi à étudier les évaluations et les avis clients afin de déterminer si une note élevée semble davantage refléter un véritable rapport qualité-prix et une satisfaction liée aux qualités intrinsèques du produit, ou si elle pourrait être associée à un simple « effet d'aubaine » lié à la baisse du prix.**

---

# 🎯 Objectif du projet

L'objectif est d'analyser les données produits et les avis clients afin d'étudier la relation entre :

- 💰 le prix et les remises ;
- ⭐ les évaluations des produits ;
- 💬 le contenu des avis clients ;
- 📊 le volume d'avis.

L'analyse cherchera notamment à déterminer si les produits fortement remisés présentent une satisfaction comparable à celle des produits moins remisés, et à identifier les éventuels signaux de faiblesse liés à la qualité ou à la performance.

---

# 🔎 Axes d'analyse

### 1. 💰 Prix et satisfaction

Étudier la relation entre le niveau de remise et les évaluations obtenues par les produits.

### 2. ⭐ Effet d'aubaine vs qualité intrinsèque

Analyser les avis clients afin de rechercher si les évaluations positives semblent davantage associées :

- au prix et au rapport qualité-prix ;
- ou aux qualités intrinsèques du produit : performance, fiabilité, durabilité, facilité d'utilisation, etc.

### 3. 📊 Remises et engagement client

Examiner la relation entre les remises, les évaluations et le volume d'avis afin d'identifier d'éventuelles tendances dans le comportement des clients.

### 4. 💬 Identification des signaux de faiblesse

Explorer le contenu des avis afin d'identifier les problèmes récurrents pouvant concerner :

- la qualité ;
- la durabilité ;
- la performance ;
- la fiabilité ;
- les défauts techniques ;
- la conformité du produit ;
- ou d'autres éléments susceptibles d'influencer la satisfaction.

### 5. 💡 Recommandations

À partir des résultats obtenus, formuler des recommandations permettant d'identifier les leviers d'amélioration potentiels du produit et de mieux comprendre le rôle des promotions dans la perception de la valeur.

---

# 🛠️ 1. Préparation et nettoyage des données — Power Query

## 📌 Source et structure initiale des données

Le jeu de données utilisé dans ce projet provient de **Kaggle**.

**Nom du dataset :** `Amazon Sales Dataset`  
**Format :** fichier CSV  
**Structure initiale :**

- **1 465 lignes**
- **16 colonnes**

Le jeu de données brut regroupe dans une même structure des informations relatives aux produits, aux prix, aux catégories, aux évaluations ainsi qu'aux avis clients.

La structure initiale présentait notamment plusieurs informations d'avis regroupées dans une même ligne, avec plusieurs utilisateurs et plusieurs avis pouvant être associés à un même produit.

Cette organisation nécessitait donc une phase de préparation et de restructuration avant la modélisation.

### 📸 Structure initiale du jeu de données

![Structure initiale des données](Screenshot%20(102).png)

---

## 🔢 Typage et normalisation des données

La première étape a consisté à vérifier et adapter les types de données dans Power Query.

### `actual_price` et `discounted_price`

- Suppression des symboles monétaires.
- Conversion du type **Texte** en **nombre décimal fixe**, adapté aux données monétaires.

### `discount_percentage`

- Conversion du type de données en **nombre décimal**.

### `rating`

- Conservation du type **nombre décimal**, déjà adapté aux valeurs de notation.

### `rating_count`

- Conservation du type **nombre entier**.

---

## 🔍 Profilage et contrôle de la qualité des données

Les fonctionnalités de profilage de Power Query ont été utilisées afin d'identifier les valeurs valides, les valeurs vides, les erreurs et les éventuels doublons.
### 📸 Contrôle de la qualité des données

![Profilage et qualité des données](Data_Quality.png)

### `rating_count`

Le profilage a révélé environ **1 % de valeurs vides**.

- Identification des lignes concernées.
- Suppression des **2 lignes vides**.

### `rating`

Le profilage a révélé environ **1 % de valeurs en erreur**.

- Identification de la ligne concernée.
- Suppression de la ligne contenant l'erreur.

---

## 🔎 Identification et traitement des doublons

La distribution de `product_id` a initialement montré :

- **1 351 valeurs distinctes**
- **1 259 valeurs uniques**

La fonctionnalité **Keep Duplicates** de Power Query a été utilisée afin d'observer les doublons avant leur suppression.

L'analyse des lignes concernées a montré que certains `product_id` apparaissaient deux fois avec des informations produit identiques ou très similaires, notamment :

- le nom du produit ;
- le prix actuel ;
- le prix remisé ;
- le niveau de remise.

Les différences observées concernaient principalement les informations relatives aux avis.

Après vérification, ces lignes ont été considérées comme des doublons au niveau de la table produit.

Les doublons ont donc été supprimés sur l'ensemble des lignes.

Après nettoyage :

- **1 351 valeurs distinctes**
- **1 351 valeurs uniques**

La colonne `product_id` constitue ainsi un identifiant unique dans la table `Products`.

---

# 🗂️ Restructuration des catégories

La colonne `category` contenait plusieurs niveaux de classification concaténés et séparés par le caractère `|`.

Exemple de structure :

```text
Main category | Sub category | Product family | Product category | ...
