# Examen-IA

## Description:

Les Réseaux Adversaires Génératifs (GAN) génèrent un image suivant une distribution apprise en utilisant un vecteur aleatoire dans un
un espace latent. Les GAN se sont avérés être des outils de synthèse puissants mais difficiles à manipuler. L'adversaire génératif convolutionnel profond
Réseau (DCGAN) GAN entièrement convolutif qui a atteint l'état des resultats artistiques lors de sa sortie en 2016. 
des résultats artistiques lors de sa sortie en 2016. 

###  Definition:

Un réseau antagoniste génératif (GAN) est une classe de système d'apprentissage automatique inventé par Ian Goodfellow en 2014. Deux réseaux de neurones se font concurrence dans un jeu. Étant donné un ensemble d'apprentissage, cette technique apprend à générer de nouvelles données avec les mêmes statistiques que l'ensemble d'apprentissage.
Dans cette compétition, vous entraînerez des modèles génératifs pour créer des images de chiens. Seulement cette fois… il n'y a pas de données de vérité terrain à prédire. Ici, vous soumettrez les images et serez notés en fonction de la qualité de ces images comme des chiens provenant de réseaux de neurones pré-entraînés. Prenez ces images, par exemple. Pouvez-vous dire lesquels sont réels ou générés?

<p align='center'>
<img width="365" alt="fid" src="https://user-images.githubusercontent.com/72967454/100002925-9a920480-2dc5-11eb-8684-fc796f81a36c.jpg">

### Objectif
L'idée  generale est de joindre deux reseaux de neurones sur un ensemble de données: un generateur et un discriminateur

- un Generateur vise à generer des echantillons de bonne qualite similaire à ceux dans l'ensemble de données en apprenant sa distribution.
En effet il prend en entrée un vecteur  de bruit Z qui sera  generé  à partir d'une distribution PZ qui suit une loi normale centrée reduite et produit une fausse image.

-un discriminateur  vise à distinguer les images generées par le generateur à partir d'images de l'ensemble de données.
En effet cela prend en entrée les images generées par le generateur ou des images réelles suivant la distribution de l'ensemble de données  Pdata en utilisant l'entropie croisée binaire de maniere classique (BCE).
De plus le generateur utilise le discriminateur pour evaluer la qualité de ses echantillon generés et vise à produire des images  à partir des image d'origine c'est à dire maximiser la conversion  de sortie.
Suivant ce principe, la perte  suivante est optimisée conjointement :

<img width="287" alt="GAN" src="https://user-images.githubusercontent.com/72967454/100371822-a4f01080-3008-11eb-98b1-e42be6e83c19.PNG">


La formation GAN peut etre considere comme un jeu minimal à deux joueur et ces types de jeux sont generalement instable par consequent il y a peu de garanties sur la convergence.

## Evaluation:
Les soumissions sont évaluées sur MiFID (Memorization-Informed Fréchet Inception Distance), qui est une modification de Fréchet Inception Distance (FID) .
Plus le MiFID est petit, meilleures sont vos images générées.


#### Definition FID

Publié à l'origine ici ( github ), le FID, ainsi que le score de démarrage (IS) , sont tous deux couramment utilisés dans les publications récentes comme norme pour les méthodes d'évaluation des GAN.
<p align='center'>

<img width="365" alt="fid" src="https://user-images.githubusercontent.com/72967454/100006259-83a1e100-2dca-11eb-96d1-afc09a378d8f.PNG">


Dans FID, nous utilisons le réseau Inception pour extraire des entités d'une couche intermédiaire. Ensuite, nous modélisons la distribution des données pour ces caractéristiques en utilisant une distribution gaussienne multivariée avec moyenne µ et covariance Σ. Le FID entre les images réellesr et images générées g est calculé comme suit:
<p align='center'>
<img width="456" alt="formule" src="https://user-images.githubusercontent.com/72967454/100007161-ee075100-2dcb-11eb-8f67-1d204e325e55.PNG">

Tr: donne le resume de tous les elements diagonnaux

######  Qu'est ce que le MFID

La distance de mémorisation est définie comme la distance cosinus minimale de tous les échantillons d'apprentissage dans l'espace des fonctionnalités, moyennée sur tous les échantillons d'image générés par l'utilisateur. Cette distance est seuillée et attribuée à 1.0 si la distance dépasse un epsilon prédéfini.

Sous forme mathématique:
<p align='center'>
<img width="254" alt="formule 1" src="https://user-images.githubusercontent.com/72967454/100007882-0461dc80-2dcd-11eb-9d35-02860b227a0c.PNG">

Fg represente les images generées réelles dans l'espace des fonctionalités
Fgje et Fr represente le je h jt et h vecteurs de Fg et Fr respectivement.
<p align='center'>
<img width="139" alt="formule2PNG" src="https://user-images.githubusercontent.com/72967454/100007910-0d52ae00-2dcd-11eb-8390-53cdc97cb9f7.PNG">

re definit la distance minimale d'une certaine image à travers les images réelles puis moyennée sur toutes les images generées.
<p align='center'>
<img width="365" alt="fid" src="https://user-images.githubusercontent.com/72967454/100006259-83a1e100-2dca-11eb-96d1-afc09a378d8f.PNG">



ce terme de memorisation est applique au FID


<p align='center'>
<img width="206" alt="formule3" src="https://user-images.githubusercontent.com/72967454/100007952-1774ac80-2dcd-11eb-9524-387eb50a2bcf.PNG">


Si vous souhaitez évaluer les résultats localement, assurez-vous d'ajouter le jeu de données de métrique, modifiez les variables PATH et décommentez les lignes de score

#### Difficultes:

transformations applied are horizontal flipping and re-scaling the images between- -1 et 1. En effet , lactivation tanh sur la derniere couche du generateur permet pour les images de cette gamme. Nous pouvons remarquez la difficulte de latache. Les imagespresentent une variance beaucoup elevée que celle de l'ensemble des données .
Deja ,DCGAN a eu du mal à produire des resultats de haute qualité en utilisant cet ensemble de données par consequent je m'attends a ce que les chiens generes soient de mauvaise qualite.

####  Deroulemnt du code

Sous-échantillonnage à l'aide de convolutions foulées
Suréchantillonnage à l'aide de convolutions foulées
Utilisez LeakyReLU
Utiliser la normalisation par lots
Utiliser l'initialisation du poids gaussien
Utiliser la descente de gradient stochastique Adam
Mettre les images à l'échelle [-1,1]
Utiliser un espace latent gaussien
Lots séparés d'images réelles et fausses
Utiliser le lissage d'étiquette
Utiliser des étiquettes bruyantes

#### Concept
