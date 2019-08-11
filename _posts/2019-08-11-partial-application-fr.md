---
layout: post
title: Les closures, l'application partielle et la curryfication (Partie 2/3) (French version)
author: Damien
date: '2019-08-10'
category:
    - tutorial
summary: Application partielle et précision sur les closures
thumbnail: blog/poupee_russe.jpg
---

Si vous n'avez pas encore lu la première partie, c'est [ici](../../../2019/08/09/closure-fr.html) !
Il est important de bien comprendre les notations utilisées lors de la précédente partie.

## L'application partielle

J'espère déjà que la boîte noire que représente les closures pour certains ne vous concerne plus.
Vous vous doutez bien que si nous avons vu les closures, ce n'est pas pour tout oublier et changer complètement de registre !

L'application **partielle** a pour principe d'initialiser des variables en fonction d'où nous nous situons dans l'appel d'une fonction/closure.
Imaginons que vous fassiez des pizzas (de A à Z).
En premier lieu vous allez faire la pâte, et seulement ensuite vous mettrez les ingrédients.
Vous êtes d'accord que la même pâte est utilisé peu importe les ingrédients que vous mettez (Les Italiens me pardonneront) ?
C'est un peu la même chose pour nous, parfois nous allons avoir des ingrédients qui seront toujours présent peu importe ce que nous faisons par la suite.

### L'exemple du poids

Prenons un exemple plus concret !
Vous souvenez vous de vos cours de physique ?
Ne vous inquiétez pas, nous n'allons rien voir de bien compliqué.
Vous savez sûrement que la gravité n'est pas la même sur la Terre que sur la Lune.
Nous avons la formule suivante en physique :

`P = m * g` (*P* étant le poids, *m* la masse et *g* la gravité).
La masse et le poids sont deux choses distinctes !

La gravité de la Terre est de 9.81 N/kg, et supposons une masse de 70 kg.
Nous obtenons alors *P = 9.81* \* *70* (N), qui est le poids sur Terre d'une personne avec une masse particulière.
Après cette légère paranthèse, vous allez comprendre pourquoi je vous ai introduit ces quelques notions.

Je veux désormais créer une fonction permettant de calculer le poids d'une personne sur Terre.

```swift
func poidsTerre(m: Double) -> Double {
  return m * 9.81
}
print(poidsTerre(m: 70, g: 9.81))
```

Si maintenant j'aimerai avoir le poids d'une personne sur la Lune, comment suis-je censé faire ?
Je dois écrire une nouvelle fonction pour changer une seule valeur ?

Heureusement, il y a un moyen plus générique de le faire pour ne pas avoir à écrire une nouvelle fonction à chaque fois que nous voulons connaître notre poids sur une planète différente !

Je vous propose le code ci-dessous faisant appel à l'application partielle:

```swift
func poids(g: Double) -> (Double) -> Double {
  func foo(m: Double) -> Double {
    return m * g
  }
  return foo
}
```

Wooooooow, c'est peut-être ce que vous êtes entrain de vous dire.
Je vous l'accorde, il y a pas mal de différences entre les deux versions, mais nous allons décortiquer tout cela ensemble.
La première principale différence est la signature de la fonction.
En effet, nous ne retournons pas directement un "*Double*", mais une fonction qui prend un "*Double*" et retourne un "*Double*".
C'est ce que signifie le retour "*(Double) -> Double*".
Cette syntaxe vous rappelle sûrement ce que nous avons vu avec les opérations de la partie précédente.

Il nous faut donc retourner une fonction, c'est pour cette raison que nous avons créé "*foo*", qui a la signature "*(Double) -> Double*" (exactement ce que nous voulons).
Cette seconde fonction fait juste l'application de la formule vu plus haut.
Nous prenons bien soin de remarquer que nous retournons "*foo*" et non "*foo(...)*".
Ce n'est absolument pas la même chose, d'un côté nous retournons la fonction, et de l'autre nous retournons l'appel à cette fonction !

La signature de "*foo*" est "*(Double) -> Double*" tandis que celle de l'appel de "*foo(...)*" est "*Double*".
Surtout que pour appeler la fonction *foo(...)* il nous faudrait un paramètre que nous n'avons pas encore.
Vous voulez vous en convaincre ?
À ce moment tenter d'afficher le résultat de l'appel de "*poids*" avec "*g*".
Réfléchissez deux minutes avant de passer à la suite.

...

Allez on regarde !

```swift
// Retourne (Function)
let poidsTerre = poids(g:9.81)
print(poidsTerre)
// Retourne (Double) -> Double
print(type(of: poidsTerre))
```
Swift nous dit que le résultat est une fonction, dont vous connaissez la signature !
Si nous désirons connaître le résultat pour une masse particulière, il nous faut appeler cette nouvelle fonction avec le paramètre de "*foo*".

```swift
// Possibilité 1
print(poids(g:9.81)(70))

// Possibilité 2
let poidsTerre = poids(g:9.81)
print(poidsTerre(70))
```

Tout l'intérêt réside dans la possibilité 2, nous pouvons initialiser une variable avec une gravité particulière.
Nous nous en servons ensuite comme d'une nouvelle fonction qui attend en paramètre la masse.
C'est quand même plus pratique d'avoir à déclarer une seule fonction, que vous pouvez aisément mettre dans une variable.

Je viens de vous montrer le fonctionnement général, cependant il vous faudra pratiquer ces notions pour que cela devienne intuitif.

### Une définition plus complète de la closure

Avez-vous remarqué le lien avec les closures ?
Depuis le début nous nous en servons, les closures sont partout !
Quand nous déclarons "*poidsTerre*", nous gardons en mémoire une closure.
Le concept que nous avons vu jusqu'à maintenant restait simple, sans aborder vraiment sa consistance même !

Au final, une closure ce n'est rien d'autre qu'une fonction accompagnée de son environnement lexical.
Un environnement lexical ? What is it ?
C'est simplement toutes les valeurs qui ont été capturées et qui peuvent être utilisées dans l'appel de notre fonction.
Prenons l'exemple de "*poidsTerre*" pour mieux comprendre.
Dans notre cas, "*poidsTerre*" est une closure contenant comme valeur capturé "*g*" étant égal à *9.81*, et une fonction foo.
Dire que nous avons capturé "*g*" revient à dire que notre variable sera connue dans l'appel de "*foo*".
Cette notion est très large et fait partie de ce que nous appelons la [portée](https://fr.wikipedia.org/wiki/Port%C3%A9e_(informatique)) (scope).
Nous ne nous étendrons pas sur ce sujet dans cet article, mais si vous désirez en savoir plus sur le fonctionnement je vous y encourage vivement !

Vous avez peut-être remarqué le nom de fonction "*foo*" qui n'a pas vraiment de signification.
Ce nom sera inconnu en dehors du corps de la fonction "*poids*", et vous ne vous en servirez quasiment jamais (parce qu'il ne faut jamais dire jamais) !
Au moment de l'appelée avec "*poidsTerre*", pas besoin de donner un nom de fonction, et pas non plus besoin de donner le nom du label.

Alors ce serait pas mieux de retourner directement une closure plutôt qu'une fonction dont le nom ne nous intéresse guère ?
Bien vu !

```swift
func poids(g: Double) -> (Double) -> Double {
  return {(m: Double) -> Double in
    m*g
  }
}
```

Vous pouvez essayer de votre côté, nous avons exactement le même résultat attendu.
Nous avons repris la syntaxe des closures que nous avons vu dans la partie précédente.
C'est comme retournée une fonction sans nom, et ça tombe bien vu que nous n'en avons pas besoin !

L'application partielle permet de donner autant de paramètres que souhaités, de même que nous pouvons faire une cascade de closure qui demanderont chacunes divers paramètres.

Nous sommes enfin parés pour attaquer la dernière partie avec la curryfication !

[Partie 3: La curryfication](../../../2019/08/11/curryfication.html)
