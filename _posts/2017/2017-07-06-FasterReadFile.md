---
layout : post
title: "Benchmark : Lecture d'un fichier"
description: "Quelle méthode est la plus rapide pour lire le contenu d'un fichier ?"
permalink:
tags:
   - Powershell
   - Astuces
   - Optimisation
categories:
   - Powershell

image:
 feature: headers/Time_1436x500.jpg
 categorie: categories/Time_300x300.png
 caption: ""
 teaserlogo:
---

# Introduction
Afin de mettre en pratique le post précédent, j'ai fait pour vous un petit benchmark : **Quelle est la méthode la plus rapide pour lire le contenu d'un fichier ?**

Au passage, ça permet de faire un point sur les différentes méthodes qui permettent de récupérer le contenu d'un fichier.

# Les tests
Afin de faire les tests, j'ai utilisé ma fonction `Measure-MyScript`, cette fonction est disponible dans <a href='https://christophekumor.github.io/powershell/2017/07/Benchmark.html' target = '_blank'>l'article précédent</a> ou sur mon repo : <a href='https://github.com/christophekumor/Measure-MyScript' target = '_blank'>Measure-MyScript Repository
</a>

Chaque test sera fait 5 fois de suite afin de pouvoir faire une moyenne.

## 1. Fichier de 1 mo

Fichier utilisé : une log

 ```powershell

#Array pour stocker les résultats
 $Mytest =@()

#Les Tests
$query01 = Measure-MyScript -ScriptBlock { gc $File } -name 'Get-Content' -Repeat 5

$query02 = Measure-MyScript -ScriptBlock { gc $File -Raw } -name 'Get-Content -Raw' -Repeat 5

$query03 = Measure-MyScript -ScriptBlock { gc $File -ReadCount 0}  -name 'Get-Content -ReadCount' -Repeat 5

$query04 = Measure-MyScript -ScriptBlock { [System.IO.File]::ReadAllLines($File)} -name 'ReadAllLines' -Repeat 5

$query05 = Measure-MyScript -ScriptBlock { [System.IO.File]::ReadLines($File)} -name 'ReadLines' -Repeat 5

$query06 = Measure-MyScript -ScriptBlock { [System.IO.File]::ReadAllText($File)} -name 'ReadAllText' -Repeat 5

$query07 = Measure-MyScript -ScriptBlock { [System.IO.File]::OpenText($File).ReadToEnd()} -name 'OpenText' -Repeat 5

$query08 = Measure-MyScript -ScriptBlock { switch -File $File {default {$PSItem}}} -name 'Switch' -Repeat 5

#Stockage des résulats
$Mytest += $query01
$Mytest += $query02
$Mytest += $query03
$Mytest += $query04
$Mytest += $query05
$Mytest += $query06
$Mytest += $query07
$Mytest += $query08

$Mytest | ConvertTo-JSON


##Résultat affiché :
[
    {
        "name":  "Get-Content",
        "Avg":  "225,5944 Milliseconds",
        "Min":  "160,2451 Milliseconds",
        "Max":  "254,4781 Milliseconds"
    },
    {
        "name":  "Get-Content -Raw",
        "Avg":  "21,9815 Milliseconds",
        "Min":  "8,704 Milliseconds",
        "Max":  "63,6287 Milliseconds"
    },
    {
        "name":  "Get-Content -ReadCount",
        "Avg":  "9,0629 Milliseconds",
        "Min":  "5,2262 Milliseconds",
        "Max":  "14,347 Milliseconds"
    },
    {
        "name":  "ReadAllLines",
        "Avg":  "12,3614 Milliseconds",
        "Min":  "6,2696 Milliseconds",
        "Max":  "29,8394 Milliseconds"
    },
    {
        "name":  "ReadLines",
        "Avg":  "18,7241 Milliseconds",
        "Min":  "6,3518 Milliseconds",
        "Max":  "32,2493 Milliseconds"
    },
    {
        "name":  "ReadAllText",
        "Avg":  "6,1684 Milliseconds",
        "Min":  "2,8069 Milliseconds",
        "Max":  "13,1279 Milliseconds"
    },
    {
        "name":  "OpenText",
        "Avg":  "3,8668 Milliseconds",
        "Min":  "2,3281 Milliseconds",
        "Max":  "6,2222 Milliseconds"
    },
    {
        "name":  "Switch",
        "Avg":  "18,3933 Milliseconds",
        "Min":  "5,4352 Milliseconds",
        "Max":  "38,2238 Milliseconds"
    }
]

```
Je vous laisse exploiter les résultats dans leur ensemble, mais en se basant sur la moyenne de chaque test, voici le top 3 :  

**1** OpenText  
**2** ReadAllText  
**3** Get-Content -ReadCount  

## 2. Fichier de 10 mo
Fichier utilisé : une dll  

Résultat  :
```powershell
[
    {
        "name":  "Get-Content",
        "Avg":  "1082,3159 Milliseconds",
        "Min":  "849,3544 Milliseconds",
        "Max":  "1232,8958 Milliseconds"
    },
    {
        "name":  "Get-Content -Raw",
        "Avg":  "240,8748 Milliseconds",
        "Min":  "209,8534 Milliseconds",
        "Max":  "261,9167 Milliseconds"
    },
    {
        "name":  "Get-Content -ReadCount",
        "Avg":  "127,8985 Milliseconds",
        "Min":  "124,01 Milliseconds",
        "Max":  "131,0417 Milliseconds"
    },
    {
        "name":  "ReadAllLines",
        "Avg":  "341,2046 Milliseconds",
        "Min":  "262,051 Milliseconds",
        "Max":  "375,1091 Milliseconds"
    },
    {
        "name":  "ReadLines",
        "Avg":  "336,7476 Milliseconds",
        "Min":  "258,6543 Milliseconds",
        "Max":  "426,0836 Milliseconds"
    },
    {
        "name":  "ReadAllText",
        "Avg":  "283,3367 Milliseconds",
        "Min":  "196,5505 Milliseconds",
        "Max":  "350,5371 Milliseconds"
    },
    {
        "name":  "OpenText",
        "Avg":  "252,0441 Milliseconds",
        "Min":  "192,9475 Milliseconds",
        "Max":  "312,9283 Milliseconds"
    },
    {
        "name":  "Switch",
        "Avg":  "311,4755 Milliseconds",
        "Min":  "265,3099 Milliseconds",
        "Max":  "367,3169 Milliseconds"
    }
]
```
le top 3 :  

**1** Get-Content -ReadCount  
**2** Get-Content -Raw  
**3** OpenText  

## 2. Fichier de 100 mo
Fichier utilisé : un .msi

Résultat  :
```powershell
[
    {
        "name":  "Get-Content",
        "Avg":  "19121,7832 Milliseconds",
        "Min":  "14082,6904 Milliseconds",
        "Max":  "23007,7092 Milliseconds"
    },
    {
        "name":  "Get-Content -Raw",
        "Avg":  "1496,3492 Milliseconds",
        "Min":  "1155,6948 Milliseconds",
        "Max":  "2323,6894 Milliseconds"
    },
    {
        "name":  "Get-Content -ReadCount",
        "Avg":  "1277,0399 Milliseconds",
        "Min":  "1178,0537 Milliseconds",
        "Max":  "1342,7741 Milliseconds"
    },
    {
        "name":  "ReadAllLines",
        "Avg":  "4508,6765 Milliseconds",
        "Min":  "4395,1866 Milliseconds",
        "Max":  "4626,7495 Milliseconds"
    },
    {
        "name":  "ReadLines",
        "Avg":  "4382,3011 Milliseconds",
        "Min":  "4154,5562 Milliseconds",
        "Max":  "4506,9596 Milliseconds"
    },
    {
        "name":  "ReadAllText",
        "Avg":  "4533,2936 Milliseconds",
        "Min":  "4074,2803 Milliseconds",
        "Max":  "4878,3172 Milliseconds"
    },
    {
        "name":  "OpenText",
        "Avg":  "4714,209 Milliseconds",
        "Min":  "4141,13 Milliseconds",
        "Max":  "5213,4161 Milliseconds"
    },
    {
        "name":  "Switch",
        "Avg":  "4298,6053 Milliseconds",
        "Min":  "4074,2839 Milliseconds",
        "Max":  "4572,0346 Milliseconds"
    }
]
```
le top 3 :  

**1** Get-Content -ReadCount  
**2** Get-Content -Raw  
**3** Switch 

## 2. Fichier de 500 mo
Fichier utilisé : un .vhdx
Résultat  :
```powershell
[
    {
        "name":  "Get-Content",
        "Avg":  "11,7023254 Seconds",
        "Min":  "10,6548748 Seconds",
        "Max":  "13,2257065 Seconds"
    },
    {
        "name":  "Get-Content -Raw",
        "Avg":  "5,9147242 Seconds",
        "Min":  "5,6469605 Seconds",
        "Max":  "6,3595907 Seconds"
    },
    {
        "name":  "Get-Content -ReadCount",
        "Avg":  "4,6402844 Seconds",
        "Min":  "4,3205262 Seconds",
        "Max":  "5,097218 Seconds"
    },
    {
        "name":  "ReadAllLines",
        "Avg":  "5,5661756 Seconds",
        "Min":  "5,499562 Seconds",
        "Max":  "5,6096121 Seconds"
    },
    {
        "name":  "ReadLines",
        "Avg":  "5,4476708 Seconds",
        "Min":  "5,378654 Seconds",
        "Max":  "5,503547 Seconds"
    },
    {
        "name":  "ReadAllText",
        "Avg":  "4,9078279 Seconds",
        "Min":  "4,5778266 Seconds",
        "Max":  "5,15476 Seconds"
    },
    {
        "name":  "OpenText",
        "Avg":  "5,0273475 Seconds",
        "Min":  "4,7988291 Seconds",
        "Max":  "5,173162 Seconds"
    },
    {
        "name":  "Switch",
        "Avg":  "5,5409315 Seconds",
        "Min":  "5,2515487 Seconds",
        "Max":  "5,755654 Seconds"
    }
]
```
le top 3 :  

**1** Get-Content -ReadCount  
**2** ReadAllText  
**3** OpenText 

# Conclusion
Les tests ont tous été fait sur la même machine et dans les mêmes conditions hormis concernant le type de fichier.

Plusieurs facteurs influencent les résultats, la taille des fichiers, mon ssd, mon processeur, ma ram, peut être même le type de fichier...   

Je ne prétends donc pas avec cet article vous donner la méthode la plus rapide, je voulais juste montrer le côté pratique de cette fonction et aussi l'intérêt de faire des tests de rapidité.

Je vous invite à faire les tests de votre côté et partager vos résultats dans les commentaires, ça serait intéressant d'avoir plusieurs retours.