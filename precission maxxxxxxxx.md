# 🧵 1) **Chaînes de caractères : détails avancés**

## ✔ Les chaînes ne sont pas un type : ce sont des tableaux

En C, il n’existe **aucun type string**.
Une chaîne = **un tableau de `char` terminé par `'\0'`**.

## ✔ Le caractère nul `'\0'` est obligatoire

Sans `'\0'`, les fonctions (`printf`, `strlen`, etc.) **continuent de lire la mémoire** jusqu’à tomber sur un `0` par hasard → danger.

## ✔ Différence "tableau de char" vs "pointeur vers char"

```c
char t[] = "Hello";    // t = tableau local, taille fixe
char *p = "Hello";     // p = pointeur vers une zone en lecture seule
```

`p` pointe souvent vers la **zone constante du programme**, et la modifier est interdit.

## ✔ Taille réelle d’une chaîne

```c
char s[] = "ABC";  // taille 4 : 'A' 'B' 'C' '\0'
```

Toujours 1 caractère supplémentaire.

## ✔ Fonctions importantes (mais dangereuses)

### ❌ strcpy, strcat

Elles n’ont **aucune vérification** de taille → source de dépassements.

### ✔ Versions sûres

* `strncpy`
* `strncat`
* `snprintf`
* `fgets`

## ✔ Une chaîne peut être manipulée comme un pointeur

```c
char *p = s;
p++;   // avance d’un caractère
```

---

# 🔗 2) **Listes chaînées : détails essentiels**

## ✔ Structure minimale d’une liste simple

```c
typedef struct Node {
    int x;
    struct Node *suiv;
} Node;
```

Chaque maillon contient :

* une donnée
* un pointeur vers le prochain maillon

## ✔ Avantages des listes

* insertion/suppression **sans déplacer toute la mémoire**
* taille dynamique
* pas besoin de connaître la taille à l’avance

## ✔ Inconvénients

* pas d’accès direct par index (contrairement aux tableaux)
* surcoût mémoire (un pointeur par élément)
* moins efficaces pour les accès aléatoires

## ✔ Opérations à connaître

* insertion en tête
* insertion en queue
* recherche
* suppression d’un élément
* libération (`free`) de toute la liste

## ✔ Listes vs tableaux

| Caractéristique     | Tableau   | Liste      |
| ------------------- | --------- | ---------- |
| Accès indexé        | ✔ O(1)    | ✖ O(n)     |
| Insertion au milieu | ✖ coûteux | ✔ facile   |
| Taille dynamique    | ✖ non     | ✔ oui      |
| Localité mémoire    | ✔ bonne   | ✖ mauvaise |

## ✔ Importance du **free**

Chaque `malloc` doit avoir un `free`.
Sinon → fuite mémoire.

---

# 🧭 3) **Pointeurs : les détails qui changent tout**

## ✔ Un pointeur stocke une adresse

Exemple :

```c
int a = 10;
int *p = &a;
```

`p` contient l’**adresse** de `a`.

## ✔ Déréférencement

```c
*p
```

permet d’accéder à la valeur pointée.

## ✔ Arithmétique des pointeurs

Avancer un pointeur ne fait pas `+1` octet, mais `+sizeof(type)` :

```c
int *p;   // p++ avance de 4 octets sur la plupart des machines
```

## ✔ Pointeurs et tableaux

Le nom d’un tableau **devient un pointeur** sur son premier élément :

```c
char t[] = "Salut";
char *p = t; // équivalent à &t[0]
```

## ✔ Pointeurs de pointeurs

Permettent :

* tableaux dynamiques
* listes chaînées complexes
* modification d’un pointeur dans une fonction

Exemple :

```c
void f(int **pp) { ... }
```

## ✔ Pointeur NULL

Toujours initialiser un pointeur.

```c
int *p = NULL;
```

Tester avant utilisation :

```c
if (p != NULL) { ... }
```

## ✔ Dangling pointers (dangereux)

Pointeurs qui ne pointent plus vers une zone valide (ex : après un free).

---

# 🔥 4) **Interaction chaînes + pointeurs + listes**

Tu peux combiner tout ça :

### ✔ Une chaîne peut être transformée en liste

→ un maillon par caractère
→ utile pour des algorithmes qui insèrent au milieu

### ✔ Une liste peut devenir une chaîne

→ nécessite un tableau suffisamment grand
→ attention au `'\0'`

### ✔ Les pointeurs permettent :

* de parcourir une chaîne (`p++`)
* de parcourir une liste (`p = p->suiv`)
* de transformer l’une en l’autre
* d’insérer/supprimer dynamiquement

### ✔ Ces notions se complètent :

| Concept       | Utilisation                          |
| ------------- | ------------------------------------ |
| **Chaînes**   | Stockage séquentiel de texte         |
| **Pointeurs** | Manipulation flexible de la mémoire  |
| **Listes**    | Structures dynamiques avec pointeurs |

---
