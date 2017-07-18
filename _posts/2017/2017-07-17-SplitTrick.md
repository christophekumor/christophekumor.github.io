---
layout : post
title: "Split : astuce à connaitre"
description: "Astuce pour la commande split qui peut rendre de bons services"
permalink:
tags:
   - Powershell
   - Astuces
categories:
   - Powershell

image:
 feature: headers/Coding_1436x500.png
 categorie: categories/PS_300x300.png
 caption: ""
 teaserlogo:
---

Petit partage rapide... je viens de lire un article sur l'excellent blog de <a href='https://richardspowershellblog.wordpress.com/' target = '_blank'>Richard Siddaway</a> et j'ai eu envie de le partager avec vous en français.

L'article parle d'une astuce (un parametre pas trop utilisé) pour la commande split.  
En effet nous connaissons tous son fonctionnement :

```powershell
PS C:\> 'Pour cette occasion nous vous donnons rendez-vous à Paris le 16 Septembre 2017 pour le PowerShell Saturday Paris 2017' -split ' '
```
Ce qui donne : 

```powershell
Pour
cette
occasion
nous
vous
donnons
rendez-vous
à
Paris
le
16
Septembre
2017
pour
le
PowerShell
Saturday
Paris
2017
```
et maintenant avec la magie d'une virgule :

```powershell
PS C:\> 'Pour cette occasion nous vous donnons rendez-vous à Paris le 16 Septembre 2017 pour le PowerShell Saturday Paris 2017' -split ' ',9
```
nous obtenons :

```powershell
Pour
cette
occasion
nous
vous
donnons
rendez-vous
à
Paris le 16 Septembre 2017 pour le PowerShell Saturday Paris 2017
```

Ce paramètre est en fait l'argument : `MaxSubstrings`  qui correspond au nombre maximum de sous chaines (substrings) retournées.  
En fait, en indiquant 9, il renvoie les 9 premières chaines de caractère délimitées par notre ' ' et le reste il le laisse dans une seule et même chaine de caractère.

Personnellement ce que je trouve intéressant, ce ne sont pas les 9 premières chaines mais bien le reste ! Cela nous permettra de récupérer une chaine complète sur la fin d'une phrase sans avoir à faire de `join` par exemple.  
Avec notre exemple :

```powershell
PS C:\> $evenement = 'Pour cette occasion nous vous donnons rendez-vous à Paris le 16 Septembre 2017 pour le PowerShell Saturday Paris 2017' -split ' ',9  

PS C:\> $evenement[-1]
```
nous obtenons :

```powershell
Paris le 16 Septembre 2017 pour le PowerShell Saturday Paris 2017
```

and voila !

Pour les anglophones, vous pouvez jeter un oeil à la <a href='https://richardspowershellblog.wordpress.com/2017/07/15/control-split-output/' target = '_blank'>source</a> de cet article, Richard a fait un exemple avec des dates de vacances


