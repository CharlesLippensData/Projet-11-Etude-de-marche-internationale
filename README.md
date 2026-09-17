# Projet 11 : Produisez une étude de marché avec R ou Python

![Statut](https://img.shields.io/badge/Statut-Valid%C3%A9-2ea44f)
![Charge](https://img.shields.io/badge/Charge-90h-blue)
![Domaine](https://img.shields.io/badge/Domaine-Machine%20Learning%20non%20supervis%C3%A9-informational)
![Formation](https://img.shields.io/badge/OpenClassrooms-Data%20Analyst-7451eb)
![Python](https://img.shields.io/badge/Python-3776AB)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E)

> Étude de marché internationale par clustering de pays (ACP, classification ascendante hiérarchique et K-Means) pour recommander un groupe de pays cibles à un exportateur de poulet bio.
>
> Projet validé en soutenance le 16 août 2026.

## Contexte
Data Analyst chez « La poule qui chante », entreprise agroalimentaire française spécialisée dans le poulet bio. Le PDG, Patrick, souhaite évaluer une expansion à l'international sans qu'aucun pays n'ait encore été choisi. La mission consiste à recommander des groupes de pays cibles, à partir de données ouvertes uniquement, et à présenter les résultats au COMEX.

## Objectifs
- Exploiter un modèle d'apprentissage non supervisé pour en apprendre davantage sur les données.
- Réaliser des analyses multivariées pour comprendre la corrélation entre variables.
- Réduire la dimension du jeu de données.
- Sélectionner les variables pertinentes d'un modèle.

## Démarche
1. Collecte et nettoyage de neuf fichiers sources (FAO : bilans alimentaires 2017, population 2000 à 2018, table des pays ; Banque mondiale : PIB par habitant, croissance, inflation, stabilité politique ; taux de change figé au 30 juillet 2026) : 172 pays conservés, soit 97,6 % de la population mondiale.
2. Feature engineering : 14 variables dont 5 construites (potentiel de marché, taux de dépendance aux importations, etc.), analyse exploratoire et matrice de corrélations.
3. Réduction de dimension par ACP : quatre composantes retenues, 65 % de l'inertie, cercles des corrélations et projection des individus.
4. Clustering par classification ascendante hiérarchique et par K-Means, choix du nombre de groupes (dendrogramme, coude, silhouette), comparaison des deux partitions (indice de Rand ajusté 0,58) et profil de chaque cluster.
5. Recommandation d'un cluster cible de 21 pays, classés par potentiel de marché, et plan d'action à 24 mois présenté au COMEX.

## Résultats clés
- 172 pays analysés, 14 variables, ACP à 4 composantes (65 % de l'inertie).
- CAH et K-Means convergent (indice de Rand ajusté 0,58), ce qui consolide la segmentation.
- Cluster cible de 21 pays et 2,1 milliards de dollars de potentiel de marché, États-Unis, Chine, Brésil, Japon et Royaume-Uni en tête.
- Pilote recommandé en Allemagne ou au Royaume-Uni, plan d'action à 24 mois.

## Livrables
| Fichier | Format | Description |
|---|---|---|
| `Lippens_Charles_1_preparation_nettoyage_analyse_exploratoire_072026.ipynb` | Notebook (+ .pdf et .html) | Préparation, nettoyage, feature engineering, analyse exploratoire |
| `Lippens_Charles_2_clustering_visualisations_072026.ipynb` | Notebook (+ .pdf et .html) | ACP, CAH, K-Means, visualisations, recommandation |
| `Lippens_Charles_3_presentation_072026.pdf` | PDF | Support de présentation pour le COMEX (25 diapositives) |
| `outputs/recommendations.csv` | CSV | Les 21 pays du cluster cible, classés par potentiel de marché |
| `data/raw/`, `data/processed/`, `outputs/figures/` | Dossiers | Sources officielles, jeux de données produits par les notebooks, figures exportées (dont la carte interactive des clusters) |
| `requirements.txt` | Texte | Dépendances Python de l'environnement |

Livrables disponibles dans le dossier `Etude_de_marche_Python_Lippens_Charles` (également fournis en archive `.zip`). Le README du dossier détaille l'exécution des notebooks, les deux modes de récupération des données (local ou FAOSTAT en direct) et le millésime figé du taux de change qui garantit la reproductibilité des résultats.

## Compétences développées
- Analyses multivariées et feature engineering.
- Réduction de dimension (ACP).
- Clustering (CAH, K-Means) et comparaison de partitions.
- Sélection de variables et reproductibilité (données figées, environnement versionné).
- Restitution pour un public non technique.

## Outils et méthodes
`Python` · `pandas` · `scikit-learn` · `scipy` · `ACP (PCA)` · `CAH` · `K-Means` · `plotly` · `Feature engineering`

## Résultat
Projet **validé** (évaluateur Michel Perez, 16 août 2026). Quatre compétences validées : exploiter un modèle d'apprentissage (plusieurs modèles réalisés), analyses multivariées (matrice de corrélations), réduction de dimension (ACP), sélection des variables pertinentes. Point fort : la présentation. Aucun axe d'amélioration relevé ; « bonne soutenance ». Les deux méthodes de clustering convergent vers le même groupe cible, la recommandation est chiffrée et le plan d'action est opérationnel pour le COMEX.

---

Projet réalisé dans le cadre du parcours certifiant **Data Analyst** d'OpenClassrooms.
Voir l'ensemble de mes projets sur [mon profil GitHub](https://github.com/CharlesLippensData).
