---
layout: post
title: Création d'objets et Héritage en JavaScript
---

Une des plus grosses lacunes des "nouveaux" développeurs JS ayant pratiqué depuis quelques années des langages orientés objets basés sur les classes (Java, C#, PHP) , est sans doute leur compréhension de l'orienté objet en JavaScript et plus particulièrement le prototypage.

Deux raisons à celà à mon sens:

* La paresse d'apprendre
* Dans es2015, c'est réglé avec `class X {}`

Si la première raison est humaine mais ne vous concerne pas (ce n'est sûrement pas par paresse que vous êtes entrain de lire ce billet technique sur le sujet, hein ?), la seconde n'est tout simplement pas valide car

* De l'es2015, on commence effectivement à en faire pas mal, c'est bien, mais la plupart des projets OpenSource et propriétaires en entreprise n'utilisent pas encore es2015 tout simplement car il date à peine d'un an (17 Juin 2015), n'est pas encore supporté complètement par tous les navigateurs du marché mais en plus, quand on en fait aujourd'hui, on passe forcément par des transpilers type Babel, Traceur ou encore TypeScript qui transforment finalement le code es2015 en vieux code es5 avec des prototypes à tout va.
* La notion de classes introduite dans es2015 n'est que sucre syntaxique (comme diraient nos profs d'implémentation de langages de programmation) puisque derrière, le code est littéralement converti en fonctions objets (hé oui) avec du bon prototypage pûr et dur derrière, on va expliquer tout celà. C'est à dire que même le jour où on passera plus par un transpiler car le code es2015 sera nativement supporté par les navigateurs modernes, le résultat d'un `class Car {}` n'est pas une classe mais une fonction !

Pour preuve, testez les expression suivantes sur votre dernier navigateur supportant partiellement es2015.

```JavaScript
class Car {};
typeof Car;
Car.prototype;
```

Et c'est bien dommage tout ça, car notre développeur qui s'est mangé je ne sais combien d'années de Design Patterns, d'orienté objet, de polymorphisme et d'encapsulation se trouve à faire des callbacks à tout va, sous pretexte que de toute façon son fichier JS il y a que lui qui bosse dessus alors que du JEE, on est forcément une équipe puisqu'on bosse sur un gros projet... Je parle en connaissances de causes !

Bref, en gros, il faut s'y mettre quelques minutes (bon un peu plus) et comprendre ce que c'est une fois pour toute.

Dans ce (premier) post, je vais vraiment y aller molo car c'est important, ouvrez votre console Chrome, votre Plunkr ou votre Visual Code et testez moi ça au fil de l'eau. :)

### Classes et Prototypes

Un langage orienté objet type Java, basé sur des classes se fonde sur deux entités distinctes:

* Une classe: une représentation abstraite d'un ensemble de propriétés caractérisant un certain ensemble d'objets

* Un objet: (dit une instance) correspond à un instanciation de la classe

```Java

// La classe
class Voiture {
  private int nbPortes = 4; //Disons que par défaut nous avons 4 portes;

  public Voiture () {}
  public Voiture(int nbPortes){
    this.nbPortes = nbPortes;
  }
  public int getNbPortes(){
    return this.nbPortes;
  }
}

// L'instance
Voiture v1 = new Voiture();
Voiture v2 = new Voiture(5);
System.out.println(v1.getNbPortes());
System.out.println(v2.getNbPortes());

```

Un langage basé sur des prototypes ne possède que des objets !

Un objet est un ensemble de propriétés, une propriété est une association entre un clé et une valeur.

```JavaScript
var Voiture = {
  nbPortes: 4, // Disons que par défaut nous avons 4 portes;
  getNbPortes: function() {
    return this.nbPortes;
  }
}
```

Si la valeur est une fonction, la propriété est appelée méthode.

#### Création d'objets via {} et new Object

```JavaScript
var o1 = {
  "name": "value"
}
var o2 = new Object();
o2.name = "value";

console.log(o1 == o2); // false
console.log(o1 === o2); // false
console.log(o1.name === o2.name); // true

o1.link = o2;
console.log(o1.link === o2); //true
```

Notre objet `Voiture` est un objet simple mais que nous souhaitons utiliser dans la suite du code comme base ou modèle d'objet, ce qu'on appelle en JavaScript: Le prototype.

Object.create permet de choisir le prototype pour l'objet qu'on souhaite créer, nous aurons donc un nouvel objet dont les propriétés sont celles du prototype sauf si surchargées.

```
Voiture.couleur = "rouge";
console.log(v1.couleur); // rouge
console.log(Voiture.couleur); // rouge
v1.couleur = "bleue";
console.log(v1.couleur); // bleue
console.log(Voiture.couleur); // rouge
```

Il faut le voir pour comprendre:
