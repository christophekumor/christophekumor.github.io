---
layout: single
title: "Mettre en place son Blog sur GitHub"
excerpt: "Pas à Pas pour la mise en place d'un blog sur github"
permalink:
tags:
    - draft
categories:
    - GitHub
    - Tutorial
published: true
comments: true
author_profile: false
header:
  image: assets/images/headers/HelloWorld_1436x200.png
  caption: ""
  teaserlogo: 
---
{% include toc title="Table of content" icon="file-text" %}

## Introduction

Je vais essayer dans cet article de vous présenter la mise en place pas à pas d'un blog sur Github.

il n'y aura pas forcément outs les petis details ni de la configuration en profondeur, mais à minima comment creer son blog et qu'il soit hebergé par Github.

Dédicasse à <b>Thierry Degols</b> ;-) j'espere répondre à tes attentes. 

## GO

1. Creation d'un "repository"

Pour creer un blog sur Github la premiere chose à faire est de creer un nouveau "repository"

Sur sa page Github, on se rend donc dans "Repositories" 

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step0a.jpg)


puis on clique sur le bouton "New" :

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step0b.jpg)

On arrive alors dans le process normal de création d'un "repository"

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step1.jpg)


<p><img src="/assets/images/articles/2017-04-28-GitHubBlogInstall/Step1.jpg"></p>

Il faut :
- un nom. Exemple : christophekumor.github.io

il faut respecter ce format de nom cela permetra d'avoir une url en https://christophekumor.github.io

<a href='https://help.github.com/articles/user-organization-and-project-pages/' target = '_blank'>Plus d'informations..
</a>

- Une description : Apparaitra juste dans le "repository" mais c'est toujours plus sympas.
- Choisir public ou Private. Private étant payant. à vous de voir
- Cocher "Initialize this repository with a README" 

Pour finir cliquer sur :

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step2.jpg)

2. petite config

Vous allez revenir sur votre repo fraichement créé, Je vous conseil d'ajouter un lien vers votre blog directement sur la page de votre repo, pour cela dans la barre de Titre du repo, cliquez sur le bouton "Edit":

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step3.jpg)

Ajoutez le lien en https (ne pas faire comme sur la photo ci dessous ou j avais juste mis http) de votre blog, en principe si vous avez bien utilisé le formalisme lors de la déclaration, celui ci doit etre de la forme https://votrenomdecomptegithub.github.io

Il ne reste plus qu'a sauvegarder avec le bouton "Save"

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step4.jpg)

Vous voila revenu sur la page d'acceuil de votre repository, avec un jolie lien vers votre blog.

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step5.jpg)

3. Choix d'un theme

Ce n'est pas fini, il vous faut choisir un theme pour votre site, pour cela toujours dans votre nouveau repository, vous cliquez sur l'onglet "Settings"

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step6.jpg)

Puis vers le bas de la page, cliquez sur "Choose a theme"

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step7.jpg)

Faite votre choix parmis les themes proposés par github via les vignettes du haut de la page, une fois votre choix effectué, cliquez sur le bouton "Select theme"

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step8.jpg)

Il se peut que l'on vous demande de modifier le README.md de votre site, vous pouvez faire cancel et le faire plus tard..

Pour les besoins du tutorial j'ai refait un repo de test, donc ici on me propose de modifier le Readme.md. J'ai donc fait cancel

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step9.jpg)

Une fois revenu sur la page de votre repository, vous avez le message avec le choix du theme qui a été appliqué

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step10.jpg)

4. vérification

Pour vérifier l'url de votre Blog, il faut aller à nouveau dans l'onglet "setting"

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step6.jpg)

Vers le bas de la page, vous aurez l'information de publication de votre site :

![image-center](/assets/images/articles/2017-04-28-GitHubBlogInstall/Step11.jpg)

> ici on voit clairement ce qui ce passe lorsque l'on ne repecte pas le formalisme dont j'ai parlé à l'etape du choix de nom de repository ou lorsqu'un repository "site/blog" existe deja, il va publier votre "site/blog" comme un sous repertoire de votre "site/blog" principal.

And Voila :sparkles: ! vous etes en ligne !!

## CONCLUSION
Github supporte nativement le language Jekyll pour la conception de vos pages ou de vos posts, je n'en ai pas du tout parlé dans ce post car etant moi meme en plein apprentissage, je vais avoir du mal à donner des conseils..

En revanche voici le lien vers le site officiel : 

<a href='https://jekyllrb.com/' target = '_blank' alt = 'Lien vers le site de jekyll'>![image-center](/assets/images/jekyll.png)</a>


