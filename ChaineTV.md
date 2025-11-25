# 🧵 Cours : Les chaînes de caractères en C (niveau débutant)

Les **chaînes de caractères** (strings) en C ne sont pas un type de données intégré comme dans d’autres langages.
Elles sont représentées sous forme de **tableaux de caractères** terminés par un caractère spécial : `'\0'` (nul terminator).

---

# 1️⃣ Qu’est-ce qu’une chaîne de caractères ?

En C :

* Une chaîne = **un tableau de `char`**
* Fin obligatoire par le caractère **nul** `'\0'`

Exemple :

```c
char mot[] = "Bonjour";
```

En mémoire, cela donne :

```
B | o | n | j | o | u | r | \0
```

➡️ Le `\0` marque la fin de la chaîne. Sans lui, impossible de connaître la longueur.

---

# 2️⃣ Déclarer une chaîne de caractères

### ✔ Méthode 1 : affectation directe

```c
char mot[] = "Hello";
```

La taille s’adapte automatiquement (6 cases ici : 5 lettres + `\0`).

### ✔ Méthode 2 : tableau avec taille fixe

```c
char nom[20] = "Alice";
```

Tu peux y stocker jusqu’à 19 caractères + `\0`.

### ✔ Méthode 3 : caractere par caractere

```c
char t[] = {'C', 'o', 'u', 'c', 'o', 'u', '\0'};
```

---

# 3️⃣ Lire une chaîne au clavier

### ⚠ Danger : `scanf("%s", ...)` s’arrête au premier espace

```c
char nom[20];
scanf("%19s", nom);   // protège contre le dépassement
```

➡️ Ne lit pas les espaces.

### ✔ Pour lire une ligne entière :

```c
fgets(nom, 20, stdin);
```

`fgets` lit **les espaces** et évite les débordements.
Elle stocke aussi le '\n' de fin de ligne, que tu peux retirer si besoin.

---

# 4️⃣ Afficher une chaîne

Très simple :

```c
printf("Nom : %s\n", nom);
```

---

# 5️⃣ Fonctions essentielles (bibliothèque `<string.h>`)

Inclure la bibliothèque :

```c
#include <string.h>
```

## ✔ `strlen` – longueur (sans le `\0`)

```c
size_t len = strlen("Bonjour"); // len = 7
```

## ✔ `strcpy` – copier une chaîne

```c
char a[20];
strcpy(a, "Salut");
```

⚠ Attention à la taille du tableau !

## ✔ `strcat` – concaténer

```c
char phrase[50] = "Bonjour ";
strcat(phrase, "tout le monde");
```

## ✔ `strcmp` – comparaison

```c
int r = strcmp("abc", "abd");  
// r < 0 : première < seconde
// r = 0 : égales
// r > 0 : première > seconde
```

---

# 6️⃣ Exemple complet

Voici un programme simple qui :

* lit le nom et le prénom,
* les concatène,
* affiche un message.

```c
#include <stdio.h>
#include <string.h>

int main() {
    char prenom[20];
    char nom[20];
    char complet[50];

    printf("Entrez votre prénom : ");
    fgets(prenom, 20, stdin);

    printf("Entrez votre nom : ");
    fgets(nom, 20, stdin);

    // On enlève les '\n' éventuels
    prenom[strcspn(prenom, "\n")] = '\0';
    nom[strcspn(nom, "\n")] = '\0';

    // Construction de la chaîne complète
    strcpy(complet, prenom);
    strcat(complet, " ");
    strcat(complet, nom);

    printf("Bonjour %s !\n", complet);

    return 0;
}
```

---

# 7️⃣ À retenir absolument

* Une **chaîne = tableau de char terminé par `'\0'`**
* Toujours prévoir **une case de plus** pour le `\0`
* Utiliser `fgets` plutôt que `scanf`
* Gérer les dépassements de tableaux (source de bugs en C)
* Utiliser `<string.h>` pour manipuler les chaînes

---

# ⚠️ Cours : Que se passe-t-il si je dépasse la capacité d’une chaîne en C ?

En C, le langage **ne vérifie jamais** que tu restes dans les limites d’un tableau.
Si tu écris trop de caractères dans un tableau (ex : plus de 20 dans `char nom[20]`), tu provoques ce qu’on appelle un **buffer overflow** (dépassement de tampon).

C’est l’une des erreurs les plus dangereuses du langage.

---

# 1️⃣ Pourquoi le dépassement de tableau est dangereux ?

Parce qu’un tableau est placé dans la **mémoire**, juste à côté d’autres variables.
Si tu dépasses la taille, tu écris **sur autre chose**.

## Exemple :

```c
char nom[5];  // espace pour 4 lettres + '\0'
strcpy(nom, "Bonjour");
```

"Bonjour" = 8 caractères + `\0` → 9 cases
Mais le tableau n'en a que **5** !

➡️ Tu écris dans la mémoire voisine (qui appartient à d’autres variables), ce qui peut provoquer :

* valeurs corrompues
* comportement imprévisible
* crash (segmentation fault)
* faille de sécurité exploitable

---

# 2️⃣ Les conséquences possibles

Voici ce qui **peut** arriver quand tu dépasses :

## ✔ 1. Le programme continue… mais se comporte bizarrement

Certaines variables changent de valeur comme par magie.

## ✔ 2. Le programme plante

Souvent :

```
Segmentation fault (core dumped)
```

## ✔ 3. Le programme semble fonctionner… mais est en réalité corrompu

Les erreurs invisibles sont les pires.

## ✔ 4. Faille de sécurité

Beaucoup d’attaques historiques exploitent cela.

---

# 3️⃣ Exemple concret de corruption

```c
#include <stdio.h>
#include <string.h>

int main() {
    char nom[5];
    int age = 25;

    strcpy(nom, "Robert");  // trop long !

    printf("Nom = %s\n", nom);
    printf("Age = %d\n", age);
}
```

Possible résultat :

```
Nom = Robert
Age = 4791232   <-- valeur corrompue
```

➡️ Le mot "Robert" a débordé et écrasé l’espace mémoire où était stocké `age`.

---

# 4️⃣ Pourquoi C laisse faire ?

Parce que C est un langage **bas niveau**, très proche de la machine.
Il ne met **aucune barrière** pour des raisons de performance.
C’est au programmeur de s’assurer :

* que les tailles sont correctes
* que les chaînes rentrent dans les tableaux
* que le `\0` est présent

---

# 5️⃣ Comment éviter ces dépassements ?

## ✔ Toujours prévoir une taille suffisante

```c
char nom[100];
```

## ✔ Utiliser `fgets` pour les entrées

```c
fgets(nom, sizeof(nom), stdin);
```

## ✔ Utiliser `strncpy` plutôt que `strcpy`

```c
strncpy(nom, source, sizeof(nom) - 1);
nom[sizeof(nom) - 1] = '\0';  // sécurité
```

## ✔ Toujours laisser place au `'\0'`

Un tableau qui contient 10 caractères doit avoir **au moins 11 cases**.

---

# 6️⃣ Résumé simple

* ➤ Dépasser un tableau = **buffer overflow**
* ➤ Le programme peut : planter, mal se comporter, corrompre la mémoire
* ➤ Le C ne vérifie rien → tu dois toujours calculer la taille
* ➤ Utilise `fgets`, tailles explicites, et vérifie le `\0`

---
