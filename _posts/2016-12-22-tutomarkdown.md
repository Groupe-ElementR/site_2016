---
layout: post
title: "Comment écrire des billets pour ce blog?"
subtitle: "Hadrien Commenges"
tags: [markdown, git, knitr]
---



Ce blog est la page github.io du Groupe ElementR. Il rend possible la mise en ligne de billets directement produit par une fichier RMarkdown (.Rmd). Voici quelques conseils pour savoir comment procéder : (1) l'essentiel pour ceux qui maîtrisent déjà **RMarkdown** et **GitHub** ; (2) un point plus détaillé pour ceux qui ne connaissent pas **RMarkdown** ; (3) un point plus détaillé pour ceux qui ne connaissent pas **git** et **GitHub**.

## Comment publier un billet en deux minutes

**Pour faire un billet sans code R**, écrire directement un document Markdown, le coller dans le répertoire `_posts`, coller les images dans le répertoire `img` et pusher.

**Pour faire un billet avec du code R** :

- Concevoir un document RMarkdown à partir de [ce modèle](https://raw.githubusercontent.com/Groupe-ElementR/Groupe-ElementR.github.io/master/owndata/Template.Rmd)
- Tricoter (knit) le document avec le package `knitr`:


```r
knit(input = "chemin/fichier.Rmd", output = "chemin/fichier.md")
```

- Nommer le document .md de la façon suivante : `aaaa-mm-jj-montitre.md`, par exemple `2016-10-15-condorcet.md`
- Coller le document .md dans le dossier `_posts` et coller les images dans le répertoire `img`
- Faire un `commit` et un `push` sur le dépot

> IMPORTANT : (1) écrire avec un encodage UTF-8 ; (2) donner un nom à tous chunks qui produisent des graphiques.


## Création de documents dynamiques avec R et markdown

### Qu'est-ce qu'un document dynamique ? 

L'utilisateur de R peut visualiser des résultats numériques et graphiques, mais il a souvent besoin d'exporter ces sorties pour les intégrer dans des documents rédigés. Deux types de flux de travail (*workflow*) peuvent être distingués. Le plus classique consiste à découpler l'écriture du texte et la production des sorties numériques ou graphiques : l'utilisateur produit un graphique avec R, l'exporte dans un format d'image et l'insère dans un logiciel de traitement de texte avec lequel il écrit le contenu. L'autre façon de procéder, de plus en plus développée, s'inscrit dans la vague de la *reproducible research*. Elle consiste à combiner l'écriture des contenus textuels et des traitements numériques et graphiques pour générer un document tout-en-un. Ce flux de travail est souvent qualifié de *dynamic report generation* ou parfois (de façon abusive) *literate programming*.

Pour produire un document qui fourmille de traitements et de graphiques, le document dynamique, qui combine l'écriture et les traitements dans un même document, est très pratique. R et RStudio disposent d'outils très pratiques pour cela grâce au *package* `knitr` (voir [le site dédié](http://yihui.name/knitr)). Dans l'interface de RStudio, le menu `File~>~New file` propose plusieurs solutions : `R Sweave` pour une combinaison avec Latex (le manuel *R et espace* est écrit de la sorte), `R Markdown`, `R HTML` et `R Presentation` pour produire des documents écrits en markdown et en HTML. À la date d'écriture de ces lignes, la version de RStudio la plus récente (version 0.98.1062) permet d'écrire un document en markdown et de compiler le texte et le code pour obtenir une sortie en pdf, en HTML et en docx (Microsoft Word).

Cette première section présente l'utilisation de R avec Markdown pour intégrer les contenus textuels, les scripts et les sorties numériques ou graphiques de ces scripts. Il s'agit d'écrire le texte en Markdown et le code en R, ensuite le *package* `knitr` se charge de tout compiler et de créer le document.


### Qu'est-ce que Markdown ?

[Markdown](https://fr.wikipedia.org/wiki/Markdown) est le plus simple des langages à balises. Sur des logiciels comme MS Word ou LibreOffice Writer, l'utilisateur voit le résultat final mais les commandes de mise en forme lui sont invisible (logiciels WYSIWYG - *What You See Is What You Get*). Avec Markdown, Latex ou HTML, l'utilisateur écrit et voit les commandes de mise en forme mais pas directement le résultat final.

Trois avantages à utiliser Markdown plutôt que Latex : l'installation est très simple, quel quel soit le système opérationnel ; le langage est très simple et s'apprend en cinq minutes ; la transformation en d'autres formats (pdf, docx, HTML) est très simple. Bref, tout est très simple !



### Installation et aide

Pour travailler avec R + Markdown :

- Utiliser RStudio et une version récente de RStudio (version 0.98.1028 actuellement). Ce n'est pas la seule manière, mais RStudio intègre de nombreuses fonctionnalités destinées à cet usage.
- Installer les *packages* suivants : `knitr, yaml, htmltools, caTools, bitops, rmarkdown`. Dans RStudio, en essayant de créer un fichier Markdown pour la première fois (`File > New file > R Markdown`), ces *packages* sont installés automatiquement.
- Créer ou ouvrir un fichier .Rmd (`File > New file > R Markdown`). Le résultat peut être visualisé en HTML, pdf et docx. Le plus simple et rapide est de produire une sortie HTML, pour produire des documents pdf et docx il faut des installations supplémentaires. Pour visualiser, cliquer sur `Knit HTML`

Des références pour la syntaxe Markdown : 

- sur la page Wikipedia [en français](https://fr.wikipedia.org/wiki/Markdown) ou [en anglais](https://en.wikipedia.org/wiki/Markdown)
- sur le [site de RStudio](http://rmarkdown.rstudio.com)
- sur plusieurs sites web, par exemple le [site de John Gruber](http://daringfireball.net/projects/markdown/syntax)

La référence pour l'utilisation du *package* `knitr` qui se charge de la compilation est le [site de Yihui](http://yihui.name/knitr/options)


### Le codage des caractères

Pour éviter des migraines au moment de mettre en forme des textes écrits par plusieurs utilisateurs sur des systèmes opérationnels différents, **il faut absolument utiliser le codage de caractères UTF-8 pour la rédaction du fichier .Rmd** (cf. <http://fr.wikipedia.org/wiki/UTF-8>). Dans RStudio, ce réglage se fait à `Tools > Global options > General > Default text encoding`. 


### Utilisation


Concernant l'intégration des traitements et des sorties dans le document, le fonctionnement est le suivant : on crée des *chunks* (des morceaux ?) comme ci-dessous dans lesquels on écrit le code qui doit être interprété par R. On crée le ckunk soit en cliquant sur l'onglet `Chunks > Insert chunk`, soit en tapant `CTRL+ALT+I`, soit en copiant-collant un *chunk* existant.

Le *chunk* sans option se présente ainsi :

![](/img/ScreenShotChunk.png")

Dans ce premier exemple minimal, voici les réglages par défaut de plusieurs arguments qui peuvent être modifiés :

- le *chunk* n'a pas de nom. Le nom vient juste après la lettre `r`, séparé par un espace. Si plusieurs *chunks* partagent le même nom, le fichier ne peut pas être compilé.
- le code R est interprété (`eval`)
- le code est affiché dans la sortie (`echo`)
- le résultat numérique ou graphique est affiché dans la sortie (`results` et `fig.show`)
- le résultat des traitements est mis en mémoire cache (`cache`)

En général, il est conseillable de toujours nommer le *chunk*. Si le *chunk* produit un graphique, il est absolument nécessaire de le nommer parce qu'il donnera son nom au fichier image produit. Sans spécifier de noms, les graphiques s'appelleront `unnamed-chunk-1.png`,  `unnamed-chunk-2.png`, etc. 

L'autocomplétion avec la touche `Tab` fonctionne aussi dans ce cadre (d'où l'intérêt d'utiliser RStudio) : se placer dans l'entête du *chunk* après une virgule et taper `Tab` pour afficher tous les arguments. Voici quelques exemples utiles :

- afficher du code sans l'exécuter : `eval=FALSE`
- afficher la sortie mais masquer le code : `echo=FALSE`
- afficher le code mais masquer la sortie numérique : `results='hide'`
- afficher le code mais masquer la sortie graphique : `fig.show='hide'`
- ne pas conserver les résultats en cache : `cache=FALSE`

Attention avec le cache : lors de la rédaction du document, l'utilisateur compile régulièrement le fichier pour voir le rendu final. À chaque compilation, le résultat des traitements est mis par défaut en mémoire cache pour éviter de ré-exécuter des traitements déjà exécutés et diminuer le temps de compilation. C'est très utile mais ça peut être source de bugs. 

Toute modification dans l'entête du *chunk* (changer le nom du *chunk*, ou changer une option) ou dans le corps du *chunk*, les modifications sont prises en compte et le résultat (un graphique par exemple) écrase la version antérieure. Ceci peut poser problème si on fait une modification dans un autre *chunk* (modification d'une valeur par exemple) qui affecte un graphique créé par ailleurs et mis dans le cache. Dans ce cas, le graphique ne serait pas créé à nouveau et ne prendra pas en compte le modification de la valeur. 

On peut forcer à tout ré-exécuter à chaque compilation. Cependant, le plus pratique est de travailler avec l'option par défaut qui met les résultats dans la mémoire cache et de s'assurer de temps en temps que tout fonctionne en faisant une compilation "fraîche" : supprimer les dossiers qui stockent tous ces résultats ("chemin/Dossiercache" et "chemin/Dossierfiles").


## Git, Github et le contrôle de versions

[Git](https://fr.wikipedia.org/wiki/Git) est un logiciel de contrôle de versions créé par Linus Torvald. [GitHub](https://fr.wikipedia.org/wiki/GitHub) est une plateforme qui héberge des projets gérés avec git.

Les outils git et GitHub sont utilisés pour développer de gros projets logiciels, par exemple Boostrap (framework utilisé pour ce blog) et RStudio (environnement de développement utilisé pour produire les documents RMarkdown). Pour publier des billets de blog il suffira de connaître quelques termes et commandes simples, en suivant par exemple [ce tutoriel](http://christopheducamp.com/2013/12/15/github-pour-nuls-partie-1).

