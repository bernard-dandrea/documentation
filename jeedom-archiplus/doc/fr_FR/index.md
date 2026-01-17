<!--  
  Last Modified : 2026/01/17 14:36:23
-->
- [La gestion des historiques dans Jeedom](#la-gestion-des-historiques-dans-jeedom)
  - [Fonctionnement](#fonctionnement)
  - [Volume des historiques](#volume-des-historiques)
  - [Les limites de l'archivage dans Jeedom](#les-limites-de-larchivage-dans-jeedom)
  - [Les PLUS du plugin archiplus](#les-plus-du-plugin-archiplus)
  - [Avertissement](#avertissement)
- [Plugin archiplus](#plugin-archiplus)
  - [Installer le Plugin archiplus](#installer-le-plugin-archiplus)
  - [Configurer le plugin](#configurer-le-plugin)
  - [Les modules du plugin](#les-modules-du-plugin)
- [Interface](#interface)
  - [Les boutons de commandes](#les-boutons-de-commandes)
  - [La colonne de sélection des lignes](#la-colonne-de-sélection-des-lignes)
  - [Les entêtes de colonne](#les-entêtes-de-colonne)
  - [Les lignes](#les-lignes)
  - [Les totaux de bas de tableau](#les-totaux-de-bas-de-tableau)
- [le module Monitor](#le-module-monitor)
  - [Statistiques](#statistiques)
  - [Visualisation](#visualisation)
  - [Modifications](#modifications)
  - [Données modifiables](#données-modifiables)
    - [KLV (Keep Last Value):](#klv-keep-last-value)
    - [Uniq](#uniq)
    - [Délai](#délai)
    - [Cadrage](#cadrage)
    - [Pond](#pond)
    - [Pack](#pack)
    - [Arrondi](#arrondi)
  - [Fonctions accessibles via le menu contextuel](#fonctions-accessibles-via-le-menu-contextuel)
- [Données historiques](#données-historiques)
  - [Accès](#accès)
  - [Modification](#modification)
  - [Suppression](#suppression)
  - [Export](#export)
- [Le module Import](#le-module-import)
- [Le module Restore](#le-module-restore)
- [FAQ](#faq)
  - [Keep Last Value](#keep-last-value)
  - [Uniq](#uniq-1)
  - [Délai et Cadrage](#délai-et-cadrage)
  - [Lissage et pondération](#lissage-et-pondération)
  - [Pack](#pack-1)
  - [Arrondi](#arrondi-1)
  - [copier les données de historyArch vers history](#copier-les-données-de-historyarch-vers-history)
  - [Lancer l'archivage d'une commande à partir d'un scénario](#lancer-larchivage-dune-commande-à-partir-dun-scénario)



La fonction principale du plugin est de fournir un ensemble complets d'outils permettant:

*   **de gérer les paramètres d'archivage des commandes de type info**
*   **de visualiser les volumes de données et de détecter les anomalies**
*   **d'insérer facilement des données historique à partir de fichiers de type excel**
*   **de récupérer les historiques à partir des archives Jeedom**
*   **d'étendre les options d'archivage standard de Jeedom**

L'activation optionnelle de l'archivage intégré au plugin permet d'étendre très significativement les fonctions d'archivage proposée par Jeedom.

# La gestion des historiques dans Jeedom

## Fonctionnement

L'historique dans Jeedom a peu évolué depuis les premières versions et se base sur 2 tables:

* la table history qui reçoit les mises à jour des valeurs des commandes de type INFO pour lesquelles l'historique est activé
* la table historyArch qui reçoit lors de chaque archivage (habituellement chaque jour à 5h00) les valeurs de history consolidées ou non suivant le paramétrage défini pour la commande.

La structure des deux tables est identique et très simple: une valeur est enregistrée par commande Id et datetime (géré à la seconde). 

L'historique peut être affiché dans l'interface Jeedom sous forme de graphe.

La documentation officielle concernant la gestion des historiques dans Jeedom se trouve [ici](https://doc.jeedom.com/fr_FR/core/4.4/history).

## Volume des historiques

L'utilisateur de Jeedom commencera à s'intéresser à l'historique lorsqu'ils constatera une base de données qui grossit de façon exagérée, des temps d'affichage de l'historique qui deviennent très longs, une taille de sauvegarde qui devient importante.

Le lien suivant permet d'accéder à un tuto qui explique comment créer un scénario qui listera les volumes des tables les plus volumineuses et les commandes INFO avec les plus gros historiques [Tuto - Analyser les archives](https://community.jeedom.com/t/tuto-analyser-les-archives-pour-detecter-des-pbs-lenteurs-espaces-disques/104384).

Plus simplement, vous pouvez voir les volumes des tables en interrogeant directement la base de données (menu Réglages / Système / Configuration puis onglet OS / DB (le dernier) puis bouton "Administration base de données" (bouton rouge le plus bas) puis sur la gauche interrogation "taille").

Dans une installation standard, il faut commencer à s'interroger lorsque le volume global des archives dépasse le million d'enregistrements ou qu'une commande info à plus de 10 000 enregistrements. Dans ca cas, il est nécessaire d'analyser les commandes concernées et de jouer sur les différents paramètres de l'historisation et de l'archivage afin de réduire ce volume. Si ce n'est pas possible, il faudra peut-être se tourner vers d'autres méthodes d'archivage, par exemple influxDB qui peut s'interfacer en standard avec Jeedom.

Le plugin archiplus donne immédiatement les volumes de history et historyArch et permet de cibler facilement les problèmes et d'y apporter des solutions.

## Les limites de l'archivage dans Jeedom

Bien que dans de nombreuses installations, le fonctionnement standard sera suffisant, on peut relever les limitations suivantes:

* Difficulté à visualiser et modifier les paramètres d'archivage: le seul outil disponible (menu Analyse / Historique puis Configuration) est très lent, peu pratique et présente peu de champs à configurer
* Difficulté pour visualiser les volumes d'historique par commande et repérer les volumes anormaux: il faut passer par des requêtes SQL et des processus peu pratiques 
* Paramètres pour le regroupement de données dans historyArch défini de façon globale et non personnalisable par commande
* Pas de visibilité concernant le processus d'archivage (pas de log)
* Archivage global: pas de possibilité de lancer l'archivage pour une commande spécifique
* Lissage par moyenne aprroximatif
* Outils basiques pour exporter/importer les données (plugin dataexport). Rien n'est proposé pour restorer les données d'historique contenues dans les sauvegardes.

## Les PLUS du plugin archiplus

Le plugin archiplus permet de visualiser dans un tableau les commandes de type INFO avec l'ensemble des paramètres relatifs à l'archivage. Le nombre d'enregistrements dans history et historyArch est également présenté ce qui permet de détecter très facilement les volumes excessifs. Le plugin utilise la librairie javascript Tabulator qui est extrèmement performante et permet un accès très facile aux fonctions offertes par le plugin.

Toutes les fonctions offertes par Jeedom sont disponibles directement et d'autres fonctions ont été ajoutées:

* Configuration avancée de la commande
* Affichage des graphiques et extraction des données
* Purge de l'historique
* Export standard CSV
* Copie de la configuration de l'historique (ou d'un seul paramètre) vers plusieurs commandes
* Lancement de l'archivage pour une commande donnée
* Copie de l'historique d'une commande vers une autre commande
* Copie de historyArch vers history afin de lancer une consolidation par intervalle
* Importation de l'historique d'une commande à partir d'un fichier Excel
* Extraction de l'historique sous plusieurs format (xlsx, CSV, JSON, HTML) d'une ou plusieurs commandes depuis Jeedom ou une sauvegarde standard Jeedom

De plus, le processus d'archivage du plugin peut être activé en remplacement de la fonction archive native offerte par Jeedom. Celui-ci permet:

* de lancer l'archivage pour une commande donnée
* d'enregistrer dans la log archiplus l'ensemble des opérations effectuées et les paramètres pris en compte pour chaque commande
* de personnaliser la période de calcul (pour min, max, moyenne), le délai avant archivage et la taille de paquet pour chaque commande
* de caler la date de purge sur un jour, une heure ou une minute
* de lancer l'archivage pour une commande depuis un scenario (en code PHP)
* d'ajouter des options non prévues dans Jeedom (voir les explications plus loin dans la documentation)
  * Keep Last Value : conserver toujours au moins une valeur dans l'historique
  * Uniq : éliminer les valeurs consécutives identiques dans historyArch
  * Pond : dans le lissage par moyenne, calculer la valeur pondérée sur la durée de l'intervalle (et non la moyenne des valeurs)

le plugin archiplus a été développé sous Debian 12 et n'utilise pas Jquery (de même que les bibliothèques 3rd party utilisées). Il respecte les standards de développement de Jeedom. Le code de la classe archiplus est très structuré et largement documenté : l'auteur du plugin étudiera toutes les propositions de correction ou d'amélioration.

Jeedom n'ayant pas de plan d'évolution de la gestion de l'historique, le plugin ne devrait pas nécessiter de refonte dans un avenir proche. 

## Avertissement

Le plugin et son processus spécifique d'archivage ont été testés très rigoureusement mais ne sont cependant pas à l'abri d'anomalies. Dans ce cas, l'équipe Jeedom n'est évidemment pas tenue d'apporter son support. Les demandes d'analyses et de correction devront être adressées impérativement à l'auteur du plugin via la demande d'assistance standard. 

L'activation du plugin et en particulier du procesus d'archivage impliquent donc une pleine acceptation de cette situation.

# Plugin archiplus

## Installer le Plugin archiplus

Aller dans le Market Jeedom, trouver le plugin archiplus et installer la version **stable**. Puis **Activer le plugin**.

![001](../images/001.png)

Le plugin est accessible via le menu.

## Configurer le plugin

Dans la configuration, vous pouvez paramétrer les paramètres habituels des plugins et les valeurs par défaut qui seront utilisées par le plugin.

![003](../images/003.png)

Pour avoir un maximum d'informations sur le processus d'archivage du plugin et les actions réalisées, il est conseillé de mettre les logs en mode Debug.

Noter que les demandes de support devront se faire via le bouton **Assistance**.

![002](../images/002.png)

Dans la section configuration, vous pouvez:

* Activer l'archivage spécifique (désactivé par défaut)
* Définir le format pour les exports
* Définir le cadrage par défaut pour les dates de purge et fin d'archivage

L'activation de l'archivage spécifique crée un nouveau cron dans le moteur de taches et désactive l'archivage standard. La désactivation de l'archivage spécifique effectue l'opération inverse.

Si vous voulez tester le processus d'archivage du plugin, vous pouvez l'activer temporairement, faire des tests d'archivage sur des commandes individuelles puis désactiver l'archivage du plugin. Le processus d'archivage de Jeedom se lançant habituellement à 5 heures du matin, il n'y aura pas d'impact sur les commandes non testées.

## Les modules du plugin

![004](../images/004.png)

A partir du menu Plugins / Monitoring / archiplus, vous avez accès à la totalité des fonctions du plugin

* Configuration du plugin (voir ci-dessus)
* Accès aux paramètres globaux du paramétrage de l'archivage
* Monitoring: visualiser et modifier le paramètrage et  réaliser les principales opérations concernant l'archivage
* Import: importer des données historiques à partir d'un fichier de type Excel
* Restore: extraire les données historiques à partir d'une archive standard Jeedom

La visualisation des données historique est accessible à partir du module Monitoring et Restore.

# Interface

Les modules sont lancés à partir de la configuration du plugin.

![005](../images/005.png)

La base de l'interface est une table Tabulator remplie avec les données pertinentes.

Par exemple, avec le module Monitor, une table est affichée avec les commandes de type INFO ayant la fonction historique activée.

L'écran comporte plusieurs parties.

## Les boutons de commandes 

![006](../images/006.png)

Les boutons permettent des actions globales concernant l'affichage, les lignes sélectionnées, les mises à jour, ...

![013](../images/013.png)

Les boutons ci-dessus sont communs à tous les modules et permettent:

* d'afficher le fichier log de archiplus
* d'aller à la première ou à la dernière ligne du tableau
* d'annuler les filtres qui ont été activés
* de revenir au tri initial
* d'exporter les données sélectionnées 
* de revenir aux différents modules proposés par archiplus

![019](../images/019.png)

Le bouton standard "Aide sur la page en cours" vous permet d'accéder à la documentation du plugin.

## La colonne de sélection des lignes

![007](../images/007.png)

La première colonne permet de sélectionner les lignes sur lesquelles on souhaite agir.

En cliquant sur l'entête de colonne, on sélectionne toutes les lignes affichées du tableau.

On peut sélectionner chaque ligne individuellement en cliquant sur la case à cocher ou à n'importe quel endroit de la ligne.

On peut sélectionner aussi une suite de lignes en cliquant sur la première à sélectionner puis en cliquant sur la dernière en maintenant la touche MAJ enfoncée.

## Les entêtes de colonne

![008](../images/008.png)

Les entêtes de colonne décrivent le contenus des cellules situées dans la colonne.

Elles permettent :

* d'obtenir une information complémentaire via une infobulle en laissant la souris une seconde sur le champ
* de trier les lignes selon la valeur du champ en cliquant sur le libellé de la colonne (noter que le bouton "Tri initial" permet d'annuler tous les tris effectués)
* de filtrer les lignes affichées en entrant un critère de sélection dans le champ situé sous le nom de la colonne (noter que le bouton "Reset" permet d'annuler toutes les sélections).

Dans le cas du plugin Monitor, un regroupement des colonnes permet de sélectionner uniquement certains type d'information.

## Les lignes

![009](../images/009.png)

Les  lignes présentent les informations demandées.

En fonction du contexte, un clic droit fait apparaître un menu contextuel avec les actions possibles.

![010](../images/010.png)

En cliquant sur un champ modifiable, on peut entrer une nouvelle valeur.

![011](../images/011.png)

Les champs modifiés apparaissent sur un fond magenta qui disparait après validation des modifications.

## Les totaux de bas de tableau

![012](../images/012.png)

En bas du tableau sont affichés les totaux correspondant aux lignes affichées ou sélectionnées.

# le module Monitor

Il s'agit du module principal de archiplus.

![005](../images/005.png)

Après avoir cliqué sur Monitor, les commandes INFO avec un historique actif sont affichées en quelques secondes.

![014](../images/014.png)

En cliquant sur le bouton ci-dessus, on peut basculer sur l'affichage de toutes les commandes INFO, même celles qui ne demandent pas d'historique ou celles dont l'équipement est inactif.

## Statistiques

![016](../images/016.png)

Le nombre d'enregistrements dans history et historyArch correspond généralement à celui du dernier archivage (on peut voir la date de mise à jour en laissant la souris sur un des compteurs). En cliquant sur l'entête de colonne #All, on peut voir immédiatement les commandes avec le plus gros historique.

![015](../images/015.png)

En cliquant sur le bouton ci-dessus, on peut relancer un calcul ce qui prendra quelques secondes.

![017](../images/017.png)

Les totaux en bas du tableau vous permettent de connaitre immédiatement la taille de votre historique.

## Visualisation

![018](../images/018.png)

Les boutons de visualisation vous permettent de sélectionner les données affichées

* la configuration de l'historique
* les calculs
* les valeurs interdites
* l'affichage via les graphiques
* les statistiques

Suivant ce qui vous intéresse, vous pouvez activer ou non la partie que vous voulez gérer. Afin de ne pas surcharger l'écran de démarrage de Monitor, seules les données d'identification, de configuration et les statistiques sont présentées.

## Modifications

![020](../images/020.png)

Pour modifier une donnée, il suffit de cliquer sur la zone concernée et d'entrer une nouvelle valeur. 

![021](../images/021.png)

Les données modifiées apparaissent sur fond magenta. 

![022](../images/022.png)

Avec un clic droit sur une ligne, il est possible de copier sa configuration sur les lignes sélectionnées. 

![023](../images/023.png)

Afin de contrôler les données avant validation, il est possible d'afficher uniquement les lignes modifiées. 

![024](../images/024.png)

Après avoir cliqué sur le bouton Valider, les données sont mises à jour et le fond des cellules modifiées est effacé.

![025](../images/025.png)

Noter qu'un clic droit sur une ligne permet de lancer directement la configuration avancée de commande de Jeedom.

## Données modifiables

L'ensemble des données de paramètrage de l'historique standard de Jeedom et celles spécifiques au plugin archiplus peuvent être modifiées directement à partir de Monitor.

Ci-dessous sont détaillées les options spécifiques à archiplus dans leur ordre d'apparition:

### KLV (Keep Last Value): 

Permet de toujours conserver au moins un enregistrement dans l'historique. Voir la FAQ suivante pour comprendre l'utilisation de cette option [Keep Last Value](#keep-last-value).

### Uniq 

Permet de supprimer les valeurs consécutives identiques dans historyArch. Voir la FAQ suivante pour  comprendre l'utilisation de cette option [Uniq](#uniq-1).

### Délai

Il s'agit du délai à partir duquel on transfert les enregistrements de history vers historyArch. Dans Jeedom, ce paramètre est le même pour toutes les commandes. Avec archiplus, ce délai peut-être spécifié par commande.

### Cadrage 

Permet de fixer le moment jusqu'auquel on purge les données historique et aussi celui du transfert des données de history vers historyArch sur une limite de jour, heure ou minute. Voir la FAQ suivante pour comprendre l'utilisation de cette option [Délai et Cadrage](#délai-et-cadrage).

### Pond

Permet de faire une moyenne pondérée en tenant compte du temps et non une moyenne des valeurs enregistrées sur la période. Voir la FAQ suivante pour comprendre l'utilisation de cette option [Lissage et pondération](#lissage-et-pondération).

### Pack

Définit selon quel intervalle les données vont être regroupées lors du lissage. Dans l'archivage standard de Jeedom, ce paramètre est le même pour toutes les commandes et est un multiple d'heures. Avec archiplus, on peut préciser l'intervalle pour chaque commande et aussi exprimer la valeur en minutes (entrer le nombre de minutes suivi de la lettre m).  Voir la FAQ suivante pour comprendre l'utilisation de cette option [Pack](#pack-1).

### Arrondi

Dans Jeedom, on peut préciser l'arrondi pour chaque commande. Le plugin permet en plus de préciser un arrondi différent lors du lissage des données dans historyArch. Voir la FAQ suivante pour comprendre l'utilisation de cette option [Arrondi](#arrondi-1).

## Fonctions accessibles via le menu contextuel

![026](../images/026.png)

En faisant un clic droit n'importe où sur une ligne de la table, on fait apparaître le menu contextuel de la commande. En plus des actions déjà vues, celui-ci permet:

* d'afficher l'historique sous forme de graphique  (appel de la fonction standard de Jeedom)
* d'afficher les données stockées dans les tables history et historyArch
* de purger l'historique jusqu'à une date donnée
* d'exporter les données historique au format CSV (appel de la fonction standard de Jeedom)
* de mettre à jour les statistiques pour la ligne concernée
* de lancer l'archivage uniquement pour la commande concernée
* de copier les données de historyArch vers history:  Voir la FAQ suivante pour comprendre l'utilisation de cette action  [historyArch vers history](#copier-les-données-de-historyarch-vers-history)
* de copier l'historique de la commande sélectionnée vers une autre commande

# Données historiques

## Accès

![027](../images/027.png)

L'accès aux données dans les tables history et historyArch se fait via:

* le menu contextuel de Monitor (voir plus haut)
* la sélection de une ou plusieurs lignes suivi de l'appui sur le bouton Data.

![028](../images/028.png)

Les données sont présentées dans une fenêtre modale triées par datetime décroissant.

## Modification 

![029](../images/029.png)

Il arrive parfois que l'on ait des valeurs abérantes, ici suite à la maintenance de la chaudière.

![030](../images/030.png)

Le menu contextuel permet de modifier ou supprimer la valeur concernée. 

![031](../images/031.png)

Après correction, l'affichage de l'historique est alors bien plus significatif.

## Suppression

![032](../images/032.png)

Il est également possible de supprimer plusieurs données historique en les sélectionnant et cliquant sur le bouton supprimer.

## Export

![033](../images/033.png)

Le bouton Export permet d'exporter les données.

Noter que celles-ci peuvent être retravaillées dans Excel afin d'être importées via le module Import.

# Le module Import

# Le module Restore

# FAQ

## Keep Last Value

## Uniq

## Délai et Cadrage

## Lissage et pondération

## Pack

## Arrondi

## copier les données de historyArch vers history

## Lancer l'archivage d'une commande à partir d'un scénario