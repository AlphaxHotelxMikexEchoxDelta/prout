# 📘 **Cours : Organisation du code en C – utiliser un fichier dans un autre**

En C, on sépare souvent le code en plusieurs fichiers pour mieux organiser un projet.
Quand on veut utiliser depuis un fichier Y ce qui est dans un fichier X, on utilise le modèle :

```
X.h     → déclarations
X.c     → définition du code
Y.c     → utilise X via “#include"
```

---

# 1️⃣ Pourquoi séparer le code en plusieurs fichiers ?

## ✔️ Pour organiser le code

Éviter un énorme fichier C illisible.

## ✔️ Pour réutiliser du code facilement

Une fonction écrite dans X peut être réutilisée dans 10 autres fichiers Y, Z, T, etc.

## ✔️ Pour faciliter la maintenance

Modifier X n’oblige pas à changer Y.

## ✔️ Pour réduire les erreurs

Les prototypes dans X.h disent à tout le projet :

> « Voici comment utiliser les fonctions de X »

---

# 2️⃣ Le rôle des fichiers `.h` et `.c`

## 🔹 **Le fichier .c : contient le code réel**

Ex : `maths.c`

```c
int addition(int a, int b) {
    return a + b;
}
```

Le `.c` contient les **définitions** :
👉 le vrai code exécuté.

---

## 🔹 **Le fichier .h : contient les déclarations**

Ex : `maths.h`

```c
#ifndef MATHS_H
#define MATHS_H

int addition(int a, int b);

#endif
```

Le `.h` dit au compilateur :

> « Cette fonction existe quelque part, tu peux l’utiliser. »

Mais il ne contient **pas le code complet**.

---

# 🎯 Pourquoi un .h ?

Voici les raisons essentielles :

## ✔️ 1. Éviter de copier les prototypes dans chaque fichier

Sans `.h`, tu serais obligé de recopier à la main :

```c
int addition(int a, int b);
```

dans 5, 10, 20 fichiers…

C’est ingérable.

---

## ✔️ 2. Permettre au compilateur de vérifier les appels

Avec un .h, si tu appelles la fonction avec les mauvais types, le compilateur te prévient.

---

## ✔️ 3. Éviter les doublons grâce aux “include guards”

Les lignes :

```c
#ifndef MATHS_H
#define MATHS_H
```

empêchent de charger le fichier deux fois → éviter les conflits.

---

# 3️⃣ Comment utiliser X dans Y ? (La méthode standard)

Voici la **structure correcte** :

```
maths.h  
maths.c  
main.c  
```

---

### 🔹 1. `maths.h` — déclarations

```c
#ifndef MATHS_H
#define MATHS_H

int addition(int a, int b);
void afficherResultat(int r);

#endif
```

---

### 🔹 2. `maths.c` — définitions du code

```c
#include <stdio.h>
#include "maths.h"

int addition(int a, int b) {
    return a + b;
}

void afficherResultat(int r) {
    printf("Résultat : %d\n", r);
}
```

Note :
`#include "maths.h"` garantit que les prototypes et les définitions sont cohérents.

---

### 🔹 3. `main.c` — utilisation des fonctions de maths.c

```c
#include <stdio.h>
#include "maths.h"

int main() {
    int r = addition(5, 7);
    afficherResultat(r);
    return 0;
}
```

---

# 4️⃣ Comment compiler ?

Tu dois compiler *tous les fichiers .c* :

```bash
gcc main.c maths.c -o programme
```

---

# 5️⃣ Ce qu'il ne faut pas faire (mais que les débutants font)

## ❌ 1. Inclure directement un fichier .c

```c
#include "maths.c"
```

Pourquoi c’est mauvais ?

* Ça casse la compilation séparée
* Ça duplique le code
* Ça crée des erreurs de lien
* C’est un anti-pattern total

---

## ❌ 2. Déclarer les prototypes à la main dans chaque fichier

Oui, ça marche, mais…

* Si tu modifies la fonction dans maths.c, tu dois corriger tous les fichiers
* Risque d’erreurs monstrueux
* Tu perds l’intérêt des includes

---

# 6️⃣ Processus complet résumée

| Étape | Fichier     | Rôle                            |
| ----- | ----------- | ------------------------------- |
| 1     | X.h         | Déclare les fonctions publiques |
| 2     | X.c         | Implémente les fonctions        |
| 3     | Y.c         | Utilise X via `#include "X.h"`  |
| 4     | Compilation | gcc X.c Y.c                     |

---

# 7️⃣ Pourquoi C fonctionne comme ça ?

Parce que :

* C compile **séparément** chaque fichier `.c`
* Mais a besoin de connaître **les prototypes** pour vérifier les appels
* Le `.h` sert de contrat entre les fichiers
  → c’est exactement comme une interface en Java ou un module export en JS.

---

# 8️⃣ Exemple très visuel

### Ta situation :

```
main.c veut utiliser addition() de maths.c
```

### Avec un `.h`

```
main.c → (include) → maths.h → (lien) → maths.c
```

### Sans `.h`

Le compilateur ne sait pas ce qu’est `addition()` → erreurs.
