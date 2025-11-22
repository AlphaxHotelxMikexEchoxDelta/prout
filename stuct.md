# 📘 **COURS COMPLET ET DÉTAILLÉ : LES STRUCTURES EN C**

---

# 🔷 0. Pourquoi les structures existent ? (explication complète)

Le langage C manipule des **types simples** :

* `int` → nombre entier
* `char` → caractère
* `float` → réel
* `double` → réel double précision
* `char[]` → tableau de caractères (chaîne)

Mais **dans la vraie vie**, les données ne viennent jamais seules.

Exemple : un étudiant, ce n’est pas juste un nom.
C’est :

* un nom
* un âge
* une note
* peut-être un numéro d’étudiant
* une adresse

Tu pourrais faire :

```c
char nom[20];
int age;
float note;
```

Mais tous ces éléments **forment un ensemble logique**.

Le langage C introduit donc :

# 🎯 **La structure (`struct`) = un type personnalisé qui regroupe plusieurs variables**

C’est un mécanisme pour créer un *type composite*.

Autrement dit :

> Une structure est un “mini-objet” sans méthodes.

---

# 🔷 1. Définir une structure (explication détaillée)

Voici la forme générale :

```c
struct NomDeLaStructure {
    type champ1;
    type champ2;
    type champX;
};
```

## 📌 Exemple détaillé

```c
struct Etudiant {
    char nom[20];  // un tableau de 20 caractères
    int age;       // un entier
    float note;    // un flottant
};
```

### 📌 Ce qui se passe réellement

Quand le compilateur voit :

```c
struct Etudiant {
    char nom[20];
    int age;
    float note;
};
```

Il crée un **nouveau type**.
Ce type n’existe que *dans le langage* → aucune mémoire n’est encore allouée.

---

# 🔷 2. Déclarer une variable structure

```c
struct Etudiant e1;
```

### 📌 Ce qui se passe réellement

* `e1` est alloué en mémoire
* la taille de `e1` est la somme des tailles de ses champs (plus éventuels alignements)

Tu accèdes aux champs avec le point :

```c
e1.age = 18;
e1.note = 15.5;
strcpy(e1.nom, "Alice");
```

---

# 🔷 3. Pourquoi `typedef` est utilisé dans 95% des codes réels ?

En C, sans `typedef`, tu dois toujours écrire :

```c
struct Etudiant e;
```

Ce qui est long, répétitif, moche.

Avec `typedef` :

```c
typedef struct {
    char nom[20];
    int age;
    float note;
} Etudiant;
```

Tu peux écrire :

```c
Etudiant e;
```

C’est plus court, plus propre, plus lisible, plus moderne.

---

# 🔷 4. Initialisation complète

## ✔️ Méthode champ par champ

```c
Etudiant e;
e.age = 20;
e.note = 15.5;
strcpy(e.nom, "Bob");
```

### Pourquoi ?

Parce qu’une structure n’est pas automatiquement initialisée en C.
La mémoire contient n’importe quoi si tu ne l’initialises pas.

---

## ✔️ Méthode directe

```c
Etudiant e = {"Bob", 20, 15.5};
```

### Comment ça marche ?

Les valeurs sont assignées **dans l’ordre des champs**.

---

# 🔷 5. Pointeurs sur structures (explication hyper claire)

```c
Etudiant *p = &e;
```

* `p` est un **pointeur**
* `&e` donne **l’adresse** de `e` en mémoire

Pour accéder aux champs via un pointeur :

## ❌ Mauvais :

```c
(*p).age = 22;
```

## ✔️ Bon :

```c
p->age = 22;
```

### Pourquoi `->` existe ?

Parce que accéder à un champ via un pointeur est tellement courant qu’on a créé un opérateur spécial.

---

# 🔷 6. Passer une structure à une fonction

## ✔️ Par valeur (copie totale)

```c
void afficher(Etudiant e) {
    printf("%s %d %f\n", e.nom, e.age, e.note);
}
```

Quand tu fais :

```c
afficher(e);
```

➡️ **La structure entière est copiée.**

Si elle fait 1000 octets → 1000 octets copiés.

### Inconvénient :

* lent si la structure est grosse
* les modifications ne sont pas renvoyées

---

## ✔️ Par adresse (recommandé)

```c
void changerAge(Etudiant *e, int nouvelAge) {
    e->age = nouvelAge;
}
```

Appel :

```c
changerAge(&e, 25);
```

### Ce qui se passe :

* seul un **pointeur de 8 octets** est passé
* aucune copie lourde
* la fonction modifie l’original

---

# 🔷 7. Structures dans plusieurs fichiers (très important)

Voici comment organiser un projet proprement :

```
etudiant.h  
etudiant.c  
main.c  
```

## Pourquoi mettre la structure dans le .h ?

Parce que :

* une structure est un **type**
* un type doit être visible partout
* le .h est l’endroit où on met les types et les fonctions publiques

---

## 🔹 etudiant.h — “l’interface”

```c
#ifndef ETUDIANT_H
#define ETUDIANT_H

typedef struct {
    char nom[20];
    int age;
    float note;
} Etudiant;

void afficher(Etudiant e);
void changerAge(Etudiant *e, int age);

#endif
```

### Explication :

* `typedef struct { ... } Etudiant;` → création du type
* `void afficher(...)` → déclaration (pas de code)
* `#ifndef ETUDIANT_H` → empêche les inclusions multiples

---

## 🔹 etudiant.c — “l’implémentation”

```c
#include <stdio.h>
#include <string.h>
#include "etudiant.h"

void afficher(Etudiant e){
    printf("Nom: %s | Age : %d | Note: %.2f\n",
           e.nom, e.age, e.note);
}

void changerAge(Etudiant *e, int a){
    e->age = a;
}
```

---

## 🔹 main.c — utilisation

```c
#include <stdio.h>
#include "etudiant.h"

int main() {
    Etudiant e = {"Alice", 19, 16.0};

    afficher(e);
    changerAge(&e, 25);
    afficher(e);

    return 0;
}
```

---

# 🔷 8. Structures imbriquées

Tu peux mettre une structure dans une autre.

```c
typedef struct {
    int jour, mois, annee;
} Date;

typedef struct {
    char nom[20];
    Date naissance;
} Personne;
```

Accès :

```c
Personne p;
p.naissance.jour = 15;
```

---

# 🔷 9. Structures et tableaux

```c
Etudiant classe[30];
classe[0].age = 17;
```

Avec pointeur :

```c
Etudiant *p = &classe[5];
p->note = 14;
```

---

# 🔷 🔟 Structures et fichiers

### 🎯 En binaire :

```c
fwrite(&e, sizeof(Etudiant), 1, fichier);
```

Lecture :

```c
fread(&e, sizeof(Etudiant), 1, fichier);
```

➡️ La structure entière est transférée d’un coup.

---

# 🎉 **Résumé hyper clair**

| Concept                     | Explication                             |
| --------------------------- | --------------------------------------- |
| `struct`                    | crée un type personnalisé               |
| `typedef`                   | permet d’écrire le type plus facilement |
| `.`                         | accès aux champs via une variable       |
| `->`                        | accès aux champs via un pointeur        |
| passage par adresse         | modifie l’original                      |
| structure dans .h           | visibilité globale                      |
| structure dans .c           | code réel                               |
| pointeur sur struct         | essentiel pour la performance           |
| lecture/écriture en binaire | structure écrite d’un bloc              |
