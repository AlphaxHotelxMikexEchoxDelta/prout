## 🧩 Exercice 1 — Déclaration et affichage simple

Écrire un programme qui :

→ déclare une chaîne char prenom[15]
→ y stocke le texte "Paul"
→ affiche le prénom avec printf

Aucune lecture clavier dans cet exercice.

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>

int main() {
    char prenom[15] = "Paul";

    printf("Prénom : %s\n", prenom);
    return 0;
}


🧠 Explication

La chaîne "Paul" tient dans le tableau : 4 caractères + '\0'.

L’indexation et la fin de chaîne sont gérées automatiquement.

%s permet d’afficher une chaîne terminée par '\0'.

</details>

---

## 🧩 Exercice 2 — Lecture sécurisée avec fgets

On veut lire un nom de maximum 19 caractères depuis le clavier.
Comme scanf("%s") ne lit pas les espaces et peut dépasser, on utilise fgets.

Travail demandé :

✔ déclarer char nom[20]
✔ lire la ligne avec fgets
✔ enlever le \n s’il existe
✔ afficher : Bonjour, NOM !

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>
#include <string.h>

int main() {
    char nom[20];

    printf("Entrez votre nom : ");
    fgets(nom, 20, stdin);

    // Supprime le '\n'
    nom[strcspn(nom, "\n")] = '\0';

    printf("Bonjour, %s !\n", nom);
    return 0;
}


🧠 Explication

fgets lit au maximum 19 caractères + '\0'.

strcspn trouve l’index du \n et permet de le remplacer par '\0'.

Cela évite un dépassement de tableau et lit les espaces.

</details>

---
## 🧩 Exercice 3 — Vérifier la longueur avant affichage

On veut empêcher l’utilisateur d’entrer une chaîne trop longue.
On limitera l’affichage si la longueur dépasse 20.

Travail demandé :

✔ lire une chaîne dans char mot[30]
✔ mesurer la longueur avec strlen
✔ si len > 20 → afficher un message d’erreur
✔ sinon → afficher la chaîne

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>
#include <string.h>

int main() {
    char mot[30];

    printf("Entrez un mot : ");
    fgets(mot, 30, stdin);

    mot[strcspn(mot, "\n")] = '\0';

    if (strlen(mot) > 20)
        printf("Erreur : chaîne trop longue !\n");
    else
        printf("Vous avez entré : %s\n", mot);

    return 0;
}


🧠 Explication

strlen retourne le nombre de caractères sans compter '\0'.

On vérifie la longueur après lecture pour détecter un texte trop long.

Comme fgets empêche déjà le dépassement, le programme est sûr.

</details>

---
## 🧩 Exercice 4 — Concaténation de deux chaînes

On veut construire un nom complet à partir d’un prénom et d’un nom.

Travail demandé :

✔ déclarer char prenom[20], char nom[20], char complet[50]
✔ lire prénom et nom
✔ retirer les \n
✔ copier prenom dans complet
✔ concaténer " " puis nom
✔ afficher le résultat

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>
#include <string.h>

int main() {
    char prenom[20];
    char nom[20];
    char complet[50];

    printf("Prénom : ");
    fgets(prenom, 20, stdin);

    printf("Nom : ");
    fgets(nom, 20, stdin);

    prenom[strcspn(prenom, "\n")] = '\0';
    nom[strcspn(nom, "\n")] = '\0';

    strcpy(complet, prenom);
    strcat(complet, " ");
    strcat(complet, nom);

    printf("Nom complet : %s\n", complet);

    return 0;
}


🧠 Explication

On utilise strcpy pour initialiser complet.

strcat ajoute du texte à la fin, mais requiert une taille suffisante.

Le tableau de 50 caractères est assez grand pour éviter un dépassement.

</details>


---
## 🧩 Exercice 5 — Observer un dépassement de tableau

On va volontairement écrire trop de données dans un tableau pour observer le comportement (dangereux) d’un buffer overflow.

Travail demandé :

✔ déclarer char t[5]
✔ déclarer int a = 123
✔ copier "ABCDEFG" dans t (trop long)
✔ afficher t et a
✔ noter les comportements anormaux

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>
#include <string.h>

int main() {
    char t[5];
    int a = 123;

    strcpy(t, "ABCDEFG"); // Dépasse largement !

    printf("t = %s\n", t);
    printf("a = %d\n", a);

    return 0;
}


🧠 Explication

"ABCDEFG" = 7 caractères + '\0' → 8 octets écrits

t ne possède que 5 octets → écrasement de mémoire

Les effets possibles :

valeur de a corrompue

plantage

affichage incohérent

comportement non déterministe

➡️ C’est exactement pourquoi les chaînes doivent toujours être manipulées avec prudence en C.

</details>

--- 

## 🧩 Exercice 6 — Parcourir une chaîne avec un pointeur

On veut afficher chaque caractère d’une chaîne en utilisant **un pointeur qui avance** dans le tableau.

Travail demandé :

✔ déclarer `char mot[] = "Bonjour";`
✔ déclarer un pointeur `char *p` pointant sur le début
✔ parcourir et afficher chaque caractère jusqu’à `'\0'`
✔ afficher les caractères **sur une seule ligne**, séparés par un espace

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

int main() {
    char mot[] = "Bonjour";
    char *p = mot;

    while (*p != '\0') {
        printf("%c ", *p);
        p++; // on avance dans la chaîne
    }

    return 0;
}
```

🧠 **Explication**

* `p` pointe directement sur les cases du tableau.
* `*p` lit le caractère pointé.
* `p++` déplace le pointeur d’un octet (un char).

</details>

---

##🧩 Exercice 7 — Compter la longueur d’une chaîne avec un pointeur

Écrire une fonction qui calcule la longueur d’une chaîne **sans utiliser `strlen`** mais uniquement un pointeur.

Prototype :

```c
int longueur(const char *p);
```

Travail demandé :

✔ avancer le pointeur jusqu’à `'\0'`
✔ compter les caractères
✔ tester dans `main`

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

int longueur(const char *p) {
    int c = 0;
    while (*p != '\0') {
        c++;
        p++;
    }
    return c;
}

int main() {
    char texte[] = "Programmation";
    printf("Longueur = %d\n", longueur(texte));
    return 0;
}
```

🧠 **Explication**

* On ne bouge pas le tableau, seulement le pointeur.
* `*p` permet de lire le caractère courant.
* On s’arrête au caractère nul.

</details>

---

## 🧩 Exercice 8 — Copier une chaîne avec des pointeurs

Écrire une fonction qui copie une chaîne source vers une destination, comme `strcpy`, mais en utilisant uniquement des pointeurs.

Prototype :

```c
void copie(char *dest, const char *src);
```

Travail demandé :

✔ copier caractère par caractère via `*dest = *src`
✔ avancer les deux pointeurs
✔ ajouter le `'\0'` final
✔ tester dans `main`

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

void copie(char *dest, const char *src) {
    while (*src != '\0') {
        *dest = *src;
        dest++;
        src++;
    }
    *dest = '\0'; // fin de chaîne
}

int main() {
    char source[] = "Salut";
    char destination[20];

    copie(destination, source);
    printf("Copie : %s\n", destination);

    return 0;
}
```

🧠 **Explication**

* Le pointeur source lit, le pointeur destination écrit.
* On avance simultanément jusqu’au `'\0'`.
* Le programme réimplémente un mini-`strcpy`.

</details>

---

## 🧩 Exercice 9 — Trouver un caractère dans une chaîne

Écrire une fonction qui retourne l’adresse de la **première occurrence** d’un caractère dans une chaîne.

Prototype :

```c
char* chercher(char *s, char c);
```

Travail demandé :

✔ parcourir la chaîne avec un pointeur
✔ si `*p == c` → retourner `p`
✔ si fin → retourner `NULL`
✔ tester dans `main`

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

char* chercher(char *s, char c) {
    while (*s != '\0') {
        if (*s == c)
            return s;
        s++;
    }
    return NULL;
}

int main() {
    char texte[] = "chaine";
    char *pos = chercher(texte, 'a');

    if (pos)
        printf("Trouvé à l'index %ld\n", pos - texte);
    else
        printf("Non trouvé\n");

    return 0;
}
```

🧠 **Explication**

* Retourner `p` revient à rendre l’**adresse** du caractère trouvé.
* On peut obtenir l’index via `pos - texte`.

</details>

---

## 🧩 Exercice 10 — Détecter un dépassement de tableau via pointeurs

On veut écrire une fonction qui :

✔ parcourt une chaîne
✔ arrête la lecture dès que le pointeur dépasse une limite donnée
✔ retourne 1 si la chaîne est valide, 0 si elle dépasse
(? simulation de détection de dépassement)

Prototype :

```c
int chaineValide(char *s, char *limite);
```

Dans `main` :
→ tester un tableau trop petit et un tableau assez grand.

<details>
<summary>✅ Corrigé + explication</summary>

```c
#include <stdio.h>

int chaineValide(char *s, char *limite) {
    while (s < limite) {
        if (*s == '\0')
            return 1; // chaîne OK
        s++;
    }
    return 0; // dépassement potentiel
}

int main() {
    char petit[5] = "Salut"; 
    char grand[10] = "Salut";

    // limite = adresse après la dernière case
    printf("petit : %d\n", chaineValide(petit, petit + 5));
    printf("grand : %d\n", chaineValide(grand, grand + 10));

    return 0;
}
```

🧠 **Explication**

* `s < limite` signifie : “on reste dans le tableau”.
* Si on atteint la limite sans trouver `'\0'`, la chaîne n'est pas correctement terminée.
* Cet exercice illustre pourquoi oublier `'\0'` est dangereux.

</details>

---


## 🧩 Exercice 11 — Longueur d’une chaîne dans une liste chaînée

On veut représenter une chaîne de caractères non pas dans un tableau, mais dans une liste chaînée, un caractère par maillon.

Définir la structure :

typedef struct Node {
    char c;
    struct Node *suiv;
} Node;


Travail demandé :

✔ écrire une fonction

int longueurListe(Node *tete);


qui compte le nombre de maillons de la liste
✔ dans main, construire la liste correspondant à "Salut"
✔ afficher la longueur

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    char c;
    struct Node *suiv;
} Node;

int longueurListe(Node *tete) {
    int len = 0;
    while (tete != NULL) {
        len++;
        tete = tete->suiv;
    }
    return len;
}

int main() {
    // Construction de la liste "Salut"
    char *texte = "Salut";
    Node *tete = NULL, *p = NULL;

    for (int i = 0; texte[i] != '\0'; i++) {
        Node *n = malloc(sizeof(Node));
        n->c = texte[i];
        n->suiv = NULL;

        if (tete == NULL)
            tete = n;
        else
            p->suiv = n;

        p = n;
    }

    printf("Longueur : %d\n", longueurListe(tete));
    return 0;
}


🧠 Explication

Chaque maillon contient un caractère.

On avance dans la liste avec un pointeur.

La longueur est le nombre de maillons.

</details>

---
## 🧩 Exercice 12 — Construire une liste depuis une chaîne

Écrire une fonction qui transforme une chaîne classique (char[]) en liste chaînée de caractères.

Prototype :

Node* construireListe(const char *s);


Travail demandé :

✔ allouer un maillon par caractère
✔ retourner le pointeur de début
✔ tester avec "Bonjour"

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    char c;
    struct Node *suiv;
} Node;

Node* construireListe(const char *s) {
    Node *tete = NULL, *p = NULL;

    while (*s != '\0') {
        Node *n = malloc(sizeof(Node));
        n->c = *s;
        n->suiv = NULL;

        if (tete == NULL)
            tete = n;
        else
            p->suiv = n;

        p = n;
        s++;
    }

    return tete;
}

int main() {
    Node *liste = construireListe("Bonjour");

    while (liste) {
        printf("%c ", liste->c);
        liste = liste->suiv;
    }
}


🧠 Explication

On avance dans la chaîne via un pointeur s.

On crée un maillon pour chaque caractère.

La liste représente la chaîne, mais sans tableau.

</details>

---
## 🧩 Exercice 13 — Convertir une liste chaînée en chaîne (tableau)

Cette fois, on veut transformer une liste chaînée de caractères en une chaîne C classique dans un tableau.

Prototype :

void listeVersChaine(Node *tete, char *dest, int max);


Travail demandé :

✔ copier chaque caractère dans dest
✔ ne pas dépasser max - 1
✔ ajouter '\0' à la fin
✔ tester dans main

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>

typedef struct Node {
    char c;
    struct Node *suiv;
} Node;

void listeVersChaine(Node *tete, char *dest, int max) {
    int i = 0;
    while (tete != NULL && i < max - 1) {
        dest[i++] = tete->c;
        tete = tete->suiv;
    }
    dest[i] = '\0';
}

int main() {
    // Liste "ABC"
    Node c = {'C', NULL};
    Node b = {'B', &c};
    Node a = {'A', &b};

    char texte[10];
    listeVersChaine(&a, texte, 10);

    printf("Résultat : %s\n", texte);
    return 0;
}


🧠 Explication

dest est traité comme un tableau où l’on écrit.

On protège contre les dépassements via i < max - 1.

On ajoute '\0' pour former une chaîne valide.

</details>

---
## 🧩 Exercice 14 — Chercher un caractère dans une liste

Écrire une fonction qui recherche un caractère dans une liste chaînée de caractères.

Prototype :

Node* chercher(Node *tete, char c);


Travail demandé :

✔ avancer maillon par maillon
✔ comparer n->c au caractère recherché
✔ retourner l'adresse du maillon trouvé
✔ sinon retourner NULL

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>

typedef struct Node {
    char c;
    struct Node *suiv;
} Node;

Node* chercher(Node *tete, char c) {
    while (tete != NULL) {
        if (tete->c == c)
            return tete;
        tete = tete->suiv;
    }
    return NULL;
}

int main() {
    Node z = {'z', NULL};
    Node e = {'e', &z};
    Node t = {'t', &e};

    Node *res = chercher(&t, 'e');
    if (res)
        printf("Trouvé : %c\n", res->c);
    else
        printf("Non trouvé\n");

    return 0;
}


🧠 Explication

On parcourt la liste de manière séquentielle avec un pointeur.

Si un maillon contient le caractère, on retourne son adresse.

</details>


---
## 🧩 Exercice 15 — Concaténer deux listes chaînées

On veut concaténer deux chaînes représentées sous forme de listes chaînées.

Prototype :

Node* concat(Node *L1, Node *L2);


Travail demandé :

✔ retourner la tête de la liste concaténée
✔ si L1 est vide → retourner L2
✔ sinon → aller au dernier maillon de L1 et le relier à L2
✔ tester dans main

<details> <summary>✅ Corrigé + explication</summary>
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    char c;
    struct Node *suiv;
} Node;

Node* concat(Node *L1, Node *L2) {
    if (L1 == NULL) return L2;

    Node *p = L1;
    while (p->suiv != NULL)
        p = p->suiv;

    p->suiv = L2;
    return L1;
}

int main() {
    Node b = {'b', NULL};
    Node a = {'a', &b};

    Node d = {'d', NULL};
    Node c = {'c', &d};

    Node *res = concat(&a, &c);

    while (res) {
        printf("%c ", res->c);
        res = res->suiv;
    }
    return 0;
}


🧠 Explication

On utilise des pointeurs pour naviguer dans la première liste.

On relie le dernier maillon de L1 au début de L2.

La concaténation est purement structurelle, sans copie.

</details>
