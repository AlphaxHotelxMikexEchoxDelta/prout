# 📘 **Cours : Les tableaux multidimensionnels en C**

## 🔹 1. Les tableaux en C

En C, un tableau (**array**) est une zone mémoire contenant des éléments **de même type**, avec une **taille fixe** définie à la compilation :

```c
int a[10];     // tableau de 10 entiers
```

Contrairement à Python :

* un tableau C **n'est pas dynamique**
* un tableau C **ne peut pas changer de taille**
* chaque dimension doit être connue à la compilation (sauf cas avancés avec malloc)

---

# 🔹 2. Tableaux à 2 dimensions : matrices

Pour créer une matrice 4×4 :

```c
int matrix[4][4] = {
    {0, 0, 0, 0},
    {0, 0, 0, 0},
    {0, 0, 0, 0},
    {0, 0, 0, 0}
};
```

👉 C’est l’équivalent de :

```python
matrix = [[0,0,0,0], [0,0,0,0], [0,0,0,0], [0,0,0,0]]
```

### Pourquoi `matrix[x][y]` ?

Parce que :

* `matrix[x]` = la ligne x (un tableau)
* `matrix[x][y]` = l’élément de la ligne x et colonne y

👉 Un tableau 2D en C est **un tableau de tableaux**.

---

# 🔹 3. Tableaux à 3 dimensions

Oui, c’est possible :

```c
int cube[4][4][4];
```

Accès :

```c
cube[x][y][z];
```

Cela représente un volume en 3D.

---

# 🔹 4. Tableaux à N dimensions

Il est possible d’aller beaucoup plus loin :

```c
int hypercube[2][2][2][2][2][2][2][2]; // 8 dimensions
```

En théorie :
➡️ **Dimensions illimitées**

En pratique :
➡️ limité par la **mémoire** et la **lisibilité du code**

---

# 🔹 5. Limites en pratique

Même si tu peux écrire jusqu’à 10 ou 20 dimensions, chaque dimension multiplie la taille :

Exemple : 10 dimensions de taille 10 :

```
10^10 = 10 000 000 000 éléments
```

Avec des `int` (4 octets) :

```
→ 40 Go de RAM !
```

➡️ **Impossible en pratique**

### Usages réels :

| Dimensions | Usages                            |
| ---------- | --------------------------------- |
| 1D         | listes                            |
| 2D         | matrices, tableaux                |
| 3D         | volumes, pixels RGB, jeux         |
| 4D         | rares (simulations scientifiques) |
| 5D+        | presque jamais utilisés           |

---

# 🔹 6. Résumé global

* En C, chaque paire de `[]` ajoute **une dimension de tableau**.
* `matrix[x][y]` = tableau 2D → tableau de tableaux.
* Tu peux avoir des tableaux 3D, 4D, etc.
* Limite théorique : **illimitée**
* Limite pratique : mémoire + lisibilité
* C ≠ Python : les tableaux en C ont une **taille fixe** et sont beaucoup plus stricts.
