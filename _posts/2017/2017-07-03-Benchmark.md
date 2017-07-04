---
layout : post
title: "Benchmark ou comment chronométrer"
description: "Comment tester le temps d'exécution de votre code"
permalink:
tags:
   - Powershell
   - Astuces
   - Les Bases
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
Je suis un gros fan d'optimisation, et qui dit optimisation, dit test de rapidité.  
Pour faire mes tests j'utilise principalement 3 méthodes que je vais vous décrire dans la suite de ce billet.

# Méthodes

## 1. Le chronomètre
La première méthode peut être comparée à un chronomètre, on le déclenche au début, puis on le stop à la fin, et on affiche le temps

```powershell
$Debut = (Get-Date)

< YOUR CODE HERE >

$Fin = (Get-Date)

'Temps écoulé : {0} secondes' -f ($Fin-$Debut).totalseconds
```

Ce qui donne par exemple :

```powershell
PS C:\> $Debut = (Get-Date)

sleep 10

$Fin = (Get-Date)

'Temps écoulé : {0} secondes' -f ($Fin-$Debut).totalseconds

#Résultat affiché :
Temps écoulé : 10.0050553 secondes
```

Si vous faites plusieurs tests avec cette méthode, il faudra rajouter des variables afin de collecter les résultats

Exemple :

```powershell
#1er test
$Debut = (Get-Date)
sleep 2
$Fin = (Get-Date)
$FirstTest = ($Fin-$Debut).TotalSeconds

#2eme test
$Debut = (Get-Date)
sleep 5
$Fin = (Get-Date)
$SecondTest = ($Fin-$Debut).TotalSeconds

#Affichafe résultat
"Temps écoulé 1er Test : $FirstTest secondes"
"Temps écoulé 2eme Test : $SecondTest secondes"
```

## 2. La commande Powershell

Pour la deuxième méthode j'utilise une commande powershell spécialement conçu pour ce genre de test : `Measure-Command`

```powershell
(Measure-Command{ <YOUR CODE HERE> }).TotalSeconds
```

En pratique :

```powershell
PS C:\> (Measure-Command{ sleep 5 }).TotalSeconds

#Résultat affiché :
5,0017303
```

Comme pour le chronomètre, si vous faites plusieurs tests, il vous faudra passer par des variables afin de stocker les résultats.

Cette fois je vais faire un peu différemment et stocker mes temps dans une `hashtable`

```powershell

#création de la hashtable
$result = @{}

#mes tests
$query01 = (Measure-Command{ <YOUR CODE HERE> }).TotalSeconds
$query02 = (Measure-Command{ <YOUR CODE HERE> }).TotalSeconds
$query03 = (Measure-Command{ <YOUR CODE HERE> }).TotalSeconds

#Ajout des résultats dans la hashtable
$result.Add("Test 1", "$query01")
$result.Add("Test 2", "$query02")
$result.Add("Test 3", "$query03")

#Affichage des résultats trié par temps écoulé 
$result.GetEnumerator() | Sort-Object Value
```

En pratique :

```powershell
#création de la hashtable
$result = @{}

#mes tests
$query01 = (Measure-Command{ sleep 2 }).TotalSeconds
$query02 = (Measure-Command{ sleep 5 }).TotalSeconds
$query03 = (Measure-Command{ sleep 1 }).TotalSeconds

#Ajout des résultats dans la hashtable
$result.Add("Test 1", "$query01")
$result.Add("Test 2", "$query02")
$result.Add("Test 3", "$query03")

#Affichage des résultats
$result.GetEnumerator() | Sort-Object Value

#Résultat affiché :
Name                           Value                   
----                           -----                   
Test 3                         1.0005092               
Test 1                         2.0006838               
Test 2                         5.0003401 
```

Voilà j'ai mon classement de la plus rapide à la plus lente.  
La hashtable peut être utilisée avec la 1ere méthode aussi, elle a l'avantage de pouvoir trier à la fin les résultats afin d'avoir le plus rapide en 1er.

>La méthode employée pour créer et alimenter la hashtable n'est pas la plus rapide, mais la plus simple pour les débutants. Si vous le souhaitez dans un prochain billet, j'exposerai différentes méthodes de création et d'utilisation des hashtables.

## 2. La classe .Net
Pour cette troisième et dernière méthode , je vais utiliser une class .Net : `Stopwatch  `  

Ce qui donne :

```powershell
$sw = New-Object Diagnostics.Stopwatch

$sw.Start()

< YOUR CODE HERE >

$sw.Stop()

$sw.Elapsed
```

En pratique :

```powershell
$sw = New-Object Diagnostics.Stopwatch
$sw.Start()
sleep 2
$sw.Stop()
$sw.Elapsed

#Résultat affiché :

Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 2
Milliseconds      : 1
Ticks             : 20019911
TotalDays         : 2,3171193287037E-05
TotalHours        : 0,000556108638888889
TotalMinutes      : 0,0333665183333333
TotalSeconds      : 2,0019911
TotalMilliseconds : 2001,9911

```

A partir de cette méthode j'ai fait une fonction afin de pouvoir tester du code de façon plus industriel et aussi, par exemple, faire plusieurs fois le même test afin d'avoir une moyenne, un minimun et un maximun.

Avant d'afficher la fonction, voici le résultat

```powershell
#Array pour stocker les résultats
$test =@()

#les tests
$t1 = Measure-MyScript -ScriptBlock { Start-Sleep 1 } -name 'test1' -Repeat 2 -Unit s
$t2 = Measure-MyScript -ScriptBlock { Start-Sleep 3 } -name 'test2' -Repeat 2 -Unit s
$t3 = Measure-MyScript -ScriptBlock { Start-Sleep 5 } -name 'test3' -Repeat 3 -Unit s

#Stockage des résulats
$test += $t1
$test += $t2
$test += $t3

#Vue sur les résultats. 
#Le ConvertTo-JSON est là uniquement pour afficher le résultat dans le terminal.
$test | ConvertTo-JSON

#Résultat affiché :
[
    {
        "test1":  {
                      "Min":  "1,0000809 Seconds",
                      "Max":  "1,0001528 Seconds",
                      "Avg":  "1,0001168 Seconds"
                  }
    },
    {
        "test2":  {
                      "Min":  "2,9998739 Seconds",
                      "Max":  "3,0003697 Seconds",
                      "Avg":  "3,0001218 Seconds"
                  }
    },
    {
        "test3":  {
                      "Min":  "5,0001505 Seconds",
                      "Max":  "5,00086 Seconds",
                      "Avg":  "5,0004759 Seconds"
                  }
    }
]

```

Voici maintenant la fonction `Measure-MyScript`, c'est un 1er jet, je viens juste de la faire pour cet article. N'hesitez pas à commenter si vous avez des améliorations à apporter, ou tout autres remarques..

```powershell
function Measure-MyScript {
    <#
.SYNOPSIS
    Measure speed execution of a scriptblock

.DESCRIPTION
   Measure speed execution of a scriptblock, use it to optimze your code.

.PARAMETER Name
    Specifies the name of the test

.PARAMETER ScriptBlock
    Specifies the name of your script block or put your code here.

.PARAMETER Repeat
    Specifies the numbers of time you want the test run   

.PARAMETER Unit
    Specifies the unit for the result. 
    Accepted : d,h,m,s,ms 
    for Days,hours,minutes,seconds,milliseconds

.EXAMPLE
    Measure-MyScript -Name 'Montest' -ScriptBlock { Start-Sleep 2 }

    Will execute Sleep for 2 secondes and give you the result in milliseconds associed to 'Montest' name

.EXAMPLE
     Measure-MyScript -Name 'Montest' -ScriptBlock { Start-Sleep 2 } -name 'mon test' -Unit 's'

    Will execute Sleep for 2 secondes, and give you the result in seconds associed to 'Montest' name

.EXAMPLE
     Measure-MyScript -Name 'Montest' -ScriptBlock { Start-Sleep 2 } -Repeat 5 -Unit 's'

    Will execute Sleep for 2 secondes, 5 times, and give you the miniminal,maximun and average time in seconds

 
.NOTES
    Christophe Kumor
    https://christophekumor.github.io 


#>

    [CmdletBinding()]
    param (
        [string]$Name = "Test", 

        [Parameter(Mandatory = $true)]
        [ScriptBlock]$ScriptBlock, 

        [ValidateScript({ if (@('d','h','m','s','ms') -contains $_) {$true} else {throw "$_ is not supported. Autorised : d,h,m,s,ms ->Days,hours,minutes,seconds,milliseconds"} })]
        [String]$Unit = 'ms',
        
        [int]$Repeat = 1
    )


    if (-not $PSBoundParameters['Name']) {
        $Name = ('{0}{1}' -f $Name, $Repeat)

    }

    $timings = @()
    do {
        $sw = New-Object Diagnostics.Stopwatch
        if ($PSBoundParameters['Verbose']) {

     
            $sw.Start()
    
            try {
                &$ScriptBlock
            }
   
            catch {
                return $_.Exception.Message
            }  
    
            $sw.Stop()

        }
        else {
            $sw.Start()
            try {
                $null = &$ScriptBlock
            }
   
            catch {
                return $_.Exception.Message
            } 
            $sw.Stop()
            Write-Host "o." -NoNewLine
        }
   

        $timings += $sw.Elapsed
    
        $Repeat--
    }
    while ($Repeat -gt 0)
  
    $stats = $timings | Measure-Object -Average -Minimum -Maximum -Property Ticks
  
      switch ($Unit)
      {
        d            { $u = 'TotalDays';$msg='Days'; break}
        h            { $u = 'TotalHours';$msg='Hours'; break}
        m            { $u = 'TotalMinutes';$msg='Minutes'; break}
        s            { $u = 'TotalSeconds';$msg='Seconds'; break}
        ms            { $u = 'TotalMilliseconds';$msg='Milliseconds'; break}
        default      { $u = 'TotalMilliseconds';$msg='Milliseconds'}
      }
   


    if ($PSBoundParameters['Repeat'] -gt 1) { 

            $hash = @{
                Avg = "{0} {1}" -f (New-Object System.TimeSpan $stats.Average).$u, $msg
                Min   = "{0} {1}" -f (New-Object System.TimeSpan $stats.Minimum).$u, $msg
                Max  = "{0} {1}" -f (New-Object System.TimeSpan $stats.Maximum).$u, $msg
                }
            
            $hash2=@{
            $name = $hash
            }
              



    }
   else {
       
            $hash2=@{
            $name = "{0} {1}" -f (New-Object System.TimeSpan $stats.Average).$u, $msg
            }

       
    } 
    return $hash2;
}
```

# Conclusion

And Voilà ! (Oui je sais..., mais j'aime bien cette expression)

A vos chronomètres ! testez votre code et partagez vos trouvailles en termes d'optimisation.



