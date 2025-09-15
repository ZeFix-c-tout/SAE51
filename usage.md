# SAE51

**Auteurs :** SIMAO Julien et BREANT Hugo  
**Date :** Du 05/09/2025 au 15/09/2025  

---

## Résumé

Ce document présente le travail réalisé autour du script `genMV.sh` permettant la gestion de machines virtuelles sous Linux. Il décrit l’organisation de notre projet, les étapes de développement, les difficultés rencontrées, ainsi que les fonctionnalités finales implémentées. Enfin, un guide d’utilisation explique comment exécuter et tester le code.  

---

## Organisation du projet

Nous avons d’abord commencé par installer nos machines virtuelles sur nos ordinateurs personnels.  
Ensuite, nous avons créé deux projets sur GitHub afin de synchroniser nos programmes et travailler en collaboration.  
Après la lecture du sujet, nous avons défini une organisation claire : répartition des tâches, qui code quoi, et quelles extensions logicielles installer (comme `git`).  

---

## Recherche et premières étapes

Chacun de notre côté, nous avons cherché des commandes utiles à la réalisation du projet et nous les avons testées pour bien comprendre leur fonctionnement et leurs arguments.  
Nous avons ensuite attaqué l’étape 1 : la création d’une machine virtuelle Debian 64 bits avec des conditions particulières.  

Lors de cette étape, un problème bloquait l’ouverture de la machine virtuelle dans nos VM. Nous avons résolu cela en cochant une option dans notre logiciel de virtualisation sur nos machines réelles. Il a aussi fallu autoriser la virtualisation imbriquée sur nos ordinateurs personnels.  
Une fois réglé, nous avons pu vérifier que notre programme créait bien une machine virtuelle utilisable.  

---

## Développement des fonctionnalités

Nous avons ensuite ajouté une vérification permettant de supprimer une machine virtuelle si elle existait déjà, comme demandé dans l’étape 2.  

Pour l’étape 4, il fallait donner un argument pour créer, supprimer, démarrer, arrêter ou afficher les machines virtuelles existantes.  
Nous avons choisi d’utiliser une structure avec `getopts` plutôt que d'utiliser `if`/`fi`, car cela nous semblait plus adapté à la demande.  

Au cours de nos tests, nous avons dû créer plusieurs machines virtuelles. Cela nous a donné l’idée d’ajouter une fonctionnalité supplémentaire : l’argument `-T`, qui permet de supprimer toutes les machines virtuelles créées.  
Nous avons ensuite modifié notre code pour implémenter les étapes suivantes du projet.  

---

## Guide d’utilisation

Pour utiliser notre script, il faut ouvrir un terminal Linux et exécuter :  

```bash
./genMV.sh [argument] [nom_de_la_machine_virtuelle]
```
Liste des arguments:

- `-L` : afficher la liste des machines virtuelles existantes.  
- `-N` : créer une nouvelle machine virtuelle.  
- `-S` : supprimer la machine virtuelle indiquée.  
- `-D` : démarrer la machine virtuelle indiquée.  
- `-A` : arrêter la machine virtuelle indiquée.  
- `-T` : supprimer toutes les machines virtuelles créées.

---

## Explication de `getopts`

Nous avons choisi d’utiliser `getopts` pour gérer les arguments passés en ligne de commande.  
C’est une commande intégrée au shell qui permet de lire facilement les options (`-N`, `-S`, etc.) sans avoir à écrire plusieurs conditions `if/fi`.  
Avec `getopts`, chaque option est récupérée automatiquement et stockée dans une variable (`opt`). Si l’option attend un argument supplémentaire (par exemple le nom d’une VM pour `-S`), celui-ci est placé dans la variable spéciale `$OPTARG`.  
Cela rend le script plus clair, plus concis et plus facile à maintenir.  

Exemple:

```bash
while getopts "NS:" opt; do
  case $opt in
    N) echo "Créer une VM" ;;
    S) echo "Supprimer la VM nommée : $OPTARG" ;;
  esac
done

