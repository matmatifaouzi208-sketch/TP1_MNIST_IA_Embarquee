# TP1_MNIST_IA_Embarquee

## Introduction :

Ce projet a pour objectif de résoudre le problème MNIST. Le jeu de données MNIST est un benchmark classique en apprentissage automatique pour la classification d'images , qui contient des images en niveaux de gris de chiffres manuscrits (de 0 à 9).  L'objectif de ce modèle est de prédire correctement la classe (le chiffre) associée à chaque image parmi les 10 classes possibles. Sachant que chaque image est de taille 28/28 pixels, soit [1x784] pixels au total , celle-ci est systématiquement transformée en un vecteur aplati de dimension 784 pour la phase d'entraînement.

## 1. Choix de la Méthode d'Apprentissage (Fonction d'Activation)

Dans cette première partie, notre but est de comprendre comment la "forme" de notre intelligence artificielle (ses couches) et sa méthode d'activation influencent ses résultats. [cite_start]Pour que le test soit juste, nous gardons la même base de calcul pour tout le monde : l'optimiseur `adam` et la fonction d'erreur `categorical_crossentropy`[cite: 29].

### Les 4 modèles testés

Nous avons créé quatre versions de notre réseau d'apprentissage  :

* **a. Modèle A (Le plus basique) :** C'est un réseau direct, sans étape intermédiaire de réflexion (sans "couche cachée"). [cite_start]L'image entre d'un côté et le résultat sort de l'autre avec la fonction "Softmax"[cite: 30]. C'est une approche très simple et très légère.
* **b. Modèle B (Le moderne avec ReLU) :** C'est un réseau plus profond. [cite_start]On lui a ajouté deux étapes intermédiaires de calcul (des couches cachées) et on utilise la fonction "ReLU" pour l'aider à apprendre[cite: 31].
* **c. [cite_start]Modèle C (Le classique avec Tanh) :** C'est exactement la même construction que le Modèle B, mais on utilise une méthode mathématique différente appelée "Tanh"[cite: 33].
* **d. [cite_start]Modèle D (L'ancien avec Sigmoid) :** Toujours la même construction, mais avec une méthode plus ancienne appelée "Sigmoid"[cite: 34].

### Les résultats après 5 lectures des données (5 Epochs)

![Comparaison des fonctions d'activation](réulstat/image_0514e3.png)
*(Note : Le Modèle A n'apparaît pas sur ce graphique pour qu'il reste facile à lire, mais ses résultats sont discutés en dessous).*

Voici un tableau très simple pour résumer ce que nous avons observé :

| Modèle testé | Complexité (Taille en mémoire) | Taux de bonnes réponses | Vitesse d'apprentissage |
| :--- | :--- | :--- | :--- |
| **A (Softmax - Basique)** | Très léger (~7 800 paramètres) | ~92.6% | Moyenne (le score bloque vite) |
| **B (ReLU)** | Lourd (~109 000 paramètres) | **~97.5%** | **Très rapide** |
| **C (Tanh)** | Lourd (~109 000 paramètres) | ~97.6% | Très rapide |
| **D (Sigmoid)** | Lourd (~109 000 paramètres) | ~97.4% | Lente (il a besoin de plus de temps) |

---

### Explication des compromis : Mémoire, Temps et Précision

[cite_start]Pour bien juger une intelligence artificielle, il faut trouver le bon équilibre (le compromis) entre trois choses : la place qu'elle prend en mémoire sur l'ordinateur, le temps qu'elle met à apprendre (le nombre d'epochs) et son taux de réussite (la précision)[cite: 37].

**1. Le combat "Taille en Mémoire" contre "Précision"**
Le Modèle A prend très peu de place sur l'ordinateur. C'est génial, mais comme il est trop simple, il n'est pas capable de comprendre des images trop complexes. Son score de bonnes réponses bloque très vite à 92.6%.
Pour les Modèles B, C et D, nous avons ajouté des étapes de calcul supplémentaires. Conséquence : le réseau devient 14 fois plus gros en mémoire ! Par contre, ce sacrifice est largement récompensé. La machine devient beaucoup plus fine dans ses analyses et dépasse les 97% de bonnes réponses. [cite_start]On accepte donc de consommer plus de mémoire pour gagner en précision[cite: 37].

**2. Le combat "Temps d'apprentissage" contre "Méthode de calcul"**
[cite_start]La méthode choisie (ReLU, Tanh ou Sigmoid) change complètement la vitesse à laquelle l'intelligence mémorise les informations[cite: 37].
Avec les fonctions ReLU (Modèle B) et Tanh (Modèle C), l'apprentissage est un sprint. Dès qu'elles ont lu les images une seule fois (la première epoch), elles dépassent déjà les 96% de réussite. C'est parfait si on veut gagner du temps.
À l'inverse, avec la fonction Sigmoid (Modèle D), le réseau a beaucoup de mal au début (c'est un souci mathématique connu appelé la "disparition du gradient"). Il a besoin de réviser les images beaucoup plus de fois (plus d'epochs) pour finir par atteindre le niveau des autres. [cite_start]Ce n'est donc pas très efficace en temps de calcul[cite: 37].

**En conclusion :** Le Modèle B avec la fonction **ReLU** est le grand gagnant. [cite_start]Il est ultra-rapide pour apprendre et offre d'excellents résultats, ce qui justifie totalement le fait qu'il prenne un peu plus de place en mémoire sur l'ordinateur[cite: 37].
