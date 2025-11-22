## 🧩 Exercice 1

Créer une matrice `4x4` initialisée à `0` et l’afficher.

Fonction à définir :

```c
void afficherMatrice4x4(int matrix[4][4]);
```

<details>
<summary>💡 Corrigé et explication</summary>

```c
#include <stdio.h>

void afficherMatrice4x4(int matrix[4][4]) {
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            printf("%d ", matrix[i][j]);
        }
        printf("\n");
    }
}

int main() {
    int matrix[4][4] = {0};  // Initialisation à 0
    afficherMatrice4x4(matrix);
    return 0;
}
```

🧠 **Explication**

* `matrix[4][4] = {0};` initialise tous les éléments à zéro.
* Boucles imbriquées pour parcourir lignes et colonnes.
* `matrix[i][j]` accède à l’élément à la ligne `i` et colonne `j`.

</details>

---

## 🧩 Exercice 2

Remplir la matrice `4x4` avec la **somme de ses indices** et l’afficher.

Fonction à définir :

```c
void remplirMatriceSommeIndices(int matrix[4][4]);
```

<details>
<summary>💡 Corrigé et explication</summary>

```c
#include <stdio.h>

void remplirMatriceSommeIndices(int matrix[4][4]) {
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            matrix[i][j] = i + j;
        }
    }
}

void afficherMatrice4x4(int matrix[4][4]) {
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            printf("%d ", matrix[i][j]);
        }
        printf("\n");
    }
}

int main() {
    int matrix[4][4];
    remplirMatriceSommeIndices(matrix);
    afficherMatrice4x4(matrix);
    return 0;
}
```

🧠 **Explication**

* `matrix[i][j] = i + j;` remplit chaque cellule avec la somme des indices.
* On utilise une fonction séparée pour l’affichage, comme dans l’exercice 1.

</details>

---

## 🧩 Exercice 3

Créer un tableau 3D `3x3x3` et le remplir avec le **produit des indices**.
Afficher toutes les valeurs avec leurs indices.

Fonction à définir :

```c
void afficherCube3x3x3(int cube[3][3][3]);
```

<details>
<summary>💡 Corrigé et explication</summary>

```c
#include <stdio.h>

void afficherCube3x3x3(int cube[3][3][3]) {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 3; k++) {
                cube[i][j][k] = i * j * k;
                printf("cube[%d][%d][%d] = %d\n", i, j, k, cube[i][j][k]);
            }
        }
    }
}

int main() {
    int cube[3][3][3];
    afficherCube3x3x3(cube);
    return 0;
}
```

🧠 **Explication**

* Tableau 3D : `cube[x][y][z]`.
* Trois boucles imbriquées pour parcourir toutes les dimensions.
* On affiche chaque élément avec ses indices pour visualiser la structure.

</details>

---

## 🧩 Exercice 4

Pour une matrice `4x4` remplie aléatoirement (0 à 9) :

1. Calculer la **somme de chaque ligne**.
2. Calculer la **somme de chaque colonne**.

Fonction à définir :

```c
void sommeLignesColonnes(int matrix[4][4]);
```

<details>
<summary>💡 Corrigé et explication</summary>

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void sommeLignesColonnes(int matrix[4][4]) {
    int sommeLigne, sommeColonne;

    for (int i = 0; i < 4; i++) {
        sommeLigne = 0;
        for (int j = 0; j < 4; j++) {
            sommeLigne += matrix[i][j];
        }
        printf("Somme ligne %d = %d\n", i, sommeLigne);
    }

    for (int j = 0; j < 4; j++) {
        sommeColonne = 0;
        for (int i = 0; i < 4; i++) {
            sommeColonne += matrix[i][j];
        }
        printf("Somme colonne %d = %d\n", j, sommeColonne);
    }
}

int main() {
    srand(time(NULL));
    int matrix[4][4];

    // Remplissage aléatoire
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            matrix[i][j] = rand() % 10;
        }
    }

    sommeLignesColonnes(matrix);
    return 0;
}
```

🧠 **Explication**

* `rand() % 10` génère des nombres entre 0 et 9.
* Boucles séparées pour lignes et colonnes.
* On cumule les valeurs pour obtenir la somme.

</details>

---

## 🧩 Exercice 5

Créer une **matrice dynamique** avec des dimensions données par l’utilisateur :

1. Demander `rows` et `cols`.
2. Créer la matrice dynamiquement.
3. Remplir et afficher.
4. Libérer la mémoire.

Fonction à définir :

```c
int** creerMatriceDynamique(int rows, int cols);
void afficherMatriceDynamique(int** matrix, int rows, int cols);
void libererMatrice(int** matrix, int rows);
```

<details>
<summary>💡 Corrigé et explication</summary>

```c
#include <stdio.h>
#include <stdlib.h>

int** creerMatriceDynamique(int rows, int cols) {
    int **matrix = malloc(rows * sizeof(int*));
    for (int i = 0; i < rows; i++) {
        matrix[i] = malloc(cols * sizeof(int));
    }
    return matrix;
}

void afficherMatriceDynamique(int** matrix, int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            printf("%d ", matrix[i][j]);
        }
        printf("\n");
    }
}

void libererMatrice(int** matrix, int rows) {
    for (int i = 0; i < rows; i++) {
        free(matrix[i]);
    }
    free(matrix);
}

int main() {
    int rows, cols;
    printf("Nombre de lignes : ");
    scanf("%d", &rows);
    printf("Nombre de colonnes : ");
    scanf("%d", &cols);

    int **matrix = creerMatriceDynamique(rows, cols);

    // Remplissage
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            printf("matrix[%d][%d] = ", i, j);
            scanf("%d", &matrix[i][j]);
        }
    }

    afficherMatriceDynamique(matrix, rows, cols);
    libererMatrice(matrix, rows);

    return 0;
}
```

🧠 **Explication**

* `malloc` permet de créer une matrice à **taille variable**.
* `free` libère la mémoire pour éviter les fuites.
* Boucles imbriquées pour remplir et afficher la matrice dynamiquement.
