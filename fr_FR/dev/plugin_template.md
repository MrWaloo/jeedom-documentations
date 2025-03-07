# Documentation plugin template

Vous pouvez retrouver une base de plugin vide [ici](https://github.com/jeedom/plugin-template)

# Création plugin partie 1 : l’arborescence

Voici sa structure : tout d’abord un dossier du nom de votre plugin (son identifiant unique plus exactement) qui doit contenir les sous-dossiers suivants :

- ``3rdparty`` : Dossier contenant les bibliothèques externes utilisées dans le plugin (exemple pour le plugin SMS une bibliothèque pour la communication série en php).
- ``core`` : Dossier contenant tous les fichiers de fonctionnement interne.
  - ``class`` : Dossier contenant la classe du plugin.
  - ``php`` : Dossier pouvant contenir des fonctions ne devant pas forcément appartenir à une classe (souvent utilisé pour permettre l’inclusion de multiples classes ou fichiers de configuration en une fois).
  - ``config`` : Fichier de configuration du plugin.
  - ``ajax`` : Dossier contenant les fichiers cibles d’appels AJAX.
  - ``i18n`` : Dossier contenant les fichiers .json de traduction du plugin.
  - ``template`` : Dossier contenant les templates html pour des tuiles sépcifiques aux équipements du plugin, dans les sous-dossiers dashboard et mobile.
- ``desktop`` : Dossier contenant la vue "bureau" du plugin (en opposition avec la vue "mobile").
  - ``js`` : Dossier contenant tous les fichiers de type javascript pour l'interface du plugin.
  - ``php`` : Dossier contenant tous les fichiers de type php pour l'interface du plugin.
  - ``css`` : Si besoin, tous les fichiers css du plugin, y compris éventuellement des *fonts*.
  - ``modal`` : Dossier contenant le code des modals du plugin.
  - ``img`` : Dossier pour les images (png, jpg etc) nécessaires au plugin.
- ``plugin_info`` : Contient les fichiers permettant à Jeedom de qualifier le plugin, de faire son installation et sa configuration.
  - ``info.json`` : Fichier contenant les informations de base du plugin. Il est obligatoire sinon Jeedom ne verra pas le plugin. Il contient entre autre l’identifiant du plugin, la description, les instructions d’installation…​
  - ``install.php`` : Fichier contenant les méthodes d’installation et de désinstallation du plugin (si besoin).
  - ``configuration.php`` : Fichier contenant les paramètres du plugin à configurer indépendamment des équipements de celui-ci (exemple pour le module Zwave l’ip du Raspberry Pi ayant la carte Razberry)
- ``docs`` : Peut contenir la doc du plugin au format markdown, la racine et le fichier index.md. Toutes les images sont dans docs/images. La doc elle-même est dans un dossier en fonction de la langue (ex en francais : ``docs/fr\_FR``)
- ``ressources`` : Dossier pour d'éventuels démon et dépendances.
- ``data`` : Dossier utilisé pour des fichiers générés par le plugin propre au Jeedom de l’utilisateur.

Pour ce qui est de la convention de nommage des fichiers voici les impératifs :

- les fichiers de class php doivent obligatoirement se finir par ``.class.php``
- si ce n’est pas géré par un fichier d’inclusion, le nom du fichier doit être ``nom\_class.class.php``
- les fichiers servant uniquement de point d’entrée pour inclure de multiples fichiers doivent se finir par ``.inc.php``
- les fichiers de configuration doivent se finir par ``.config.php``

Voici les recommandations :

- les fichiers de type AJAX doivent se finir par ``.ajax.php``
- le nom de la première page de vue d’un plugin doit être le même que l’ID du plugin
- le nom du fichier JS (s’il y en a un) de la première page de vue du plugin doit être l’ID du plugin

# Création plugin partie 2 : dossier plugin info

## ``info.json``

Voir [ici](structure_info_json)

## ``install.php``

Fichier donnant les instructions d’installation d’un plugin.

Il est composé de la manière suivante :

La première partie commentée contient la licence (c’est mieux). Celle utilisée ici indique que le fichier appartient à Jeedom et qu’il est open source. Ensuite vient l’inclusion du core de Jeedom (cela permet d’accéder aux fonctions internes). Ensuite viennent les 3 fonctions :

- ``pluginid_install()`` : méthode permettant d’installer le plugin. Ici l’installation ajoute une tâche cron à Jeedom
- ``pluginid_update()`` : méthode permettant d’installer le plugin. Utilisée ici pour redémarrer la tache cron
- ``pluginid_remove()`` : méthode permettant de supprimer le plugin. Ici la fonction supprime la tâche cron de Jeedom lors de la désinstallation

Exemple :

````php
<?php
/* This file is part of Jeedom.
 *
 * Jeedom is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * Jeedom is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with Jeedom. If not, see <http://www.gnu.org/licenses/>.
 */
require_once dirname(__FILE__) . '/../../../core/php/core.inc.php';

function openzwave_install() {
    $cron = cron::byClassAndFunction('zwave', 'pull');
    if (!is_object($cron)) {
        $cron = new cron();
        $cron->setClass('zwave');
        $cron->setFunction('pull');
        $cron->setEnable(1);
        $cron->setDeamon(1);
        $cron->setSchedule('* * * * *');
        $cron->save();
    }
}

function openzwave_update() {
    $cron = cron::byClassAndFunction('zwave', 'pull');
    if (!is_object($cron)) {
        $cron = new cron();
        $cron->setClass('zwave');
        $cron->setFunction('pull');
        $cron->setEnable(1);
        $cron->setDeamon(1);
        $cron->setSchedule('* * * * *');
        $cron->save();
    }
    $cron->stop();
}

function openzwave_remove() {
    $cron = cron::byClassAndFunction('zwave', 'pull');
    if (is_object($cron)) {
        $cron->remove();
    }
}
?>
````

## ``configuration.php``

Fichier permettant de demander des informations de configuration à l’utilisateur :

Le fichier est constitué de :

- La licence comme précédemment
- L’inclusion du core de Jeedom
- La vérification que l’utilisateur est bien connecté (j’inclus le fichier 404 car ce fichier est un fichier de type vue)

Ensuite vient le paramètre demandé (il peut y en avoir plusieurs), c’est une syntaxe standard Bootstrap pour les formulaires, les seules particularités à respecter sont la classe (``configKey``) à mettre sur l’élément de paramètre ainsi que le "data-l1key" qui indique le nom du paramètre. Pour récupérer la valeur de celui-ci ailleurs dans le plugin il suffit de faire : ``config::byKey(NOM_PARAMETRE, PLUGIN_ID)``

Exemple :

````php
<?php
/* This file is part of Jeedom.
 *
  * Jeedom is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * Jeedom is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
 * GNU General Public License for more details.
 *
  * You should have received a copy of the GNU General Public License
 * along with Jeedom. If not, see <http://www.gnu.org/licenses/>.
  */

 require_once dirname(__FILE__) . '/../../../core/php/core.inc.php';
include_file('core', 'authentification', 'php');
if (!isConnect()) {
    include_file('desktop', '404', 'php');
    die();
 }
 ?>
 <form class="form-horizontal">
     <fieldset>
         <div class="form-group">
             <label class="col-lg-2 control-label">Zway IP</label>
             <div class="col-lg-2">
                 <input class="configKey form-control" data-l1key="zwaveAddr" />
             </div>
         </div>
         <div class="form-group">
             <label class="col-lg-4 control-label">Supprimer automatiquement les périphériques exclus</label>
             <div class="col-lg-4">
                 <input type="checkbox" class="configKey" data-l1key="autoRemoveExcludeDevice" />
             </div>
         </div>
         <div class="form-group">
             <label class="col-lg-4 control-label">J'utilise un serveur openzwave</label>
             <div class="col-lg-4">
                 <input type="checkbox" class="configKey" data-l1key="isOpenZwave" />
             </div>
         </div>
     </fieldset>
 </form>
 ````

# Création plugin partie 3 : dossier desktop

## PHP

Ce dossier contient la vue à proprement parler. Dedans on retrouve obligatoirement la page de configuration du plugin (celle qui apparaîtra quand l’utilisateur fera plugin ⇒ catégorie ⇒ votre plugin). Il est conseillé de nommer celle-ci avec l’id de votre plugin. Il peut aussi contenir le panel (page que l’utilisateur trouvera dans accueil → nom de votre plugin).

Tous les fichiers dans ce dossier doivent finir par ``.php`` et doivent obligatoirement commencer par :

````php
<?php
if (!isConnect('admin')) {
    throw new Exception('{{401 - Accès non autorisé}}');
 }
 sendVarToJS('eqType', 'mail');
 ?>
 ````

Une fois sur cette page vous aurez accès en php à toutes les fonctions du core de jeedom (voir [ici](https://www.jeedom.com/doc/documentation/code/) ) ainsi qu’à celles de tous les plugin installés donc le vôtre aussi.

Toutes ces pages étant des vues elles utilisent principalement la syntaxe HTML. Pour tout ce qui est présentation, Jeedom se base principalement sur bootstrap donc toute la [documentation](http://getbootstrap.com/) est applicable.

Pour simplifier la création de plugin vous pouvez inclure le script javascript de template pour les plugins dans votre page :

````php
<?php include_file('core', 'plugin.template', 'js'); ?>
````

A mettre tout en bas de votre page et utile uniquement sur la page de configuration de votre plugin. Ce script permet de réduire le javascript obligatoire à une seule fonction (voir partie sur les fichiers JS).

Dans votre page de configuration une syntaxe HTML a été mise en place pour vous simplifier la vie. Donc pour la plupart des plugins vous n’aurez à faire que du HTML pour stocker vos informations en base de données et vous en resservir du coté de votre classe.

La syntaxe est assez simple : votre élément (input, select…​) doit avoir la classe css eqLogicAttr (ou cmdAttr pour les commandes) et un attribut indiquant le nom de la propriété :

````html
<input type="text" class="eqLogicAttr form-control" data-l1key="name" placeholder="{{Nom de l'équipement mail}}"/>
````

Là, par exemple, lors du chargement des données jeedom mettra la valeur du nom de l’équipement dans l’input et lors de la sauvegarde récupérera celle-ci pour la remettre en base de données. Petite astuce : certaines propriétés sont en fait des chaînes JSON en BDD (cela permet d’avoir vraiment pas mal de liberté pour le plugin), dans ce cas il suffit de faire :

````html
<input class="eqLogicAttr form-control" data-l1key='configuration' data-l2key='fromName' />
````

Pour la liste des propriétés des équipements et des commandes c’est ici (pour voir les propriétés qui sont de JSON il suffit de regarder le getter ou le setter, si celui-ci prend 2 paramètres alors c’est du JSON).

Dernier point important sur la page de configuration : celle-ci peut contenir autant d’équipements et de commandes que nécessaire. Cependant il y a quelques règles à respecter :

Tous les éléments ayant la classe eqLogicAttr doivent être dans un élément ayant la classe css eqLogic. Idem pour les éléments de classe css cmdAttr qui doivent être dans un élément de classe cmd. Toutes les commandes d’un équipement doivent se trouver dans l’élément ayant la classe eqLogic correspondante.

## JS

Tous les fichiers JS doivent se trouver dans le dossier JS (facile !!!). Il est conseillé de le nommer du même ID que votre plugin (dans la partie configuration, pour le panel vous faîtes comme vous voulez). Ce fichier JS (celui de la configuration du plugin) doit au minimum contenir une méthode addCmdToTable qui prend en paramètre l’objet commande à ajouter. Voici un exemple simple :

````php
function addCmdToTable(_cmd) {
    if (!isset(_cmd)) {
        var _cmd = {configuration: {}};
     }
    var tr = '';
    tr += '<input class="cmdAttr form-control input-sm" data-l1key="id" style="display : none;">';
    tr += '<input class="cmdAttr form-control input-sm" data-l1key="name">';
    tr += '<input class="cmdAttr form-control input-sm" data-l1key="configuration" data-l2key="recipient">';
    tr += '<input class="cmdAttr form-control input-sm" data-l1key="type" value="action" style="display : none;">';
    tr += '<input class="cmdAttr form-control input-sm" data-l1key="subType" value="message" style="display : none;">';
    if (is_numeric(_cmd.id)) {
        tr += '<a class="btn btn-default btn-xs cmdAction" data-action="test"><i class="fa fa-rss"></i> {{Tester}}</a>';
    }
    tr += '<i class="fa fa-minus-circle pull-right cmdAction cursor" data-action="remove"></i></td>';
    
    $('#table_cmd tbody').append(tr);
    $('#table_cmd tbody tr:last').setValues(_cmd, '.cmdAttr');
}
````

Vous remarquerez qu’il y a une ligne par commande et que celle-ci a bien la classe css cmd. Vous pouvez aussi voir les éléments qui ont la classe cmdAttr.

Plusieurs points importants :

- cette fonction peut être appelée avec un objet vide (d’où les 3 premières lignes) lors de l’ajout d’une nouvelle commande
- la dernière ligne permet d’initialiser tous les champs une fois la ligne insérée

Dernier point : un exemple plus complet avec type et sous-type de commande :

````php
function addCmdToTable(_cmd) {
    if (!isset(_cmd)) {
        var _cmd = {};
    }
     if (!isset(_cmd.configuration)) {
        _cmd.configuration = {};
    }
     var selRequestType = '<select style="width : 90px;" class="cmdAttr form-control input-sm" data-l1key="configuration" data-l2key="requestType">';
     selRequestType += '<option value="script">{{Script}}</option>';
     selRequestType += '<option value="http">{{Http}}</option>';
     selRequestType += '</select>';
    var tr = '';
    tr += '<input class="cmdAttr form-control input-sm" data-l1key="name" style="width : 140px;">';
    tr += '<input class="cmdAttr form-control input-sm" data-l1key="id"  style="display : none;">';
    tr += '' + selRequestType;
    tr += '<div class="requestTypeConfig" data-type="http">';
    tr += '<input type="checkbox" class="cmdAttr" data-l1key="configuration" data-l2key="noSslCheck" />Ne pas vérifier SSL';
    tr += '</div>';
    tr += '';
    tr += '<span class="type" type="' + init(_cmd.type) + '">' + jeedom.cmd.availableType() + '</span>';
    tr += '<span class="subType" subType="' + init(_cmd.subType) + '"></span>';
    tr += '';
    tr += '<textarea style="height : 95px;" class="cmdAttr form-control input-sm" data-l1key="configuration" data-l2key="request"></textarea>';
    tr += '<a class="btn btn-default browseScriptFile cursor input-sm" style="margin-top : 5px;"><i class="fa fa-folder-open"></i> {{Parcourir}}</a> ';
    tr += '<a class="btn btn-default editScriptFile cursor input-sm" style="margin-top : 5px;"><i class="fa fa-edit"></i> {{Editer}}</a> ';
    tr += '<a class="btn btn-success newScriptFile cursor input-sm" style="margin-top : 5px;"><i class="fa fa-file-o"></i> {{Nouveau}}</a> ';
    tr += '<a class="btn btn-danger removeScriptFile cursor input-sm" style="margin-top : 5px;"><i class="fa fa-trash-o"></i> {{Supprimer}}</a> ';
    tr += '<a class="btn btn-warning bt_shareOnMarket cursor input-sm" style="margin-top : 5px;"><i class="fa fa-cloud-upload"></i> {{Partager}}</a> ';
    tr += '</div>';
    tr += '';
    tr += '<input class="cmdAttr form-control tooltips input-sm" data-l1key="unite"  style="width : 100px;" placeholder="{{Unité}}" title="{{Unité}}">';
    tr += '<input class="tooltips cmdAttr form-control input-sm" data-l1key="configuration" data-l2key="minValue" placeholder="{{Min}}" title="{{Min}}"> ';
    tr += '<input class="tooltips cmdAttr form-control input-sm" data-l1key="configuration" data-l2key="maxValue" placeholder="{{Max}}" title="{{Max}}">';
    tr += '';
    tr += '<span><input type="checkbox" class="cmdAttr" data-l1key="isHistorized" /> {{Historiser}}<br/></span>';
    tr += '';
    if (is_numeric(_cmd.id)) {
    tr += '<a class="btn btn-default btn-xs cmdAction" data-action="test"><i class="fa fa-rss"></i> {{Tester}}</a>';
    }
    tr += '<i class="fa fa-minus-circle pull-right cmdAction cursor" data-action="remove"></i></td>';
    tr += '';
    $('#table_cmd tbody').append(tr);
    $('#table_cmd tbody tr:last').setValues(_cmd, '.cmdAttr');

    if (isset(_cmd.configuration.requestType)) {
        $('#table_cmd tbody tr:last .cmdAttr[data-l1key=configuration][data-l2key=requestType]').value(init(_cmd.configuration.requestType));
        $('#table_cmd tbody tr:last .cmdAttr[data-l1key=configuration][data-l2key=requestType]').trigger('change');
    }

     if (isset(_cmd.type)) {
        $('#table_cmd tbody tr:last .cmdAttr[data-l1key=type]').value(init(_cmd.type));
    }
    jeedom.cmd.changeType($('#table_cmd tbody tr:last'), init(_cmd.subType));
    initTooltips();
}
````

Ici on peut remarquer :

- ``jeedom.cmd.availableType()`` va insérer un select avec la liste des types connus (action et info pour le moment)
- ``<span class="subType" subType="' + init(\_cmd.subType) + '"><\span>`` : l’endroit où le select de sous type doit être posé
- ``jeedom.cmd.changeType(\$('\#table\_cmd tbody tr:last'), init(\_cmd.subType))`` qui permet d’initialiser le sous type avec la bonne valeur

D’autres fonctions javascript peuvent être utilisées :

- ``printEqLogic`` qui prend en paramètre tout l’objet de l’équipement (utile en cas de traitement de données avant de les restituer). Elle est appelée lors de l’affichage des données de l’équipement.
- ``saveEqLogic`` qui prend en paramètre l’objet équipement qui va être sauvegardé en base de données (utile si vous devez faire du traitement avant sauvegarde). Dernière chose, pour les fichiers JS, voici comment les inclure de manière propre sur votre page php :

````php
<?php include_file('desktop', 'weather', 'js', 'weather'); ?>
````

Le premier argument donne le dossier dans lequel le trouver (attention c’est le dossier père du dossier JS), le deuxième le nom de votre javascript, le troisième indique à Jeedom que c’est un fichier JS et le dernier dans quel plugin il se trouve.

## CSS

Ce dossier contient vos fichiers CSS (il ne devrait pas être trop utilisé), voici comment les inclure sur votre page :

````php
<?php include_file('desktop', 'weather', 'css', 'weather'); ?>
````

Le premier argument donne le dossier dans lequel le trouver (attention c’est le dossier père du dossier CSS), le deuxième le nom de votre fichier css, le troisième indique à Jeedom que c’est un fichier CSS et le dernier dans quel plugin il se trouve.

## MODAL

Le dossier modal vous permet de stocker vos fichiers php destinés à afficher des modals. Voici comment les appeler à partir de votre page principale (ce code se met dans un fichier javascript) :

On peut voir :

````js
$('#md_modal').dialog({title: "{{Classe du périphérique}}"}).load('index.php?v=d&plugin=zwave&modal=show.class&id=' + $('.eqLogicAttr[data-l1key=id]').value()).dialog('open')
````

La première ligne permet de mettre un titre à votre modal.

La deuxième ligne charge votre modal et l’affichage. La syntaxe est assez simple : plugin, l’id de votre plugin, modal, le nom de votre modal sans le php et ensuite les paramètres que vous voulez lui passer

## API JS

Ce n’est pas un dossier mais dans les dernières versions de Jeedom celui-ci offre au développeur toute une api javascript (cela évite d’écrire des appels ajax dans tous les sens). J’essayerai de faire un article pour expliquer les différentes fonctionnalités mais vous pouvez déjà trouver le code ici.

Voilà pour les détails du dossier desktop. Je me doute qu’il n’est pas des plus complets (j’essayerai de le compléter en fonction des différentes demandes reçues) mais j’espère que grâce à lui vous pourrez commencer à faire des plugins pour Jeedom.

## Trucs et astuces

### Assitant cron

````js
$('body').delegate('.helpSelectCron','click',function() {
  var el = $(this).closest('.schedule').find('.scenarioAttr[data-l1key=schedule]')
  jeedom.getCronSelectModal({},function (result) {
    el.value(result.value)
  })
})
````

Quand on clique sur le bouton assistant, on récupère l’input dans lequel écrire puis on appelle l’assistant. Une fois la configuration finie dans l’assistant, le résultat est récuperé puis écrit dans l’input précédemment sélectionné.

# Création plugin partie 4 : dossier core

De loin le dossier le plus important de votre plugin, il peut comporter 4 sous dossiers.

Note : tout le long de cette partie l’id de votre plugin sera référencé par : plugin\_id

## PHP

Contient les fichiers PHP annexes, j’ai pris l’habitude de mettre par exemple un fichier d’inclusion si, bien sûr, vous avez plusieurs fichiers de class ou des 3rparty à inclure.

## Template

Qui peut contenir 2 sous-dossiers, dashboard et mobile, c’est un dossier que Jeedom scanne automatiquement à la recherche de widgets, donc si vous utilisez des widgets spécifiques c’est ici qu’il faut mettre leur fichier HTML

## i18n

C’est ici que votre traduction doit se trouver sous forme de fichier json (le mieux est de regarder par exemple le plugin [zwave](https://github.com/jeedom/plugin-openzwave) pour voir la forme du fichier)

## ajax

Ce dossier est pour tous vos fichiers ajax, voici un squelette de fichier ajax :

````php
<?php

/* This file is part of Jeedom.
 *
 * Jeedom is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * Jeedom is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with Jeedom. If not, see <http://www.gnu.org/licenses/>.
 */

try {
    require_once dirname(__FILE__) . '/../../../../core/php/core.inc.php';
    include_file('core', 'authentification', 'php');

    if (!isConnect('admin')) {
        throw new Exception(__('401 - Accès non autorisé', __FILE__));
    }

    if (init('action') == 'votre action') {
        ajax::success($result);
    }

    throw new Exception(__('Aucune méthode correspondante à : ', __FILE__) . init('action'));
    /*     * *********Catch exeption*************** */
} catch (Exception $e) {
    ajax::error(displayExeption($e), $e->getCode());
}
?>
````

## class

Dossier très important, c’est le moteur de votre plugin. C’est là que viennent les 2 classes obligatoires de votre plugin :

- ``plugin\_id``
- ``plugin\_idCmd``

La première devant hériter de la classe eqLogic et la seconde de cmd. Voici un template :

````php
<?php

/* This file is part of Jeedom.
 *
 * Jeedom is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * Jeedom is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with Jeedom. If not, see <http://www.gnu.org/licenses/>.
 */

/* * ***************************Includes********************************* */
require_once dirname(__FILE__) . '/../../../../core/php/core.inc.php';

class plugin_id extends eqLogic {

    /*     * *************************Attributs****************************** */


    /*     * ***********************Methode static*************************** */


    /*     * *********************Methode d'instance************************* */


    /*     * **********************Getteur Setteur*************************** */

}

class plugin_idCmd extends cmd {

    /*     * *************************Attributs****************************** */


    /*     * ***********************Methode static*************************** */


    /*     * *********************Methode d'instance************************* */


    /*     * **********************Getteur Setteur*************************** */

}

?>
````

Pour la définition des classes jeedom, je vous invite à consulter ce [site](https://doc.jeedom.com/dev/phpdoc/4.4/)

La seule méthode obligatoire est la méthode d’instance sur la classe cmd execute, voici un exemple avec le plugin S.A.R.A.H :

````php
public function execute($_options = array()) {
      if (!isset($_options['title']) && !isset($_options['message'])) {
          throw new Exception(__("Le titre ou le message ne peuvent être tous les deux vide", __FILE__));
      }
      $eqLogic = $this->getEqLogic();
      $message = '';
      if (isset($_options['title'])) {
          $message = $_options['title'] . '. ';
      }
      $message .= $_options['message'];
      $http = new com_http($eqLogic->getConfiguration('addrSrvTts') . '/?tts=' . urlencode($message));
      return $http->exec();
  }
  ````

Exemple assez simple mais complet, le principe est le suivant, si la commande est une action ou une info (mais pas en événement seulement et que son cache est dépassé) alors jeedom appelle cette méthode.

Dans notre exemple ici c’est une commande pour faire parler S.A.R.A.H, où le plugin récupère les paramètres dans \$\_options (attention c’est un tableau et ses attributs changent en fonction du sous-type de la commande : color pour un sous-type color, slider pour un sous-type slider, title et message pour un sous-type message et vide pour un sous-type other).

Voila pour la partie obligatoire, voila maintenant ce qui peut être utilisé à coté (avec exemple) :

### toHtml(\$\_version = 'dashboard')

Fonction utilisable dans la commande ou dans l’équipement, en fonction des besoins, voici un exemple pour l’équipement

````php
public function toHtml($_version = 'dashboard') {
    $replace = $this->preToHtml($_version);
    if (!is_array($replace)) {
        return $replace;
    }
    $version = jeedom::versionAlias($_version);
    $replace['#forecast#'] = '';
    if ($version != 'mobile' || $this->getConfiguration('fullMobileDisplay', 0) == 1) {
        $forcast_template = getTemplate('core', $version, 'forecast', 'weather');
        for ($i = 0; $i < 5; $i++) {
            $replaceDay = array();
            $replaceDay['#day#'] = date_fr(date('l', strtotime('+' . $i . ' days')));

            if ($i == 0) {
                $temperature_min = $this->getCmd(null, 'temperature_min');
            } else {
                $temperature_min = $this->getCmd(null, 'temperature_' . $i . '_min');
            }
            $replaceDay['#low_temperature#'] = is_object($temperature_min) ? $temperature_min->execCmd() : '';

            if ($i == 0) {
                $temperature_max = $this->getCmd(null, 'temperature_max');
            } else {
                $temperature_max = $this->getCmd(null, 'temperature_' . $i . '_max');
            }
            $replaceDay['#hight_temperature#'] = is_object($temperature_max) ? $temperature_max->execCmd() : '';
            $replaceDay['#tempid#'] = is_object($temperature_max) ? $temperature_max->getId() : '';

            if ($i == 0) {
                $condition = $this->getCmd(null, 'condition');
            } else {
                $condition = $this->getCmd(null, 'condition_' . $i);
            }
            $replaceDay['#icone#'] = is_object($condition) ? self::getIconFromCondition($condition->execCmd()) : '';
            $replaceDay['#conditionid#'] = is_object($condition) ? $condition->getId() : '';
            $replace['#forecast#'] .= template_replace($replaceDay, $forcast_template);
        }
    }
    $temperature = $this->getCmd(null, 'temperature');
    $replace['#temperature#'] = is_object($temperature) ? $temperature->execCmd() : '';
    $replace['#tempid#'] = is_object($temperature) ? $temperature->getId() : '';

    $humidity = $this->getCmd(null, 'humidity');
    $replace['#humidity#'] = is_object($humidity) ? $humidity->execCmd() : '';

    $pressure = $this->getCmd(null, 'pressure');
    $replace['#pressure#'] = is_object($pressure) ? $pressure->execCmd() : '';
    $replace['#pressureid#'] = is_object($pressure) ? $pressure->getId() : '';

    $wind_speed = $this->getCmd(null, 'wind_speed');
    $replace['#windspeed#'] = is_object($wind_speed) ? $wind_speed->execCmd() : '';
    $replace['#windid#'] = is_object($wind_speed) ? $wind_speed->getId() : '';

    $sunrise = $this->getCmd(null, 'sunrise');
    $replace['#sunrise#'] = is_object($sunrise) ? $sunrise->execCmd() : '';
    $replace['#sunid#'] = is_object($sunrise) ? $sunrise->getId() : '';
    if (strlen($replace['#sunrise#']) == 3) {
        $replace['#sunrise#'] = substr($replace['#sunrise#'], 0, 1) . ':' . substr($replace['#sunrise#'], 1, 2);
    } else if (strlen($replace['#sunrise#']) == 4) {
        $replace['#sunrise#'] = substr($replace['#sunrise#'], 0, 2) . ':' . substr($replace['#sunrise#'], 2, 2);
    }

    $sunset = $this->getCmd(null, 'sunset');
    $replace['#sunset#'] = is_object($sunset) ? $sunset->execCmd() : '';
    if (strlen($replace['#sunset#']) == 3) {
        $replace['#sunset#'] = substr($replace['#sunset#'], 0, 1) . ':' . substr($replace['#sunset#'], 1, 2);
    } else if (strlen($replace['#sunset#']) == 4) {
        $replace['#sunset#'] = substr($replace['#sunset#'], 0, 2) . ':' . substr($replace['#sunset#'], 2, 2);
    }

    $wind_direction = $this->getCmd(null, 'wind_direction');
    $replace['#wind_direction#'] = is_object($wind_direction) ? $wind_direction->execCmd() : 0;

    $refresh = $this->getCmd(null, 'refresh');
    $replace['#refresh_id#'] = is_object($refresh) ? $refresh->getId() : '';

    $condition = $this->getCmd(null, 'condition_now');
    $sunset_time = is_object($sunset) ? $sunset->execCmd() : null;
    $sunrise_time = is_object($sunrise) ? $sunrise->execCmd() : null;
    if (is_object($condition)) {
        $replace['#icone#'] = self::getIconFromCondition($condition->execCmd(), $sunrise_time, $sunset_time);
        $replace['#condition#'] = $condition->execCmd();
        $replace['#conditionid#'] = $condition->getId();
        $replace['#collectDate#'] = $condition->getCollectDate();
    } else {
        $replace['#icone#'] = '';
        $replace['#condition#'] = '';
        $replace['#collectDate#'] = '';
    }
    if ($this->getConfiguration('modeImage', 0) == 1) {
        $replace['#visibilityIcon#'] = "none";
        $replace['#visibilityImage#'] = "block";
    } else {
        $replace['#visibilityIcon#'] = "block";
        $replace['#visibilityImage#'] = "none";
    }
    $html = template_replace($replace, getTemplate('core', $version, 'current', 'weather'));
    cache::set('widgetHtml' . $_version . $this->getId(), $html, 0);
    return $html;
}
````

Plusieurs choses intéressantes ici :

Pour convertir la version demandée en dashboard ou mobile (mview devient mobile par exemple, cela permet par exemple de rajouter le nom des objets sur les vues)

````php
$_version = jeedom::versionAlias($_version);
````

Récupération d’un template de commande, ici le template de commande : plugins/weather/core/template/\$\_version/forecast.html (\$\_version valant mobile ou dashboard)

````php
$forcast_template = getTemplate('core', $_version, 'forecast', 'weather');
````

Ici remplacement des tags préalablement remplis dans \$replace du HTML pour contenir les valeurs

````php
$html_forecast .= template_replace($replace, $forcast_template);
````

Cela permet de récupérer la commande ayant le logical\_id : temperature\_min

````php
$this->getCmd(null, 'temperature_min');
````

Là cela permet de mettre la valeur dans le tag, seulement si la commande a bien été récupérée

````php
$replace['#temperature#'] = is_object($temperature) ? $temperature->execCmd() : '';
````

Passage important : cela permet de récupérer les personnalisations faites par l’utilisateur sur la page Générale → Affichage et de les réinjecter dans le template

````php
$parameters = $this->getDisplay('parameters');
if (is_array($parameters)) {
    foreach ($parameters as $key => $value) {
        $replace['#' . $key . '#'] = $value;
    }
}
````

Sauvegarde du widget dans le cache : pour que lors de la prochaine demande on le fournisse plus rapidement, on peut remarquer le 0 ici qui indique une durée de vie infinie, sinon la durée est en secondes (on verra dans la partie suivante comment le plugin weather remet à jour son widget).

````php
cache::set('weatherWidget' . $_version . $this->getId(), $html, 0);
````

Enfin envoi du html à Jeedom :

````php
return $html;
````

Il faut aussi dire à Jeedom ce que votre widget autorise au niveau de la personnalisation. C’est un peu complexe (et encore) mais normalement flexible et simple à mettre en place.

Il fonctionne de la même façon sur votre équipement ou commande, c’est un attribut static de la class \$\_widgetPossibility qui doit être un tableau multidimensionnel, mais c’est là que cela se complique si une dimension du tableau vaut true ou false. Il considère alors que tout les enfants possibles sont à cette valeur (je vais donner un exemple).

En premier lieu les cas où vous devez vous en servir : si dans votre class héritant de eqLogic ou de cmd a une fonction toHtml sinon ce n’est pas la peine de lire la suite.

### Méthode pre et post

Lors de la création ou la suppression de vos objets (équipement, commande ou autre) dans Jeedom, celui-ci peut appeler plusieurs méthodes avant/après l’action :

- ``preInsert`` ⇒ Méthode appellée avant la création de votre objet
- ``postInsert`` ⇒ Méthode appellée après la création de votre objet
- ``preUpdate`` ⇒ Méthode appellée avant la mise à jour de votre objet
- ``postUpdate`` ⇒ Méthode appellée après la mise à jour de votre objet
- ``preSave`` ⇒ Méthode appellée avant la sauvegarde (creation et mise à jour donc) de votre objet
- ``postSave`` ⇒ Méthode appellée après la sauvegarde de votre objet
- ``preRemove`` ⇒ Méthode appellée avant la supression de votre objet
- ``postRemove`` ⇒ Méthode appellée après la supression de votre objet

Exemple de la création des commandes ou mise à jour de celles-ci après la sauvegarde (l’exemple est simplifié), toujours avec le plugin weather :

````php
public function postUpdate() {
      $weatherCmd = $this->getCmd(null, 'temperature');
      if (!is_object($weatherCmd)) {
          $weatherCmd = new weatherCmd();
      }
      $weatherCmd->setName(__('Température', __FILE__));
      $weatherCmd->setLogicalId('temperature');
      $weatherCmd->setEqLogic_id($this->getId());
      $weatherCmd->setConfiguration('day', '-1');
      $weatherCmd->setConfiguration('data', 'temp');
      $weatherCmd->setUnite('°C');
      $weatherCmd->setType('info');
      $weatherCmd->setSubType('numeric');
      $weatherCmd->save();

      $cron = cron::byClassAndFunction('weather', 'updateWeatherData', array('weather_id' => intval($this->getId())));
      if (!is_object($cron)) {
          $cron = new cron();
          $cron->setClass('weather');
          $cron->setFunction('updateWeatherData');
          $cron->setOption(array('weather_id' => intval($this->getId())));
      }
      $cron->setSchedule($this->getConfiguration('refreshCron', '*/30 * * * *'));
      $cron->save();
}
````

Le début est assez standard avec la création d’une commande, la fin est plus intéressante avec la mise en place d’un cron qui va appeler la méthode ``weather::updateWeatherData`` en passant l’id de l’équipement à mettre à jour toute les 3 minutes par défaut.

Ici la méthode updateWeatherData (simplifiée aussi) :

````php
public static function updateWeatherData($_options) {
  $weather = weather::byId($_options['weather_id']);
  if (is_object($weather)) {
      foreach ($weather->getCmd('info') as $cmd) {
        $weather->checkAndUpdateCmd($cmd, $cmd->execute());
      }
  }
}
````

On voit ici que lors de l’appel on récupère l’équipement concerné puis on exécute les commandes pour récupérer les valeurs et les mettre à jour si nécessaire.

Partie très importante :

````php
$weather->checkAndUpdateCmd($cmd, $cmd->execute());
````

La fonction ``checkAndUpdateCmd`` permet de signaler à Jeedom une nouvelle mise à jour de la valeur, avec déclenchement de toutes les actions qui doivent être faites : mise à jour du dashboard, vérification des scénarios…​.

Pour la classe commande, un petit truc à savoir si vous utilisez le template js de base. Lors de l’envoi de l’équipement Jeedom fait du différentiel sur les commandes et va supprimer celles qui sont en base mais pas dans la nouvelle définition de l’équipement. Voilà comment l’éviter :

````php
public function dontRemoveCmd() {
  return true;
}
````

Pour finir voici quelques trucs et astuces :

- évitez (à moins de savoir ce que vous faites) d’écraser une méthode de la classe héritée (cela peut causer beaucoup de problèmes)
- Pour remonter la batterie (en %) d’un équipement, faites sur celui-ci (Jeedom se chargera du reste et de prévenir l’utilisateur si nécessaire) :

````php
$eqLogic->batteryStatus(56);
````

- Sur les commandes au moment de l’ajout d’une valeur Jeedom applique la méthode d’instance ``formatValue($_value)`` qui, en fonction du sous-type, peut la remettre en forme (en particulier pour les valeurs binaires)
- ne faites JAMAIS une méthode dans la class héritant de cmd s’appelant : execCmd ou event
- si dans la configuration de votre commande vous avez renseigné returnStateTime (en minute) et returnStateValue, Jeedom changera automatiquement la valeur de votre commande par returnStateValue au bout de X minute(s)
- toujours pour la commande vous pouvez utiliser addHistoryValue pour forcer la mise en historique (attention votre commande doit être historisée)
