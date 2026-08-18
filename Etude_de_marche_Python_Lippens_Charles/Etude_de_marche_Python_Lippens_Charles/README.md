# Étude de marché Python - La poule qui chante

## Projet 11 - Parcours Data Analyst - OpenClassrooms

**Auteur** : Charles Lippens
**Date de démarrage** : juillet 2026
**Commanditaire** : Patrick, PDG de La poule qui chante
**Objectif** : proposer un groupe de pays cibles pour un premier export de poulet bio, à partir de données ouvertes uniquement.

## Livrables

| Fichier | Contenu |
|---|---|
| `Lippens_Charles_1_preparation_nettoyage_analyse_exploratoire_072026.ipynb` | Notebook 1 : préparation, nettoyage, feature engineering, analyse exploratoire |
| `Lippens_Charles_2_clustering_visualisations_072026.ipynb` | Notebook 2 : ACP (analyse en composantes principales), CAH (classification ascendante hiérarchique), K-Means, recommandation |
| `Lippens_Charles_3_presentation_072026.pdf` | Support de présentation, 25 diapositives |
| `outputs/recommendations.csv` | Les 21 pays du cluster cible, classés par potentiel de marché |

Chaque notebook est aussi fourni exporté, à côté du fichier source : en `.html` (fidèle au notebook, carte interactive comprise) et en `.pdf` (version imprimable, la carte y figure en image statique).

## Structure du dossier

```
Lippens_Charles_1_..._072026   notebook 1, avec ses exports .pdf et .html
Lippens_Charles_2_..._072026   notebook 2, avec ses exports .pdf et .html
Lippens_Charles_3_..._072026   le support de présentation (.pdf)
data/raw/         les 9 fichiers sources, téléchargés sur les sites officiels
data/processed/   les jeux de données fabriqués par les notebooks
outputs/          la recommandation et les figures exportées
requirements.txt  dépendances Python
README.md         ce fichier
```

Les trois livrables sont à la racine du dossier, nommés selon la convention demandée.

Les dossiers `data/processed/` et `outputs/figures/` sont livrés déjà remplis, avec les sorties de la dernière exécution complète : un évaluateur peut donc ouvrir le notebook 2 directement, sans repasser par le notebook 1. Une nouvelle exécution les réécrit, et les recrée d'elle-même s'ils ont été supprimés.

## Comment exécuter

```bash
# 1) environnement virtuel
python -m venv .venv-p11
.venv-p11\Scripts\activate        # Windows
source .venv-p11/bin/activate     # Linux ou macOS

# 2) dépendances
pip install -r requirements.txt

# 3) Jupyter
jupyter lab

# 4) exécuter dans cet ordre
#    notebook 1 (à la racine) : produit data/processed/dataset_p11_clean.csv et data/processed/fx_par_pays.csv
#    notebook 2 : lit ces deux fichiers, produit les clusters et la recommandation
```

Les notebooks retrouvent seuls la racine du projet : ils remontent l'arborescence jusqu'à trouver `data/raw`. Ils fonctionnent donc aussi bien placés à la racine du dossier décompressé que dans un sous-dossier.

## Deux modes de récupération des données

Le notebook 1 accepte deux modes, pilotés par la variable d'environnement `P11_MODE` :

- `local`, le mode par défaut : aucune question posée, aucun appel réseau vers la FAO, tout est lu depuis `data/raw`. La table des libellés de pays vient de `data/raw/fao_pays_regions.csv`, qui est l'export officiel du site. Le notebook se déroule d'un bout à l'autre sans intervention.
- `auto` : la deuxième cellule propose de se connecter à FAOSTAT pour récupérer cette même table en direct. Valider à vide, sans rien saisir, bascule sur le fichier local, sans erreur. Aucun compte n'est donc nécessaire, dans un mode comme dans l'autre.

Le réglage ne porte que sur la FAO et ne change aucun résultat : la table en ligne et l'export local sont le même document, seule la ligne de provenance affichée diffère. Les 172 pays et tous les chiffres qui suivent sont identiques dans les deux modes. Les cotations de change de la section 8, elles, sont interrogées quel que soit le mode.

Pour passer en mode auto sous Windows, avant de lancer Jupyter :

```powershell
$env:P11_MODE = 'auto'
jupyter lab
```

## Le taux de change, et pourquoi il est figé

Le taux de change entre dans l'analyse comme quatorzième variable, sous forme de logarithme. La section 8 du notebook 1 interroge trois services de cotation en cascade : CurrencyBeacon, puis ExchangeRate-API, puis un endpoint public ouvert. Les deux premiers demandent une clé personnelle, lue dans les variables d'environnement. Le troisième n'en demande aucune : sans configuration particulière, la cellule fonctionne quand même.

Un taux bouge tous les jours, et il peut différer fortement d'un service à l'autre. Pour que les clusters et les montants restent reproductibles à l'identique, l'entrée d'analyse du notebook 2 est un millésime figé, `data/raw/fx_millesime_20260730.csv`, relevé le 30 juillet 2026. La cotation du jour est bien récupérée et affichée, mais elle sert de contrôle de fraîcheur : le notebook mesure l'écart au millésime et enregistre le relevé à part, dans `data/processed/fx_du_jour.csv`, sans toucher au fichier d'analyse.

## Sources des données

- **FAO, Bilans alimentaires 2017** (`DisponibiliteAlimentaire_2017.csv`) : production, importations, exportations et disponibilités alimentaires en quantité, en protéines et en matières grasses, pour les principaux produits, sur 174 pays. Source : https://www.fao.org/faostat/fr/#data/FBS
- **FAO, Population 2000-2018** (`Population_2000_2018.csv`) : estimations annuelles de population par pays. Source : https://www.fao.org/faostat/fr/#data/OA
- **FAO, table des pays et régions** (`fao_pays_regions.csv`) : correspondance officielle entre libellés FAO et codes ISO3. Source : https://www.fao.org/faostat/fr/#definitions
- **Banque mondiale, indicateurs 2017** : PIB par habitant courant (NY.GDP.PCAP.CD), croissance du PIB (NY.GDP.MKTP.KD.ZG), inflation des prix à la consommation (FP.CPI.TOTL.ZG). Source : https://data.worldbank.org
- **Banque mondiale, Worldwide Governance Indicators** (`DataBank_PV.EST_WGI_2017.csv`) : stabilité politique et absence de violence (PV.EST). Source : https://databank.worldbank.org/source/worldwide-governance-indicators
- **Cotations de change** : CurrencyBeacon, ExchangeRate-API et open.er-api.com pour la table complète par devise, Yahoo Finance pour les quatre paires cibles en temps réel.

## Résultats

L'analyse porte sur **172 pays**, soit **97,6 %** de la population mondiale de 2017, décrits par **13 variables structurelles** auxquelles s'ajoute le taux de change en logarithme, soit **14 variables actives**. Sept variables ont été construites par feature engineering au notebook 1, cinq sont conservées dans le jeu de données final.

L'ACP retient **4 composantes** selon le critère de Kaiser (valeur propre supérieure à 1), qui portent ensemble **65,4 %** de l'inertie. Le clustering est mené sur ces quatre composantes plutôt que sur les variables brutes, pour travailler sur une entrée décorrélée.

Deux méthodes sont comparées sur la même entrée. La CAH avec la méthode de Ward donne quatre groupes de 95, 48, 18 et 11 pays. Le K-Means, avec la même valeur de K, donne 70, 68, 21 et 13 pays. L'indice de Rand ajusté entre les deux partitions vaut **0,578** et l'information mutuelle normalisée **0,679** : les deux méthodes s'accordent sur la structure générale et divergent sur les pays frontière, ce qui est le comportement attendu quand les groupes ne sont pas nettement séparés.

Un contrôle mesure l'effet du change sur la partition : en retirant cette seule variable et en relançant la chaîne complète, l'indice de Rand ajusté entre les deux résultats vaut **0,811**. Le change affine les groupes sans les redessiner.

Le cluster cible est désigné par un scoring recalibré, une pondération des variables du profil recherché : PIB par habitant, disponibilité alimentaire en volaille, potentiel de marché, stabilité politique et importations. Il rassemble **21 pays**. Le top 5 par potentiel de marché réunit les États-Unis (1 079,4 MUSD), la Chine (157,3), le Brésil (100,6), le Japon (91,6) et le Royaume-Uni (87,2), soit **1 516,2 MUSD** à eux cinq et **2 111,3 MUSD** pour les 21. Le Brésil, premier exportateur mondial et quasi autosuffisant, reste hors du pilote : les marchés d'entrée privilégiés sont les grands importateurs proches (Allemagne, 6e, et Royaume-Uni). La France figure dans le cluster, au 9e rang avec 57,4 MUSD, mais elle est écartée de la liste d'export : c'est le marché domestique de l'entreprise.

## Correspondance avec les critères d'évaluation

| Critère | Exigence | Réalisé |
|---|---|---|
| Variables actives | au moins 8 | 14 |
| Pays couverts | au moins 100 | 172 |
| Couverture de la population mondiale | au moins 60 % | 97,6 % |
| Variables construites par feature engineering | au moins 3 | 7 créées, 5 conservées |
| Valeurs manquantes dans le jeu final | 0 | 0 |
| ACP réalisée | oui | oui, 4 composantes retenues |
| Éboulis des valeurs propres | oui | figure 06 |
| Cercle des corrélations | oui | figures 07 et 08 |
| Projection des individus | oui | figure 09 |
| CAH avec dendrogramme | oui | figure 11 |
| K-Means | oui | figures 13 et 14 |
| Comparaison des deux méthodes | oui | tableau croisé, ARI et NMI |
| Notebooks séparés, exploration puis clustering | oui | notebooks 1 et 2 |
