---
layout: post
title: Les closures, l'application partielle et la curryfication (Partie 3/3) (Swift)
author: Damien
date: '2019-08-11'
category:
    - tutorial
summary: La curryfication (ceci n'est pas commestible)
thumbnail: blog/poupee_russe.jpg
---

Si vous n'avez pas encore lu les deux précédentes parties, vous pouvez les retrouver ci-dessous:
- [Partie 1: Les closures](../../../2019/08/09/closure-fr.html) !
- [Partie 2: L'application partielle et précision sur les closures](../../../2019/08/10/partial-application-fr.html)
Il est important de bien comprendre les notions abordés dans nos deux précédents chapitres !

## La curryfication

J'espère que vous êtes bien au clair avec les closures et l'application partielle vu que cette dernière partie va tout mettre à profit.

La curryfication est le principe de prendre une fonction avec "*n*" paramètres et de la transformer en "*n*" fonctions avec chacune un argument.

À l'inverse l'application partielle prend une fonction à "*n*" arguments et la transforme en "*m*" fonctions, avec "*m <= n*" (Nous avons au plus autant de fonctions que d'arguments).
Prenez le temps de relire ces phrase plusieurs fois si besoin.

Vous vous souvenez de l'exemple du poids de la dernière partie ?
Même si l'objectif n'était pas de curryfier notre fonction, c'est pourtant ce que nous avons fait.

Si nous regardons juste en terme de signature de fonctions, voici un exemple:
`(Int, Int) -> Int` devient `(Int) -> (Int) -> Int`

Nous pouvons faire encore plus:
`(Int, Int, Int) -> Int` devient `(Int) -> (Int) -> (Int) -> Int`

C'est bien joli mais je pense que vous voulez voir des exemples un peu plus concret.

```swift
// Version classique
func add(_ x: Int, _ y: Int) -> Int {
  return x + y
}
print(add(20,22))

// Version curry
func addCurry(_ x: Int) -> (Int) -> Int {
  return {y in
    x + y
  }
}
print(addCurry(20)(22))
```

La version classique est plus intuitive à réaliser de prime abord.
La version "*addCurry*" reprend exactement toutes les notions abordées jusqu'à présent.
Nous avons simplement décomposé le second argument de la fonction dans notre closure.

Je vous montre aussi l'exemple à trois arguments tant que nous y sommes !

```swift
func add(_ x: Int, _ y: Int, _ z: Int) -> Int {
  return x + y + z
}
print(add(10, 10, 22))

func addCurry(_ x: Int) -> (Int) -> (Int) -> Int {
  return {y in
    return {z in
      x + y + z
    }
  }
}
print(add(10)(10)(22))
```

Nous prenons simplement chaque argument de la fonction "*add*" pour faire en sorte d'avoir des fonctions avec un unique argument.

Le principe de curryfication peut s'appliquer dans les deux sens, nous pouvons prendre une fonction pour la curryfier ou la decurryfier.

### Différence entre application partielle et curryfication

Au final, nous pourrions croire que la curryfication est un cas particulier de l'application partielle mais il existe une différence qui sépare ces deux notions.
L'application partielle n'a pas nécessité à avoir un type de retour, tandis que la curryfication retournera toujours une fonction à un argument jusqu'au dernier type de retour qui sera la valeur.
Cette nuance est subtile mais à le mérite d'exister, mais nous ne sommes pas ici pour débattre plus en détail sur ce sujet.


### Avantage de la curryfication

Quand vous curryfier une fonction, vous la transformez en ce que nous appelons une fonction "**pure**", possédant une propriété très intéressante.
En effet les fonctions pures n'ont aucun effet de bord, c'est-à-dire qu'aucun comportement (mutation) non désirable dans le programme n'est possible.

Vous avez les mêmes avantages qu'avec l'application partielle:
- Une meilleure décomposition du code
- Plus de clarté et de lisibilité du code
- La possibilité de déclarer des variables à différents niveaux de l'exécution.
- Vous pouvez plus facilement cibler à quel niveau vous désirez appliquer une modification.

Je vous conseille vivement de tester par vous-même, c'est le meilleur moyen de vous en rendre compte.

### Conclusion

Nous arrivons à la fin de notre série de tutoriel sur les closures, l'application partielle et la curryfication.
J'espère avoir démystifier ces principes, et surtout vous avoir apporté une meilleure compréhension globale sur le sujet.

Je n'ai pas couvert tous les cas, et certains sujets méritent encore d'être explorés plus en profondeur.
J'aurai l'occasion de revenir dans un article sur l'utilisation des closures dans les fonctions "*map*", "*filter*" et "*reduce*".

Si vous avez des remarques / suggestions n'hésitez pas, je tâcherai de les prendre en compte au mieux.

Merci de m'avoir lu et j'espère vous retrouver dans d'autres articles !
