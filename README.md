# Attractivité Économique des Départements Français

> Étude des disparités de développement économique entre les départements métropolitains français - identification des déterminants structurels de l'entrepreneuriat par estimation MCO.

**Pour plus de détails, lire "Rapport_de_l'attractivité_des_départements.pdf"**

---

## Table des matières

- [Contexte](#-contexte)
- [Objectif](#-objectif)
- [Variables](#-variables)
- [Méthodologie](#-méthodologie)
- [Résultats](#-résultats)
- [Limites](#-limites)
- [Sources](#-sources-des-données)

---

## Contexte

La création d'entreprises est un indicateur clé du dynamisme territorial. Des données récentes de l'INSEE font état d'un ralentissement des immatriculations de nouvelles entreprises en France, soulevant des questions sur les facteurs locaux qui favorisent ou freinent l'entrepreneuriat.

Ce projet s'appuie sur les travaux antérieurs de **Levratto et al. (2013)** sur les dynamiques territoriales et la création d'entreprises dans les départements français.

L'analyse porte sur **94 départements métropolitains** pour l'année **2022**.

> Les territoires d'outre-mer et la Corse ont été exclus en raison de données manquantes et de problèmes de comparabilité.

---

## Objectif

Expliquer les variations de la **proportion de créations d'entreprises** entre les départements français à l'aide d'un ensemble de variables explicatives socioéconomiques, et fournir des bases pour des recommandations de politique publique visant à réduire les inégalités territoriales.

Pour plus de détails, lire le **[Rapport de l'attractivité des départements](./rapport.pdf)**.

---

## Variables

### Variable dépendante

| Variable | Description |
|----------|-------------|
| `pcENT` | Proportion de nouvelles entreprises créées en 2022 par rapport aux entreprises existantes en 2021 (%) |

### Variables explicatives

| Variable | Description | Source |
|----------|-------------|--------|
| `nbENT` | Nombre d'entreprises existantes en 2021 (milliers) | INSEE |
| `POP` | Population municipale en 2021 (milliers) | INSEE |
| `DIPL` | Part des diplômés de niveau Bac+3 ou supérieur (%) | INSEE |
| `REV` | Revenu médian annuel en 2021 (milliers EUR) | INSEE |
| `gndENT` | Nombre de grandes entreprises (250+ salariés) en 2021 | data.gouv |
| `txCHOM` | Taux de chômage en 2021 (%) | INSEE |
| `METRO` | Indicateur binaire : présence d'une métropole | collectivites-locales.gouv |

---

## Méthodologie

### 1. Analyse Descriptive

- Statistiques univariées (moyenne, médiane, écart-type, min, max) pour chaque variable
- Graphiques de distribution pour identifier l'asymétrie et les valeurs aberrantes
- Analyse bivariée : régressions linéaires simples entre `pcENT` et chaque variable explicative
- Comparaison par boîtes à moustaches selon le statut métropolitain

### 2. Analyse des Corrélations

- Matrice de corrélation de Pearson pour détecter la multicolinéarité
- Analyse des Facteurs d'Inflation de la Variance (FIV) :

| Variable | FIV | Niveau de colinéarité |
|----------|-----|----------------------|
| `nbENT` | 7,6 |  Forte |
| `gndENT` | 5,5 |  Modérée |

### 3. Sélection du Modèle

Trois spécifications ont été estimées et comparées à l'aide du **Critère d'Information d'Akaike (AIC)** :

| Modèle | Spécification | R² ajusté |
|--------|--------------|:---------:|
| Modèle 1 | Niveau-niveau | 0,590 |
| Modèle 2 | Niveau-log | 0,614 |
| ✅ **Modèle 3** | **Log-log** | **0,686** |

Le **modèle log-log** a été retenu. Les variables `nbENT`, `POP` et `gndENT` ont été log-transformées afin de linéariser les relations et de réduire l'influence des valeurs extrêmes.

### 4. Tests de Robustesse

| Test | Objectif | Résultat |
|------|----------|----------|
| Test de White | Hétéroscédasticité | Détectée — corrigée par les écarts-types de White et Newey-West |
| Shapiro-Wilk + QQ-plot | Normalité des résidus | Rejetée — présence d'observations influentes |
| Distance de Cook | Points influents | Départements **91, 93, 95** identifiés |
| Test de Chow | Rupture structurelle (avec/sans métropole) | Aucune rupture détectée (p = 0,97) |
| Test RESET | Mauvaise spécification du modèle | Correctement spécifié (p = 0,47) |

---

## Résultats

Le modèle final atteint :

-  **R² ajusté = 0,758**
-  Toutes les variables significatives au seuil de **5%**, à l'exception de `METRO`
-  Homoscédasticité et normalité des résidus confirmées

---

## Limites

- **Variables omises** : les facteurs culturels locaux, les politiques publiques régionales et les infrastructures ne sont pas inclus.
- **Endogénéité potentielle** entre certaines variables explicatives et la variable dépendante.
- **Données en coupe transversale uniquement** : absence de dimension temporelle pour saisir les tendances au fil du temps.

---

## Sources des Données

| Source | Lien |
|--------|------|
| INSEE - Statistiques locales | [statistiques-locales.insee.fr](https://statistiques-locales.insee.fr) |
| data.gouv - Annuaire des entreprises | [annuaire-entreprises.data.gouv.fr](https://annuaire-entreprises.data.gouv.fr) |
| Collectivités locales | [collectivites-locales.gouv.fr](https://www.collectivites-locales.gouv.fr) |
