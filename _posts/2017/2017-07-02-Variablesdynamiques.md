---
layout : post
title: "Noms de variable dynamiques"
description: "Comment créer et utiliser des noms dynamiques pour des variables"
permalink:
tags:
   - Powershell
   - Variables
   - Les Bases
categories:
   - Powershell
   - LesBases
image:
 feature: headers/Coding_1436x500.png
 categorie: categories/PS_300x300.png
 caption: ""
 teaserlogo:
---

# Introduction

Dans cet article je vais partager avec vous des méthodes que j'utilise afin de travailler avec des noms de variable dynamiques. 

On parle bien ici du **nom de la variable** et non de son contenu.

Car travailler avec du contenu dynamique n'a rien d'extraordinaire, c'est ce que font nos scripts en règle générale.

En revanche générer un nom de variable à "la volée" pour ensuite le réutiliser, ce n’est pas forcément si évident que cela.

Au premier abord on pourrait être tenté par des choses du genre : 

```powershell
PS C:\> $i = 1

PS C:\> Mavar$i = "test"
PS C:\> Mavar$i

    # Ce qui renvoi l'erreur suivante : 
    # Mavar$i : Le terme « Mavar$i » n'est pas reconnu comme nom d'applet de commande, fonction, fichier de script ou programme exécutable. 
```

Ou

```powershell
PS C:\> $(Mavar$i) = "test"
PS C:\> Mavar$i

    # Ce qui renvoi l'erreur suivante : 
    # Au caractère Ligne:2 : 1
    # + $(Mavar$i) = "test"
    # + ~~~~~~~~~~
    # L’expression d’affectation n’est pas valide.

```

Ou on rajoute des " '  car on est sait qu'une chaine de caractère se met entre quote..

```powershell
PS C:\> $("Mavar"$i) = "test"
PS C:\> Mavar$i

    # Mais le résultat est le même, une erreur :
    # Au caractère Ligne:2 : 10
    # + $("Mavar"$i) = "test"
    # +          ~~
    # Jeton inattendu « $i » dans l’expression ou l’instruction.
```
Bon je vais arrêter là pour les exemples qui ne fonctionnent pas... Mais il y en a plein d'autres :stuck_out_tongue_winking_eye:

# Deux environnements
Je vais distinguer 2 cas de figure pour cet article. 
Le premier cas est celui de l'utilisation des noms dynamiques dans un script et le deuxième sera l'utilisation de ces noms dans un GUI fait en powershell. 

## 1. Dans un script

### a. Définition des variables dynamiques
Il arrive parfois que l'on ait besoin de générer des noms de variables lors de l'exécution d'un script.
Le recourt aux variables dynamiques arrive souvent lorsque l'on ne connait pas le nombre de variable que l'on aura à créer. 

En powershell v1+, `New-Variable` est la commande qui permet de créer une variable est de lui assigner un contenu 

```powershell
PS C:\> new-Variable -Name "MaVar" -Value "test"

PS C:\> $Mavar
test
```

Mais bon, personnellement je ne l'utilise jamais ainsi... Préférant un bon vieux :

```powershell
PS C:\> $MaVar = "test"

PS C:\> $Mavar
test
```
En revanche cette commande a l'avantage de pouvoir utiliser d'autres variables pour créer le nom de notre variable.

Exemple : je dois créer 5 variables et leurs affecter du contenu venant d'un Array

```powershell
$MyArray = @('pomme','poire','banane','cerise','orange')

for ($i=0; $i -lt $MyArray.Length; $i++)
{
    New-Variable -Name "MaVar$i" -Value $MyArray[$i]
}

```

Ce qui nous donnera comme résultats :

```powershell
PS C:\> $MaVar0
pomme

PS C:\> $MaVar1
poire

PS C:\> $MaVar2
banane

```

On voit bien ici que l'on a maintenant **5 variables** avec des **noms différent** et une valeur propre à chacune.

### b. récupération des variables dynamiques

Nous avons vu comment définir un nom de variable de façon dynamique, nous allons maintenant voir que l'on peut faire la même chose mais cette fois ci, pour la récupération.

En effet la commande `Get-Variable` permet de récupérer le contenu d'une variable

```powershell
# Pour l'exemple on définit une variable $MaVar2
PS C:\> $MaVar2 = 'banane'

# On suppose que nous sommes dans une boucle et que $i = 2
PS C:\> $i = 2

# Maintenant récupérons le contenu de la variable $MaVar2.
PS C:\> Get-Variable -Name "MaVar$i" -ValueOnly
banane
```
Voilà on a bien le contenu de la variable $MaVar2 tout en partant d'une chaine de caractère et d'une autre variable, nous sommes donc bien dans la récupération de la **valeure** d'une variable avec un nom dynamique.

## 2. Dans un GUI"

Dans un GUI, la définition est la même, on part toujours avec un  `New-Variable -Name "Mavar$i" -Value $i`, en revanche c'est au moment de l'utilisation qu'il y a une petite astuce.

Pour l'exemple on va prendre 3 Checkbox avec les noms suivant ;

```powershell
$CheckboxPomme
$CheckboxPoire
$CheckboxBanane
```
Imaginons une requete qui nous indique quel fruit on va manger aujourdhui :smiley:(oui je sais il en faut 5).
On aura donc par exemple : Poire.

Mais comment cocher la bonne checkbox ?

Afin de faire ce que nous voulons, on va procéder ainsi :

```powershell
#notre information d'entrée
$i = Poire #Information obtenu avec notre requete imaginaire

 (Get-Variable -name "Checkbox{0}" -f $i)).value.Checked = $true

```

Voilà avec notre `Get-Variable` de l'exemple ci-dessus, on a coché la checkbox `$CheckboxPoire`.

L'astuce dont je parlais en début concerne le fait de passer par la `value` de la checkbox et non pas directement par le checked, comme on le ferait avec le nom complet de la checkbox.

```powershell
#Avec le nom complet de la checkbox
$Checkbox2.Checked = $true

#Avec un nom à construire dynamiquement 
(Get-Variable -name "Checkbox{0}" -f $i)).value.Checked = $true
```

> concernant le `-f` vous pouvez lire l'article : <a href="{{ site.url }}/powershell/les%20bases/2017/07/OperateurDeFormat.html" title="variables dans les chaines de caractères">variables dans les chaines de caractères</a>

# Synthèse 

## Script
$i = 1

Pour creer un nom de variable dynamqiue : 

```powershell
New-Variable -Name "MaVar$i" -Value 'test'` 
```
Pour récupérer une valeur avec un nom de variable dynamique

```powershell
Get-Variable -Name "Mavar$i" -ValueOnly
```

## GUI
$i = Poire

Pour agir sur un element GUI

```powershell
(Get-Variable -name "Checkbox{0}" -f $i)).value.Checked = $true
```
Pour récupérer une valeur avec un nom de variable dynamique

```powershell
(Get-Variable -name "Checkbox{0}" -f $i)).value.CheckState
```

# Conclusion
Voia Voila...
Dans cet article, je n'ai pas eu la volonté d'être exhaustif, il y a donc peut-être d'autres méthodes... Ce que je sais c'est que ce que je vous propose fonctionne.
Cependant, si vous avez envie de partager d'autres méthode, n'hésitez pas, je les ajouterai, n'hésitez pas non plus à laisser un commentaire, cela fait toujours plaisir d'avoir un retour.

# Update 
**@Thierry Degols** m'a fait remarquer qu'une solution de contournement est possible, pour les variables avec doubles quotes, en utilisant `$($item.name)`

Soit :
```powershell
write-host "Je fais une $($item.name) avec une variable"
```
A vous de choisir entre ça et le `-f`, en toute honnêteté j'utilise autant les doubles quotes que le -f, cela dépend de la variable et du contexte.

**@Nicolas BAUDIN** amène un élément supplémentaire concernant la récupération du contenu d'une variable avec `Get-Variable`. En effet avec cette commande, les wildcard sont possible

exemple :
```powershell
$vSwitch1 = "test"
$AllvSwitch = Get-Variable -Name "vSwitch?" -ValueOnly
# ? remplace un caractère après vSwitch
```
Ou
```powershell
$vSwitch1 = "test"
$AllvSwitch = Get-Variable -Name "vSwitch*" -ValueOnly
#* remplace tous les caractères qui suivent vSwitch
```

Attention, **Attention**, **Attention**, l'utilisation de wildcard peut amener à avoir des valeurs que l'on ne souhaite pas... En effet cela va récupérer toutes les valeurs de votre scope.

Partons du principe que l'on a lancé une fois chacun des tests précédent

Si je lance :

```powershell
$vSwitch10 = "test"
$AllvSwitch = Get-Variable -Name "vSwitch?" -ValueOnly
```

En principe il ne devrait rien me donner vu qu'il y a deux caractères après vSwitch, or là il me donne :

```powershell
PS C:\> $vSwitch10 = "test"
$AllvSwitch = Get-Variable -Name "vSwitch?" -ValueOnly
$AllvSwitch
test
```

Pourquoi ? car il a encore en "mémoire" l'ancienne variable $vSwitch1, donc là il ne nous donne pas la valeur de $vSwitch10 mais celle de vSwitch1.

Donc vraiment à utiliser en connaissance de cause... 
Et si vous n'avez pas compris mon explication c'est qu'il ne faut pas utiliser les Wildcards. eheh.