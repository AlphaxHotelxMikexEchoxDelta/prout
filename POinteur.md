# ✅ **Cours : Les pointeurs en C (niveau débutant)**

Les pointeurs peuvent sembler compliqués, mais on va aller **lentement** avec des exemples simples.

---

# 🌟 1) Qu’est-ce qu’une variable ?

Une variable, c’est comme une **boîte qui contient une valeur**.

```c
int x = 5;
```

Ici :

* `x` → le nom de la boîte
* `5` → la valeur dedans

---

# 🌟 2) Une adresse mémoire

Dans la mémoire de l’ordinateur, chaque variable est rangée quelque part.
On peut connaître sa position avec l’opérateur `&`.

```c
int x = 5;
printf("%p", &x);   // affiche l'adresse de x
```

`%p` sert à afficher une adresse.

---

# 🌟 3) Qu’est-ce qu’un pointeur ?

Un pointeur est une variable qui contient **une adresse**.

Donc si une variable normale contient une valeur,
→ un pointeur contient l’adresse d’une variable.

Exemple :

```c
int x = 5;   // variable normale
int *p;      // pointeur vers un int

p = &x;      // p reçoit l'adresse de x
```

📌 `int *p` : signifie que p est un pointeur vers un int.

---

# 🌟 4) Lire la valeur via un pointeur

`*p` signifie : “la valeur à l’adresse contenue dans p”.

```c
printf("%d", *p);   // affiche 5
```

✅ `p` = adresse
✅ `*p` = valeur à cette adresse

---

# ✅ Résumé important

| Symbole | Signifie                         |
| ------- | -------------------------------- |
| `&x`    | adresse de x                     |
| `*p`    | valeur pointée                   |
| `p`     | adresse stockée dans le pointeur |

---

# 🌟 5) Exemple complet

```c
#include <stdio.h>

int main() {
    int x = 5;
    int *p;

    p = &x;

    printf("Valeur de x = %d\n", x);
    printf("Adresse de x = %p\n", &x);
    printf("Valeur de p = %p\n", p);
    printf("Valeur pointee par p = %d\n", *p);

    return 0;
}
```

---

# 🌟 6) Modifier la valeur avec un pointeur

```c
int x = 5;
int *p = &x;

*p = 10;

printf("%d", x);   // affiche 10
```

On modifie la valeur de `x` grâce à `*p`.

---

# 🌟 7) Pourquoi utiliser des pointeurs ?

✅ Pour modifier une variable dans une fonction
✅ Pour travailler avec des tableaux
✅ Pour gérer la mémoire (malloc)
✅ Pour des structures plus avancées (ex : listes chaînées)

Exemple simple : modifier une variable dans une fonction

```c
void change(int *a) {
    *a = 20;
}

int main() {
    int x = 5;
    change(&x);
    printf("%d\n", x);   // affiche 20
    return 0;
}
```

---

# 🌟 8) Le pointeur NULL

Si un pointeur ne pointe sur rien, on peut le mettre à `NULL`.

```c
int *p = NULL;
```

Toujours vérifier avant d’utiliser `*p`.

---


# ✅ Conclusion

Tu sais maintenant :
✔ ce qu’est une adresse
✔ ce qu’est un pointeur
✔ utiliser `&` et `*`
✔ modifier une variable grâce à un pointeur

---


# ✅ **5 exercices — Pointeurs (débutant avancé)

---

## 🧩 Exercice 1 — Mise à jour d’un score

Un programme de gestion de partie stocke le score du joueur dans une variable entière.
On souhaite appliquer un bonus de points au score pendant le jeu.

Écrire une fonction :

```c
void appliquerBonus(int *score, int bonus);
```

Cette fonction doit :
✔ Ajouter `bonus` à `score` si `bonus >= 0`.
✔ Si `bonus < 0`, afficher :

> `"Bonus invalide"`
> et **ne pas modifier le score**.

Dans `main` :

1. Déclarer un score initial (ex : 120).
2. Appeler la fonction avec plusieurs valeurs (positives et négatives).
3. Afficher le score après chaque appel.

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

void appliquerBonus(int *score, int bonus) {
    if (bonus < 0) {
        printf("Bonus invalide\n");
        return;
    }
    *score += bonus;
}

int main() {
    int score = 120;
    appliquerBonus(&score, 30);
    printf("Score = %d\n", score);

    appliquerBonus(&score, -10);
    printf("Score = %d\n", score);

    return 0;
}
```

🧠 **Explication**

* Le paramètre `score` est un pointeur → modification directe.
* Si bonus négatif → aucun changement.

</details>

---

## 🧩 Exercice 2 — Ajustement du volume

Une application multimédia stocke le volume sonore sous forme d’un entier allant de 0 à 100.

Écrire une fonction :

```c
void reglerVolume(int *volume, int delta);
```

Cette fonction doit :
✔ Ajouter `delta` au volume existant.
✔ Limiter le volume dans l’intervalle [0,100].
→ Si le résultat dépasse 100 → le fixer à 100.
→ Si le résultat devient < 0 → le fixer à 0.

Dans `main` :

1. Initialiser un volume (ex : 80).
2. Test : +30 puis -120
3. Afficher après chaque modification.

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

void reglerVolume(int *volume, int delta) {
    *volume += delta;
    if (*volume > 100) *volume = 100;
    if (*volume < 0)   *volume = 0;
}

int main() {
    int vol = 80;
    reglerVolume(&vol, 30);
    printf("Volume = %d\n", vol);

    reglerVolume(&vol, -120);
    printf("Volume = %d\n", vol);
    return 0;
}
```

🧠 **Explication**

* Le pointeur permet de modifier la valeur directement dans `main`.
* Les bornes sont gérées après modification.

</details>

---

## 🧩 Exercice 3 — Détection de température négative

On enregistre la température horaire sur une journée dans un tableau d'entiers.
On veut détecter la **première température négative** pour lancer une alerte.

Écrire une fonction :

```c
int* trouverNegative(int *tab, int n);
```

Cette fonction doit :
✔ Parcourir le tableau via pointeur
✔ Retourner l’adresse du premier élément < 0
✔ Retourner `NULL` si aucune valeur négative n’existe

Dans `main`:

1. Déclarer un tableau d’exemple (positif + négatif).
2. Vérifier le retour de la fonction.
3. Afficher la valeur négative trouvée ou “Aucune”.

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

int* trouverNegative(int *tab, int n) {
    for (int i = 0; i < n; i++)
        if (*(tab + i) < 0)
            return tab + i;
    return NULL;
}

int main() {
    int t[] = {12, 5, 3, -2, 6, -8};
    int *p = trouverNegative(t, 6);

    if (p != NULL)
        printf("1ère négative = %d\n", *p);
    else
        printf("Aucune valeur négative\n");

    return 0;
}
```

🧠 **Explication**

* `tab + i` → pointeur vers l’élément.
* Retour d’adresse → permet une utilisation ultérieure.

</details>

---

## 🧩 Exercice 4 — Nettoyage d’inventaire

Un stock d’objets est représenté par un tableau d’entiers.
Certaines quantités peuvent être négatives (erreurs) ou nulles (épuisées).
On souhaite nettoyer l’inventaire.

Écrire une fonction :

```c
void nettoyerInventaire(int *inv, int n);
```

✔ Parcourir le tableau
✔ Chaque valeur ≤ 0 est remplacée par 0
✔ Les autres restent inchangées

Dans `main` :

1. Déclarer un tableau avec différentes valeurs.
2. Appeler la fonction.
3. Afficher l’inventaire nettoyé.

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

void nettoyerInventaire(int *inv, int n) {
    for (int i = 0; i < n; i++)
        if (*(inv + i) <= 0)
            *(inv + i) = 0;
}

int main() {
    int stock[] = {3, -1, 0, 7, -5};
    nettoyerInventaire(stock, 5);

    for (int i = 0; i < 5; i++)
        printf("%d ", stock[i]);
    return 0;
}
```

🧠 **Explication**

* Accès via `*(inv + i)`
* Remplacement direct dans le tableau

</details>

---

## 🧩 Exercice 5 — Échange de notes extrêmes

On dispose d’un tableau contenant des notes d’étudiants.
On veut analyser et mettre en évidence les extrêmes :
→ échanger la plus petite note avec la plus grande.

Écrire une fonction :

```c
void echangerExtremes(int *tab, int n);
```

La fonction doit :
✔ Identifier l’adresse du plus petit élément
✔ Identifier l’adresse du plus grand élément
✔ Échanger leurs valeurs
❗ Si le minimum et maximum sont au même endroit (tableau constant), ne rien faire

Dans `main`:

1. Déclarer un tableau de notes.
2. Appeler la fonction.
3. Afficher le tableau après échange.

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

void echangerExtremes(int *tab, int n) {
    int *pMin = tab;
    int *pMax = tab;

    for (int i = 1; i < n; i++) {
        if (*(tab + i) < *pMin) pMin = tab + i;
        if (*(tab + i) > *pMax) pMax = tab + i;
    }

    if (pMin != pMax) {
        int tmp = *pMin;
        *pMin = *pMax;
        *pMax = tmp;
    }
}

int main() {
    int notes[] = {12, 3, 18, 9, 7};
    echangerExtremes(notes, 5);

    for (int i = 0; i < 5; i++)
        printf("%d ", notes[i]);
    return 0;
}
```

🧠 **Explication**

* pMin et pMax stockent les **adresses** des extrêmes.
* Échange via `*pMin` et `*pMax`.
* Vérification pour éviter un échange inutile.

</details>
