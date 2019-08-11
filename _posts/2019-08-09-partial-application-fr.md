---
layout: post
title: Les closures, l'application partiel et la curryfication (Partie 2/3) (French version)
author: Damien
date: '2019-08-10'
category:
    - tutorial
summary: Introduction aux notions de closures, application partiel et curryfication
thumbnail: blog/poupee_russe.jpg
---

Si vous n'avez pas encore lu la première partie, c'est [ici](closure-fr.html) !
Il est important de bien comprendre les notations utilisées lors de la précédente partie.

## L'application partiel

J'espère déjà que la boîte noire que représente les closures pour certains ne vous concernera plus.
Vous vous doutez bien que si nous avons vu les closures, ce n'est pas pour tout oublier et changer complètement de registre !

L'application **partiel** a pour principe d'initialiser des variables en fonction d'où nous nous situons dans l'appel d'une fonction/closure.
Imaginons que vous fassiez des pizzas (de A à Z).
En premier lieu vous allez faire la pâte, et seulement ensuite vous mettrez les ingrédients.
Vous êtes d'accord que la même pâte est utilisé peu importe les ingrédients que vous mettez (Les Italiens me pardonneront) ?
C'est un peu la même chose pour nous, parfois nous allons avoir des ingrédients qui seront toujours présent peu importe ce que nous ferons par la suite.

Prenons un exemple plus concret !
Supposons que vous aimeriez faire une fonction pour connaître la valeur d'une monnaie en fonction du change.

```swift
func euroToUs(_ x: Double) -> Double {
  return x * 1.12
}

euroToUs(42)
```

Nous avons ici une fonction très simple qui converti les euros en dollars américains.
Comment faire si je veux maintenant ajouter une nouvelle conversion ?
Nous devons alors créer une nouvelle fonction.
N'y a-t-il pas un moyen plus élégant et générique pour le faire ?
Et si vous avez vu juste !
Pourquoi ne pas simplement utiliser ce que nous avons vu précédemment ?

```swift
func change(x: Double, changeFunction: (Double) -> Double) -> Double {
  return changeFunction(x)
}
```

Plutôt que de créer une fonction pour chaque type de conversion, prenons une fonction plus générique qui prend une somme, et une closure qui se chargera de faire la conversion.

En reprenant le même exemple nous obtenons :

```swift
change(42) {$0 * 1.12}
```

J'utilise bien entendu tous les éléments syntaxiques vu plus tôt, donc n'hésitez pas à revenir dessus si nécessaire !

```swift
func change(changeFunction: (Double) -> Double) -> Double {
  return changeFunction(x)
}
```

Suite à venir ...
