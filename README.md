# Examen-IA

## Description:

Les concurrents doivent utiliser des méthodes génératives pour créer leurs images de soumission et ne sont pas autorisés à faire des soumissions qui incluent des images déjà classées comme des chiens ou des versions modifiées de ces images.

###  objectif:

####  Remarque
Utilisez vos compétences de formation pour créer des images, plutôt que de les identifier. Vous utiliserez des GAN, qui sont à la frontière créative de l'apprentissage automatique. Vous pourriez penser aux GAN comme à des artistes robots en un sens, capables de créer des images étrangement réalistes, et même des mondes numériques.

#### Explication:

Un réseau antagoniste génératif (GAN) est une classe de système d'apprentissage automatique inventé par Ian Goodfellow en 2014. Deux réseaux de neurones se font concurrence dans un jeu. Étant donné un ensemble d'apprentissage, cette technique apprend à générer de nouvelles données avec les mêmes statistiques que l'ensemble d'apprentissage.
Dans cette compétition, vous entraînerez des modèles génératifs pour créer des images de chiens. Seulement cette fois… il n'y a pas de données de vérité terrain à prédire. Ici, vous soumettrez les images et serez notés en fonction de la qualité de ces images comme des chiens provenant de réseaux de neurones pré-entraînés. Prenez ces images, par exemple. Pouvez-vous dire lesquels sont réels ou générés?

(Images des chiens png)

Pourquoi les chiens? Nous avons choisi des chiens parce que, eh bien, qui n'aime pas regarder des photos d'adorables chiots? De plus, les chiens peuvent être classés en plusieurs sous-catégories (race, couleur, taille), ce qui en fait des candidats idéaux pour la génération d'images.
Les méthodes génératives (en particulier les GAN) sont actuellement utilisées à divers endroits sur Kaggle pour l'augmentation des données. Leur potentiel est vaste; ils peuvent apprendre à imiter toute distribution de données dans n'importe quel domaine: photographies, dessins, musique et prose. En cas de succès, non seulement vous contribuerez à faire progresser l'état de l'art dans la création d'images génératives, mais vous nous permettrez de créer plus d'expériences dans une variété de domaines à l'avenir.

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

Une démo de notre code d'évaluation MiFID peut être vue ici .

Notre flux de travail de calcul du MiFID public / privé est illustré ci-dessous

#### xxxxx()


Les versions soumises sont les versions 4 et 5 qui obtiennent un score de ** 30,7 et 30,3 sur le public **. La seule différence est que j'ai utilisé différentes probabilités d'échantillonnage de classe pour générer des images. La solution proposée ici est légitime, aucune mémorisation d'aucune sorte n'est faite.

Si vous souhaitez évaluer les résultats localement, assurez-vous d'ajouter le jeu de données de métrique, modifiez les variables PATH et décommentez les lignes de score.

### References

- NVIDIA's Progressive Growing of GANs paper : https://research.nvidia.com/publication/2017-10_Progressive-Growing-of
- CGANs with Projection Discriminator : https://arxiv.org/pdf/1802.05637.pdf
- The modeling part of the kernel is taken from : https://github.com/akanimax/pro_gan_pytorch
