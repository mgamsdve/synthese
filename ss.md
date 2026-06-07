# Synthèse – Examen UAA11 Juin

## Analyse + C# + Tableaux + Matrices

---

# 1. Structure générale d’un programme C#

## Structure de base

```csharp
namespace NomDuProjet;

internal class Program
{
    static void Main(string[] args)
    {
        // Déclarations

        // Programme principal
    }
}
```

---

# 2. Classe de méthodes

Pour l’examen, les procédures et fonctions doivent souvent être placées dans une classe séparée.

```csharp
namespace NomDuProjet;

public static class MethodesProgram
{
    // procédures et fonctions ici
}
```

## Appel depuis le Main

```csharp
MethodesProgram.NomMethode();
```

Exemple :

```csharp
MethodesProgram.LireEntier("Entrez un nombre : ", out nombre);
```

---

# 3. Procédure ou fonction ?

## Procédure

Une procédure ne retourne pas directement une valeur.

```csharp
public static void NomProcedure()
{
    // traitement
}
```

Elle utilise souvent `out` pour renvoyer un résultat.

```csharp
public static void LireEntier(string question, out int resultat)
```

---

## Fonction

Une fonction retourne une valeur avec `return`.

```csharp
public static int NomFonction()
{
    return resultat;
}
```

Exemple :

```csharp
public static int LireEntier(string question)
{
    int resultat;

    do
    {
        Console.WriteLine(question);
    }
    while (!int.TryParse(Console.ReadLine(), out resultat));

    return resultat;
}
```

---

# 4. Paramètres d’entrée et de sortie

## Paramètre d’entrée

Il donne une information à la méthode.

```csharp
int tailleTableau
```

## Paramètre de sortie

Il renvoie une information depuis la méthode.

```csharp
out int resultat
```

## Exemple

```csharp
public static void CreationTableauAleatoire(
    int tailleTableau,
    int borneInf,
    int borneSup,
    out int[] tableau)
```

| Paramètre       | Rôle   |
| --------------- | ------ |
| `tailleTableau` | entrée |
| `borneInf`      | entrée |
| `borneSup`      | entrée |
| `tableau`       | sortie |

---

# 5. Variables locales

Une variable locale est utilisée uniquement dans une méthode.

Exemple :

```csharp
int iPlace;
```

Elle sert souvent à parcourir un tableau.

## Différence importante

| Élément         | Sert à quoi ?                          |
| --------------- | -------------------------------------- |
| Paramètre       | communiquer avec la méthode            |
| Variable locale | travailler à l’intérieur de la méthode |

---

# 6. Types de variables à connaître

```csharp
int nombre;
double prix;
bool trouve;
string texte;
```

| Type     | Exemple          | Utilisation               |
| -------- | ---------------- | ------------------------- |
| `int`    | `5`              | entier                    |
| `double` | `4.18`           | nombre réel               |
| `bool`   | `true` / `false` | vrai ou faux              |
| `string` | `"bonjour"`      | texte                     |
| `int[]`  | tableau          | tableau à une dimension   |
| `int[,]` | matrice          | tableau à deux dimensions |

---

# 7. Lecture sécurisée avec TryParse

## Lire un entier

```csharp
public static void LireEntier(string question, out int resultat)
{
    do
    {
        Console.WriteLine(question);
    }
    while (!int.TryParse(Console.ReadLine(), out resultat));
}
```

## Utilisation

```csharp
int nombre;

LireEntier("Entrez un nombre : ", out nombre);
```

---

## Lire un réel

```csharp
public static void LireDouble(string question, out double resultat)
{
    do
    {
        Console.WriteLine(question);
    }
    while (!double.TryParse(Console.ReadLine(), out resultat));
}
```

---

## À retenir

| Élément              | Signification                  |
| -------------------- | ------------------------------ |
| `Console.ReadLine()` | lit ce que l’utilisateur écrit |
| `int.TryParse()`     | essaie de convertir en entier  |
| `double.TryParse()`  | essaie de convertir en réel    |
| `!`                  | inverse le résultat            |
| `out`                | permet de renvoyer une valeur  |

---

# 8. Vérification des hypothèses

Après une lecture, on vérifie souvent si la valeur est correcte.

## Exemple : taille positive

```csharp
do
{
    LireEntier("Taille du tableau (> 0) : ", out tailleTableau);
}
while (tailleTableau <= 0);
```

## Exemple : choix entre 1 et 2

```csharp
do
{
    LireEntier("Choix : 1 ou 2", out choix);
}
while (choix != 1 && choix != 2);
```

## Exemple : jour entre 1 et 7

```csharp
do
{
    LireEntier("Jour : ", out jour);
}
while (jour < 1 || jour > 7);
```

---

# 9. Boucle de reprise du programme

Permet de relancer le programme.

```csharp
string recommencer;

do
{
    // programme

    Console.WriteLine("Entrez espace pour recommencer.");
    recommencer = Console.ReadLine();
}
while (recommencer == " ");
```

---

# 10. Conditions

## Structure if

```csharp
if (condition)
{
    // traitement si vrai
}
```

## Structure if / else

```csharp
if (condition)
{
    // traitement si vrai
}
else
{
    // traitement si faux
}
```

## Plusieurs conditions

```csharp
if (choix == 1)
{
    // addition
}
else if (choix == 2)
{
    // multiplication
}
else
{
    // erreur
}
```

---

# 11. Opérateurs importants

| Opérateur | Signification        |   |    |
| --------- | -------------------- | - | -- |
| `=`       | affectation          |   |    |
| `==`      | comparaison          |   |    |
| `!=`      | différent            |   |    |
| `<`       | plus petit           |   |    |
| `>`       | plus grand           |   |    |
| `<=`      | plus petit ou égal   |   |    |
| `>=`      | plus grand ou égal   |   |    |
| `&&`      | ET                   |   |    |
| `         |                      | ` | OU |
| `!`       | NON                  |   |    |
| `%`       | reste de la division |   |    |

---

# 12. Tableaux à une dimension

## Déclaration

```csharp
int[] tableau;
```

## Création

```csharp
tableau = new int[taille];
```

## Déclaration + création

```csharp
int[] tableau = new int[5];
```

Crée un tableau de 5 entiers.

---

# 13. Accéder à une case d’un tableau

```csharp
tableau[i]
```

Exemple :

```csharp
Console.WriteLine(tableau[2]);
```

Attention : la première case est toujours l’indice `0`.

Pour un tableau de 5 cases :

| Case     | Indice |
| -------- | ------ |
| 1re case | `0`    |
| 2e case  | `1`    |
| 3e case  | `2`    |
| 4e case  | `3`    |
| 5e case  | `4`    |

---

# 14. Taille d’un tableau

```csharp
tableau.Length
```

---

# 15. Parcourir un tableau

```csharp
for (int i = 0; i < tableau.Length; i++)
{
    // traitement
}
```

À retenir :

| Élément              | Signification                  |
| -------------------- | ------------------------------ |
| `i = 0`              | première case                  |
| `i < tableau.Length` | tant qu’on est dans le tableau |
| `i++`                | case suivante                  |

---

# 16. Remplir un tableau manuellement

```csharp
for (int i = 0; i < tableau.Length; i++)
{
    LireEntier("Valeur : ", out tableau[i]);
}
```

---

# 17. Générer des nombres aléatoires

## Création

```csharp
Random alea = new Random();
```

## Génération

```csharp
alea.Next(min, max);
```

Attention :

```text
max n’est pas inclus
```

Donc si on veut inclure la borne supérieure :

```csharp
alea.Next(min, max + 1);
```

---

# 18. Créer un tableau aléatoire

```csharp
public static void CreationTableauAleatoire(
    int tailleTableau,
    int borneInf,
    int borneSup,
    out int[] tableau)
{
    Random alea = new Random();

    tableau = new int[tailleTableau];

    for (int iPlace = 0; iPlace < tableau.Length; iPlace++)
    {
        tableau[iPlace] = alea.Next(borneInf, borneSup + 1);
    }
}
```

---

# 19. Concaténer un tableau dans une string

## But

Transformer un tableau en texte pour pouvoir l’afficher.

```csharp
public static void ConcateneTableau(int[] tableau, out string contenuTableau)
{
    contenuTableau = "";

    for (int iPlace = 0; iPlace < tableau.Length; iPlace++)
    {
        contenuTableau += tableau[iPlace] + " ; ";
    }
}
```

## Exemple de résultat

```text
4 ; 7 ; 2 ; 9 ;
```

---

# 20. Tester pair ou impair

## Nombre pair

```csharp
nombre % 2 == 0
```

## Nombre impair

```csharp
nombre % 2 != 0
```

---

# 21. Séparer pairs et impairs

## Principe

1. Créer un tableau pour les pairs.
2. Créer un tableau pour les impairs.
3. Parcourir le tableau de départ.
4. Tester si chaque nombre est pair ou impair.
5. Ranger dans le bon tableau.

```csharp
int iPair = 0;
int iImpair = 0;

for (int i = 0; i < tableau.Length; i++)
{
    if (tableau[i] % 2 == 0)
    {
        tableauPairs[iPair] = tableau[i];
        iPair++;
    }
    else
    {
        tableauImpairs[iImpair] = tableau[i];
        iImpair++;
    }
}
```

---

# 22. Redimensionner un tableau

```csharp
Array.Resize(ref tableau, nouvelleTaille);
```

Exemple :

```csharp
Array.Resize(ref placesOccurrences, nbOccurrences);
```

Très utile quand on a créé un tableau trop grand au départ.

---

# 23. Rechercher une valeur dans un tableau

## Principe

On veut savoir :

1. si une valeur est présente ;
2. combien de fois elle apparaît ;
3. à quelles places elle se trouve.

---

## Méthode type

```csharp
public static void TrouveEtCompteOccurrences(
    int[] tValeurs,
    int valeur,
    out int[] placesOccurrences,
    out bool trouve,
    out int nbOccurrences)
{
    nbOccurrences = 0;
    int iOccurrence = 0;

    placesOccurrences = new int[tValeurs.Length];
    trouve = false;

    for (int iPlace = 0; iPlace < tValeurs.Length; iPlace++)
    {
        if (tValeurs[iPlace] == valeur)
        {
            trouve = true;
            nbOccurrences++;
            placesOccurrences[iOccurrence] = iPlace;
            iOccurrence++;
        }
    }

    Array.Resize(ref placesOccurrences, nbOccurrences);
}
```

---

## À retenir

| Variable        | Rôle                                    |
| --------------- | --------------------------------------- |
| `iPlace`        | parcourt le tableau de départ           |
| `iOccurrence`   | position dans le tableau des places     |
| `nbOccurrences` | nombre de fois où la valeur est trouvée |
| `trouve`        | vrai si la valeur existe                |

---

# 24. Affichage correct d’une recherche

```csharp
if (trouve)
{
    Console.WriteLine("La valeur est présente.");
    Console.WriteLine("Nombre d'occurrences : " + nbOccurrences);
    Console.WriteLine("Places : " + contenuPlaces);
}
else
{
    Console.WriteLine("La valeur n'est pas présente.");
}
```

---

# 25. Chaînes de caractères

## Déclaration

```csharp
string texte;
```

## Longueur d’une chaîne

```csharp
texte.Length
```

## Accéder à un caractère

```csharp
texte[i]
```

Exemple :

```csharp
Console.WriteLine(texte[0]);
```

---

# 26. Parcourir une chaîne de caractères

```csharp
for (int i = 0; i < texte.Length; i++)
{
    Console.WriteLine(texte[i]);
}
```

---

# 27. Caractère ou chaîne ?

## Caractère

```csharp
'a'
```

## Chaîne

```csharp
"bonjour"
```

Attention :

```csharp
texte[i] == 'a'
```

et pas :

```csharp
texte[i] == "a"
```

---

# 28. Compter un caractère dans une chaîne

```csharp
int compteur = 0;

for (int i = 0; i < texte.Length; i++)
{
    if (texte[i] == 'a')
    {
        compteur++;
    }
}
```

---

# 29. Matrices

Une matrice est un tableau à deux dimensions.

On utilise :

```csharp
int[,] matrice;
```

La première dimension correspond aux lignes.

La deuxième dimension correspond aux colonnes.

---

# 30. Déclaration d’une matrice

```csharp
int[,] matrice;
```

---

# 31. Création d’une matrice

```csharp
matrice = new int[nombreLigne, nombreColonne];
```

Exemple :

```csharp
int[,] matrice = new int[3, 4];
```

Crée une matrice de 3 lignes et 4 colonnes.

---

# 32. Accéder à une case d’une matrice

```csharp
matrice[iLigne, iColonne]
```

Exemple :

```csharp
Console.WriteLine(matrice[1, 2]);
```

---

# 33. Nombre de lignes et de colonnes

## Nombre de lignes

```csharp
matrice.GetLength(0)
```

## Nombre de colonnes

```csharp
matrice.GetLength(1)
```

À retenir :

| Code           | Signification      |
| -------------- | ------------------ |
| `GetLength(0)` | nombre de lignes   |
| `GetLength(1)` | nombre de colonnes |

---

# 34. Parcourir une matrice

Il faut deux boucles `for`.

```csharp
for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
{
    for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
    {
        // traitement de matrice[iLigne, iColonne]
    }
}
```

---

# 35. Remplir une matrice aléatoirement

```csharp
public static int[,] RemplirMatrice(
    int nombreLigne,
    int nombreColonne,
    int borneMin,
    int borneMax)
{
    Random alea = new Random();

    int[,] matrice = new int[nombreLigne, nombreColonne];

    for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
    {
        for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
        {
            matrice[iLigne, iColonne] = alea.Next(borneMin, borneMax + 1);
        }
    }

    return matrice;
}
```

---

# 36. Lire / concaténer une matrice

## But

Transformer une matrice en texte pour l’afficher.

```csharp
public static string LireMatrice(int[,] matrice)
{
    string contenu = "";

    for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
    {
        for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
        {
            contenu += matrice[iLigne, iColonne] + " ; ";
        }

        contenu += "\n";
    }

    return contenu;
}
```

---

## Exemple de résultat

```text
1 ; 4 ; 8 ;
2 ; 7 ; 3 ;
```

Le `\n` permet de passer à la ligne.

---

# 37. Addition de matrices

## Condition pour additionner deux matrices

Deux matrices peuvent être additionnées si elles ont :

```text
le même nombre de lignes
ET
le même nombre de colonnes
```

Exemple :

```text
A : 2 x 3
B : 2 x 3
Addition possible
```

Exemple impossible :

```text
A : 2 x 3
B : 3 x 2
Addition impossible
```

---

## Formule

```text
résultat[i, j] = matriceUn[i, j] + matriceDeux[i, j]
```

---

## Code type

```csharp
public static void AdditionMatrice(
    int[,] matriceUn,
    int[,] matriceDeux,
    out int[,] matriceAddition,
    out bool additionPossible)
{
    int matriceUnLigne = matriceUn.GetLength(0);
    int matriceUnColonne = matriceUn.GetLength(1);

    int matriceDeuxLigne = matriceDeux.GetLength(0);
    int matriceDeuxColonne = matriceDeux.GetLength(1);

    if (matriceUnLigne == matriceDeuxLigne &&
        matriceUnColonne == matriceDeuxColonne)
    {
        additionPossible = true;

        matriceAddition = new int[matriceUnLigne, matriceUnColonne];

        for (int iLigne = 0; iLigne < matriceUnLigne; iLigne++)
        {
            for (int iColonne = 0; iColonne < matriceUnColonne; iColonne++)
            {
                matriceAddition[iLigne, iColonne] =
                    matriceUn[iLigne, iColonne] + matriceDeux[iLigne, iColonne];
            }
        }
    }
    else
    {
        additionPossible = false;
        matriceAddition = null;
    }
}
```

---

# 38. Multiplication de matrices

## Condition pour multiplier deux matrices

On peut multiplier deux matrices si :

```text
nombre de colonnes de la première matrice
=
nombre de lignes de la deuxième matrice
```

Exemple possible :

```text
A : 2 x 3
B : 3 x 4
Résultat : 2 x 4
```

Exemple impossible :

```text
A : 2 x 3
B : 2 x 4
Multiplication impossible
```

---

## Taille du résultat

```text
A : lignesA x colonnesA
B : lignesB x colonnesB

Résultat : lignesA x colonnesB
```

---

## Formule

```text
résultat[i, j] =
A[i, 0] * B[0, j]
+ A[i, 1] * B[1, j]
+ A[i, 2] * B[2, j]
+ ...
```

---

## Code type

```csharp
public static void MultiplicationMatrice(
    int[,] matriceUn,
    int[,] matriceDeux,
    out int[,] matriceMulti,
    out bool multiplicationPossible)
{
    int matriceUnLigne = matriceUn.GetLength(0);
    int matriceUnColonne = matriceUn.GetLength(1);

    int matriceDeuxLigne = matriceDeux.GetLength(0);
    int matriceDeuxColonne = matriceDeux.GetLength(1);

    if (matriceUnColonne == matriceDeuxLigne)
    {
        multiplicationPossible = true;

        matriceMulti = new int[matriceUnLigne, matriceDeuxColonne];

        for (int iLigne = 0; iLigne < matriceUnLigne; iLigne++)
        {
            for (int iColonne = 0; iColonne < matriceDeuxColonne; iColonne++)
            {
                matriceMulti[iLigne, iColonne] = 0;

                for (int iColonne2 = 0; iColonne2 < matriceUnColonne; iColonne2++)
                {
                    matriceMulti[iLigne, iColonne] +=
                        matriceUn[iLigne, iColonne2] *
                        matriceDeux[iColonne2, iColonne];
                }
            }
        }
    }
    else
    {
        multiplicationPossible = false;
        matriceMulti = null;
    }
}
```

---

# 39. Comprendre les 3 boucles de la multiplication

```csharp
for (int iLigne = 0; iLigne < matriceUnLigne; iLigne++)
```

Parcourt les lignes de la première matrice.

```csharp
for (int iColonne = 0; iColonne < matriceDeuxColonne; iColonne++)
```

Parcourt les colonnes de la deuxième matrice.

```csharp
for (int iColonne2 = 0; iColonne2 < matriceUnColonne; iColonne2++)
```

Calcule la somme des multiplications.

---

# 40. Exemple de multiplication

```text
A =
1 2
3 4

B =
5 6
7 8
```

Calcul de la case `[0,0]` :

```text
A[0,0] * B[0,0] + A[0,1] * B[1,0]
= 1 * 5 + 2 * 7
= 5 + 14
= 19
```

Calcul de la case `[0,1]` :

```text
A[0,0] * B[0,1] + A[0,1] * B[1,1]
= 1 * 6 + 2 * 8
= 6 + 16
= 22
```

Résultat :

```text
19 22
43 50
```

---

# 41. Programme principal avec matrices

Structure typique :

```csharp
int[,] matriceUn;
int[,] matriceDeux;
int[,] matriceOperation;

bool operationPossible;

int nombreLigne;
int nombreColonne;
int borneMin;
int borneMax;

string contenu;
string contenu2;
string contenuOperation;

int choixOperation;
```

---

## Logique générale

```text
Lire dimensions matrice 1
Lire bornes matrice 1
Créer matrice 1

Lire dimensions matrice 2
Lire bornes matrice 2
Créer matrice 2

Afficher les matrices

Demander opération :
1 = addition
2 = multiplication

Faire l’opération

Afficher le résultat ou dire que c’est impossible
```

---

# 42. Analyse : structure à connaître

Pour un morceau de programme, il faut pouvoir écrire :

```text
MP = nom du morceau de programme
Paramètres
Hypothèses
But
Variables locales
GNS
```

---

# 43. Exemple d’analyse : tableau aléatoire

## MP

```text
CreationTableauAleatoire
```

## Paramètres

| Nom           | Type  | Description                 | I/O |
| ------------- | ----- | --------------------------- | --- |
| tailleTableau | int   | nombre de places du tableau | I   |
| borneInf      | int   | plus petite valeur possible | I   |
| borneSup      | int   | plus grande valeur possible | I   |
| tableau       | int[] | tableau créé et rempli      | O   |

## Hypothèses

```text
tailleTableau non vide et > 0
borneInf non vide
borneSup non vide
borneSup >= borneInf
```

## But

```text
Créer un tableau d’entiers contenant tailleTableau valeurs aléatoires comprises entre borneInf et borneSup.
```

## Variables locales

| Nom    | Type   | Description                       |
| ------ | ------ | --------------------------------- |
| alea   | Random | générateur aléatoire              |
| iPlace | int    | pointeur de place dans le tableau |

## GNS

```text
Créer le générateur aléatoire
Créer le tableau avec tailleTableau places

Pour chaque place du tableau
    Générer une valeur aléatoire entre borneInf et borneSup
    Placer cette valeur dans le tableau
Fin Pour
```

---

# 44. Exemple d’analyse : addition de matrices

## MP

```text
AdditionMatrice
```

## Paramètres

| Nom              | Type   | Description                     | I/O |
| ---------------- | ------ | ------------------------------- | --- |
| matriceUn        | int[,] | première matrice                | I   |
| matriceDeux      | int[,] | deuxième matrice                | I   |
| matriceAddition  | int[,] | résultat de l’addition          | O   |
| additionPossible | bool   | vrai si l’addition est possible | O   |

## Hypothèses

```text
matriceUn non vide
matriceDeux non vide
```

## But

```text
Additionner deux matrices si elles ont les mêmes dimensions.
```

## Variables locales

| Nom                | Type | Description                               |
| ------------------ | ---- | ----------------------------------------- |
| iLigne             | int  | pointeur de ligne                         |
| iColonne           | int  | pointeur de colonne                       |
| matriceUnLigne     | int  | nombre de lignes de la première matrice   |
| matriceUnColonne   | int  | nombre de colonnes de la première matrice |
| matriceDeuxLigne   | int  | nombre de lignes de la deuxième matrice   |
| matriceDeuxColonne | int  | nombre de colonnes de la deuxième matrice |

## GNS

```text
Récupérer les dimensions des deux matrices

Si les deux matrices ont le même nombre de lignes et de colonnes
    additionPossible reçoit true
    Créer matriceAddition avec les bonnes dimensions

    Pour chaque ligne
        Pour chaque colonne
            matriceAddition[iLigne, iColonne] reçoit
            matriceUn[iLigne, iColonne] + matriceDeux[iLigne, iColonne]
        Fin Pour
    Fin Pour
Sinon
    additionPossible reçoit false
    matriceAddition reçoit null
Fin Si
```

---

# 45. Exemple d’analyse : multiplication de matrices

## MP

```text
MultiplicationMatrice
```

## Paramètres

| Nom                    | Type   | Description                            | I/O |
| ---------------------- | ------ | -------------------------------------- | --- |
| matriceUn              | int[,] | première matrice                       | I   |
| matriceDeux            | int[,] | deuxième matrice                       | I   |
| matriceMulti           | int[,] | résultat de la multiplication          | O   |
| multiplicationPossible | bool   | vrai si la multiplication est possible | O   |

## Hypothèses

```text
matriceUn non vide
matriceDeux non vide
```

## But

```text
Multiplier deux matrices si le nombre de colonnes de la première est égal au nombre de lignes de la deuxième.
```

## Variables locales

| Nom                | Type | Description                               |
| ------------------ | ---- | ----------------------------------------- |
| iLigne             | int  | pointeur de ligne                         |
| iColonne           | int  | pointeur de colonne                       |
| iColonne2          | int  | pointeur pour calculer la somme           |
| matriceUnLigne     | int  | nombre de lignes de la première matrice   |
| matriceUnColonne   | int  | nombre de colonnes de la première matrice |
| matriceDeuxLigne   | int  | nombre de lignes de la deuxième matrice   |
| matriceDeuxColonne | int  | nombre de colonnes de la deuxième matrice |

## GNS

```text
Récupérer les dimensions des deux matrices

Si nombre de colonnes de matriceUn = nombre de lignes de matriceDeux
    multiplicationPossible reçoit true
    Créer matriceMulti avec :
        nombre de lignes de matriceUn
        nombre de colonnes de matriceDeux

    Pour chaque ligne de matriceUn
        Pour chaque colonne de matriceDeux
            Initialiser matriceMulti[iLigne, iColonne] à 0

            Pour chaque colonne de matriceUn
                Ajouter à matriceMulti[iLigne, iColonne] :
                matriceUn[iLigne, iColonne2] * matriceDeux[iColonne2, iColonne]
            Fin Pour
        Fin Pour
    Fin Pour
Sinon
    multiplicationPossible reçoit false
    matriceMulti reçoit null
Fin Si
```

---

# 46. Clean Code : bons noms de variables

## Mauvais noms

```csharp
x
y
a
b
tab
res
```

## Bons noms

```csharp
tailleTableau
borneInf
borneSup
nombreLigne
nombreColonne
contenuTableau
operationPossible
additionPossible
multiplicationPossible
```

---

# 47. Commentaires utiles

On peut commenter les variables.

```csharp
int nombreLigne; // nombre de lignes de la matrice
int nombreColonne; // nombre de colonnes de la matrice
bool operationPossible; // indique si l'opération est possible
```

---

# 48. Ordre logique d’un programme

```text
1. Déclarer les variables
2. Lire les données
3. Vérifier les hypothèses
4. Appeler les traitements
5. Afficher les résultats
6. Demander si on recommence
```

---

# 49. Erreurs classiques à éviter

## Erreur 1 : dépasser la taille d’un tableau

Faux :

```csharp
for (int i = 0; i <= tableau.Length; i++)
```

Correct :

```csharp
for (int i = 0; i < tableau.Length; i++)
```

---

## Erreur 2 : confondre `=` et `==`

Faux :

```csharp
if (choix = 1)
```

Correct :

```csharp
if (choix == 1)
```

---

## Erreur 3 : oublier `out` à l’appel

Faux :

```csharp
LireEntier("Nombre : ", nombre);
```

Correct :

```csharp
LireEntier("Nombre : ", out nombre);
```

---

## Erreur 4 : oublier `return` dans une fonction

Faux :

```csharp
public static int LireEntier(string question)
{
    int resultat;
}
```

Correct :

```csharp
public static int LireEntier(string question)
{
    int resultat;
    return resultat;
}
```

---

## Erreur 5 : mauvaise borne avec Random

Faux :

```csharp
alea.Next(0, 25);
```

Si on veut aller de 0 à 25, il faut écrire :

```csharp
alea.Next(0, 26);
```

ou :

```csharp
alea.Next(min, max + 1);
```

---

## Erreur 6 : oublier d’initialiser une string

Faux :

```csharp
string contenu;

contenu += tableau[i];
```

Correct :

```csharp
string contenu = "";

contenu += tableau[i];
```

---

## Erreur 7 : confondre lignes et colonnes

```csharp
matrice.GetLength(0)
```

= lignes.

```csharp
matrice.GetLength(1)
```

= colonnes.

---

## Erreur 8 : additionner des matrices de tailles différentes

Addition possible seulement si :

```text
mêmes lignes
ET
mêmes colonnes
```

---

## Erreur 9 : multiplier avec la mauvaise condition

Multiplication possible seulement si :

```text
colonnes de la première = lignes de la deuxième
```

---

## Erreur 10 : oublier d’initialiser une case dans la multiplication

Avant d’additionner dans la case :

```csharp
matriceMulti[iLigne, iColonne] = 0;
```

Puis seulement après :

```csharp
matriceMulti[iLigne, iColonne] += ...
```

---

# 50. Les 25 choses essentielles à retenir

1. `int[] tableau`
2. `tableau = new int[taille]`
3. `tableau.Length`
4. `tableau[i]`
5. `for (int i = 0; i < tableau.Length; i++)`
6. `Random alea = new Random()`
7. `alea.Next(min, max + 1)`
8. `out`
9. `return`
10. `void`
11. `int.TryParse`
12. `double.TryParse`
13. `do while`
14. `if / else`
15. `==` pour comparer
16. `=` pour affecter
17. `int[,] matrice`
18. `new int[lignes, colonnes]`
19. `matrice[iLigne, iColonne]`
20. `matrice.GetLength(0)` = lignes
21. `matrice.GetLength(1)` = colonnes
22. Addition matrices : mêmes dimensions
23. Multiplication matrices : colonnes A = lignes B
24. `Array.Resize(ref tableau, taille)`
25. Une méthode dans une classe static s’appelle avec `NomClasse.NomMethode()`

---

# 51. Mini fiche spéciale matrices

## Déclaration

```csharp
int[,] matrice;
```

## Création

```csharp
matrice = new int[nombreLigne, nombreColonne];
```

## Accès

```csharp
matrice[iLigne, iColonne]
```

## Nombre de lignes

```csharp
matrice.GetLength(0)
```

## Nombre de colonnes

```csharp
matrice.GetLength(1)
```

## Parcours

```csharp
for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
{
    for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
    {
        // traitement
    }
}
```

## Addition possible si

```text
lignes A = lignes B
colonnes A = colonnes B
```

## Multiplication possible si

```text
colonnes A = lignes B
```

## Taille du résultat d’une multiplication

```text
A : lignesA x colonnesA
B : lignesB x colonnesB

Résultat : lignesA x colonnesB
```

---

# 52. Mini fiche spéciale analyse

## Toujours écrire

```text
MP
Paramètres
Hypothèses
But
Variables locales
GNS
```

## Paramètres

```text
Ce qui entre et sort du morceau de programme.
```

## Hypothèses

```text
Conditions sur les paramètres d’entrée.
```

## But

```text
Ce que le morceau de programme doit faire.
```

## Variables locales

```text
Variables temporaires utilisées seulement dans le traitement.
```

## GNS

```text
Suite logique des étapes de l’algorithme.
```

---

# 53. Méthode pour réussir l’examen

Avant de coder :

```text
1. Lire tout l’énoncé
2. Repérer les données d’entrée
3. Repérer les résultats attendus
4. Écrire les hypothèses
5. Choisir procédure ou fonction
6. Écrire la signature
7. Écrire les variables locales
8. Écrire le GNS
9. Coder
10. Tester
```

---

# 54. À mettre sur la feuille A4 manuscrite

```csharp
int[] tableau = new int[taille];
int[,] matrice = new int[lignes, colonnes];
```

```csharp
for (int i = 0; i < tableau.Length; i++)
{
}
```

```csharp
for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
{
    for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
    {
    }
}
```

```csharp
while (!int.TryParse(Console.ReadLine(), out resultat))
```

```csharp
Random alea = new Random();
alea.Next(min, max + 1);
```

```csharp
Array.Resize(ref tableau, nouvelleTaille);
```

```csharp
public static void NomProcedure(type entree, out type sortie)
```

```csharp
public static type NomFonction(type entree)
{
    return resultat;
}
```

```text
matrice.GetLength(0) = lignes
matrice.GetLength(1) = colonnes
```

```text
Addition : mêmes dimensions
Multiplication : colonnes A = lignes B
```
