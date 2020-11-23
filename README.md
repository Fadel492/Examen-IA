# Examen-IA

## Description:

Les Réseaux Adversaires Génératifs (GAN) génèrent un image suivant une distribution apprise en utilisant un vecteur aleatoire dans un
un espace latent. Les GAN se sont avérés être des outils de synthèse puissants mais difficiles à manipuler. L'adversaire génératif convolutionnel profond
Réseau (DCGAN) GAN entièrement convolutif qui a atteint l'état des resultats artistiques lors de sa sortie en 2016. Plusieurs
des résultats artistiques lors de sa sortie en 2016. Plusieurs
les détails de mise en œuvre sont donnés dans le document DCGAN qui améliorent la convergence. Dans ce projet, nous proposons d'implémenter un DCGAN pour générer des chiens, en utilisant un sous-ensemble de l'ensemble de données ImageNet. 

###  objectif:

####  Remarque

Un réseau antagoniste génératif (GAN) est une classe de système d'apprentissage automatique inventé par Ian Goodfellow en 2014. Deux réseaux de neurones se font concurrence dans un jeu. Étant donné un ensemble d'apprentissage, cette technique apprend à générer de nouvelles données avec les mêmes statistiques que l'ensemble d'apprentissage.
Dans cette compétition, vous entraînerez des modèles génératifs pour créer des images de chiens. Seulement cette fois… il n'y a pas de données de vérité terrain à prédire. Ici, vous soumettrez les images et serez notés en fonction de la qualité de ces images comme des chiens provenant de réseaux de neurones pré-entraînés. Prenez ces images, par exemple. Pouvez-vous dire lesquels sont réels ou générés?

![chien](https://user-images.githubusercontent.com/72967454/100002925-9a920480-2dc5-11eb-8684-fc796f81a36c.jpg)

. L'idée est de conjointement former deux réseaux de neurones sur un ensemble de données: un générateur et un discriminateur. - Le générateur G vise à générer des échantillons de bonne qualité similaires à ceux dans l'ensemble de données en apprenant sa distribution. Il prend en entrée un vecteur de bruit z qui sera généré à partir d'une distribution pz (classiquement, pz = N (0, 1)), et produit une fausse image. - Le discriminateur D vise à distinguer les images générées par le générateur à partir d'images de l'ensemble de données. Cela prendra soit comme images d'entrée générées par le générateur, ou des images réelles suivant la distribution de l'ensemble de données pdata et renvoie la probabilité que l'échantillon choisi soit réel. Ensuite, le discriminateur est un simple classificateur d'image qui apprendra la probabilité que l'image est réelle, en utilisant classiquement l'entropie croisée binaire (BCE). Sur le 2 T. VIEL d'autre part, le générateur utilisera le discriminateur pour évaluer la qualité de son échantillons générés, et visent à produire des images que le discriminateur estime être à partir des données d'origine, c'est-à-dire maximiser la probabilité de sortie. Suivant ce principe, la perte suivante est optimisée conjointement: min g max ré Ex∼pdata [log D (x)] + Ez∼pz [log (1 - D (G (z)))] (1) Où D est l'ensemble des paramètres du discriminateur et G celui des Générateur. Dans le cas du réseau de neurones, nous visons à les apprendre en utilisant la rétropropagation. La formation GAN peut être vue comme un jeu minimax à deux joueurs, et ces types de les jeux sont généralement instables, par conséquent, il y a peu de garanties sur la convergence des GAN.

## Evaluation:
Les soumissions sont évaluées sur MiFID (Memorization-Informed Fréchet Inception Distance), qui est une modification de Fréchet Inception Distance (FID) .
Plus le MiFID est petit, meilleures sont vos images générées.


#### Definition FID

Publié à l'origine ici ( github ), le FID, ainsi que le score de démarrage (IS) , sont tous deux couramment utilisés dans les publications récentes comme norme pour les méthodes d'évaluation des GAN.

Dans FID, nous utilisons le réseau Inception pour extraire des entités d'une couche intermédiaire. Ensuite, nous modélisons la distribution des données pour ces caractéristiques en utilisant une distribution gaussienne multivariée avec moyenne µ et covariance Σ. Le FID entre les images réellesr et images générées g est calculé comme suit:

FID = | |μr-μg||2+ Tr (Σr+Σg- 2 (ΣrΣg)1 / 2)
où Trrésume tous les éléments diagonaux. Le FID est calculé en calculant la distance de Fréchet entre deux Gaussiens ajustés pour représenter les caractéristiques du réseau Inception.

Qu'est-ce que MiFID (Memorization-Informed FID)?
En plus du FID, Kaggle prend en compte la mémorisation des échantillons d'entraînement.

La distance de mémorisation est définie comme la distance cosinus minimale de tous les échantillons d'apprentissage dans l'espace des fonctionnalités, moyennée sur tous les échantillons d'image générés par l'utilisateur. Cette distance est seuillée et attribuée à 1.0 si la distance dépasse un epsilon prédéfini.

Sous forme mathématique:

réje j= 1 - c o s (Fgje,Fr j) = 1 -Fgje⋅Fr j|Fgje| |Fr j|
où Fg et Frreprésenter les images générées / réelles dans l'espace des fonctionnalités (défini dans les réseaux pré-formés); etFgje et Fr j représenter le jet h et jt h vecteurs de Fg et Fr, respectivement.

ré=1N∑jeminjréje j
définit la distance minimale d'une certaine image générée ( ) à travers toutes les images réelles (( ), puis moyennée sur toutes les images générées.jej
entrez la description de l'image ici
définit le seuil du poids ne s'applique que lorsque le ( ) est inférieur à un certain seuil déterminé empiriquement.ré
Enfin, ce terme de mémorisation est appliqué au FID:
Mje FjeD = FjeD ⋅1rét h r
Le workflow de Kaggle calcule la MiFID pour les scores publics et privés
Kaggle calcule les scores MiFID publics et privés avec le même code, mais avec différents modèles pré-entraînés et images d'évaluation. Le réseau neuronal public pré-train est Inception , et les images publiques utilisées pour l'évaluation sont les chiens ImageNet (les 120 races). Nous ne partagerons pas le modèle privé ou l'ensemble de données utilisé pour le score MiFID privé.

Si vous souhaitez évaluer les résultats localement, assurez-vous d'ajouter le jeu de données de métrique, modifiez les variables PATH et décommentez les lignes de score.

####  Concept:
techniques utilisés

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

### References

- NVIDIA's Progressive Growing of GANs paper : https://research.nvidia.com/publication/2017-10_Progressive-Growing-of
- CGANs with Projection Discriminator : https://arxiv.org/pdf/1802.05637.pdf
- The modeling part of the kernel is taken from : https://github.com/akanimax/pro_gan_pytorch
