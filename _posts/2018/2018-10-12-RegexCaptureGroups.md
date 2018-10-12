---
layout: post
title: "Regex : les captures groupe avec .Net"
description: "Expressions régulières, résultat de groupes avec le moteur .Net"
permalink:
tags:
- Powershell
- Regex
- .Net
categories:
- Powershell
- Regex
- .Net

image:
  feature: headers/Coding_1436x500.png
  categorie: categories/PS_300x300.png
  caption: ""
  teaserlogo:
---

# Introduction

Dans cet article, je vais vous parler d'Expressions régulières communément appelées regex ou regexp.

Pour la suite de cet article, je vais considérer que vous savez manipuler et utiliser les expressions régulières. Je ferai d'autres articles sur les REGEXs, mais aujourd’hui, je souhaite parler d'une particularité du moteur d'expressions régulières .Net

# Capturer des groupes

Pour le début de cette démonstration, je vais utiliser le moteur d'expressions régulières natif à powershell.

Vous savez que l'on peut capturer un groupe dans une chaine de caractère.

Ci-dessous un exemple dans lequel je vais récupérer un groupe :

```Powershell
$string = 'param1;param2;param3;param4;param5;'

$string -match '([a-z0-9]+);'

$matches

Name                           Value                                                                         
----                           -----                                                                         
1                              param1
0                              param1;
```

Dans cet exemple il n'y a qu'un seul groupe demandé, celui ci est bien délimité par les parenthèses `()`. Le moteur de regex Powershell me renvoie donc la valeure du groupe trouvé lors du **premier match** dans ma chaine de caractères.

- groupe : param1

Cela correspond bien à ma demande.

Dans ma chaine de caractères, il y a une répétition de ma recherche, je vais donc indiquer dans ma REGEX que mon pattern se répète plusieurs fois.

```Powershell
$string = 'param1;param2;param3;param4;param5;'

$string -match '(?:([a-z0-9]+);)+'

$matches

Name                           Value
----                           -----
1                              param5
0                              param1;param2;param3;param4;param5; 
```

Le résultat contient cette fois-ci le groupe du **dernier match** du pattern dans ma chaine de caractères.

- groupe : param5

Je pense que comme moi, vous vous attendiez à avoir le groupe de chaque match.

```Powershell
Résultat espéré :

Match 1
groupe : param1

Match 2
groupe : param2

Match 3
groupe : param3

Match 4
groupe : param4

Match 5
groupe : param5
```
y a-t-il une erreur dans ma regex ? 

On peut se rendre compte que mon expression régulière est bonne, car elle renvoie correctement en `Matches[1]` une des valeurs attendues ( `param5` ) et le `Matches[0]` me renvoie bien la totalité de ma chaine de caractères qui match avec la REGEX.

En fait, en cas de **quantification** d'un pattern, le moteur d'expressions régulières de powershell comme **tous** les moteurs d'expressions régulières (PHP,JAVA,PERL, PYTHON, etc..) ne créent pas plusieurs groupes de capture, mais au lieu de cela, ils renvoient le dernier groupe correspondant à la capture. 

### Passons maintenant à la raison d'être de cet article !

Avec le moteur regex .NET, tout cela change ! En effet le moteur .NET est le seul à garder une trace de toutes les captures qu'il a faites lors des matchs.

De façon normale, le moteur .NET indiquera toujours que le groupe capturé est `param5`. Mais si vous creusez plus profondément dans ce groupe, .Net retournera également une collection avec toutes les valeurs que le groupe a capturées successivement compte tenu du quantificateur **`+`**.

Cette fonctionnalité change tout !! Elle vous permet d'analyser facilement les chaînes avec un nombre **inconnu** de jetons.

Faisons un test avec le moteur .Net :

```Powershell
$string = 'param1;param2;param3;param4;param5;'

$resultat = [regex]::Match($string ,'(?:([a-z0-9]+);)+')
``` 
Regardons la différence entre le moteur powershell et le moteur .Net

```Powershell
#Powershell
$Matches.GetType()

IsPublic IsSerial Name                                     BaseType                                          
-------- -------- ----                                     --------                                          
True     True     Hashtable                                System.Object

#.Net
$Resultat.GetType()

IsPublic IsSerial Name                                     BaseType                                          
-------- -------- ----                                     --------
True     True     Match                                    System.Text.RegularExpressions.Group              
```
Regardons de plus près la variable **`$Resultat`**

```Powershell
$Resultat | gm

TypeName : System.Text.RegularExpressions.Match

Name        MemberType Definition                                                      
----        ---------- ----------                                                      
Equals      Method     bool Equals(System.Object obj)                                  
GetHashCode Method     int GetHashCode()                                               
GetType     Method     type GetType()                                                  
NextMatch   Method     System.Text.RegularExpressions.Match NextMatch()                
Result      Method     string Result(string replacement)                               
ToString    Method     string ToString()                                               
Captures    Property   System.Text.RegularExpressions.CaptureCollection Captures {get;}
Groups      Property   System.Text.RegularExpressions.GroupCollection Groups {get;}    
Index       Property   int Index {get;}                                                
Length      Property   int Length {get;}                                               
Name        Property   string Name {get;}                                              
Success     Property   bool Success {get;}                                             
Value       Property   string Value {get;}  

```

Suite à ce résultat, nous allons utiliser la propriété `Groups` afin d'obtenir le ou les groupes renvoyés par notre recherche.

```Powershell
Groups      Property   System.Text.RegularExpressions.GroupCollection Groups {get;} 
```

Afin de bien comprendre, je vais refaire l'exercice en mettant en comparaison les resultats du moteur powershell.

```Powershell
$string = 'param1;param2;param3;param4;param5;'

#Powershell
$string -match '(?:([a-z0-9]+);)+'

  $matches.Count
  ## on obtient -> 2
  ## il n'y a donc que 2 index possibles 0 et 1

  $matches[0]
  # on obtient -> param1;param2;param3;param4;param5;

  $matches[1]
  # on obtient -> param5

#.Net
$resultat = [regex]::Match($string ,'(?:([a-z0-9]+);)+')

  $resultat.Groups.Count
  # on obtient -> 2
  #il n'y a donc que 2 index possibles 0 et 1

  $resultat.Groups[0].Value
  # on obtient -> param1;param2;param3;param4;param5;

  $resultat.Groups[1].Value
  # on obtient -> param5
```

Nous avons donc exactement le meme résultat avec les 2 méthodes.

Oui, mais....

Faisons un petit focus sur le résultat du groupe. Pour cela, nous allons regarder la structure de l'object `$resultat.Groups[1]`

```Powershell
$resultat.Groups[1] | gm

  TypeName : System.Text.RegularExpressions.Group

Name        MemberType Definition                                                      
----        ---------- ----------                                                      
Equals      Method     bool Equals(System.Object obj)                                  
GetHashCode Method     int GetHashCode()                                               
GetType     Method     type GetType()                                                  
ToString    Method     string ToString()                                               
Captures    Property   System.Text.RegularExpressions.CaptureCollection Captures {get;}
Index       Property   int Index {get;}                                                
Length      Property   int Length {get;}                                               
Name        Property   string Name {get;}                                              
Success     Property   bool Success {get;}                                             
Value       Property   string Value {get;}   
```
Parmi les méthodes et propriétés, celle qui va nous intéresser et qui est introduite par le moteur .Net est : 

```Powershell
Captures    Property   System.Text.RegularExpressions.CaptureCollection Captures {get;}
```

Cette propriété va nous permettre d'obtenir toutes les valeurs que le groupe recherché a prises au fur et à mesure de l'analyse de la chaine de caractères par le moteur REGEX .NET

### Démonstration :

```Powershell
$string = 'param1;param2;param3;param4;param5;'

$resultat = [regex]::Match($string ,'(?:([a-z0-9]+);)+')

  $resultat.Groups[1].Captures.Count
  # on obtient -> 5
  #il y a donc 5 index possibles : 0 1 2 3 4

  $resultat.Groups[1].Captures[0].Value
  # on obtient -> param1

  $resultat.Groups[1].Captures[1].Value
  # on obtient -> param2

  $resultat.Groups[1].Captures[2].Value
  # on obtient -> param3

  $resultat.Groups[1].Captures[3].Value
  # on obtient -> param4

  $resultat.Groups[1].Captures[4].Value
  # on obtient -> param5
```
And voila !

On retrouve toutes les valeurs prises par le groupe et l'on obtient ainsi toutes les valeurs que nous voulions récupérer.

# Conclusion
Cette propriété `Captures` ouvre la porte à de jolies possibilités en termes de scripting.
J'ai pris dans cet article un exemple simple pour la démonstration, mais on peut aller beaucoup plus loin avec une expression régulière plus complexe.

 Cela montre encore une fois la puissance des expressions régulières ainsi que celle du moteur .Net, à saluer au passage.