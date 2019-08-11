---
layout: post
title: Les closures, l'application partielle et la curryfication (Partie 1/3) (French version)
author: Damien
date: '2019-08-09'
category:
    - tutorial
summary: Introduction aux closures
thumbnail: blog/poupee_russe.jpg
---

## Introduction

Ce titre vous fait peur ? Ou plutôt les notions abordées sont effrayantes ?
Ne fuyez pas et prenez le temps à mes côtés de démystifier ses concepts, comprendre le fonctionnement derrière ce qui peut vous sembler être de la magie.

## Bienvenue dans le monde fantastique des closures (ou fermetures)

Qu'est-ce que nous dit la documentation [Swift](https://docs.swift.org/swift-book/LanguageGuide/Closures.html) à ce sujet ?

> "Closures are self-contained blocks of functionality that can be passed around and used in your code."

Finalement, c'est pas un peu comme une boîte à outils ? Dans cette boîte nous pouvons imaginer avoir des outils (des fonctions ?), qui nous permettent de travailler sur des vis, clous ou autres (nos Types).

Et que Swift nous dit-il par rapport à la représentation de ces closures ?

> "Closure expression syntax has the following general form:"
```swift
{ (parameters) -> return type in
    statements
}
```

Cette syntaxe vous a l'air peut-être rude, et c'est sûrement le premier point qui fait défaut quand nous désirons manipuler des closures.
Il faut avant tout comprendre la syntaxe pour l'utiliser convenablement.

Prenons un petit exemple en utilisant ce que nous venons de voir.

```swift
let clos = {(x: Int, y: Int) -> Int in
  return x + y
}

// Retourne 42
print(clos(20,22))
```

Nous pouvons voir une closure comme une fonction sans nom, qui est appelé grâce à la variable à laquelle elle a été liée (clos dans notre cas).
Comme une fonction nous avons des paramètres d'entrée, le type de ces paramètres et le type de sortie.
Plutôt que d'avoir la syntaxe des accolades pour séparer la signature et le corps de la fonction, nous avons le mot-clé "*in* ".
Nous avons pour terminer la valeur de retour que nous signalons aussi avec le mot-clé "*return* ".
L'appel de cette closure se fait exactement de la même manière que si le nom de notre fonction était *clos*.
C'est-à-dire qu'il nous suffit de passer à notre closure les paramètres attendus.
Vous aurez compris que notre closure fait simplement l'addition de nos deux paramètres, rien de très passionnant n'est-ce pas ?

Passons un cran au-dessus en supposant que nous avons cette fois une fonction prenant en paramètre une closure !

```swift
func apply(_ x: Int,_ y: Int, op: (Int, Int) -> Int) -> Int {
  return op(x,y)
}
```

Nous avons désormais la fonction *apply*, qui prend deux entiers et va appliquer une opération sur ceux-ci.
Il faut être au clair sur la notation "*op: (Int, Int) -> Int* ".
Cela signifie que le paramètre *op* attend une fonction ou une closure qui prend deux entiers comme paramètres d'entrée, et retourne un entier.
Vous l'avez peut-être remarqué, mais la closure que nous avons écrite avant répond parfaitement à ces conditions.
Nous pouvons donc simplement appelé notre fonction ainsi :

```swift
// Retourne 42
print(apply(20, 22, op: clos))
```

Vous commencez à comprendre ?
Nous pouvons même faire plus simple sans avoir à déclarer notre closure au préalable comme ceci :

```swift
apply(20, 22, op: {(x: Int, y: Int) -> Int in
  return x + y
})
```

Toutes les fonctions/closures ayant pour signature "*(Int, Int) -> Int* " peuvent être utilisé comme paramètre de *op*, peu importe que celles-ci aient été déclaré avant ou directement à l'appel.

Vous voulez quelques petites astuces syntaxiques pour vous simplifier la vie ?
C'est parti !

```swift
apply(20, 22, op: {(x, y) in
  return x + y
})
```

Vous avez peut-être remarqué mais nous avons omis le type de nos paramètres, et même le type de sortie ! Pourquoi ça fonctionne alors ?
Dites merci à l'[inférence de type](https://fr.wikipedia.org/wiki/Inf%C3%A9rence_de_types) et non à une magie obscure.
Au moment de déclarer la fonction *apply*, nous avons déjà dit à Swift les types que nous attendions.
Swift a automatiquement fait le rapprochement entre ce qu'il attend comme type et ce que vous lui donnez. C'est franchement génial non ?
Pourtant nous sommes loin d'avoir vu toutes les simplifications possibles.
Next !

```swift
apply(20, 22, op: {(x, y) in
  x + y
})
```

Pas besoin de return ? Quand il n'y a aucune ambiguïté pour un cas aussi simple, Swift comprend que "*x+y* " est la valeur de retour. Bien entendu, vous pouvez mettre autant de code que vous désirez après le "*in* ", cependant il faudra remettre un "*return* " !

C'est pas encore fini, nous pouvons encore faire mieux !

```swift
apply(20, 22, op: {$0 + $1})
```

J'ai été trop loin ? Testez et vous verrez que ça marche !
Dans ce code, nous n'avons même pas eu besoin de faire une quelconque différence entre signature et corps de notre closure.
Par le même principe que l'inférence de type, dans cette situation Swift sait ce qu'il attend.
Comment récupérons-nous les valeurs de nos paramètres ?
"*$0* " et "*$1* " sont des noms conventionnels que Swift utilise par défaut, prenant les valeurs des paramètres d'entrée.

Un peu perdu ? Reprenons la signature de "*op: (Int, Int) -> Int* ".
Donc "*$0* " correspond au premier entier et "*$1* " au second.
L'ordre est important, car si nous avions par exemple "*op: (Int, String) -> Int* ", "*$1* " serait alors une chaîne de caractères.
Swift a donc lié à des noms prédéfinis les valeurs, plutôt que nous donnions nous-même le nom de celle-ci.
Pour le "*return* " qui n'est pas nécessaire je vous renvoie à ce que j'ai dit plus haut.

Encore un petit truc pour la route purement syntaxique, mais qui vous facilitera sûrement la vie !

```swift
apply(20, 22) {$0 + $1}
```

Quand le dernier paramètre de votre fonction est une closure, vous pouvez l'appeler de cette manière.
Même pas besoin de donner le nom du paramètre, vous écrivez juste entre accolade votre closure.
Toutes les closures vues précédemment fonctionne avec cette syntaxe.

Quand nous manipulons des fonctions aussi simple, vous conviendrez que nous pouvons nous simplifier la vie.
Il faut cependant toujours faire attention à la lisibilité.

Nous en avons terminé avec cette première partie sur les closures, passons sans plus tarder
à l'application partielle !

[Partie 2: L'application partielle](../../../2019/08/10/partial-application-fr.html)
