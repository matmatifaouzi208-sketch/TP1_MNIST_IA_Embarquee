# TP1_MNIST_IA_Embarquee

## Introduction :

Ce projet a pour objectif de résoudre le problème MNIST. Le jeu de données MNIST est un benchmark classique en apprentissage automatique pour la classification d'images , qui contient des images en niveaux de gris de chiffres manuscrits (de 0 à 9).  L'objectif de ce modèle est de prédire correctement la classe (le chiffre) associée à chaque image parmi les 10 classes possibles. Sachant que chaque image est de taille 28/28 pixels, soit [1x784] pixels au total , celle-ci est systématiquement transformée en un vecteur aplati de dimension 784 pour la phase d'entraînement.

## 1. Choix de la Fonction d’Activation

Dans cette première partie, notre but est de comprendre comment la "forme" de notre intelligence artificielle (ses couches) et sa méthode d'activation influencent ses résultats. [cite_start]Pour que le test soit juste, nous gardons la même base de calcul pour tout le monde : l'optimiseur `adam` et la fonction d'erreur `categorical_crossentropy`.

### Les 4 modèles testés

Nous avons créé quatre versions de notre réseau d'apprentissage  :

* **a. Modèle A (Le plus basique) :** C'est un réseau direct, sans étape intermédiaire de réflexion (sans "couche cachée"). L'image entre d'un côté et le résultat sort de l'autre avec la fonction "Softmax". C'est une approche très simple et très légère.
* **b. Modèle B (ReLU) :** C'est un réseau plus profond. On lui a ajouté deux étapes intermédiaires de calcul (des couches cachées) et on utilise la fonction "ReLU" pour l'aider à apprendre.
* **c. Modèle C (Tanh) :** C'est exactement la même construction que le Modèle B, mais on utilise une méthode mathématique différente appelée "Tanh".
* **d. Modèle D (Sigmoid) :** Toujours la même construction, mais avec une méthode plus ancienne appelée "Sigmoid".

### Les résultats après 5 lectures des données (5 Epochs)

![Comparaison des fonctions d'activation](résultat/graph.png)


Voici un tableau pour résumer ce que nous avons observé :

| Modèle testé | Taille | Précision | Vitesse d'apprentissage |
| :--- | :--- | :--- | :--- |
| **A (Softmax - Basique)** | Très léger (~7 800 paramètres) | ~92.6% | Moyenne  |
| **B (ReLU)** | Lourd (~109 000 paramètres) | **~97.5%** | Très rapide |
| **C (Tanh)** | Lourd (~109 000 paramètres) | ~97.6% | Très rapide |
| **D (Sigmoid)** | Lourd (~109 000 paramètres) | ~97.4% | Lente |

---

L'évaluation d'une intelligence artificielle repose sur un compromis essentiel entre sa taille en mémoire, son temps d'apprentissage (les epochs) et sa précision finale. Bien que le Modèle A soit très léger en mémoire, sa simplicité limite sa précision à 92,6 %, là où les modèles B, C et D exigent 14 fois plus d'espace mais dépassent les 97 % de réussite.

## 2. Choix de l’Algorithme d’Optimisation

