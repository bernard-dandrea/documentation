<!--  
  Last Modified : 2026/02/13 13:58:18
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
- [Accès aux modules](#accès-aux-modules)
  - [Les boutons de commandes](#les-boutons-de-commandes)
  - [La colonne de sélection des lignes](#la-colonne-de-sélection-des-lignes)
  - [Les en-têtess de colonne](#les-en-têtess-de-colonne)
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
  - [Copier les données de historyArch vers history](#copier-les-données-de-historyarch-vers-history)
  - [Utiliser archiplus en PHP](#utiliser-archiplus-en-php)
- [Traduction](#traduction)
- [Avis](#avis)



La fonction principale du plugin est de fournir un ensemble complet d'outils permettant:

*   **de gérer les paramètres d'archivage des commandes de type INFO**
*   **de visualiser les volumes de données et de détecter les anomalies**
*   **d'insérer facilement des données historiques à partir de fichiers de type Excel**
*   **de récupérer les historiques à partir des archives Jeedom**
*   **d'étendre les options d'archivage standard de Jeedom**

L'activation optionnelle de l'archivage intégré au plugin permet d'étendre très significativement les fonctions d'archivage proposées par Jeedom.

# La gestion des historiques dans Jeedom

## Fonctionnement

L'historique dans Jeedom a peu évolué depuis les premières versions et se base sur 2 tables:

* la table history qui reçoit les mises à jour des valeurs des commandes de type INFO pour lesquelles l'historique est activé
* la table historyArch qui reçoit lors de chaque archivage (habituellement chaque jour à 5h00) les valeurs de history consolidées ou non suivant le paramétrage défini pour la commande.

La structure des deux tables est identique et très simple: une valeur est enregistrée par commande Id et datetime (géré à la seconde). 

L'historique peut être affiché dans l'interface Jeedom sous forme de graphe.

La documentation officielle concernant la gestion des historiques dans Jeedom se trouve [ici](https://doc.jeedom.com/fr_FR/core/4.5/history).

## Volume des historiques

L'utilisateur de Jeedom commencera à s'intéresser à l'historique lorsqu'il constatera une base de données qui grossit de façon exagérée, des temps d'affichage de l'historique qui deviennent très longs, une taille de sauvegarde qui ne cesse de croître.

Le lien suivant permet d'accéder à un tutoriel qui explique comment créer un scénario qui listera les volumes des tables les plus volumineuses et les commandes INFO avec les plus gros historiques [Tuto - Analyser les archives](https://community.jeedom.com/t/tuto-analyser-les-archives-pour-detecter-des-pbs-lenteurs-espaces-disques/104384).

Plus simplement, vous pouvez voir les volumes des tables en interrogeant directement la base de données (menu Réglages / Système / Configuration puis onglet OS / DB (le dernier) puis bouton "Administration base de données" (bouton rouge le plus bas) puis sur la gauche interrogation "taille").

Dans une installation standard, il faut commencer à s'interroger lorsque le volume global des archives dépasse le million d'enregistrements ou qu'une commande info à plus de 10 000 enregistrements. Dans ce cas, il est nécessaire d'analyser les commandes concernées et de jouer sur les différents paramètres de l'historisation et de l'archivage afin de réduire ce volume. Si ce n'est pas possible, il faudra peut-être se tourner vers d'autres méthodes d'archivage, par exemple influxDB qui peut s'interfacer en standard avec Jeedom.

Le plugin archiplus donne immédiatement les volumes de history et historyArch et permet de cibler facilement les problèmes et d'y apporter des solutions.

## Les limites de l'archivage dans Jeedom

Bien que dans de nombreuses installations, le fonctionnement standard sera suffisant, on peut relever les limitations suivantes:

* Difficulté à visualiser et modifier les paramètres d'archivage: le seul outil disponible (menu Analyse / Historique puis Configuration) est très lent, peu pratique et présente peu de champs à configurer
* Difficulté pour visualiser les volumes d'historique par commande et repérer les volumes anormaux: il faut passer par des requêtes SQL et des processus peu pratiques 
* Paramètres pour le regroupement de données dans historyArch défini de façon globale et non personnalisable par commande
* Pas de visibilité concernant le processus d'archivage (pas de log)
* Archivage global: pas de possibilité de lancer l'archivage pour une commande spécifique
* Lissage par moyenne approximatif
* Outils basiques pour exporter/importer les données (plugin dataexport). Rien n'est proposé pour restaurer les données d'historique contenues dans les sauvegardes.

## Les PLUS du plugin archiplus

Le plugin archiplus permet de visualiser dans un tableau les commandes de type INFO avec l'ensemble des paramètres relatifs à l'archivage. Le nombre d'enregistrements dans history et historyArch est également présenté ce qui permet de détecter très facilement les volumes excessifs. Le plugin utilise la librairie javascript Tabulator qui est extrêmement performante et permet un accès très facile aux fonctions du plugin.

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
* Extraction de l'historique sous plusieurs formats (xlsx, CSV, JSON, HTML) d'une ou plusieurs commandes depuis Jeedom ou une sauvegarde standard Jeedom

De plus, le processus d'archivage du plugin peut être activé en remplacement de la fonction archive native offerte par Jeedom. Celui-ci permet:

* de lancer l'archivage pour une commande donnée
* d'enregistrer dans la log archiplus l'ensemble des opérations effectuées et les paramètres pris en compte pour chaque commande
* de personnaliser la période de calcul (pour min, max, moyenne), le délai avant archivage et la taille de paquet pour chaque commande
* de caler la date de purge sur un jour, une heure ou une minute
* de lancer l'archivage pour une commande depuis un scénario (en code PHP)
* d'ajouter des options non prévues dans Jeedom (voir les explications plus loin dans la documentation)
  * Keep Last Value : conserver toujours au moins une valeur dans l'historique
  * Uniq : éliminer les valeurs consécutives identiques dans historyArch
  * Pond : dans le lissage par moyenne, calculer la valeur pondérée sur la durée de l'intervalle (et non la moyenne des valeurs)

le plugin archiplus a été développé sous Debian 12 et n'utilise pas jQuery (de même que les bibliothèques 3rd party utilisées). Il respecte les standards de développement de Jeedom. Le code de la classe archiplus est très structuré et largement documenté : l'auteur du plugin étudiera toutes les propositions de correction ou d'amélioration.

Jeedom n'ayant pas de plan d'évolution de la gestion de l'historique, le plugin ne devrait pas nécessiter de refonte dans un avenir proche. 

## Avertissement

Le plugin et son processus spécifique d'archivage ont été testés très rigoureusement mais ne sont cependant pas à l'abri d'anomalies. Dans ce cas, l'équipe Jeedom n'est évidemment pas tenue d'apporter son support. Les demandes d'analyses et de correction devront être adressées impérativement à l'auteur du plugin via la demande d'assistance standard. 

L'activation du plugin et en particulier du processus d'archivage impliquent donc une pleine acceptation de cette situation.

# Plugin archiplus

## Installer le Plugin archiplus

Aller dans le Market Jeedom, trouver le plugin archiplus et installer la version **stable**. Puis **Activer le plugin**.

![001](../images/001.png)

Le plugin est accessible via le menu.

## Configurer le plugin

Dans la configuration, vous pouvez paramétrer les paramètres habituels des plugins et les valeurs par défaut du plugin.

![003](../images/003.png)

Pour avoir un maximum d'informations sur le processus d'archivage du plugin et les actions réalisées, il est conseillé de mettre les logs en mode Debug.

Noter que les demandes de support devront se faire via le bouton **Assistance**.

![002](../images/002.png)

Dans la section configuration, vous pouvez:

* Activer l'archivage spécifique (désactivé par défaut)
* Indiquer si les enregistrements dans history et historyArch doivent être supprimés dans le cas où la commande concernée n'existe pas
* Définir le format pour les exports
* Définir le cadrage par défaut pour les dates de purge et de fin d'archivage

L'activation de l'archivage spécifique crée un nouveau cron dans le moteur de tâches et désactive l'archivage standard. La désactivation de l'archivage spécifique effectue l'opération inverse.

Si vous voulez tester le processus d'archivage du plugin, vous pouvez l'activer temporairement, faire des tests d'archivage sur des commandes individuelles puis désactiver l'archivage du plugin. Le processus d'archivage de Jeedom se lançant habituellement à 5 heures du matin, il n'y aura pas d'impact sur les commandes non testées.

## Les modules du plugin

![004](../images/004.png)

A partir du menu Plugins / Monitoring / archiplus, vous avez accès à la totalité des fonctions du plugin

* Configuration du plugin (voir ci-dessus)
* Accès aux paramètres globaux du paramétrage de l'archivage
* Monitoring: visualiser et modifier le paramétrage et  réaliser les principales opérations concernant l'archivage
* Import: importer des données historiques à partir d'un fichier de type Excel
* Restore: extraire les données historiques à partir d'une archive standard Jeedom

La visualisation des données historiques est accessible à partir du module Monitoring et Restore.

# Accès aux modules

Les modules sont lancés à partir de la configuration du plugin.

![005](../images/005.png)

La base de l'interface est un tableau Tabulator rempli avec les données pertinentes.

Par exemple, avec le module Monitor, un tableau est affiché avec les commandes de type INFO ayant la fonction historique activée.

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
* d'exporter les données affichées dans le tableau (uniquement les données filtrées)
* de revenir aux différents modules proposés par archiplus

![019](../images/019.png)

Le bouton standard "Aide sur la page en cours" permet d'accéder à la documentation du plugin.

## La colonne de sélection des lignes

![007](../images/007.png)

La première colonne permet de sélectionner les lignes sur lesquelles on souhaite agir.

En cliquant sur l'en-têtes de colonne, on sélectionne toutes les lignes affichées du tableau.

On peut sélectionner chaque ligne individuellement en cliquant sur la case à cocher ou sur n'importe quel endroit de la ligne.

On peut sélectionner aussi une suite de lignes en cliquant sur la première à sélectionner puis en cliquant sur la dernière en maintenant la touche MAJ enfoncée.

## Les en-têtess de colonne

![008](../images/008.png)

Les en-têtess de colonne décrivent le contenu des cellules situées dans la colonne.

Elles permettent :

* d'obtenir une information complémentaire via une infobulle en laissant la souris une seconde sur le champ
* de trier les lignes selon la valeur du champ en cliquant sur le libellé de la colonne (noter que le bouton "Tri initial" permet d'annuler tous les tris effectués)
* de filtrer les lignes affichées en entrant un critère de sélection dans le champ situé sous le nom de la colonne (noter que le bouton "Reset" permet d'annuler toutes les sélections).

Dans le cas du module Monitor, un regroupement des colonnes permet de sélectionner uniquement certains types d'information.

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

Le nombre d'enregistrements dans history et historyArch correspond généralement à celui du dernier archivage (on peut voir la date de mise à jour en laissant la souris sur un des compteurs). En cliquant sur l'en-têtes de colonne #All, on peut voir immédiatement les commandes avec le plus gros historique.

![015](../images/015.png)

En cliquant sur le bouton ci-dessus, on peut relancer un calcul ce qui prendra quelques secondes.

![017](../images/017.png)

Les totaux en bas du tableau vous permettent de connaitre immédiatement la taille de votre historique.

## Visualisation

![018](../images/018.png)

Les boutons de visualisation permettent de sélectionner les données affichées

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

Avec un clic droit sur une ligne, il est possible de copier sa configuration ou l'un de ses paramètres sur les lignes sélectionnées. 

![023](../images/023.png)

Afin de contrôler les données avant validation, il est possible d'afficher uniquement les lignes modifiées. 

![024](../images/024.png)

Après avoir cliqué sur le bouton Valider, les données sont mises à jour et le fond des cellules modifiées est effacé.

![025](../images/025.png)

Noter qu'un clic droit sur une ligne permet de lancer directement la configuration avancée de commande de Jeedom.

## Données modifiables

L'ensemble des données de paramètrage de l'historique standard de Jeedom et celles spécifiques au plugin archiplus peuvent être modifiées directement à partir de Monitor.

Ci-dessous sont détaillées les options spécifiques à archiplus:

### KLV (Keep Last Value): 

Permet de toujours conserver au moins un enregistrement dans l'historique. Voir la FAQ suivante pour comprendre l'utilisation de cette option [Keep Last Value](#keep-last-value).

### Uniq 

Permet de supprimer les valeurs consécutives identiques dans historyArch. Voir la FAQ suivante pour  comprendre l'utilisation de cette option [Uniq](#uniq-1).

### Délai

Il s'agit du délai à partir duquel on transfert les enregistrements de history vers historyArch. En standard dans Jeedom, ce paramètre est le même pour toutes les commandes. Avec archiplus, ce délai peut être spécifié par commande.

### Cadrage 

Permet de fixer le moment jusqu'auquel on purge les données historiques et aussi celui du transfert des données de history vers historyArch sur une limite de jour, heure ou minute. Voir la FAQ suivante pour comprendre l'utilisation de cette option [Délai et Cadrage](#délai-et-cadrage).

### Pond

Permet de faire une moyenne pondérée en tenant compte du temps et non une moyenne des valeurs enregistrées sur la période. Voir la FAQ suivante pour comprendre l'utilisation de cette option [Lissage et pondération](#lissage-et-pondération).

### Pack

Définit selon quel intervalle les données vont être regroupées lors du lissage. Dans l'archivage standard de Jeedom, ce paramètre est le même pour toutes les commandes et est un multiple d'heures. Avec archiplus, on peut préciser l'intervalle pour chaque commande et aussi exprimer la valeur en minutes (entrer le nombre de minutes suivi de la lettre m).  Voir la FAQ suivante pour comprendre l'utilisation de cette option [Pack](#pack-1).

### Arrondi

En standard dans Jeedom, on peut préciser l'arrondi pour chaque commande. Le plugin permet en plus de préciser un arrondi différent lors du lissage des données dans historyArch. Voir la FAQ suivante pour comprendre l'utilisation de cette option [Arrondi](#arrondi-1).

## Fonctions accessibles via le menu contextuel

![026](../images/026.png)

En faisant un clic droit n'importe où sur une ligne du tableau, on fait apparaître le menu contextuel de la commande. En plus des actions déjà vues, celui-ci permet:

* d'afficher l'historique sous forme de graphique  (appel de la fonction standard de Jeedom)
* d'afficher les données stockées dans les tables history et historyArch
* de purger l'historique jusqu'à une date donnée
* d'exporter les données historiques au format CSV (appel de la fonction standard de Jeedom)
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

Il arrive parfois que l'on ait des valeurs aberrantes, ici suite à la maintenance de la chaudière.

![030](../images/030.png)

Le menu contextuel permet de modifier ou supprimer la valeur concernée. 

![031](../images/031.png)

Après correction, l'affichage de l'historique est alors bien plus significatif.

## Suppression

![032](../images/032.png)

Il est également possible de supprimer plusieurs données historiques en les sélectionnant et cliquant sur le bouton supprimer.

## Export

![033](../images/033.png)

Le bouton Export permet d'exporter les données.

Noter que celles-ci peuvent être retravaillées dans Excel afin d'être importées via le module Import.

# Le module Import

Le module Import permet d'importer des données historiques dans une ou plusieurs commandes de type INFO.

![035](../images/035.png)

Le fichier à importer doit être de type Excel ou CSV et doit comporter au moins les 3 colonnnes suivantes (les autres seront ignorées):

* id : ID de la commande
* datetime: datetime de la donnée historique sous le format AAAA-MM-JJ HH:MM:SS (le format datetime interne à Excel est également supporté)
* value: valeur à importer

Noter que les données extraites des modules Monitor ou Restore sont au bon format.

![034](../images/034.png)

La première action à effectuer est de sélectionner le fichier contenant les données.

![036](../images/036.png)

Après chargement, les données historiques du fichier sont chargées. 

Les données de la commande INFO sont extraites de Jeedom.

Un contrôle est effectué et les données en erreur sont détectées.

![037](../images/037.png)

Il est possible d'affecter les lignes chargées à une autre commande en sélectionnant la ou les lignes concernées et cliquant sur le bouton "Changer Commande".

![038](../images/038.png)

Pour importer les données historiques dans Jeedom, il faut sélectionner la ou les lignes concernées (ici filtre sur une plage de dates) et cliquer sur le bouton "Importer". Les lignes en erreur sont ignorées.

![039](../images/039.png)

Noter que l'import est réalisé par la méthode standard cmd::addHistoryValue. Aussi ce sont les contrôles et traitements standard de Jeedom qui sont effectués. Les nouvelles entrées se retrouvent dans la table history.

# Le module Restore

Le module Restore permet d'extraire les données historiques depuis une archive standard Jeedom et de les exporter afin de pouvoir les importer avec le module Import.

Tous les traitements s'effectuent en local sur le navigateur WEB. L'ensemble des commandes et données historiques sont chargées dans la mémoire du navigateur. Le programme a été testé avec 1.5 million de lignes dans history plus historyArch. Le nombre maximum de données chargées dépend de la RAM allouée au navigateur et ne peut pas être connue à priori. Il devrait cependant être capable de charger la plupart des sauvegardes où l'historique n'a pas explosé.

![040](../images/040.png)

La première étape est de rapatrier la sauvegarde en local sur l'ordinateur. Voir la documentation suivante pour la gestion des sauvegardes Jeedom [ici](https://doc.jeedom.com/fr_FR/core/4.5/backup).

![041](../images/041.png)

Lancez le module Restore et sélectionnez l'archive que vous souhaitez utiliser.

![042](../images/042.png)

Après quelques secondes, les commandes avec un historique sont affichées.

![043](../images/043.png)

Vous pouvez sélectionner les commandes qui vous intéressent et lancer l'export.

![044](../images/044.png)

Vous pouvez également afficher les données historiques concernées et sélectionner celles à exporter.

![045](../images/045.png)

Dans les 2 cas, vous retrouvez un export que vous pouvez utiliser pour réaliser un import avec le module Import.

# FAQ

## Keep Last Value

Dans certains cas, il est nécessaire de disposer de la dernière valeur de la commande INFO.

![046](../images/046.png)

Prenons le cas d'une chaudière dont on relève périodiquement le compteur de gaz affecté au chauffage. 

![047](../images/047.png)

Un scenario exécuté chaque heure permet de calculer la consommation horaire en faisant la différence entre la valeur dans l'historique au début et la fin de l'heure. Pour ce faire, un historique d'une journée est suffisant.

Cependant, quand la saison de chauffe est terminée, l'historique du compteur de chauffage a disparu et il n'est plus disponible pour calculer la première consommation horaire lors de la première chauffe de la saison suivante.

L'activation de l'option Keep Last Value permet de pallier ce problème sans devoir recourir à des artifices de programmation ou garder un historique sur une année.

## Uniq

Jeedom permet d'éviter les doublons dans la table history avec l'option "Répéter les valeurs identiques" qui est désactivée par défaut.

Il y a cependant plusieurs situations dans lesquelles les valeurs consécutives identiques ne sont pas ignorées:

  * si le sous-type de la commande est Binaire ou Autre
  * si la mise à jour est effectuée avec la méthode cmd::event et non eqLogic::checkAndUpdateCmd. De nombreux plugins fonctionnent encore avec la méthode cmd::event qui est plus ancienne et de ce fait n'éliminent pas les doublons.

Lors de l'archivage, s'il n'y a pas de lissage, les données de history sont transférées directement dans historyArch et les doublons sont donc copiés.

L'activation de l'option Uniq permet de supprimer les doublons dans historyArch lors de l'archivage spécifique de archiplus.

## Délai et Cadrage

En standard, le moment à partir duquel on supprime les données dans history et historyArch est défini par le paramètre "Purger historique" exprimé en heures. Une valeur par défaut est définie dans la configuration globale de Jeedom.

Ainsi, avec une purge définie à 7 jours, si l'archivage est lancé le 20/01/2025 à 05:11:21, les enregistrements history et historyArch seront supprimés jusqu'au 13/01/2025 à 05:11:21. 

Le paramètre Cadrage spécifique à archiplus permet de fixer plus précisément le moment de la purge. Ainsi, dans l'exemple ci-dessus, le moment de la purge sera:

* le 13/01/2025 à 05:11:21 si aucun cadrage n'est défini
* le 13/01/2025 à 05:11:00 avec un cadrage sur la dernière minute
* le 13/01/2025 à 05:00:00 avec un cadrage sur la dernière heure
* le 13/01/2025 à 00:00:00 avec un cadrage sur le dernier jour

Le "Délai avant archivage" (en heure) permet de déterminer à partir de quel moment les enregistrements de history sont transférés vers historyArch (avec ou sans consolidation). En standard, il est défini de façon globale et est donc identique pour toutes les commandes. 

L'archivage spécifique de archiplus permet de définir un délai spécifique pour chaque commande INFO et d'utiliser le cadrage vu ci-dessus. Ainsi avec un délai de 2 heures, le moment de transfert de history vers historyArch sera:

* le 20/01/2025 à 03:11:21 si aucun cadrage n'est défini
* le 20/01/2025 à 03:11:00 avec un cadrage sur la dernière minute
* le 20/01/2025 à 03:00:00 avec un cadrage sur la dernière heure
* le 20/01/2025 à 00:00:00 avec un cadrage sur le dernier jour, ici quelle que soit l'heure dans la journée où l'archivage est lancé

Noter que le moment de la purge ne peut pas être postérieur au moment du transfert de history vers historyArch et sera donc ajusté automatiquement.

![048](../images/048.png)

On peut jouer sur ces paramètres si on souhaite par exemple un historique détaillé sur une courte période (ici 36 heures maxi) sans besoin d'un archivage consolidé. On évite ainsi le transfert de history vers historyArch qui n'apporte rien.

## Lissage et pondération

Le lissage intervient lors de la copie des données de history vers historyArch. Le processus d'archivage considère toutes les données de history selon l'intervalle défini (par défaut une heure) et conserve une seule valeur calculée selon le mode de lissage. Trois modes sont possibles:

* minimum: la plus petite des valeurs contenues dans l'intervalle
* maximum: la plus grande des valeurs contenues dans l'intervalle
* moyenne: la moyenne des valeurs contenues dans l'intervalle

Il faut noter que l'archivage standard ne tient pas compte de la valeur de la commande au début de l'intervalle et fait une moyenne des valeurs présentes dans l'intervalle, ce qui peut fausser significativement le résultat. 

Le processus spécifique d'archivage de archiplus propose une option Pond qui permet de corriger ce phénomène et de calculer un résultat exact sur l'intervalle considéré.

Ceci est illustré dans l'exemple ci-dessous.

![050](../images/050.png)

Considérons deux commandes avec les configurations suivantes.

![049](../images/049.png)

Elles ont les mêmes entrées dans la table history

![051](../images/051.png)

Après archivage, les entrées dans historyArch sont différentes

![052](../images/052.png)

Avec l'archivage standard, c'est la moyenne des valeurs sur la période qui est prise en compte.

Avec l'archivage spécifique de archiplus, c'est la moyenne pondérée sur la période qui est calculée. Noter également qu'une entrée est ajoutée dans history afin de connaitre lors du prochain archivage la valeur de départ de la période (sans cette entrée, on récupérerait la moyenne de la dernière période ce qui fausserait le calcul).

## Pack

En standard dans Jeedom, l'intervalle (appelé paquet dans Jeedom) sur lequel on peut faire le lissage est défini en heure et est le même pour toutes les commandes INFO.

Pourtant, on peut souhaiter un intervalle plus faible et pouvoir le spécifier pour une commande INFO particulière.

![055](../images/055.png)

![054](../images/054.png)

Pour une batterie, conserver une valeur par jour sur une longue durée peut être suffisant.

![057](../images/057.png)

![056](../images/056.png)

Pour un thermomètre, une valeur tous les quarts d'heure peut être plus utile qu'une valeur par heure.

Pour indiquer des minutes, entrer dans la zone Pack le nombre de minutes souhaitées suivi de m, par exemple 15m.

## Arrondi

En standard, Jeedom permet de préciser le nombre de décimales d'une valeur de commande INFO. 

Pour certaines commandes, il peut être intéressant d'avoir une valeur précise sur une courte période puis moins précise ultérieurement. Par exemple, la connaissance d'une température extérieure précise est intéressante sur le moment mais n'est plus nécessaire après plusieurs jours.

![064](../images/064.png)

La commande ci-dessus est configurée pour conserver un historique avec 1 décimale pendant une semaine et un historique sans décimale au delà.

![065](../images/065.png)

Avant archivage, on a 7 entrées dans l'historique entre 7.7 °C et 8.3 °C.

![066](../images/066.png)

Après archivage, les 7 entrées sont arrondies à 8 °C et l'option Uniq permet d'en conserver une seule.

## Copier les données de historyArch vers history

Après avoir installé archiplus, vous aurez peut-être envie de consolider des historiques existants.

![060](../images/060.png)

![058](../images/058.png)

Par exemple, pour cette commande, un historique par intervalle de 10 minutes serait suffisant et réduirait fortement le nombre d'enregistrements dans historyArch.

![059](../images/059.png)

Après avoir modifié le paramétrage, on peut transférer les entrées de historyArch vers history.

![061](../images/061.png)

Une fois cette mise à jour effectuée, on peut lancer un archivage sur cette commande INFO (ou attendre que l'archivage soit lancé automatiquement la nuit).

![063](../images/063.png)

![062](../images/062.png)

Après archivage, le nombre d'enregistrements est fortement réduit et l'affichage de l'historique est beaucoup plus rapide.

## Utiliser archiplus en PHP

Il est possible d'appeler les fonctions d'archivage et de traitement des historiques de archiplus directement dans un scénario ou une fonction PHP.

![053](../images/053.png)

Ici, les fonctions archiplus sont utilisées dans un scénario pour charger l'historique d'une commande et lancer l'archivage sur celle-ci.

`require_once dirname(__FILE__) . '../../../plugins/archiplus/core/class/archiplus.class.php';`

Cette ligne permet de charger le code des fonctions archiplus. Il peut être nécessaire d'adapter le chemin pour pointer sur la classe du plugin.

Les fonctions utilisables peuvent être trouvées dans le code de la classe archiplus. Les principales sont:

* `archive($_cmd_id = '')` : lance l'archivage pour une commande ou toutes les commandes si il n'y a pas de paramètre
* `History_purge($_cmd_id, $_date='')` : supprime l'historique pour une commande jusqu'à un datetime déterminé (ou tout l'historique si pas de deuxième paramètre)
* `addHistoryValue($_cmd_id, $_datetime, $_value)` : ajoute une entrée (ou remplace l'entrée existante) dans l'historique en appelant la fonction standard de Jeedom
* `historyArch2history($_cmd_id, $_date_start = '', $_date_end = '')` : transfert les enregistrements de historyArch vers history
  
Il est évidemment possible d'utiliser les fonctions disponibles dans la classe history.class.php après avoir fait la déclaration `require_once` nécessaire.

# Traduction

L'interface et les messages envoyés dans les logs sont traduits dans les 5 langues supportées par Jeedom (merci à @mips pour le développement ga-translation). Si des erreurs de traduction sont constatées, vous pouvez ouvrir une demande de support et si possible joindre le fichier de traduction corrigé (situé dans le répertoire core/i18n du plugin).

La documentation du plugin est traduite uniquement en anglais (les autres langues renvoient vers la traduction anglaise). La traduction est faite via un traducteur automatique. Par contre, les captures d'écrans ne sont pas traduites. 

# Avis

![archiplus_avis](../images/archiplus_avis.png)

Si vous appréciez ce plugin, merci de laisser une évaluation et un commentaire sur le Jeedom market, ça fait toujours plaisir: <https://jeedom.com/market/index.php?v=d&p=market_display&id=xxxx#>