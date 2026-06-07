# Synthèse – Codes à connaître

## Aléatoire, chaînes, tableaux, matrices

---

# 1. Génération de valeurs aléatoires

```csharp
Random alea = new Random();

int nombre = alea.Next(borneMin, borneMax + 1);
```

---

# 2. Parcours d’une chaîne de caractères

```csharp
public static void ParcourirChaine(string texte)
{
    for (int i = 0; i < texte.Length; i++)
    {
        Console.WriteLine(texte[i]);
    }
}
```

---

# 3. Retirer les espaces d’une chaîne et mettre en majuscules

```csharp
public static string RetirerEspacesMajuscules(string texte)
{
    string texteNettoye = "";

    for (int i = 0; i < texte.Length; i++)
    {
        if (texte[i] != ' ')
        {
            texteNettoye += char.ToUpper(texte[i]);
        }
    }

    return texteNettoye;
}
```

---

# 4. Lire un entier sécurisé

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

---

# 5. Lire un réel sécurisé

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

# 6. Tableau – déclaration / création

```csharp
int[] tableau;

tableau = new int[tailleTableau];
```

Ou directement :

```csharp
int[] tableau = new int[tailleTableau];
```

---

# 7. Remplir un tableau manuellement

```csharp
public static void RemplirTableauManuel(int tailleTableau, out int[] tableau)
{
    tableau = new int[tailleTableau];

    for (int iPlace = 0; iPlace < tableau.Length; iPlace++)
    {
        LireEntier("Entrez la valeur de la place " + iPlace + " : ", out tableau[iPlace]);
    }
}
```

---

# 8. Remplir un tableau aléatoirement

```csharp
public static void RemplirTableauAleatoire(
    int tailleTableau,
    int borneMin,
    int borneMax,
    out int[] tableau)
{
    Random alea = new Random();

    tableau = new int[tailleTableau];

    for (int iPlace = 0; iPlace < tableau.Length; iPlace++)
    {
        tableau[iPlace] = alea.Next(borneMin, borneMax + 1);
    }
}
```

---

# 9. Concaténer un tableau dans une string

```csharp
public static void ConcatenerTableau(int[] tableau, out string contenuTableau)
{
    contenuTableau = "";

    for (int iPlace = 0; iPlace < tableau.Length; iPlace++)
    {
        contenuTableau += tableau[iPlace] + " ; ";
    }
}
```

---

# 10. Rechercher une valeur dans un tableau

```csharp
public static void TrouveEtCompteOccurrences(
    int[] tableau,
    int valeur,
    out bool trouve,
    out int nbOccurrences,
    out int[] placesOccurrences)
{
    nbOccurrences = 0;
    trouve = false;

    placesOccurrences = new int[tableau.Length];

    int iOccurrence = 0;

    for (int iPlace = 0; iPlace < tableau.Length; iPlace++)
    {
        if (tableau[iPlace] == valeur)
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

# 11. Séparer les pairs et les impairs

```csharp
public static void ClasserPairsImpairs(
    int[] tableau,
    out int[] tableauPairs,
    out int[] tableauImpairs)
{
    tableauPairs = new int[tableau.Length];
    tableauImpairs = new int[tableau.Length];

    int iPair = 0;
    int iImpair = 0;

    for (int iPlace = 0; iPlace < tableau.Length; iPlace++)
    {
        if (tableau[iPlace] % 2 == 0)
        {
            tableauPairs[iPair] = tableau[iPlace];
            iPair++;
        }
        else
        {
            tableauImpairs[iImpair] = tableau[iPlace];
            iImpair++;
        }
    }

    Array.Resize(ref tableauPairs, iPair);
    Array.Resize(ref tableauImpairs, iImpair);
}
```

---

# 12. Matrice – déclaration / création

```csharp
int[,] matrice;

matrice = new int[nombreLigne, nombreColonne];
```

Ou directement :

```csharp
int[,] matrice = new int[nombreLigne, nombreColonne];
```

---

# 13. Remplir une matrice manuellement

```csharp
public static void RemplirMatriceManuel(
    int nombreLigne,
    int nombreColonne,
    out int[,] matrice)
{
    matrice = new int[nombreLigne, nombreColonne];

    for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
    {
        for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
        {
            LireEntier(
                "Entrez la valeur [" + iLigne + "," + iColonne + "] : ",
                out matrice[iLigne, iColonne]);
        }
    }
}
```

---

# 14. Remplir une matrice aléatoirement

```csharp
public static int[,] RemplirMatriceAleatoire(
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

# 15. Concaténer une matrice dans une string

```csharp
public static string ConcatenerMatrice(int[,] matrice)
{
    string contenuMatrice = "";

    for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
    {
        for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
        {
            contenuMatrice += matrice[iLigne, iColonne] + " ; ";
        }

        contenuMatrice += "\n";
    }

    return contenuMatrice;
}
```

---

# 16. Addition de deux matrices

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

# 17. Multiplication de deux matrices

```csharp
public static void MultiplicationMatrice(
    int[,] matriceUn,
    int[,] matriceDeux,
    out int[,] matriceMultiplication,
    out bool multiplicationPossible)
{
    int matriceUnLigne = matriceUn.GetLength(0);
    int matriceUnColonne = matriceUn.GetLength(1);

    int matriceDeuxLigne = matriceDeux.GetLength(0);
    int matriceDeuxColonne = matriceDeux.GetLength(1);

    if (matriceUnColonne == matriceDeuxLigne)
    {
        multiplicationPossible = true;

        matriceMultiplication = new int[matriceUnLigne, matriceDeuxColonne];

        for (int iLigne = 0; iLigne < matriceUnLigne; iLigne++)
        {
            for (int iColonne = 0; iColonne < matriceDeuxColonne; iColonne++)
            {
                matriceMultiplication[iLigne, iColonne] = 0;

                for (int iColonne2 = 0; iColonne2 < matriceUnColonne; iColonne2++)
                {
                    matriceMultiplication[iLigne, iColonne] +=
                        matriceUn[iLigne, iColonne2] *
                        matriceDeux[iColonne2, iColonne];
                }
            }
        }
    }
    else
    {
        multiplicationPossible = false;
        matriceMultiplication = null;
    }
}
```

---

# 18. Parcourir une matrice simple

```csharp
public static void ParcourirMatrice(int[,] matrice)
{
    for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
    {
        for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
        {
            Console.WriteLine(matrice[iLigne, iColonne]);
        }
    }
}
```

---

# 19. Structure complète d’une classe de méthodes

```csharp
namespace NomDuProjet;

public static class MethodesProgram
{
    public static void LireEntier(string question, out int resultat)
    {
        do
        {
            Console.WriteLine(question);
        }
        while (!int.TryParse(Console.ReadLine(), out resultat));
    }

    public static void RemplirTableauAleatoire(
        int tailleTableau,
        int borneMin,
        int borneMax,
        out int[] tableau)
    {
        Random alea = new Random();

        tableau = new int[tailleTableau];

        for (int iPlace = 0; iPlace < tableau.Length; iPlace++)
        {
            tableau[iPlace] = alea.Next(borneMin, borneMax + 1);
        }
    }

    public static string ConcatenerMatrice(int[,] matrice)
    {
        string contenuMatrice = "";

        for (int iLigne = 0; iLigne < matrice.GetLength(0); iLigne++)
        {
            for (int iColonne = 0; iColonne < matrice.GetLength(1); iColonne++)
            {
                contenuMatrice += matrice[iLigne, iColonne] + " ; ";
            }

            contenuMatrice += "\n";
        }

        return contenuMatrice;
    }
}
```

---

# 20. Appels utiles dans le Main

## Appel lecture

```csharp
int nombre;

MethodesProgram.LireEntier("Entrez un nombre : ", out nombre);
```

---

## Appel tableau aléatoire

```csharp
int[] tableau;

MethodesProgram.RemplirTableauAleatoire(10, 0, 25, out tableau);
```

---

## Appel concaténation tableau

```csharp
string contenuTableau;

MethodesProgram.ConcatenerTableau(tableau, out contenuTableau);

Console.WriteLine(contenuTableau);
```

---

## Appel matrice aléatoire

```csharp
int[,] matrice;

matrice = MethodesProgram.RemplirMatriceAleatoire(3, 4, 0, 9);
```

---

## Appel concaténation matrice

```csharp
string contenuMatrice;

contenuMatrice = MethodesProgram.ConcatenerMatrice(matrice);

Console.WriteLine(contenuMatrice);
```

---

## Appel addition matrice

```csharp
int[,] matriceAddition;
bool additionPossible;

MethodesProgram.AdditionMatrice(
    matriceUn,
    matriceDeux,
    out matriceAddition,
    out additionPossible);
```

---

## Appel multiplication matrice

```csharp
int[,] matriceMultiplication;
bool multiplicationPossible;

MethodesProgram.MultiplicationMatrice(
    matriceUn,
    matriceDeux,
    out matriceMultiplication,
    out multiplicationPossible);
```
