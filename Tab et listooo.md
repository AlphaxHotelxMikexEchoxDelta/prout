# **Cours C : Tableaux et Listes**

---

## **1️⃣ Tableaux**

### Définition

Un **tableau** est une structure de données qui contient un **ensemble d’éléments du même type**, rangés dans un ordre précis et accessibles via un **indice**.

```c
int tab[5]; // tableau de 5 entiers
```

* `tab[0]` → premier élément
* `tab[4]` → dernier élément
* Indices de 0 à taille-1

### Initialisation

```c
int tab[5] = {1, 2, 3, 4, 5};
int tab2[5] = {0};   // tous les éléments initialisés à 0
```

### Accès et modification

```c
tab[2] = 10;        // modifie le 3ème élément
printf("%d", tab[2]); // affiche 10
```

### Parcours avec boucle

```c
for (int i = 0; i < 5; i++) {
    printf("%d ", tab[i]);
}
```

---

### Tableaux et pointeurs

En C, **le nom d’un tableau est un pointeur vers son premier élément** :

```c
int *p = tab; 
printf("%d", *(p + 2)); // accède au 3ème élément
```

---

### Exercices possibles

* Calculer la somme d’un tableau
* Trouver le minimum ou maximum
* Compter les éléments positifs
* Inverser un tableau

---

## **2️⃣ Listes chaînées**

### Définition

Une **liste chaînée** est une structure dynamique qui contient des **éléments (nœuds)** reliés par des **pointeurs**.

* Chaque nœud contient :

  1. **Donnée** (valeur)
  2. **Pointeur vers le nœud suivant**

```c
struct Node {
    int data;
    struct Node *next;
};
```

* Le **dernier nœud** pointe vers `NULL`.

---

### Création d’un nœud

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *next;
};

int main() {
    struct Node *n1 = malloc(sizeof(struct Node));
    n1->data = 10;
    n1->next = NULL;

    printf("%d\n", n1->data); // affiche 10
    free(n1);
    return 0;
}
```

* `malloc(sizeof(struct Node))` → alloue un nœud dynamiquement
* `n1->data` → accès à la donnée
* `n1->next` → pointeur vers le nœud suivant

---

### Ajouter un élément au début

```c
struct Node *head = NULL;

struct Node *n1 = malloc(sizeof(struct Node));
n1->data = 5;
n1->next = head;  // le suivant du nouveau nœud devient l’ancien head
head = n1;        // head pointe maintenant vers le nouveau nœud
```

---

### Parcourir une liste

```c
struct Node *current = head;
while (current != NULL) {
    printf("%d -> ", current->data);
    current = current->next;
}
printf("NULL\n");
```

---

### Avantages et inconvénients

| Tableaux                         | Listes chaînées                     |
| -------------------------------- | ----------------------------------- |
| Taille fixe                      | Taille dynamique                    |
| Accès direct par indice          | Accès séquentiel (via pointeur)     |
| Mémoire contiguë                 | Mémoire non contiguë                |
| Facile pour boucles et fonctions | Flexible pour insertion/suppression |


---


# ✅ **5 exercices — Tableaux et Listes**

---

## 🧩 Exercice 1 — Somme et moyenne d’un tableau

**Énoncé :**
Saisir `n` entiers dans un tableau.
Écrire une fonction qui :

1. Calcule la **somme** des éléments
2. Calcule la **moyenne** des éléments
3. Affiche le résultat

Fonction :

```c
void sommeEtMoyenne(int *tab, int n);
```

<details>
<summary>💡 Corrigé</summary>

```c
#include <stdio.h>

void sommeEtMoyenne(int *tab, int n) {
    int somme = 0;
    for (int i = 0; i < n; i++)
        somme += *(tab + i);
    printf("Somme = %d\n", somme);
    printf("Moyenne = %.2f\n", (float)somme/n);
}

int main() {
    int n;
    scanf("%d", &n);
    int t[n];
    for(int i=0; i<n; i++)
        scanf("%d", &t[i]);

    sommeEtMoyenne(t, n);
    return 0;
}
```

</details>

---

## 🧩 Exercice 2 — Inverser un tableau

**Énoncé :**
Écrire une fonction qui **inverse les éléments** d’un tableau.
Exemple : `[1,2,3,4] → [4,3,2,1]`

Fonction :

```c
void inverser(int *tab, int n);
```

<details>
<summary>💡 Corrigé</summary>

```c
#include <stdio.h>

void inverser(int *tab, int n) {
    int *start = tab;
    int *end = tab + n - 1;
    while(start < end) {
        int tmp = *start;
        *start = *end;
        *end = tmp;
        start++;
        end--;
    }
}

int main() {
    int t[] = {1,2,3,4};
    inverser(t, 4);
    for(int i=0; i<4; i++)
        printf("%d ", t[i]);
    return 0;
}
```

</details>

---

## 🧩 Exercice 3 — Première valeur négative dans un tableau

**Énoncé :**
Saisir `n` entiers dans un tableau.
Écrire une fonction qui **retourne l’adresse de la première valeur négative** ou `NULL` si aucune.

Fonction :

```c
int* premiereNegative(int *tab, int n);
```

<details>
<summary>💡 Corrigé</summary>

```c
#include <stdio.h>

int* premiereNegative(int *tab, int n) {
    for(int i=0; i<n; i++)
        if(*(tab+i) < 0)
            return tab+i;
    return NULL;
}

int main() {
    int t[] = {3,5,-2,7};
    int *p = premiereNegative(t, 4);
    if(p) printf("1ère négative = %d\n", *p);
    else printf("Aucune valeur négative\n");
    return 0;
}
```

</details>

---

## 🧩 Exercice 4 — Créer et afficher une liste chaînée

**Énoncé :**
Créer une liste chaînée contenant les valeurs `[5,10,15]`.
Écrire une fonction pour **afficher tous les éléments** de la liste.

Structures et fonctions :

```c
struct Node { int data; struct Node *next; };
void afficherListe(struct Node *head);
```

<details>
<summary>💡 Corrigé</summary>

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { 
    int data; 
    struct Node *next; 
};

void afficherListe(struct Node *head) {
    struct Node *current = head;
    while(current != NULL) {
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

int main() {
    struct Node *n1 = malloc(sizeof(struct Node));
    struct Node *n2 = malloc(sizeof(struct Node));
    struct Node *n3 = malloc(sizeof(struct Node));

    n1->data = 5; n1->next = n2;
    n2->data = 10; n2->next = n3;
    n3->data = 15; n3->next = NULL;

    afficherListe(n1);

    free(n1); free(n2); free(n3);
    return 0;
}
```

</details>

---

## 🧩 Exercice 5 — Ajouter un élément au début de la liste

**Énoncé :**
Écrire une fonction qui ajoute un **nouveau nœud au début** d’une liste chaînée.
Afficher ensuite la liste.

Fonction :

```c
struct Node* ajouterDebut(struct Node *head, int val);
```

<details>
<summary>💡 Corrigé</summary>

```c
#include <stdio.h>
#include <stdlib.h>

struct Node { 
    int data; 
    struct Node *next; 
};

struct Node* ajouterDebut(struct Node *head, int val) {
    struct Node *newNode = malloc(sizeof(struct Node));
    newNode->data = val;
    newNode->next = head;
    return newNode;
}

void afficherListe(struct Node *head) {
    struct Node *current = head;
    while(current != NULL) {
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

int main() {
    struct Node *head = NULL;
    head = ajouterDebut(head, 10);
    head = ajouterDebut(head, 5);
    afficherListe(head);

    // libérer la mémoire
    struct Node *tmp;
    while(head != NULL) {
        tmp = head;
        head = head->next;
        free(tmp);
    }
    return 0;
}
```

</details>

