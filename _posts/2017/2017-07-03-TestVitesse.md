---
layout : post
title: "Benchmark"
description: "Comment tester l'exécution de votre code"
permalink:
tags:
   - Powershell
   - Astuces
   - Les Bases
categories:
      - Powershell
      - LesBases
      - Astuces
image:
 feature: headers/time_1436x500.png
 categorie: categories/PS_300x300.png
 caption: ""
 teaserlogo:
---

Je suis un gros fan d'optimisation, et qui dis optimisation dit test de rapidité,
pour faire mes tests j'utilsie principalement 2 methodes :

La 1ere peut etre comparé à un chonometre, on le déclance au début, puis on le stop à la fin, et on affiche le temps
# Get Start Time
$startDTM = (Get-Date)

#< CODE HERE >

# Get End Time
$endDTM = (Get-Date)

# Echo Time elapsed
"Elapsed Time: $(($endDTM-$startDTM).totalseconds) seconds"

Si vous faites plusieurs tests avec cette methode, il faudra rajouter une variable afin de collecter les resultats


======================================

La deuxieme consiste en l'utilisation de la commande powershell `Measure-Command`

$query01 = (Measure-Command{gc $wordlist}).TotalSeconds

Si vous avez plusieurs tests à faire, je créé un array au depart puis je fais mes tests, je met ensuite chaque résulatt dans le tableau et enfin j'affiche la loste des resulats
$result = @{}

$query01 = (Measure-Command{gc $wordlist}).TotalSeconds
$query02 = (Measure-Command{gc $wordlist -Raw}).TotalSeconds
$query03 = (Measure-Command{gc $wordlist -ReadCount 0}).TotalSeconds

$result.Add("Get-Content", "$query01")
$result.Add("Get-Content -Raw", "$query02")
$result.Add("Get-Content -ReadCount", "$query03")

$result.GetEnumerator() | Sort-Object Value