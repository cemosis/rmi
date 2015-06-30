Rencontre Mathématiques-Industrie HPC  
============================

# 22 juin 2015

 - 50 inscrits, 30 présents

# Organisateurs

- Analisa Ambroso (AA), AREVA & chargé de mission SMAI
- Samuel Kokh (SK), CEA & Maison de la Simulation
- Christophe Prudhomme(CP), U. Strasbourg & Chargé de mission AMIES

# Introduction

- Michel Kern (Maison de la simulation)
CNRS, CEA, univesoté, applications du calcul intensif. un pool d'ingénieurs, formations au HPC : équipex meso et PATC (européen), c'est gratuit. La MdS contribue au catalogue coordonnée des formations en calcul, avec MAiMoSiNe.

- Analisa Ambroso (SMAI)
Bienvenue aux participants de la part de la SMAI.
Cette journée math-industrie a été plusieurs fois reportée.


- Christophe Prudhomme (AMIES)
CP présente le programme de la journée.
1. Helluy/Muot : calcul sur GPU, une expérience de collaboration université-entreprise
2. Garnier : le HPC au service des incertitudes
   - L. Viry : analyse de sensibilité sur grilles
   - R. Lebrun (Airbus) : la libraire h-math pour le calcul de h-matrices
3. Requena : HPC-PME
   Table ronde
puis présentation d'AMIES.
 - PEPS : appel d'offre au fil de l'eau, réponse donnée en moins de 3 semaines.
 - SEME : 4 par an.
 - Success stories : un analogue du forward look est en préparation.
 - Forum emploi math : pas cette année. Dur de trouver un lieu, 1800 inscrits dépasse la capacité de la Cité U. Il faut se préparer plus longtemps à l'avance. Le Forum 2016 aura lieu à la Villette.

# Présentations

## Atelier 1 : Calcul sur GPU pour les équations de Maxwell: Histoire d'une collaboration Math Entreprises qui passe à l'échelle

#### N. Muot (AxesSim), P. Helluy (U. Strasbourg/CNRS-INSMI)

Axessim, fondée en 2007, 10 personnes (docteurs et ingénieurs), simulation électrmagnétique. Nos clients : de grand groupes (Thalès, Dassault-Aviation, Safran,...). De la recherche en interne, et des collaborations :
- ONERA/DEMR Toulouse ; on industrialise leur travail, on l'insère dans un environnement graphique plus confortable
- Xlim (université de Limoges) : idem.

C'est à l'IRMA qu'on a trouvé une compétence en développement GPU. D'abord un stage de M2, puis un CDD de 6 mois, puis une thèse CIFRE. Dans le cadre du projet GREAT (36 mois), IRMA+Axessim ont implémenté sur GPU un logiciel développé à l'ONERA.
Résultat : gain de performance d'un facteur 10,

Philippe Helluy : la méthode s'applique plus généralement aux systèmes de lois de conservation (généralisation Maxwell) non linéaires. On a voulu faire un solver générique. On a choisi la bibliothèque OpenCL. Avantage : suffisamment abstrait pour piloter divers accélérateurs. Supporté par le réseau Khronos (qui maintient OpenGL) et pas mal d'industriels (NVIDIA,...).

Parallélisme par tâches. OpenCL décide de lui-même l'ordre de lancement des calculs avec le graphe des tâches.

Muot : on a prouvé l'intérêt de
L'avenir, c'est les objets connectés au contact du corps.
Cityzens sciences : le maillot avec capteurs tissés dans la fibre, qui communiquent avec un émetteur. Où le placer pour optimiser la consommation ? respecter la norme DAS de puissance émise ?
Body Cap : une gellule ingérée mesure la température du système digestif. Optimiser le design d'antenne ? Respecter les normes médicales ?
Notre objectif, c'est de proposer à ces acteurs un accès à des ressources de calcul. Le projet Horoch (financement du ministère de l'industrie) consiste à construire une bdd de propriétés électromagnétiques du corps humain (avec IRCAD = Institut de Recherche sur le Cancer de l'Appareil Digestif).



## Atelier 2: Le HPC au service des incertitudes

### Laurence Viry (LJK, Université de Grenoble-Alpes)

La quantification d'incertitude, ça consiste à estimer les incertitudes sur les variables d'entrée d'un modèle et à estimer la part d'incertitude qui en résulte sur les sorties. On passe souvetn par un méta-modèle, modèle simplifié.
J'ai travaillé sur un modèle hydrodynamique couplé à un modèle biologique, déterministe. On s'intéresse à la concentration en phytoplancton.
L'analyse de sensibilité, pour nous, c'est probabiliste.
Méthodologie : dépendance linéaire ? si oui, régression. Si non, dépendance monotone ? si oui, régression sur les rangs. Si non, calcul des indices de Sobol.
Qu'est-ce ? On décompose la variance de $Y=G(X)$ en somme des variances expliquées par les X_i, puis par les couples $(X_i,X_j)$, etc... Les indices de Sobol sont les
$$ Var(Y|X_i_1,...,X_i_p)/Var(Y).$$
Estimer les indices de Sobol, c'est calculer des intégrales en grand dimension. Les méthodes spectrales, peu coûteuses, imposent des hypothèses de régularité. Elles sont limitées aux indices d'ordre 1. Les méthodes Monte-Carlo sont coûteuses. La méthode des hypercubes latins répliqués présente le bon compromis. Le package sensitivity de R fabrique automatiquement les plans d'expérience. La grille de calcul de Grenoble (CIMENT) admet des runs de priorité 0, automatiquement éjectés face à un run prioritaire, et relancés quand la grille est libre. Elle optimise aussi l'écriture et la lecture de petits fichiers, elle


### Régis Lebrun (Airbus group, Suresnes)

Le krigeage : représenter une relation fonctionnelle, connue par deux nuages de points, par une espérance conditionnelle : on fait une prédiction linéaire, dont les coefficients se calculent en inversant la matrice de covariance. Celle-ci est souvent mal conditionnée, non creuse (décroissance insuffisante). D'où une représentation par une h-matrice : décomposition hiérachique en morceaux de petit rang. L'hypothèse est que l'intéraction entre données provenant d'ensembles de points éloignés peut être remplacée par une valeur moyenne, d'où rang 1. Au total, complexité en NlogN pour le stockage et N^2 logN pour la multiplication.

Le thésard de P13, Benoît Lizé, a écrit une librairie h-mat en C++ suffisamment propre pour être intégrée dans le logiciel maison et publiée par LMS. Embauché, il est parti chez Google au bout de 4 mois. On a continué à développer, on arrive à traiter sur une machine de bureau (station de travail) un panneau d'avion de 3x3 m avec toutes les attaches pour accessoires.

Un peu de pub pour OpenTurns. Ca fait 15 ans qu'on a commencé à développer ce logiciel libre, 1 personne à Airbus et 1 personne à EDF (5 mois par an chacune, en théorie), en partenariat avec une PME qui vend une version emballée. Bénéfice pour les entreprises : tests, retour de bugs, exemples d'utilisation, tendance pour le développement, qui permet d'améliorer un outil qui est utilisé en interne.

## Table ronde

### Stéphane Requena (Genci)

HPC-PME : lancé en 2010, avec les pôles de compétitivité, CNRS, Inria, X et Oseo. Objectif : convaincre des PME de l'utilité pour elles du HPC. Pour cela, mettre un chercheur en contact avec une PME. 50 PME touchées en 4 ans. Vivier potentiel : 4000. bpi-France aide les entreprises à optimiser les financements.
HPC-PME phase 2 : on est reparti pour 5 ans, en partenariat avec Teratec. Objectif : arriver à 200 par an. Moyen : pôles régionaux avec des prosélytes et des réseaux d'experts régionaux. Jusqu'à présent, on a bricolé. Maintenant, on doit changer d'échelle.


### Table ronde :
S. Requena (Genci), P. Helluy, N. Muot (Axessim)

quels besoins ?
des solutions techniques nouvelles pour les entreprises ?
accès au HPC par les entreprises ?
qu'est-ce qui est simple, qui l'est moins ?
quels financements, quelles ressources HPC ?
comment intégrer des résultats théoriques ? comment pérenniser les relations ? comment diffuser la culture HPC au sein des entreprises ?

#### Axessim: comment avez vous trouvé l'IRMA ?

**Helluy** : via un contact personnel et de manière fortuite. Le talent du stagiaire de M2 a permis de décoller.

**Muot** : on a pu bâtir progressivement le projet autour de l'étudiant. Heureusement qu'il est resté, on l'a embauché ensuite. Le doctorant (CIFRE) a été embauché dès sa première année de doctorat.

**Helluy** : la première étape a été un code open-source commun. Bon pour nous : un code OpenCL documenté et sans erreur. Axessim a enveloppé ce code dans du graphique.

**Helluy** : le code ONERA, sur hexaèdre, utilisant une propriété spéciale du rotationnel, gagnait un facteur 50. On a vu comment le modifier pour garder le même gain pour un système de lois de conservaton général. Ensuite, la programmation GPU gagne un facteur 10.

#### Quelle durée entre une idée et une réalisation ?

**Helluy/Muot** : Les 3 années de la thèse. Dont 6 mois de tuning pour optimiser les performances.

#### Comment faire un sujet de thèse lorsque l'objectif est améliorer la compétitivité d'une petite entreprise ?

**Helluy** : très souvent, il y a un problème mathématique intéressant qui est au coeur de la thèse.

**Requena** : au bout, il y a un job pour le doctorant, ça fait partie des objectifs des organismes de recherche. Noter que les PME sont très intéressées à ces embauches, elle ont de la peine à recruter.

**Prudhomme**: j'ai une collaboration avec une ETI financée par un PEPS2 AMIES autour d'une résolution non standard des équations de Navier-Stokes incompressibles. Le sujet a été très peu traité d'un point de vue théorique et numérique et il présente de sérieux challenges sérieux.

#### rôle des pépinières d'entreprises ?
**Muot**:Axessim a démarré dans un incubateur en 2007, sans contacts avec l'université. Le contact s'est effectué de manière fortuite par la suite.

#### Comment faire pour une PME/Startup de gérer un code HPC? quid du manque d'expertise en local et coût élevé de maintenance ?

**Prudhomme**: se greffer sur des communautés autour de logiciels libres comme par exemple OpenTURNS, OpenFOAM et embaucher des docteurs qui pourront faire le lien avec cette communauté et les intégrer/adapter aux besoins de l'entreprise. La maintenance devient collective et le coût réduit. Des PME peuvent développer des services autour de ces plateformes, les chercheurs ont un accès au code source pour développer de nouveaux algorithmes.
Une RMI autour de la Propriété Intellectuelle et de ce modèle de développement est prévue
**Michelin**: pourquoi ne pas créer des consortiums autour de logiciels (suivre l'exemple de MUMPS). Les industriels émargent au consortium sur plusieurs et apportent des financements à la R&D de la plateforme logicielle.
**Prudhomme**: C'est effectivement une très bonne idée. Je n'ai pas connaissance de tel montage actuellement en mathématique. Nous allons regarder ca. Le Logiciel [Feel++](http://www.feelpp.org) met en place un consortium mais on en est encore à l'étape juridique

#### Quel est l'intérêt pour les académiques de travailler sur des problématiques HPC industriels
**Helluy**: de travailler sur des applications concretes et d'en extraire des problèmes de mathématiques
**Prudhomme** : Il n'y a pas que l'aspect recherche qui peut motiver il y a aussi l'aspect formation pour nos étudiants. Les industriels amènent des sujets de stage et des contextes d'applications qui sont très intéressants pour illustrer la puissance des mathématiques et du HPC et les motiver a suivre une filière Mathématique Appliquées/HPC. les industriels sont bienvenus pour venir motiver nos étudiants dans les masters.
