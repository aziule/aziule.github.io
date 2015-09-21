Integration best practices
===

# Introduction

En tant que développeurs back-end, nous sommes souvent amenés à intervenir sur de multiples projets. Changeant fréquemment de projets et revenant constamment modifier d’anciennes applications, il est important que nous prenions en main rapidement chaque environnement et que nous puissions éditer l’ensemble du projet facilement.

Pour cela, nous suivons un ensemble de best practices ainsi que des guidelines qui sont propres à Novaway. Cela nous permet d’uniformiser chaque projet et, bien qu’ils aient tous leurs spécificités (technologies, architecture, etc.), nous ne sommes pas perdus lorsqu’il s’agit de découvrir un nouvel environnement.

Le problème est qu’à l’heure actuelle nous disposons de toutes ces guidelines pour le développement back-end mais qu’il n’en n’existe aucune pour le front-end. Résultat : le front-end est en place, compatible avec l’ensemble des navigateurs et nous retrouvons même une architecture semblable des fichiers d’un projet à l’autre, mais tout est encore trop spécifique et il est difficile, par exemple, d’intégrer rapidement des éléments.

L’objet de ce document est de définir une base commune et uniforme, servant de repère pour les projets en cours et les projets futurs, afin d’avoir une base générique à chaque projet. Nous allons couvrir ici le CSS et son préprocesseur SASS.

# CSS

## L’anglais tu utiliseras

Ces conventions existent depuis que le premier langage est né !

## Objet tu penseras

Il est important d’utiliser des noms modulaires et de penser Objet dans l’ensemble, plutôt que spécificité :

* Utilisation de classes uniquement.
* Les IDs sont à proscrire en CSS : ils seront utilisés pour les interactions en Javascript.
* Un objet peut être copié et réutilisé tel quel n’importe où dans une page et aura toujours le même rendu.
* Pour rendre un objet spécifique, utiliser les classes de l’objet puis lui rajouter une classe qui lui sera propre pour modifier quelques propriétés.

Problèmes liés à un CSS trop spécifique :

* CSS peu réutilisable, à l’intérieur d’un même projet ou d’un projet à l’autre.
* Effet "brouillon" et CSS peu performant (redondance).

Un exemple parfait utilisant ce style de CSS est le Bootstrap de Twitter.

Pour adopter cette pensée il est important de définir, en amont de l’intégration, les objets qui sont présents et qui se répètent à travers les pages afin de définir des structures communes et réutilisables à divers endroits du code HTML.

Les fichiers CSS seront découpés en conséquences, toujours en pensant objet (voir chapitre sur SASS pour le détail).

## Des noms séparés par un tiret tu emploieras

Les classes css, lorsqu’elles sont composées de plusieurs noms ou adjectifs, sont séparées par des tirets. Le camelcase est interdit.

Mauvais exemples :

```css
.tableBordered {}
.TableBordered {}
.table_bordered {}
```

Bon exemple :

```css
.table-bordered {}
```

## Pour un élément enfant, le nom du composant parent tu préfixeras.

Un composant parent peut contenir des éléments enfants. Pour que ces éléments enfants soient identifiables facilement et n’influencent pas sur d’autres éléments pouvant avoir une classe similaire, il est important d’éviter d’imbriquer les classes :

Mauvais exemple :

```css
.box {
    border: 1px solid black;
    .title {
        text-decoration: underline;
    }
}
```

Bon exemple :

```css
.box {
    border: 1px solid black;
}

.box-title {
    text-decoration: underline;
}
```

*Explications : Dans le premier exemple, la classe "title" peut être présente plusieurs fois dans le CSS et représenter plusieurs éléments totalement différents (titre de header, titre de page, titre d’article, etc.). En préfixant la classe de l’objet auquel il fait référence (ici, “.box”), nous savons tout de suite à quoi il fait référence. De plus, cela permet de définir une structure et une logique pour le composant “box”.*

## La pluralisation tu emploieras

Exemple :

```css
.todos {
    width: 100%;
    height: 30px;
}

.todo {
    background-color: red;
    color: white;
}
```

*Explications : Lorsqu’un élément parent contient plusieurs éléments enfant semblables, il n’est pas utile de préfixer les éléments enfants avec la classe du parent. Il suffit de mettre la classe du parent au pluriel.*

## Des sous-classes tu utiliseras

Exemple :

```css
.button {
    padding: 10px 20px;
    border-radius: 5px;
    background-color: white;
    color: black;
}

.button-primary {
    background-color: orange;
    color: white;
}

.button-inverse {
    background-color: black;
    color: white;
}
```

En HTML, cela donne :

```html
<button class="button button-primary">Click me!</button>
```

*Explications : Nous avons rajouté des sous-classes à l’objet "button", que nous pouvons réutiliser plus tard dans le HTML. Ainsi, chaque objet de type “button” pourra hériter de sous-classes diverses pour le personnaliser.*

**Des modificateurs globaux tu utiliseras**

La plupart des éléments doivent modifier des propriétés communes. Pour cela, il est inutile de définir une classe spécifique à cet objet mais plutôt d’utiliser des classes globales.

Exemple :

```css
.text-left { text-align: left; }
.text-center { text-align: center; }
.text-right { text-align: right; }
```

*Explications : Ces classes utiles seront réutilisables partout et seront indépendantes de chaque objet. Elles pourront être utilisées -ou pas- sur chacun des éléments.*

## La propriété "rem" tu manieras

De plus en plus à la mode de part son élégance et son aspect pratique, la propriété "rem" doit être employée dès le début d’un projet.

Cela permet :

* De pouvoir modifier l’ensemble des tailles des polices du site à partir d’une seule valeur (celle du html { font-size }.
* De manipuler les tailles des fonts avec une unité très simple et non "figée" :
    * Par rapport aux px, pt, etc., elle est variable.
    * Par rapport aux em, elle n’est pas dépendante de l’élément parent mais bien de la taille à la racine ("root em").

A intégrer dans chaque projet, au départ :

```css
html {
    font-size: 62.5%;
}
```

*Explications : En mettant cette ligne, 1rem sera égal à 1px. 1.4rem  vaudra donc 14px, 0.8rem vaudra 8px, et ainsi de suite.*

## Typer avec des balises HTML tu éviteras

Souvent nous sommes tentés de rattacher une classe à une balise particulière.

Par exemple :

```css
button.ma-classe {...}
```

Le problème est que cette classe ne fonctionnera pas si nous l’appliquons à un élément de type "span" ou “div”. Ainsi, à moins que l’objet ne le requiert (exemple : label, bouton de type submit, liste, …), il est bien d’éviter de rattacher les classes CSS à des balises particulières.

De même, lorsque l’on veut styler une classe à l’intérieur d’un élément de structure particulier, il ne faut pas oublier de le styler dans sa globalité (générique) puis ensuite à l’intérieur de l’élément de structure.

Mauvais exemple :

```css
.col-sm-2 .mon-element {
    width: 100%
}
```

Bon exemple :

```css
.mon-element {
    width: 50%;
}

.col-sm-2 .mon-element {
    width: 100%;
}
```

*Explications : dans le premier cas, si nous utilisons l’élément ".mon-element" en dehors de la structure “.col-sm-2”, pour une raison diverse telle que la modification du design ou une refonte de la structure HTML d’une page, celui-ci perdra toutes ses caractéristiques de style ainsi que ses changements en media queries.*

## Un standard de nommage tu adopteras

Il est important de garder une même logique lors du nomage des éléments.

<table>
  <tr>
    <td>Type</td>
    <td>Convention</td>
    <td>Exemple</td>
  </tr>
  <tr>
    <td>Objet</td>
    <td><pre>.nom {}</pre></td>
    <td><pre>.button {}</pre></td>
  </tr>
  <tr>
    <td>Parent-enfant</td>
    <td><pre>.nom {}<br>.nom-nom {}</pre></td>
    <td><pre>.box {}<br>.box-title {}</pre></td>
  </tr>
  <tr>
    <td>Sous-classe</td>
    <td><pre>.nom-adjectif {}</pre></td>
    <td><pre>.nav-dark {}</pre></td>
  </tr>
  <tr>
    <td>Modificateur</td>
    <td><pre>.adjectif {}</pre></td>
    <td><pre>.left {}</pre></td>
  </tr>
</table>

Dans le cas d’un objet étant relatif à un composant, il faut toujours préfixer la classe de cet objet par le nom du composant.

D’une manière générale, les noms des classes, des images, etc. doivent rester cohérents envers l’objet global qu’elles représentent et non envers un objet spécifique.

| Objet | Mauvais exemple | Bon exemple | Explications |
| --- | --- | --- | --- |
Icône de coeur | .icon-favorite .like | .icon-heart | Le mot-clé "icon-favorite" ne représente pas bien l’image, car une icône de coeur peut être utilisée à des fins différentes (d’où le nom “icon-heart”).<br>Idem pour le mot-clé “like”, qui reste beaucoup trop spécifique et qui ne représente pas l’image de coeur. |
Icône du logo du site | .icons-avisjoe | .icon-logo | “icons-avisjoe” est au pluriel et ne représente pas l’objet dans sa globalité. |
Icône de moustache orange | .icons-reco | .icon-mustache .icon-mustache-orange | Le mot-clé “icons-reco” est au pluriel et ne représente pas l’objet.<br>Le mot-clé “icon-mustache-orange” est utile si nous avons une autre icône d’une couleur différente. Un mauvais exemple, si nous avons 2 couleurs différentes pour la même icône, serait d’avoir “icon1” et “icon2”. |


Pour plus d’informations, regarder les conventions de nom des icônes du Twitter Bootstrap : [http://getbootstrap.com/components/](http://getbootstrap.com/components/).

## Les balises classiques tu styleras

Afin d’avoir une cohérence dans le design et l’intégration, il est important de styler les éléments de base du HTML.

Exemple :

```css
h1 {
    margin: 10px 0 20px 0;
    border-bottom: 1px solid red;
}

p {
    line-height: 1em;
    margin-bottom: 10px;
}
```

*Explixations : Cela uniformise toutes les balises HTML les plus fréquentes et les éléments cohérentes dès qu’elles seront utilisées ailleurs dans la structure HTML. Nous utilisons d’un côté des resets CSS puis enfin nous stylons les éléments de base pour éviter d’avoir des problèmes, par exemple de margin, de padding, de fonts, de taille de caractères, de couleur, etc. pour les balises non overridées.*

## Définitions devant être utilisées dans chacun des projets

<table>
  <tr>
    <td>Élément</td>
    <td>Fonction</td>
  </tr>
  <tr>
    <td colspan="2"><strong>Balises HTML</strong></td>
  </tr>
  <tr>
    <td>h1<br>
h2<br>
...</td>
    <td>Style global des titres</td>
  </tr>
  <tr>
    <td>p</td>
    <td>Style global des paragraphes</td>
  </tr>
  <tr>
    <td>.left<br>
.right</td>
    <td>Flottement</td>
  </tr>
  <tr>
    <td>.text-left<br>
.text-center<br>
.text-right</td>
    <td>Alignement du texte</td>
  </tr>
  <tr>
    <td>.clearfix</td>
    <td>Clear du flux HTML</td>
  </tr>
  <tr>
    <td>.margin-top0<br>
.margin-top1<br>
.margin-bottom0<br>
...</td>
    <td>Margin en accord avec le design</td>
  </tr>
  <tr>
    <td>.padding-top<br>
.padding-bottom</td>
    <td>Padding en accord avec le design</td>
  </tr>
  <tr>
    <td>.inline<br>
.inline-block<br>
.block</td>
    <td>Affichage de l’élément</td>
  </tr>
  <tr>
    <td>.absolute<br>
.fixed<br>
.relative</td>
    <td>Position de l’élément</td>
  </tr>
  <tr>
    <td>.hidden</td>
    <td>Cacher un élément</td>
  </tr>
  <tr>
    <td colspan="2"><strong>Variables (exemples)</strong></td>
  </tr>
  <tr>
    <td>.background-dark<br>
.background-light<br>
...</td>
    <td>Couleur du fond</td>
  </tr>
  <tr>
    <td>.red<br>
.blue<br>
...</td>
    <td>Couleur du texte</td>
  </tr>
  <tr>
    <td>Structure</td>
    <td></td>
  </tr>
  <tr>
    <td>.row</td>
    <td>Une ligne</td>
  </tr>
  <tr>
    <td>.col1<br>
.col2<br>
.col3<br>
...</td>
    <td>Une colonne (selon une grille de taille prédéfinie)</td>
  </tr>
  <tr>
    <td>Objets</td>
    <td></td>
  </tr>
  <tr>
    <td>.button<br>
.button-cta //call to action<br>
.button-primary<br>
.button-secondary</td>
    <td>Boutons</td>
  </tr>
</table>


## Les grosses imbrications tu banniras

Dans le cas ou nous sommes obligés d’imbriquer des classes, la limite maximum est de 3 niveaux. Il faut utiliser les imbrications lorsque cela est nécessaire (et non pas par défaut) pour s’assurer que le CSS soit clean et réutilisable (sans avoir à utiliser le paramètre "!important").

Par exemple, si l’on veut modifier le comportement d’un module à l’intérieur d’un autre module, l’imbrication peut être une bonne solution.

## Le responsive tu prévoieras

Dans le cas où on n’utilise pas un framework déjà fait (Twitter Bootstrap pour ne citer que lui) il est important de prévoir un CSS responsif.

En ce sens, il faut prévoir le comportement de chaque élément lors du redimensionnement de la fenêtre lors de la mise en place du système de grilles / du layout et non après.

Il faut faire attention à bien gérer l’ensemble des éléments de structure et de ne pas en oublier.

# SASS

L’utilisation de SASS ou de tout autre préprocesseur CSS permet d’accélérer le temps que prend l’intégration ainsi que de rendre le langage plus facile à comprendre et à réutiliser (variables, mixins, placeholders, fonctions, etc.).

Seulement ce genre de technologies transforme le CSS en un véritable langage de programmation qu’il est difficile de maîtriser lorsqu’on n’est pas développeur. Ainsi, un intégrateur CSS devra faire preuve d’une réflexion de codeur pour proposer un code simple, clair, réutilisable, performant et générique.

Nous devons donc avoir plus de rigueur avec SASS qu’en CSS classique.

## Tes fichiers tu structureras

Il est important de découper ses fichiers selon les grands axes de l’intégration, en pensant module et non page.

Voici la structure qui doit être utilisée dans SASS pour avoir une efficacité maximale :

<table>
  <tr>
    <td>Chemin</td>
    <td>Explication</td>
  </tr>
  <tr>
    <td>/scss</td>
    <td>Dossier de base</td>
  </tr>
  <tr>
    <td>/scss/base</td>
    <td>Éléments communs + structure</td>
  </tr>
  <tr>
    <td>/scss/modules</td>
    <td>Éléments réutilisables</td>
  </tr>
  <tr>
    <td>/scss/vendor</td>
    <td>Fichiers tierces</td>
  </tr>
  <tr>
    <td colspan="2">Exemple</td>
  </tr>
  <tr>
    <td>/scss/base/_variables.scss</td>
    <td>Variables <sup>TOUJOURS PRÉSENT</sup></td>
  </tr>
  <tr>
    <td>/scss/base/_grid.scss</td>
    <td>Système de grilles <sup>TOUJOURS PRÉSENT</sup></td>
  </tr>
  <tr>
    <td>/scss/base/_typography.scss</td>
    <td>Typographie <sup>TOUJOURS PRÉSENT</sup></td>
  </tr>
  <tr>
    <td>/scss/base/_placeholders.scss</td>
    <td>Placeholders de base <sup>TOUJOURS PRÉSENT</sup></td>
  </tr>
  <tr>
    <td>/scss/base/_mixins.scss</td>
    <td>Mixins de base <sup>TOUJOURS PRÉSENT</sup></td>
  </tr>
  <tr>
    <td>/scss/modules/_buttons.scss</td>
    <td>Boutons <sup>TOUJOURS PRÉSENT</sup></td>
  </tr>
  <tr>
    <td>/scss/modules/_nav.scss</td>
    <td>Eléments de navigation <sup>TOUJOURS PRÉSENT</sup></td>
  </tr>
  <tr>
    <td>/scss/modules/_tabs.scss</td>
    <td>Tableaux</td>
  </tr>
  <tr>
    <td>/scss/vendor/_colorpicker.scss</td>
    <td>CSS colorpicker</td>
  </tr>
  <tr>
    <td>/scss/vendor/_jquery.ui.scss</td>
    <td>CSS jQuery UI</td>
  </tr>
  <tr>
    <td>/scss/main.scss</td>
    <td>Include (seulement) des autres fichiers <sup>TOUJOURS PRÉSENT</sup></td>
  </tr>
</table>


Les variables, mixins, placeholders, etc. relatifs à un module doivent se situer dans le fichier de ce module.

Seuls les mixins, variables, placeholders, etc. globaux servant de point de départ sont inclus dans le dossier "base".

## Un code simple et structuré tu garderas

Le but de SASS est d’avoir un code CSS plus maintenable et plus simple que du CSS normal. Il faut donc garder ça en tête et viser la simplicité du code.

Les déclarations de variables, de mixins, de placeholders, etc. dans un fichier, doivent se situer en haut de celui-ci et doivent être regroupées par "blocs".

Exemple :

```SASS
/* Variables */
$bleu: blue;
$red: red;

/* Placeholders */
%button {
    padding: 10px;
}

/* Classes */
.button-cta {
    @extend button;
    color: $red;
}

.button-primary {
    @extend button;
    color: $blue;
}
...
```

## Les placeholders tu utiliseras et l’usage des mixins tu réduiras

La différence entre les mixins et les placeholders réside dans le fait qu’un placeholder seul ne génèrera aucun CSS à la compilation. A l’inverse, les mixins vont dupliquer leur code CSS pour tous les éléments qui en héritent.

Les placeholders sont donc préférables pour avoir un code plus friendly et performant.

Exemple avec mixin :

- SASS

```css
@mixin rounded-corner($arc) {
    -moz-border-radius: $arc;
    -webkit-border-radius: $arc;
    -border-radius: $arc;
}

.button-cta {
    @include rounded-corner(5px);
    background-color: red;
    padding: 15px;
}

.button-primary {
    @include rounded-corner(8px);
    color: blue;
    padding: 10px;
}
```

- CSS compilé

```css
.button-cta {
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    -border-radius: 5px;
    background-color: red;
    padding: 15px;
}

.button-primary {
    -moz-border-radius: 8px;
    -webkit-border-radius: 8px;
    -border-radius: 8px;
    color: blue;
    padding: 10px; 
}
```

*Explications : On voit que le code CSS du mixin a été répété deux fois. Code dupliqué, fichier plus volumineux, principe DRY ("Don’t Repeat Yourself") passé aux oubliettes.*

Exemple avec placeholder :

- SASS

```css
%button {
    padding: 10px 15px;
    color: white;
    margin-bottom: 10px;
    text-align: center;
}

.button-cta {
    @extend %button;
    background-color: red;
}

.button-primary {
    @extend %button;
    background-color: blue;
}
```

- CSS compilé

```css
.button-cta, .button-primary {
    padding: 10px 15px;
    color: white;
    margin-bottom: 10px;
    text-align: center; 
}

.button-cta {
     background-color: red;
}

.button-primary {
     background-color: blue;
}
```

*Explications : Le code du placeholder n’est présent qu’une fois dans notre fichier. Le code est plus simple, plus maintenable et plus performant.*

D’une manière générale, ne jamais déclarer de mixin sans paramètres. Un mixin sans paramètres doit se transformer en placeholder ou en classe.

De plus, il ne faut pas étendre (@extend) des sélecteurs classiques CSS ("p", “span”, “div”) car cela peut avoir des répercussions sur la maintenance du code et casser l’intégration.

## Pour les noms des éléments SASS, des conventions tu suivras

Les noms des éléments SASS (variables, mixins, placeholders, …) doivent suivre les mêmes conventions de nommage que les classes CSS.

Seules les variables peuvent être écrites en camelCase. Le reste doit être écrit en séparant les termes par des tirets.

Mauvais exemple :

/scss/modules/_mainnav.scss

```SASS
$back-color: #005522;
$front-color: #885544;

@mixin BOUTON(...){ 
    ...
}
```

Bon exemple :

/scss/modules/_mainnav.scss

```css
$mainNavBackColor: #005522;
$mainNavFrontColor: #885544;

@mixin main-nav-button(...) {
    ...
}
```

*Explications : Tout comme les noms des classes et des modules, il est important de bien différencier les noms des éléments SASS. Cela permet d’inclure beaucoup de modules sans qu’il n’y ait d’interférences entre eux ainsi que de savoir à quoi font référence chaque éléments SASS.*
