---
layout: post
title: 'Variables dans les chaines de caractères'
description: 'Comment créer une chaine de caractère avec des variables'
permalink :
tags:
   - Powershell
   - Variables
   - LesBases

categories:
   - Powershell
   - LesBases
image:
 feature: headers/Coding_1436x500.png
 categorie: categories/PS_300x300.png
 caption: ''
 teaserlogo:
---

# Introduction
Très souvent lorsque l'on souhaite intégrer une variable dans une chaine de caractère on procède ainsi :

```powershell
$test = 'phrase'
write-host "Je fais une $test avec une variable"
#Résultat :
Je fais une phrase avec une variable
```

Pourtant, il est recommandé de mettre une chaine de caractère entre simple quote et non pas entre double quote.
Mais alors comment faire ?

# La Solution

```powershell
$test = 'phrase'
write-host ('Je fais une {0} avec une variable' -f $test)
#Résultat :
Je fais une phrase avec une variable
```
**And voilà** !

Bon, je suis certain que vous allez dire : oui mais bon avec des doubles quotes ça marche bien, pourquoi s'embêter ?

La réponse : Oui ok pour une variable simple ça fonctionne. Mais qu'en est-il pour une variable de type `$item.name` ? 

```powershell
$item=@{'name'='phrase'}

write-host "Je fais une $item.name avec une variable"

#Résultat :
Je fais une System.Collections.Hashtable.name avec une variable
```

Pas vraiment concluant ...

La solution s'appelle `Opérateur de format -f` :

>Met les chaînes en forme, en utilisant la méthode de mise en forme des objets de chaîne. Entrez la chaîne de format sur le côté gauche de l'opérateur et les objets à mettre en forme sur le côté droit de l'opérateur.

Ce qui donne pour notre $item.name :

```powershell
$item=@{'name'='phrase'}

write-host ('Je fais une {0} avec une variable' -f $item.name)

#Résultat :
Je fais une phrase avec une variable
```

On retrouve bien notre phrase. 

Si nous avions plusieurs variables à remplir dans notre chaine, on utiliserait alors {0} {1} {2} etc...

Exemple :

```powershell
$item=@{'name'='phrase';'seconde'='une';'troisieme'='variable'}

write-host ('Je fais {1} {0} avec {1} {2}' -f $item.name,$item.seconde,$item.troisieme)

#Résultat :
Je fais une phrase avec une variable
```
Deux choses à remarquer :

1. On peut utiliser plusieurs fois un marqueur dans la chaine. 
Dans l'exemple du dessus, le `{1}` a été utilisé 2 fois.

2. J'ai volontairement mis le `{1}` avant le `{0}` afin de vous montrer qu'il n'y a pas d'ordre à respecter avec les marqueurs dans la chaine de caractère. 
Le seul ordre qui est pris en compte, c'est celui après le `-f`,
en effet les arguments que vous mettrez après le `-f` définissent l'ordre d'attribution des numéros. Le premier argument sera `{0}`
le deuxième sera le `{1}`
et ainsi de suite ..

>Si vous rencontrer un message d'erreur du genre : `Impossible de lier le paramètre «ForegroundColor`, cela indique que powershell interprète le `-f` comme étant le paramètre `ForegroundColor`, il faut alors, comme je l'ai fait dans l'exemple, mettre **des parenthèses** : *write-host* **(** *'Je fais une {0} avec une variable'* *-f $item.name* **)**

# Conclusion
Dans cet article je n'ai traité que de la mise en place de variable dans des chaines de caractère mais l 'opérateur -f peut faire beaucoup plus de choses, je vous invite à regarder le lien suivant : <a href='https://ss64.com/ps/syntax-f-operator.html' target = '_blank'>-f Format operator</a>

