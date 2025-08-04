### Titre
Identification des meilleurs emplacements pour des stations-service: cas du Sénégal.
### Contexte et objectif
Dans un contexte de croissance urbaine et d'augementation du parc automobile, il devient essentiel d'optimiser la localisation des stations-service afin de garantir une couverture équitable et accéssible.

Ce projet vise à identifier les emplacements stratégiques pour l'implantation de nouvelles stations-service, en tenant compte de la répartition de la population, de la distance aux garres routières, de la couverture actuelle des stations existantes de la conectivité aux réseaux routiers et de l'occupation du sol.
### Données
- **Réseau routier**, vecteurs des routes principales: extrait d'OpenStreetMap.
- **Stations-service existantes**, position actuelle des infrastructures : extraites depuis OpenStreetMap.
- **Gares routières**, localisation des terminaux de transport: localisation extraite depuis OpenStreetMap.
- **Population**: téléchargée via **Google Earth Engine(GEE)** à partir de l'ensemble de données démographique mondial(GHSL).
- **Occupation des sols**: obtenu via **GEE** à partir de l'ensemble de données ESA Worldcover.
- Une densité homogène est calculée **par commune**, puis héritée à chaque pixel du raster pour créer une carte de rasterisée de la densitée de la population ajustée.
- **Limites administratives**: polygone des communes pour aggrégation et des traitement des données de la population.
  #### Mentions légales ####
  Les données gégraphiques, issues d'**OpenStreeepMap**, utilisées dans ce projet sont accessibles sous la licence `Licencee Open databse Licence(ODbL)`.
  
  Attribution obligatoire: les contributeurs d'**OpenStreetMap - https://www.openstreetmap.org/copyright**
### Plateformes et Bibliothèques
- **Python**: utilisé pour l'automatisation des chaines de traitement géospatial(lecture, reprojection, filtrage spatial, ...).
- **Gdal**: pour les opérations de géotraitements.
- **Geopandas**: pour la manipulation de données vectorielles.
- **Osmnx**: pour la récupération des routes, des garres routières et des stations-service existantes.
- **Nomitatim**: utilisé pour la reverse géocodification.
- **Pyproj**: utilisée pour la transformation et la gestion des sytèmes de coordonnées.
- **Folium**: pour la visualisation interactive.
### Méthodologie
1. **Téléchargement des données**
    - Extraction OSM du réseau routier, des garres routières et des stations existantes
    - Téléchargement des données de la population.
    - Téléchargement des données sur l'occupation du sol.
2. **Prétraitement des données**
   - Harmonisation des systèmes de coordonnées.
   - Nettoyage des entités(routes, stations, garres) pour éviter des doublons et incohérences.
4. **Rasterisation des couches vectorielles**
   - **Réseau routier**: rasterisation en ternaire(1 = route primaire, 2 = route principale, 0 = non-route).
   - **Garres routières**: rasterisation avec présence (1 = garre, 0 = ailleurs).
   - **Stations-service existatantes**: rasterisation similaire pour identifier les zones déjà desservies.
5. **Analyse de la densité de la population**
   - Récupération des données de population via Google Earth Engine.
   - Calcul de la densité(habitants/km2) de la population par commune.
   - Attribution à chaque pixel la densité de la commune dans laquelle il appartient.
6. **Calcul de carte de score**
   - Une carte d'aptitude est générée à partir de l'aggrégation des différents critères spatiaux.
   - Chaque critère est **pondéré**: **proximité au réseau routier**(avec pondérations différenciées routes trunk > primaires), **éloignement des stations existantes**, **présence de gares**, **densité de la population**, **occupation du sol**(en éliminant les zones inappropriées, example: forêts, zones humides, plans d'eaux permenants, nodatavalue). Ces couches rasterisées et pondérées sont combinées pour obtenir une **carte finale** dans laquelle chaque cellule indique à quel point une zone est favorable à l'implantation d'une station-service.
   - **Un buffer de 1 Km** est appliquée autour des meilleurs sites identifiés, afin de prévenir la proximité exessives entre plusieurs nouveaux emplacements.
5. **Extraction des meilleurs emplacements**
   - les pixels avec le score le pus élevés sont sélectionnés comme candidat.
   - Les pixels sont convertis en coordonnées géographiques.
   - Ces coordonnées sont ensuite utlisées pour récupérer les adresses pyhysique correspondantes(via géocodage inversé).
6. **Résultats attendus**
   - Un fihier excel contenant coordonnées, classements et adresses de n meilleurs sites.
   - Une carte interactive localisant les meilleurs sites pour de nouvelles stations-service, en tenant compte de la densité de la population, de la proximité routière, de l'évitement des zones déjà couvertes et de la constructibilité du sol.
   - Un outil d'aide à la décision pour les planificateurs urbains et investisseurs.
➡ Tous les détails sont disponibles dans le notebook `.ipynb` et les images et carte dans le dossier `docs`
### Contributions
Toutes les suggestions, retours, ou contributions sont les bienvenus afin d'améliorer et enrichir ce projet.
### Contact
Email: sidiniang20@gmail.com
