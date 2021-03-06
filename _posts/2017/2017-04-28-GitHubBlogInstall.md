---
layout: post
title: "Mettre en place son Blog sur GitHub"
description: "Pas à Pas pour la mise en place d'un blog sur github"
permalink:
tags:
   - Blog
   - Github
categories:
   - GitHub
   - Tutorial
image:
 feature: headers/GithubPages_1436x500.png
 categorie: categories/Github_300x300.png
 caption: ""
 teaserlogo:
---

# Introduction

Je vais essayer dans cet article de vous présenter la mise en place pas à pas d'un blog sur Github.

il n'y aura pas forcément outs les petis details ni de la configuration en profondeur, mais à minima comment créer son blog et qu'il soit hébergé par Github.

Dédicace  à <b>Thierry Degols</b> ;-) j'espère répondre à tes attentes.

# Création

## 1. Création d'un "repository"

Pour créer un blog sur Github la première chose à faire est de créer un nouveau "repository"

Sur sa page Github, on se rend donc dans "Repositories"

![Select Repository](/images/articles/2017-04-28-GitHubBlogInstall/Step0a.jpg)


puis on clique sur le bouton "New" :

![New](/images/articles/2017-04-28-GitHubBlogInstall/Step0b.jpg)

On arrive alors dans le processus normal de création d'un "repository"

![Create a new repository](/images/articles/2017-04-28-GitHubBlogInstall/Step1.jpg)


Il faut :
- un nom. Exemple : christophekumor.github.io

il faut respecter ce format de nom cela permetra d'avoir une url en https://christophekumor.github.io <a href='https://help.github.com/articles/user-organization-and-project-pages/' target = '_blank'>Plus d'informations..
</a>

- Une description : Apparaîtra juste dans le "repository" mais c'est toujours plus sympas.
- Choisir public ou Private. Private étant payant. à vous de voir
- Cocher "Initialize this repository with a README"

Pour finir cliquer sur :

![Cliquez sur le bouton Create repository](/images/articles/2017-04-28-GitHubBlogInstall/Step2.jpg)

## 2. Petite config

Vous allez revenir sur votre repo fraîchement créé, Je vous conseil d'ajouter un lien vers votre blog directement sur la page de votre repo, pour cela dans la barre de Titre du repo, cliquez sur le bouton "Edit":

![Bandeau](/images/articles/2017-04-28-GitHubBlogInstall/Step3.jpg)

Ajoutez le lien en https (ne pas faire comme sur la photo ci dessous ou j'avais juste mis http) de votre blog, en principe si vous avez bien utilisé le formalisme lors de la déclaration, celui ci doit etre de la forme https://votrenomdecomptegithub.github.io

Il ne reste plus qu'a sauvegarder avec le bouton "Save"

![Edit your site link](/images/articles/2017-04-28-GitHubBlogInstall/Step4.jpg)

Vous voilà revenu sur la page d'accueil de votre repository, avec un jolie lien vers votre blog.

![Résultat de l'éditon](/images/articles/2017-04-28-GitHubBlogInstall/Step5.jpg)

## 3. Choix d'un thème

Ce n'est pas fini, il vous faut choisir un thème pour votre site, pour cela toujours dans votre nouveau repository, vous cliquez sur l'onglet "Settings"

![Settings](/images/articles/2017-04-28-GitHubBlogInstall/Step6.jpg)

Puis vers le bas de la page, cliquez sur "Choose a theme"

![Cliquez sur le bouton Choose a theme](/images/articles/2017-04-28-GitHubBlogInstall/Step7.jpg)

Faite votre choix parmis les thèmes proposés par github via les vignettes du haut de la page, une fois votre choix effectué, cliquez sur le bouton "Select thème"

![Choisissez un theme](/images/articles/2017-04-28-GitHubBlogInstall/Step8.jpg)

Il se peut que l'on vous demande de modifier le README.md de votre site, vous pouvez faire cancel et le faire plus tard..

Pour les besoins du tutorial j'ai refait un repo de test, donc ici on me propose de modifier le Readme.md. J'ai donc fait cancel

![Cancel Edition README.md](/images/articles/2017-04-28-GitHubBlogInstall/Step9.jpg)

Une fois revenu sur la page de votre repository, vous avez le message avec le choix du thème qui a été appliqué

![Information theme appliqué](/images/articles/2017-04-28-GitHubBlogInstall/Step10.jpg)

## 4. Vérification

Pour vérifier l'url de votre Blog, il faut aller à nouveau dans l'onglet "setting"

![Settings](/images/articles/2017-04-28-GitHubBlogInstall/Step6.jpg)

Vers le bas de la page, vous aurez l'information de publication de votre site :

![Lien de votre site](/images/articles/2017-04-28-GitHubBlogInstall/Step11.jpg)

> ici on voit clairement ce qui se passe lorsque l'on ne respecte pas le formalisme dont j'ai parlé à l'étape du choix de nom de repository ou lorsqu'un repository "site/blog" existe déjà, il va publier votre "site/blog" comme un sous répertoire de votre "site/blog" principal.

And Voila ! vous êtes en ligne !!

# CONCLUSION
Github supporte nativement le langage Jekyll pour la conception de vos pages ou de vos posts, je n'en ai pas du tout parlé dans ce post car etant moi même en plein apprentissage, je vais avoir du mal à donner des conseils..

En revanche voici le lien vers le site officiel :

<a href='https://jekyllrb.com/' target = '_blank' alt = 'Lien vers le site de jekyll'>![Site de Jekyll](/images/jekyll.png)</a>