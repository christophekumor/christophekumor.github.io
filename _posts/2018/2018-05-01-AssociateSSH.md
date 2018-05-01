---
layout : post
title: "SSH : associer putty au type d'url 'ssh:\\\\'"
description: "Outil de configuration de windows et lanceur de putty"
permalink:
tags:
   - Powershell
   - Outils
   - SSH
categories:
   - Powershell
   - Outils
   - SSH

image:
 feature: headers/Connexions_1436x500.png
 categorie: categories/Outils_300x300.png
 caption: ""
 teaserlogo:
---

# Introduction

Dans mon entreprise, nous utilisons depuis peu WhatsUp Gold pour la supervision de notre infrastructure, or cet outil a la particularité de générer des liens sous la forme <a href='ssh://user@host.example.com' target = '_blank'>ssh://user@host.example.com</a> pour se connecter à un équipement depuis la map.

# Liens SSH

## 1. Utilisation
De base Windows ne connait pas les liens <a href='SSH://IP' target = '_blank'>SSH://IP</a>.

L'un des outils le plus utilisé pour effectuer des connexions SSH est : <a href='https://www.putty.org/' target = '_blank'>PuTTY</a>. On pourrait donc croire qu'en installant PuTTY, le format d'url SSH serait reconnu automatiquement par Windows.

Que nenni !!

## 2. Pourquoi ?
Car PuTTY ne reconnait pas le formalisme utilisé pour ce genre de liens SSH, PuTTY a fait le choix d'ignorer purement et simplement les liens <a href='SSH:\\IP' target = '_blank'>SSH://IP</a>. 
D'un autre côté, il ne semble pas y avoir de validation officielle pour ce format d'adresse.

### a. Documentations
En effet, on ne trouve que des brouillons de RFC et donc l'<a href='https://fr.wikipedia.org/wiki/Uniform_Resource_Identifier' target = '_blank'>URI (Uniform Resource Identifier)</a> pour SSH (Scheme for Secure Shell) n'est pas validée.

- <a href='https://tools.ietf.org/html/draft-ietf-secsh-scp-sftp-ssh-uri-04' target = '_blank'>draft-salowey-secsh-uri-00.txt</a> 
- <a href='https://tools.ietf.org/id/draft-salowey-secsh-uri-00.html' target = '_blank'>draft-ietf-secsh-scp-sftp-ssh-uri-04.txt</a>

### b. Démo
Donc si je passe la commande suivante : 
```powershell
 .\putty.exe ssh:\\127.0.0.1
```
J'obtiens un joli message d’erreur...

![PuttySSHError](/images/articles/2018-05-01-AssociateSSH/ssherror.png)

## 3. Solutions ?
### a. Quelques pistes

Pour résoudre ce problème, il y a quelques solutions :
- Utiliser un autre outil
- Utiliser une version modifiée de PuTTY
- Utiliser un lanceur pour PuTTY

C’est cette dernière piste que j'ai choisi d'utiliser, un lanceur !! Celui-ci pouvait être dans n'importe quel langage... y compris powershell, mais pour des raisons de rapidité au lancement, j'ai préféré un bon vieux batch DOS.

### b. Le batch DOS

Mais alors pourquoi un article `DOS` sur un blog Powershell ??? Car le lanceur ne fait pas tout, il y a aussi une configuration de Windows à faire et aussi créer le lanceur en fonction du chemin de PuTTY sur votre poste. Donc, j'ai créé un petit outil pour effectuer la mise en place de tout cela et devinez quoi. Cet outil, est en PowerShell !! CQFD

## 4. Le GUI Powershell
### a. L'interface
Petit tour du propriaitaire :

![Interface](/images/articles/2018-05-01-AssociateSSH/OutilInterface.png)

Sur ce screenshot on constate que j'ai déjà une association pour les url SSH : `WinSCP.exe`
En fait, avec WinScp, les liens fonctionnent car WinScp fait office de lanceur pour PuTTY ...

![Interface](/images/articles/2018-05-01-AssociateSSH/OutilInterface1.png)

A l'aide du bouton ![Bouton](/images/articles/2018-05-01-AssociateSSH/OutilInterface2Bouton.png) naviguez jusqu'à l'emplacement de votre putty.exe.

![Interface](/images/articles/2018-05-01-AssociateSSH/OutilInterface2.png)

Il reste à cliquer sur `Start`

>/!\ Attention : l'outil doit être lancé en admin, il va modifier la base de registre et déposer le lanceur à l'endroit de votre putty.exe

![Interface](/images/articles/2018-05-01-AssociateSSH/OutilInterface3.png)

Une fois revenu sur `Ready.` dans la barre de status, on constate que l'association SSH actuelle à changer. Désormais pour les liens <a href='SSH://IP' target = '_blank'>SSH://IP</a> c'est mon lanceur `puttyssh.cmd` qui sera utilisé, et il se chargera de lancer PuTTY avec toutes les bonnes informations.

### b. Les erreurs
![Erreur](/images/articles/2018-05-01-AssociateSSH/OutilError2.png)

>Pas de chemin indiqué pour putty.exe

![Erreur](/images/articles/2018-05-01-AssociateSSH/OutilError3.png)

>Le chemin indiqué pour putty.exe n'existe pas...

![Erreur](/images/articles/2018-05-01-AssociateSSH/OutilError1.png)

>Problème de mise en place de la config, relancez en admin


### c. La compatibilité

Les liens suivants sont gérés :
- ssh://user@host.example.com
- ssh://user@host.example.com:2222
- ssh://user:password@host.example.com
- ssh://user:password@host.example.com:2222
- ssh://user@host.example.com/
- ssh://user@host.example.com:2222/
- ssh://user:password@host.example.com/
- ssh://user:password@host.example.com:2222/

## 5. Le test FINAL
Depuis, par exemple, internet Explorer, je rentre dans la barre d'URL : **SSH://127.0.0.1** +RETURN

And voila ! 

![PuttyOK](/images/articles/2018-05-01-AssociateSSH/PuttyOK.png)

J'espère que l'article vous a plu et que l'outil pourra vous être utile. N'hésitez pas à me laisser des commentaires ainsi que des suggestions. Tout est toujours perfectible...

## 6. Téléchargement

sur mon GitHub : <a href='https://github.com/christophekumor/AssociateSSH' target = '_blank'>AssociateSSH</a>

# Annexe
## 1. Le code DOS
Pour ceux que cela intéresse, voici le contenu du fichier **puttyssh.cmd** sur mon poste. 

Sur votre poste, il sera différent en fonction du chemin de putty.exe

```batchfile
@setlocal
@echo off 

set var=%1

echo.%var%|findstr /C:"@" >nul 2>&1
if not errorlevel 1 (
   echo Found
   for /f "tokens=1,2,3,4 delims=@/\" %%a in ("%var%") do (
set loginFull=%%b
set hostFull=%%c
   )
) else (
    echo Not found.
       for /f "tokens=1,2,3,4 delims=@/\" %%a in ("%var%") do (
set "loginFull="
set hostFull=%%b
       )
)

for /f "tokens=1,2 delims=:" %%a in ("%loginFull%") do set Login=%%a&set pass=%%b

for /f "tokens=1,2 delims=:" %%a in ("%hostFull%") do set ip=%%a&set port=%%b

IF NOT DEFINED port (set port=22)
IF NOT DEFINED login (set login=)
IF NOT DEFINED pass (set pass=)

if DEFINED pass ( 
    set param=-ssh %ip% -P %port% -pw "%pass%" -l "%login%" 
    ) else (
    set param=-ssh %ip% -P %port% -l "%login%" 
)

start /D "C:\Program Files (x86)\PuTTY" putty.exe %param%
)

```



