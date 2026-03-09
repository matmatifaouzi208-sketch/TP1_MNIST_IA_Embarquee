# TP1_MNIST_IA_Embarquee

## Introduction :

Ce projet a pour objectif de résoudre le problème MNIST. Le jeu de données MNIST est un benchmark classique en apprentissage automatique pour la classification d'images , qui contient des images en niveaux de gris de chiffres manuscrits (de 0 à 9).  L'objectif de ce modèle est de prédire correctement la classe (le chiffre) associée à chaque image parmi les 10 classes possibles. Sachant que chaque image est de taille 28/28 pixels, soit [1x784] pixels au total , celle-ci est systématiquement transformée en un vecteur aplati de dimension 784 pour la phase d'entraînement.

## 1. Choix de la Fonction d’Activation :

Pour cette étude, nous avons évalué 4 architectures différentes pendant 5 epochs en utilisant l'optimiseur Adam.

## 1. Choix de la Fonction d'Activation

Pour cette première partie d'étude, nous utilisons l'optimiseur `adam` et la fonction de coût `categorical_crossentropy`. L'objectif est d'observer l'impact de la présence de couches cachées et du choix de la fonction d'activation sur les performances de notre réseau.

### Implémentations réalisées (a, b, c, d)

Nous avons codé quatre architectures différentes à l'aide de Keras :

* **a. Modèle A (Softmax direct) :** Un réseau sans couche cachée, reliant directement l'entrée (784) à la sortie (10) avec une activation `softmax`. C'est un simple modèle linéaire.
* **b. Modèle B (ReLU) :** Un perceptron multicouche (MLP) avec 2 couches cachées (128 et 64 neurones) utilisant la fonction d'activation `relu`.
* **c. Modèle C (Tanh) :** La même architecture (128 -> 64 neurones), mais utilisant la fonction d'activation `tanh`.
* **d. Modèle D (Sigmoid) :** La même architecture (128 -> 64 neurones), mais utilisant la fonction d'activation `sigmoid`.

### e. Comparaisons et Justification des Compromis

Voici les courbes d'apprentissage obtenues après l'entraînement de nos modèles sur 5 epochs :

![Comparaison des fonctions d'activation](image_0514e3.png)
*(Note : Le Modèle A n'est pas affiché sur le graphe pour des raisons de lisibilité, mais ses résultats sont inclus dans l'analyse ci-dessous).*

#### Analyse des Compromis : Accuracy, Epochs et Synapses

| Modèle | Architecture | Paramètres en mémoire ($W$) | Accuracy (Test) à 5 epochs | Vitesse de convergence |
| :--- | :--- | :--- | :--- | :--- |
| **A (Softmax)** | `784 -> 10` | 7 850 | ~92.6% | Moyenne (plafonne vite) |
| **B (ReLU)** | `784 -> 128 -> 64 -> 10` | 109 386 | **~97.5%** | **Très rapide** |
| **C (Tanh)** | `784 -> 128 -> 64 -> 10` | 109 386 | ~97.6% | Très rapide |
| **D (Sigmoid)** | `784 -> 128 -> 64 -> 10` | 109 386 | ~97.4% | Lente |

**1. Compromis "Nombre de Synapses (Dimension de $W$)" vs "Accuracy" :**
* Le **Modèle A** possède un très faible nombre de paramètres de poids et biais ($784 \times 10 + 10 = 7850$). Son empreinte mémoire est minime et son exécution ultra-rapide. Cependant, sans couche cachée, il ne trace que des frontières de décision linéaires. Son *accuracy* plafonne rapidement autour de 92.6%.
* Les **Modèles B, C et D** ajoutent des couches cachées, ce qui fait exploser le nombre de synapses stockées en mémoire ($100480 + 8256 + 650 = 109386$ paramètres). L'empreinte mémoire est multipliée par 14. En contrepartie, cette complexité permet au réseau de modéliser des relations non linéaires, propulsant l'*accuracy* au-delà de 97%. Le compromis est donc clair : on sacrifie de la mémoire et de la puissance de calcul pour gagner 5 points de précision cruciaux.

**2. Compromis "Nombre d'Epochs" vs "Fonction d'Activation" :**
* **ReLU (Modèle B) et Tanh (Modèle C)** : Comme le montre le graphique, ces fonctions apprennent extrêmement vite. Dès la première epoch, leur précision de validation (lignes pleines bleue et orange) dépasse les 96%. Ce sont d'excellents choix pour minimiser le nombre d'epochs nécessaires à l'entraînement.
* **Sigmoid (Modèle D)** : La courbe rouge pleine montre une convergence beaucoup plus poussive. À la première epoch, la précision peine à dépasser 94% (et 88% en train). Cela illustre le **problème de la disparition du gradient** (vanishing gradient) : les mises à jour des poids sont si infimes que le modèle a besoin de plus de temps (plus d'epochs) pour apprendre. Bien qu'il finisse par rattraper les autres autour de la 5ème epoch, il est beaucoup moins efficace en temps de calcul.

**Conclusion :** L'architecture avec couches cachées et la fonction d'activation **ReLU** (Modèle B) représente le meilleur compromis pratique moderne. Elle offre une excellente précision très rapidement, tout en évitant les problèmes de gradient de la fonction Sigmoid, justifiant ainsi le coût en mémoire de ses 109 386 synapses.
