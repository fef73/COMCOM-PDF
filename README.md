# Fiche climat intercommunale — générateur de fiches A4

Générateur mono-fichier (HTML/CSS/JS, sans backend) de **fiches climat A4 pour les communautés de communes** : toutes les communes d'une intercommunalité sur une seule page, comparées de la plus basse à la plus haute, prêtes à imprimer ou à enregistrer en PDF.

Site : https://fef73.github.io/COMCOM-PDF/ — accès protégé par mot de passe. Projet associé : fiches communales METEO-MAIRIES, lanceur [comparateur-meteo.fr](https://comparateur-meteo.fr/).

## Choix du territoire

- Recherche d'une intercommunalité (EPCI) par son nom ; ses communes sont chargées automatiquement (`geo.api.gouv.fr`).
- Liste modifiable : cases à cocher pour exclure une commune, **ajout** de n'importe quelle commune par nom ou code INSEE.
- **Altitude modifiable** à l'ajout (par défaut celle du chef-lieu) : une même commune peut être ajoutée plusieurs fois, par exemple le village et sa station de ski.
- Titre de la fiche libre (ex. « Vallée de l'Arvan » pour un Pack territoire).
- Chaque commune est calculée au **chef-lieu** (position de la mairie), là où vivent les habitants, et non au centre géométrique du territoire communal.

## Contenu de la fiche

- En-tête : nombre de communes, habitants (une commune ajoutée plusieurs fois n'est comptée qu'une fois), plage d'altitude.
- Indicateurs : température moyenne normale 1991–2020, écart de la dernière décennie, évolution de la neige, année la plus chaude.
- Tableau par commune : altitude, habitants, température, écart récent, précipitations, cumul de neige fraîche (normale et décennie récente), jours de gel, jours ≥ 30 °C.
- Graphiques : écart annuel de température du territoire depuis 1991, température selon l'altitude.
- Tableau des **postes Météo-France proches** avec la neige fraîche réellement mesurée.
- Encadré « Comment lire » qui explique l'origine des chiffres de pluie et de neige.

## Données et méthode

- **Températures** : réanalyse ERA5-Land (Copernicus / ECMWF, maille ≈ 9 km) via [Open-Meteo](https://open-meteo.com/), corrigée selon l'altitude.
- **Précipitations** : ERA5 (maille ≈ 25 km), puis **recalées** sur la pluie mesurée par les postes Météo-France à moins de 15 km, en tenant compte de l'altitude.
- **Neige** : cumul de neige fraîche (pas l'épaisseur au sol), calculé à partir de la neige fraîche **mesurée** par les postes Météo-France à moins de 20 km, ajusté à l'altitude de chaque commune ; un poste très proche (moins de 5 km, moins de 300 m de dénivelé) sert d'ancrage local. Sans postes, une estimation à partir des précipitations et de la température locale est utilisée.
- Normales : moyenne de référence 1991–2020 ; décennie récente : les dix dernières années complètes.

## Postes Météo-France

- `preparer-postes-mf.html` lit les **Données climatologiques de base quotidiennes** de Météo-France ([meteo.data.gouv.fr](https://meteo.data.gouv.fr/), licence Etalab 2.0) et produit `postes-mf.json` (pluie annuelle et neige fraîche par poste depuis 1991).
- Les fichiers `Q_<dép>_…csv.gz` se téléchargent avec les liens de la page, puis se sélectionnent dans l'outil (Ctrl+clic, ou un par un) ; plusieurs départements possibles (ex. `73,38,05`).
- `postes-mf.json` placé à côté de `index.html` est chargé automatiquement ; sinon, bouton **Charger postes-mf.json**.
- À régénérer une fois par an pour intégrer la dernière année.

## Quotas et cache

- Open-Meteo limite le nombre de requêtes : les communes sont chargées **par lots de 4**, avec un chronomètre d'attente entre les lots et la reprise automatique après un quota atteint.
- Chaque commune chargée est gardée dans le navigateur : rien n'est demandé deux fois.
- Barre de cache : **Enregistrer** (fichier JSON), **Restaurer**, **Vider le cache**.

## Fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | Générateur de fiches |
| `preparer-postes-mf.html` | Préparation des postes Météo-France |
| `postes-mf.json` | Pluie et neige fraîche mesurées par poste |

## Licence

Tous droits réservés — voir [LICENSE](LICENSE). Les données restent soumises à leurs propres licences (Open-Meteo CC BY 4.0 ; Météo-France et geo.api.gouv.fr sous Licence Ouverte Etalab 2.0).
