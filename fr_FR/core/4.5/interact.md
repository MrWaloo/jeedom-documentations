# Interactions
**Outils → Interactions**

Dans Jeedom, le système d’interactions permet de réaliser des actions à partir de commandes textes ou vocales.

Ces commandes peuvent être obtenues par :

- SMS : envoyer un SMS pour lancer des commandes (action) ou poser une question (info).
- Chat : Telegram, Slack, etc.
- Vocal : dicter une phrase avec Siri, Google Now, SARAH, etc. Pour lancer des commandes (action) ou poser une question (info).
- HTTP : lancer une URL HTTP contenant le texte (ex. Tasker, Slack) pour lancer des commandes (action) ou poser une question (info).

L’intérêt des interactions réside dans l’intégration simplifiée dans d’autres systèmes comme smartphone, tablette, autre box domotique, etc.

> **Conseil**
>
> Vous pouvez ouvrir une interaction en faisant :
> - Clic sur l'une d'entre elle.
> - Ctrl+Clic ou Clic Centre pour l'ouvrir dans un nouvel onglet du navigateur.

Vous disposez d'un moteur de recherche permettant de filtrer l'affichage des interactions. La touche Echap annule la recherche.
À droite du champ de recherche, trois boutons que l'on retrouve à plusieurs endroits de Jeedom:
- La croix pour annuler la recherche.
- Le dossier ouvert pour déplier tous les panneaux et afficher toutes les interactions.
- Le dossier fermé pour replier tous les panneaux.

Une fois sur la configuration d'une interaction, vous disposez d'un menu contextuel au Clic Droit sur les onglets de l’interaction. Vous pouvez également utiliser un Ctrl+Clic ou Clic Centre pour ouvrir directement une autre interaction dans un nouvel onglet du navigateur.

## Interactions

En haut de page, on trouve 3 boutons :

- **Ajouter** : Permet de créer de nouvelles interactions.
- **Régénérer** : Recréer toutes les interactions (peut être très long &gt; 5mn).
- **Tester** : Permet d’ouvrir une boîte de dialogue pour écrire et tester une phrase.

> **Conseil**
>
> Si vous avez une interaction qui génère les phrases pour les lumières par exemple et que vous ajoutez un nouveau module de commande de lumière, il vous faudra soit régénérer toutes les interactions, soit aller dans l’interaction en question et la sauvegarder de nouveau pour créer les phrases de ce nouveau module.

## Principe

Le principe de création est assez simple : on va définir une phrase modèle génératrice qui va permettre à Jeedom de créer une ou plusieurs centaines d’autres phrases qui seront des combinaisons possibles du modèle.

On va définir de la même façon des réponses avec un modèle (ça permet à Jeedom d’avoir plusieurs réponses pour une seule et même question).

On peut aussi définir une commande à exécuter si par exemple l’interaction n’est pas liée à une action mais une information ou si on souhaite réaliser une action particulière après celle-ci (il est aussi possible d’exécuter un scénario, de contrôler plusieurs commandes…​).

## Configuration

La page de configuration est constituée de plusieurs onglets et de boutons :

- **Phrases** : Affiche le nombre de phrases de l’interaction (un clic dessus vous les montre).
- **Enregistrer** : Enregistre l’interaction courante.
- **Supprimer** : Supprime l’interaction courante.
- **Dupliquer** : Duplique l’interaction courante.

### Onglet Général

- **Nom** : Nom de l’interaction (peut être vide, le nom remplace le texte de la demande dans la liste des interactions).
- **Groupe** : Groupe de l’interaction, cela permet de les organiser (peut être vide, sera donc dans le groupe "aucun").
- **Actif** : Permet d’activer ou désactiver l’interaction.
- **Demande** : La phrase modèle génératrice (obligatoire).
- **Synonyme** : Permet de définir des synonymes sur les noms des commandes.
- **Réponse** : La réponse à fournir.
- **Attendre avant de répondre (s)** : Permet d'ajouter un délai de X secondes avant de générer la réponse. Ca permet par exemple d'attendre que le retour d'état d'une lampe soit actualisé avant de répondre.
- **Conversion binaire** : Permet de convertir les valeurs binaires en ouvert/fermé par exemple (uniquement pour les commandes de type info binaire).
- **Utilisateurs autorisés** : Limite l’interaction à certains utilisateurs (les logins séparés par des \|).

### Onglet Filtres

- **Limiter aux commandes de type** : Permet de n’utiliser que les types actions, infos ou les 2 types.
- **Limiter aux commandes ayant pour sous-type** : Permet de limiter la génération à un ou plusieurs sous-types.
- **Limiter aux commandes ayant pour unité** : Permet de limiter la génération à une ou plusieurs unités (Jeedom crée la liste automatiquement à partir des unités définies dans vos commandes).
- **Limiter aux commandes appartenant à l’objet** : Permet de limiter la génération à un ou plusieurs objets (Jeedom crée la liste automatiquement à partir des objets que vous avez créés).
- **Limiter au plugin** : Permet de limiter la génération à un ou plusieurs plugins (Jeedom crée la liste automatiquement à partir des plugins installés).
- **Limiter à la catégorie** : Permet de limiter la génération à une ou plusieurs catégories.
- **Limiter à l’équipement** : Permet de limiter la génération à un seul équipement/module (Jeedom crée la liste automatiquement à partir des équipements/modules que vous avez).

### Onglet Actions

A utiliser si vous voulez cibler une ou plusieurs commandes spécifiques ou passer des paramètres particuliers.

#### Exemples

> **Note**
>
> Les captures d’écran peuvent être différentes au vue des évolutions.

#### Interaction simple

La façon la plus simple de configurer une interaction, c’est de lui donner un modèle générateur rigide, sans variation possible. Cette méthode ciblera très précisément une commande ou un scénario.

Dans l’exemple qui suit, on peut voir dans le champ "Demande" la phrase exacte à fournir pour déclencher l’interaction. Ici, pour allumer le plafonnier du salon.

![interact004](./images/interact004.png)

Sur cette capture, on peut voir la configuration pour avoir une interaction liée à une action spécifique. Cette action est définie dans la partie "Action" de la page.

On peut très bien imaginer faire de même avec plusieurs actions pour allumer plusieurs lampes dans le salon comme l’exemple qui suit :

![interact005](./images/interact005.png)

Dans les 2 exemples ci-dessus, la phrase modèle est identique mais les actions qui en découlent changent en fonction de ce qui est configuré dans la partie "Action". Avec une interaction simple à phrase unique, on peut donc déjà imaginer des actions combinées entre diverses commandes et divers scénarios (on peut aussi déclencher des scénarios dans la partie action des interactions).

> **Conseil**
>
> Pour ajouter un scénario, créer une nouvelle action, écrire "scenario" sans accent, appuyer sur la touche tabulation de votre clavier pour faire apparaître le sélecteur de scénario.

#### Interaction multiple commandes

Ici, nous allons voir tout l’intérêt et toute la puissance des interactions. Avec une phrase modèle nous allons pouvoir générer des phrases pour tout un groupe de commandes.

On va reprendre ce qui a été fait plus haut, supprimer les actions que l’on avait ajoutées et, à la place de la phrase fixe, dans "Demande", nous allons utiliser les tags **\#commande\#** et **\#equipement\#**. Jeedom va donc remplacer ces tags par le nom des commandes et le nom de l’équipement (on peut voir l’importance d’avoir des noms de commande/équipement cohérents).

![interact006](./images/interact006.png)

On peut donc constater ici que Jeedom a généré 152 phrases à partir de notre modèle. Toutefois, elles ne sont pas très bien construites et l’on a un peu de tout.

Pour faire de l’ordre dans tout cela, on va utiliser les filtres (partie droite de notre page de configuration). Dans cet exemple, on veut générer des phrases pour allumer des lumières. On peut donc décocher le type de commande info (si je sauve, il ne me reste plus que 95 phrases générées), puis, dans les sous-types, on peut ne garder coché que "défaut" qui correspond au bouton d’action (ne reste donc plus que 16 phrases).

![interact007](./images/interact007.png)

C’est mieux, mais on peut faire encore plus naturel. Si je prends l’exemple généré "On entrée", il serait bien de pouvoir transformer cette phrase en "allume l’entrée" ou en "allumer l’entrée". Pour faire cela, Jeedom dispose, sous le champ demande, d’un champ synonyme qui va nous permettre de nommer différemment le nom des commandes dans nos phrases "générées", ici c’est "on", j’ai même des "on2" dans les modules qui peuvent contrôler 2 sorties.

Dans les synonymes, on va donc indiquer le nom de la commande et le(s) synonyme(s) à utiliser :

![interact008](./images/interact008.png)

On peut voir ici une syntaxe un peu nouvelle pour les synonymes. Un nom de commande peut avoir plusieurs synonymes, ici "on" a comme synonyme "allume" et "allumer". La syntaxe est donc "*nom de la commande*" ***=*** "*synonyme 1*"***,*** "*synonyme 2*" (on peut mettre autant de synonyme que l’on veut). Puis, pour ajouter des synonymes pour un autre nom de commande, il suffit d’ajouter une barre verticale "*\|*" après le dernier synonyme et à la suite de laquelle vous pouvez à nouveau nommer la commande qui va avoir des synonymes comme pour la première partie, etc.

C’est déjà mieux, mais pour la commande "on" il manque encore "entrée", le "l' " et pour d’autres le "la" ou "le" ou "un", etc. On pourrait modifier le nom de l’équipement pour l’ajouter, ce serait une solution, sinon on peut utiliser les variations dans la demande. Cela consiste à lister une série de mots possibles à un emplacement de la phrase, Jeedom va donc générer des phrases avec ces variations.

![interact009](./images/interact009.png)

On a maintenant des phrases un peu plus correctes avec des phrases qui ne sont pas justes, pour notre exemple "on" "entrée". On trouve donc "Allume entrée", "Allume un entrée", "Allume une entrée", "Allume l’entrée" etc. On a donc toutes les variantes possibles avec ce que l’on a ajouté entre les "\[ \]" et ceci pour chaque synonyme, ce qui génère rapidement beaucoup de phrases (ici 168).

Afin d’affiner et de ne pas avoir des choses improbables telles que "allume l’télé", on peut autoriser Jeedom à supprimer les demandes syntaxiquement incorrectes. Il va donc supprimer ce qui est trop éloigné de la syntaxe réelle d’une phrase. Dans notre cas, on passe de 168 phrases à 130 phrases.

![interact010](./images/interact010.png)

Il devient donc important de bien construire ses phrases modèles et synonymes ainsi que de sélectionner les bons filtres pour ne pas générer trop de phrases inutiles. Personnellement, je trouve intéressant d’avoir quelques incohérences du style "un entrée" car si chez vous, vous avez une personne étrangère qui ne parle pas correctement le français, les interactions fonctionneront tout de même.

### Personnaliser les réponses

Jusqu’à présent, comme réponse à une interaction, nous avions une simple phrase qui n’indiquait pas grand chose à part que quelque chose s’est passé. L’idée serait que Jeedom nous indique ce qu’il a fait un peu plus précisément. C’est là qu’intervient le champ réponse dans lequel on va pouvoir personnaliser le retour en fonction de la commande exécutée.

Pour ce faire, nous allons à nouveau utiliser les Tag Jeedom. Pour nos lumières, on peut utiliser une phrase du style : J’ai bien allumé \#equipement\# (voir capture ci-dessous).

![interact011](./images/interact011.png)

On peut aussi ajouter n’importe quelle valeur d’une autre commande comme une température, un nombre de personnes, etc.

![interact012](./images/interact012.png)

### Conversion binaire

Les conversions binaires s’appliquent aux commandes de type info dont le sous-type est binaire (retourne 0 ou 1 uniquement). Il faut donc activer les bons filtres, comme on peut le voir sur la capture un peu plus bas (pour les catégories, on peut toutes les cocher, pour l’exemple je n’ai gardé que lumière).

![interact013](./images/interact013.png)

Comme on peut le voir ici, j’ai conservé quasiment la même structure pour la demande (c’est volontaire pour se concentrer sur les spécificités). Bien sûr, j’ai adapté les synonymes pour avoir quelque chose de cohérent. Par contre, pour la réponse, il est **impératif** de mettre uniquement \#valeur\# qui représente le 0 ou 1 que Jeedom va remplacer par la conversion binaire qui suit.

Le champ **conversion binaire** doit contenir 2 réponses : en premier la réponse si la valeur de la commande vaut 0, puis une barre verticale "\|" de séparation et enfin la réponse si la commande vaut 1. Ici les réponses sont simplement non et oui mais on pourrait y mettre une phrase un peu plus longue.

> **Warning**
>
> Les tags ne fonctionnent pas dans les conversions binaires.

### Utilisateurs autorisés

Le champ "Utilisateurs autorisés" permet de n’autoriser que certaines personnes à exécuter la commande, vous pouvez mettre plusieurs profils en les séparant par un "\|".

Exemple : personne1\|personne2

On peut imaginer qu’une alarme peut être activée ou désactivée par un enfant ou un voisin qui viendrait arroser les plantes en votre absence.

### Regexp d’exclusion

Il est possible de créer des [Regexp](https://fr.wikipedia.org/wiki/Expression_rationnelle) d’exclusion, si une phrase générée correspond à cette Regexp elle sera supprimée. L’intérêt c’est de pouvoir supprimer des faux positifs, c’est à dire une phrase générée par Jeedom qui active quelque chose qui ne correspond pas à ce que l’on veut ou qui viendrait parasiter une autre interaction qui aurait une phrase similaire.

On a 2 endroits pour appliquer une Regexp :
- Dans l’interaction même dans le champ "Regexp d’exclusion".
- Dans le menu Administration→Configuration→Interactions→champ "Regexp général d’exclusion pour les interactions".

Pour le champ "Regex général d’exclusion pour les interactions", cette règle sera appliquée à toutes les interactions qui seront créées ou sauvegardées de nouveau par la suite. Si on veut l’appliquer à toutes les interactions existantes, il faut régénérer les interactions. Généralement, on l’utilise pour effacer des phrases incorrectement formées se retrouvant dans la plupart des interactions générées.

Pour le champ "Regexp d’exclusion" dans la page de configuration de chaque interaction, on peut mettre une Regexp spécifique qui va agir uniquement sur ladite interaction. Elle vous permet donc de supprimer plus précisément pour une interaction. Cela peut aussi permettre d’effacer une interaction pour une commande spécifique pour laquelle on ne veut pas offrir cette possibilité dans le cadre d’une génération de multiples commandes.

La capture d’écran qui suit montre l’interaction sans le Regexp. Dans la liste de gauche, je filtre les phrases pour ne vous montrer que les phrases qui vont être supprimées. En réalité il y a 76 phrases générées avec la configuration de l’interaction.

![interact014](./images/interact014.png)

Comme vous pouvez le voir sur la capture suivante, j’ai ajouté une regexp simple qui va chercher le mot "Julie" dans les phrases générées et les supprimer. Toutefois, on peut voir dans la liste de gauche qu’il y a toujours des phrases avec le mot "julie", dans les expressions régulières, Julie n’est pas égale à julie, on appelle cela une sensibilité à la casse ou en bon français une majuscule est différente d’une minuscule. Comme on peut le voir dans la capture suivante, il ne reste plus que 71 phrases, les 5 avec un "Julie" ont été supprimées.

Une expression régulière se compose comme suit :

- En premier, un délimiteur, ici c’est une barre oblique "/" placée en début et fin d’expression.
- Le point qui suit la barre oblique représente n’importe quel caractère, espace ou nombre.
- Le "\*" quant à lui indique qu’il peut y avoir 0 ou plusieurs fois le caractère qui le précède, ici un point, donc en bon français n’importe quel caractère.
- Puis Julie, qui est le mot à rechercher (mot ou autre schéma d’expression), suivi à nouveau d’un point puis barre oblique.

Si on traduit cette expression en une phrase, cela donnerait "cherche le mot Julie qui est précédé par n’importe quoi et suivi de n’importe quoi".

C’est une version extrêmement simple des expressions régulières mais déjà très compliquée à comprendre. Il m’a fallu un moment pour en saisir le fonctionnement. Comme exemple un peu plus complexe, une regexp pour vérifier une adresse URL :

/\^(https?:\\/\\/)?(\[\\da-z\\.-\]+)\\.(\[a-z\\.\]{2,6})(\[\\/\\w\\.-\]\*)\*\\/?\$/

Une fois que vous pouvez écrire cela, vous avez compris les expressions régulières.

![interact015](./images/interact015.png)

Pour résoudre le problème de majuscule et minuscule, on peut ajouter à notre expression une option qui va la rendre insensible à la casse, ou autrement dit, qui considère une lettre minuscule égale à une majuscule; pour ce faire, on doit simplement ajouter à la fin de notre expression un "i".

![interact016](./images/interact016.png)

Avec l’ajout de l’option "i" on constate qu’il ne reste plus que 55 phrases générées et dans la liste de gauche avec le filtre julie pour rechercher les phrases qui contiennent ce mot, on constate qu’il y en a bien plus.

Comme c’est un sujet extrêmement complexe, je ne vais pas aller plus en détail ici, il y a suffisamment de tutos sur le net pour vous aider et n’oubliez pas que Google est votre ami aussi car oui, c’est mon ami, c’est lui qui m’a appris à comprendre les Regexp et même à coder. Donc s’il m’a aidé, il peut aussi vous aider si vous y mettez de la bonne volonté.

Liens utiles :

- <http://www.commentcamarche.net/contents/585-javascript-l-objet-regexp>
- <https://www.lucaswillems.com/fr/articles/25/tutoriel-pour-maitriser-les-expressions-regulieres>
- <https://openclassrooms.com/courses/concevez-votre-site-web-avec-php-et-mysql/les-expressions-regulieres-partie-1-2>

### Réponse composée de plusieurs informations

Il est aussi possible de mettre plusieurs commandes info dans une réponse, par exemple pour avoir un résumé de situation.

![interact021](./images/interact021.png)

Dans cet exemple on voit une phrase simple qui va nous retourner une réponse avec 3 températures différentes, ici on peut donc mettre un peu tout ce que l’on veut afin d’avoir un ensemble d’informations en une seule fois.

### Y a-t-il quelqu’un dans la chambre ?

#### Version basique

- La question est donc "y’a-t-il quelqu’un dans la chambre"
- La réponse sera "non il n’y a personne dans la chambre" ou "oui il y a quelqu’un dans la chambre"
- La commande qui répond à ça est "\#\[Chambre de julie\]\[FGMS-001-2\]\[Présence\]\#"

![interact017](./images/interact017.png)

Cette exemple cible précisément un équipement spécifique ce qui permet d’avoir une réponse personnalisée. On pourrait donc imaginer remplacer la réponse de l’exemple par "non il n’y a personne dans la chambre de *julie*\|oui il y a quelqu’un dans la chambre de *julie*"

#### Evolution

- La question est donc "\#commande\# \[dans la \|dans le\] \#objet\#"
- La réponse sera "non il n’y a personne dans la pièce" ou "oui il y a quelqu’un dans la pièce"
- Il n’y a pas de commande qui réponde à ça dans la partie Action vu que c’est une interaction Multiple commandes
- En ajoutant une expression régulière, on peut nettoyer les commandes que l’on ne veut pas voir pour n’avoir que les phrases sur les commandes "Présence".

![interact018](./images/interact018.png)

Sans le Regexp, on obtient ici 11 phrases, or mon interaction a pour but de générer des phrases uniquement pour demander s’il y a quelqu’un dans une pièce, donc je n’ai pas besoin d’état de lampe ou autre comme les prises, ce qui peut être résolu avec le filtrage regexp. Pour rendre encore plus flexible, on peut ajouter des synonymes, mais dans ce cas il ne faudra pas oublier de modifier la regexp.

### Connaître la température/humidité/luminosité

#### Version basique

On pourrait écrire la phrase en dur comme par exemple "quelle est la température du salon", mais il faudrait en faire une pour chaque capteur de température, luminosité et humidité. Avec le système de génération de phrase Jeedom, avec une seule interaction on peut donc générer les phrases pour tous les capteurs de ces 3 types de mesure.

Ici un exemple générique qui sert à connaître la température, l’humidité, la luminosité des différentes pièces (objet au sens Jeedom).

![interact019](./images/interact019.png)

- On peut donc voir qu’une phrase générique type "Quelle est la température du salon" ou "Quelle est la luminosité de la chambre" peut être convertie en : "quelle est \[la \|l\\'\]\#commande\# objet" (l’utilisation de \[mot1 \| mot2\] permet de dire cette possibilité ou celle-là pour générer toutes les variantes possibles de la phrase avec mot1 ou mot2). Lors de la génération Jeedom va générer toutes les combinaisons possibles de phrases avec toutes les commandes existantes (en fonction des filtres) en remplaçant \#commande\# par le nom de la commande et \#objet\# par le nom de l’objet.
- La réponse sera de type "21 °C" ou "200 lux". Il suffit de mettre : \#valeur\# \#unite\# (l’unité est à compléter dans la configuration de chaque commande pour laquelle on veut en avoir une)
- Cette exemple génère donc une phrase pour toutes les commandes de type info numérique qui ont une unité, on peut donc décocher des unités dans le filtre de droite limité au type qui nous intéresse.

#### Evolution

On peut donc ajouter des synonymes au nom de commande pour avoir quelque chose de plus naturel, ajouter un regexp pour filtrer les commandes qui n’ont rien à voir avec notre interaction.

Ajout de synonyme, permet de dire à Jeedom qu’une commande s’appelant "X" peut aussi s’appeler "Y" et donc dans notre phrase si on a "allume y", Jeedom sait que c’est allumer x. Cette méthode est très pratique pour renommer des commandes qui, quand elles sont affichées à l’écran, sont écrites d’une façon qui n’est pas naturelle vocalement ou dans une phrase écrite comme les "ON". Un bouton écrit comme cela est totalement logique mais pas dans le contexte d’une phrase.

On peut aussi ajouter un filtre Regexp pour enlever quelques commandes. En reprenant l’exemple simple, on voit des phrases "batterie" ou encore "latence", qui n’ont rien à voir avec notre interaction température/humidité/luminosité.

![interact020](./images/interact020.png)

On peut donc voir un regexp :

**(batterie\|latence\|pression\|vitesse\|consommation)**

Celui-ci permet de supprimer toutes les commandes qui ont l’un de ces mots dans leur phrase

> **Note**
>
> Le regexp ici est une version simplifiée pour une utilisation simple. On peut donc soit utiliser les expressions traditionnelles, soit utiliser les expressions simplifiées comme dans cet exemple.

### Piloter un dimmer ou un thermostat (slider)

#### Version basique

Il est possible de piloter une lampe en pourcentage (variateur) ou un thermostat avec les interactions. Voici un exemple pour piloter son variateur sur une lampe avec des interactions :

![interact022](./images/interact022.png)

Comme on le voit, il y a ici dans la demande le tag **\#consigne\#** (on peut mettre ce que l’on veut) qui est repris dans la commande du variateur pour appliquer la valeur voulue. Pour ce faire, on a 3 parties : \* Demande : dans laquelle on crée un tag qui va représenter la valeur qui sera envoyée à l’interaction. \* Réponse : on réutilise le tag pour la réponse afin d’être sûr que Jeedom a correctement compris la demande. \* Action : on met une action sur la lampe que l’on veut piloter et dans la valeur on lui passe notre tag *consigne*.

> **Note**
>
> On peut utiliser n’importe quel tag excepté ceux déjà utilisés par Jeedom, il peut y en avoir plusieurs pour piloter par exemple plusieurs commandes. A noter aussi que tous les tags sont passés aux scénarios lancés par l’interaction (il faut toutefois que le scénario soit en "Exécuter en avant plan").

#### Evolution

On peut vouloir piloter toutes les commandes de type curseur avec une seule interaction. Avec l’exemple qui suit, on va pouvoir commander plusieurs variateurs avec une seule interaction et donc générer un ensemble de phrases pour les contrôler.

![interact033](./images/interact033.png)

Dans cette interaction, on n’a pas de commande dans la partie action, on laisse Jeedom générer la liste des phrases à partir des tags. On peut voir le tag **\#slider\#**. Il est impératif d’utiliser ce tag pour les consignes dans une interaction multiple commandes, il peut ne pas être le dernier mot de la phrase. On peut aussi voir dans l’exemple que l’on peut utiliser un tag qui ne fait pas partie de la demande dans la réponse. La majorité des tags disponibles dans les scénarios sont disponibles aussi dans les interactions et donc peuvent être utilisés dans une réponse.

Résultat de l’interaction :

![interact034](./images/interact034.png)

On peut constater que le tag **\#equipement\#** qui n’est pas utilisé dans la demande est bien complété dans la réponse.

### Piloter la couleur d’un bandeau de LED

Il est possible de piloter une commande couleur par les interactions en demandant par exemple à Jeedom d’allumer un bandeau de led en bleu. Voilà l’interaction à faire :

![interact023](./images/interact023.png)

Jusque là rien de bien compliqué, il faut en revanche avoir configuré les couleurs dans Jeedom pour que cela fonctionne; rendez-vous dans le menu → Configuration (en haut à droite) puis dans la partie "Configuration des interactions" :

![interact024](./images/interact024.png)

Comme on peut le voir sur la capture, il n’y a pas de couleur configurée, il faut donc ajouter des couleurs avec le "+" à droite. Le nom de la couleur, c’est le nom que vous allez passer à l’interaction, puis dans la partie de droite (colonne "Code HTML"), en cliquant sur la couleur noire on peut choisir une nouvelle couleur.

![interact025](./images/interact025.png)

On peut en ajouter autant que bon nous semble, on peut leur donner n’importe quel nom, ainsi on pourrait imaginer attribuer une couleur pour le nom de chaque membre de la famille.

Une fois configuré, vous dites "Allume le sapin en vert", Jeedom va rechercher dans la demande une couleur et l’appliquer à la commande.

### Utilisation couplée à un scénario

#### Version basique

Il est possible de coupler une interaction à un scénario afin de réaliser des actions un peu plus complexes que l’exécution d’une simple action ou d’une demande d’information.

![interact026](./images/interact026.png)

Cette exemple permet de lancer le scénario qui est lié dans la partie action. On peut bien sûr en avoir plusieurs.

### Programmation d’une action avec les interactions

Les interactions permettent de faire beaucoup de choses. Vous pouvez programmer dynamiquement une action. Exemple : "Met le chauffage à 22 pour 14h50". Pour cela rien de plus simple, il suffit d’utiliser les tags \#time\# (si on définit une heure précise) ou \#duration\# (pour dans X temps, exemple dans 1 heure) :

![interact23](./images/interact23.JPG)

> **Note**
>
> Dans la réponse, vous remarquerez le tag \#value\#. Celui-ci contient l’heure de programmation effective dans le cas d’une interaction programmée.
