 **Orientation métiers Data — Kaggle Survey 2020 (Focus Data Cleaning & EDA)**

**1) Présentation du projet**
Projet réalisé dans le cadre d’une reconversion vers **le métier de Data Analyst** : transformation d’une enquête Kaggle Survey 2020 (questionnaire complexe) 
en un jeu de données propre, cohérent et exploitable, afin d’explorer les liens entre compétences/outils et métiers de la data, puis de tester un modèle simple de recommandation de catégorie métier.

**Objectif :** orienter un profil vers une **catégorie de métier data** (ex. Analyst / Engineer / Data Scientist / Scientist) à partir des réponses du questionnaire.

 **2) Données**
- **Source :** Kaggle Survey 2020 (données publiques)
- **Données brutes :** **20 037 lignes × 355 colonnes**
- **Variable cible initiale :** `Q5` (métier actuellement exercé / le plus récent)
- **Particularité du dataset :** questionnaire avec plusieurs formats de questions  
  - réponses uniques  
  - choix multiples répartis sur plusieurs colonnes (`Qx_Part_y`)  
  - questions conditionnelles  
  - réponses ouvertes / champs “Other”
 
    
**3) Data Cleaning & Qualité des données (FOCUS Data Analyst)**
Le cœur du projet est un travail de **nettoyage structuré** pour rendre un questionnaire hétérogène exploitable en analyse et en modélisation.

**3.1 Impact du nettoyage (Avant / Après)**
| Étape | Lignes (N) | Colonnes (P) | Pourquoi |
|---|---:|---:|---|
| Dataset brut | 20 037 | 355 | enquête complète |
| Suppression des répondants arrêtés à Q4 | 19 570 | 355 | cible `Q5` absente → inutilisable |
| Suppression des répondants arrêtés à Q5 | 19 278 | 355 | réponses partielles / incohérences |
| Dataset final (après filtres qualité) | **13 847** | **109** | dataset prêt pour EDA + modélisation |

 Résultat : passage de **355 variables** à **109 variables** exploitables, et filtrage qualité sur la cible.


**3.2 Démarche de nettoyage (par type de question)**
Comme toutes les questions n’ont pas le même format, les colonnes ont été **catégorisées** selon le type de question afin d’appliquer des règles cohérentes :
- **Réponse unique**
- **Choix multiples** (colonnes `Qx_Part_y`)
- **Conditionnelles**
- **Ouvertes / “Other”**

**Règles principales appliquées :**
- Suppression des colonnes / modalités **“Other”** (trop hétérogènes et peu exploitables en automatique)
- Suppression des questions jugées **subjectives** ou portant sur un futur trop incertain (faible valeur ajoutée pour la prédiction)
- Suppression des colonnes à **100% de valeurs manquantes**
- Suppression ciblée de colonnes très peu renseignées / non pertinentes pour l’objectif (ex. `Q30`, `Q13`, `Q32`)
- Filtrage des modalités non exploitables sur des variables clés (`Other`, `Currently not employed`, `I prefer not to answer`, etc.)


 
 **3.3 Gestion des valeurs manquantes**
Les valeurs manquantes ont été traitées **colonne par colonne**, en tenant compte :
- du taux de valeurs manquantes
- du type de variable (catégorielle / numérique)
- de la pertinence pour la prédiction

**Exemple (avant imputation – variables clés) :**
- `Q6` : 157 valeurs manquantes (0.81%) → imputation **mode**
- `Q8` : 1537 (7.97%) → imputation **mode**
- `Q11` : 2540 (13.18%) → imputation **mode**
- `Q15` : 2903 (15.06%) → imputation **mode**
- `Q38` : 5987 (31.06%) → imputation **mode**
- `Q30` : 81.78% / `Q32` : 92.23% → colonnes jugées trop pauvres → **suppression** (dans le dataset final de modélisation)

 Exemple d’imputation : `Q8` (langage recommandé) → mode = **Python**, cohérent avec la population cible.

 **3.4 Standardisation / Regroupements (Feature engineering orienté “métier”)**
Pour améliorer la cohérence des données et limiter la dispersion des catégories, des regroupements ont été réalisés :

- **Âge (`Q1`)** : 18–24 / 25–34 / 35+
- **Genre (`Q2`)** : Man / Woman / Other gender
- **Pays (`Q3`)** → **continents** : Africa / America / Asia / Europe / Oceania
- **Expérience en programmation (`Q6`)** : No experience / 1 à 5 ans / Plus de 5 ans
- **Expérience en ML (`Q15`)** : Jamais / Moins de 1 an / 1 à 5 ans / Plus de 5 ans

Ce travail rend les variables plus interprétables et plus robustes pour l’analyse (et pour un modèle de validation).

 **4) Analyse exploratoire (EDA) — résultats clés**
L’EDA vise à identifier les tendances fortes et les relations entre compétences/outils et métiers.

Exemples d’insights (à adapter selon tes graphiques) :
- **Python** est le langage dominant quel que soit le métier ; **SQL** ressort fortement pour certains profils orientés bases de données.
- Les outils de visualisation (ex. Matplotlib) sont davantage associés aux profils techniques (Data Scientist / ML / Research).
- Le dataset présente des **déséquilibres** (répartition géographique, classes métiers) à considérer dans l’interprétation.

**5) Modélisation (validation uniquement)**
La modélisation est utilisée ici comme une **étape de validation** : vérifier que les données nettoyées contiennent un signal exploitable.

- Regroupement des métiers en 4 classes : **Analyst / Engineer / Data Scientist / Scientist**
- Modèles testés : Random Forest, Régression logistique
- **Meilleur modèle : Régression logistique**
  - Accuracy : **56.5%**
  - Macro F1-score : **0.55**
 Le cœur du projet reste le **nettoyage** et l’**analyse** ; le ML est volontairement limité à une validation.

**6) Limites**
- Données auto-déclarées (biais de déclaration)
- Déséquilibres de classes et de régions
- Suppression des “Other” : améliore la cohérence, mais retire une partie de la richesse qualitative
  **Contenu du repository**
- `notebooks/` : notebooks de nettoyage, EDA et validation
- `reports/` : rapport qualité (avant/après + décisions de cleaning)
- `slides/presentation.pdf` : présentation PDF du projet
