---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.6
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

<!-- #region -->
# Exercice 2 : la courbe de Von Koch 



L'objectif de cet exercice est de vous faire coder l'une des fractales les plus célèbres : la courbe de Von Koch, qui vous a été présentée la première semaine dans les exercices, et de vous faire observer l'autosimilarité de cet objet.

 

Une brève illustration des trois premières itérations de la courbe :


![media/math/Von_koch-2.png](attachment:Von_koch-2.png)

La totalité de l'exercice se fera dans ce notebook.
<!-- #endregion -->

## Fonctionnement de l'algorithme

Afin de tracer la fractale vous allez suivre l'algorithme suivant :

<!-- #raw -->
- Définir les positions des deux extrémités de la courbe.

- Créer un tableau destiné à contenir les abscisses de tous les sommets et un autre destiné à contenir les ordonnées de tous les sommets.

- Pour chaque couple de sommets entre lesquels aucun sommet n'est placé alors que toutes les itérations n'ont pas été effectuées:
    - Placer les trois sommets correspondant à une itération supplémentaire entre ces deux sommets.
    - S'il reste de la place entre les sommets nouvellement placés et les sommets de part et d'autres appliquer la même procédure.
    
- Afficher la figure obtenue.
<!-- #endraw -->

Cet algorithme utilisera donc une fonction récursive, c'est-à-dire une fonction qui fait appel à elle même. Nous aborderons ce point plus en détail au moment de coder celle-ci.


 ## 1- Initialisation du programme



Importez tout d'abord les librairies nécessaires.

```python
# On commence par importer le module matplotlib qui nous 
# permettra de tracer la figure et le module time qui 
# nous permettra de mesurer le temps.
import matplotlib.pyplot as plt
import time
from numpy import sqrt, pi, sin
```

Définissez le nombre d'itérations et calculez le nombre de segments et de sommets de la courbe qui en découlent.

```python
# On fixe le nombre d'itérations que l'on souhaite obtenir. 
# Ce nombre d'itérations doit être positif
nombre_iterations = 3

# On en déduit le nombre de segments de la fractale 
nombre_segments = 4** nombre_iterations 

# On en déduit le nombre de sommets 
nombre_sommets = nombre_segments + 1
```

Vous stockerez les positions des sommets sous la forme de deux tableaux :
- un tableau contenant tous les abscisses
- un tableau contenant toutes les ordonnées

la position dans le tableau définissant à quel sommet appartient la coordonnée. Les sommets se succéderont dans l'ordre du tracé.

Il vous faut donc créer ces tableaux.

```python
abscisses_tab = [float('inf')] * nombre_sommets
ordonnees_tab = [float('inf')] * nombre_sommets
# On remplit ici les tableaux avec la valeur inf qui signifie infini
# afin de pouvoir différentier les sommets remplis des autres.
# Ce n'est pas une obligation mais cela peut permettre de 
# mettre d'éventuelles erreurs en évidence.
```

Il vous faut désormais placer les deux extremités de la courbe.

```python
# première extrémité
abscisses_tab[0] = 0
ordonnees_tab[0] = 0

# deuxième extrémité
abscisses_tab[-1] = 100
ordonnees_tab[-1] = 0
```

Il ne reste plus qu'à fixer l'angle de la pointe de la courbe et le rapport d'homothétie entre une itération et la précédente. Pour la courbe de Von Koch, ils sont les suivants.

```python
angle = pi / 6
rapport = 3
```

## 2- Coeur du programme

Vous allez maintenant pas à pas définir la fonction placer_sommets qui constituera le coeur du programme. 
Voici ses spécifications :

```python
def placer_sommets(indice1, indice2, angle, rapport, MILIEU) :
    """
    Prérequis :
    indice1 et indice2 sont des entiers positifs strictements inférieurs à
    len(abscisses_tab)=len(ordonnees_tab). indice1 < indice2 et 
    |indice2 - indice1| - 1 est une puissance de trois. Cette 
    dernière condition assure qu'il reste bien à placer un nombre de 
    sommets cohérent avec la courbe de Von Koch.
    angle est un flottant et est exprimé en radians.
    rapport est un entier indiquant le rapport d'omothétie entre deux itérations
    successives (pour la courbe de Von Koch il vaut 3).

    
    Remplit les coordonnées des sommets d'indice compris entre indice1 et indice2
    dans les tableaux abscisses_tab et ordonnees_tab en respectant l'angle angle.
    
    Ne renvoie rien.
    """
    pass
```

On se placera dans un premier temps uniquement dans le cas où |indice2 - indice1| = 4, c'est-à-dire qu'il y a exactement trois sommets à placer entre les deux sommets considérés. Il s'agit donc de la dernière itération de la courbe.

Regardons comment placer les points à l'aide de la figure ci-dessous.

![media/math/proportion_von_koch-6.png](attachment:proportion_von_koch-6.png)

Le premier et le troisième point sont faciles à placer. Ils se situent sur l'axe entre les deux extrémité divisant le segment en rapport sous-segments de même longueur.

Pour le deuxième point, il se trouve à mi-chemin entre les deux segments mais décalé de HAUTEUR orthogonalement au segment. HAUTEUR se calcule de la façon suivante :

HAUTEUR = sin(angle)/rapport

Ainsi, les coordonnées des points B, C, C', et D sont les suivantes :

$(B_x, B_y) = (A_x + AE_x\_reduit, A_y + AE_y\_reduit) $

$(C'_x, C'_y) = (Ax + (AE / MILIEU)_x, A_t + (AE / MILIEU)_y)$

$(C_x, C_y) = (C'_x + HAUTEUR_x , C'_y + HAUTEUR_y)$ 

$(D_x, D_y) = (E_x - AEx\_reduit, E_y - AEy\_reduit) $

Remplissez maintenant la fonction placer_sommet en vous aidant de ces formules. Vous noterez que celles-ci ne sont plus valide si les coordonnées de A et E sont échangées (cela retourne le triangle). Il faudra donc s'assurer que indice1 < indice2.

À toutes fins utiles, nous rappellons que les fonctions sin et le nombre pi sont importés de la bibliothèque numpy.

```python
def placer_sommets(indice1, indice2, angle, rapport) :
    """
    Prérequis :
    indice1 et indice2 sont des entiers positifs strictements inférieurs à
    len(abscisses_tab)=len(ordonnees_tab). indice1 < indice2 et 
    |indice2 - indice1| - 1 est une puissance de trois. Cette 
    dernière condition assure qu'il reste bien à placer un nombre de 
    sommets cohérent avec la courbe de Von Koch.
    angle est un flottant et est exprimé en radians.
    rapport est un entier indiquant le rapport d'omothétie entre deux itérations
    successives (pour la courbe de Von Koch il vaut 3).

    
    Remplit les coordonnées des sommets d'indice compris entre indice1 et indice2
    dans les tableaux abscisses_tab et ordonnees_tab en respectant l'angle angle.
    
    Ne renvoie rien.
    """
    # On vérifie que indice1 et indice2 sont bien des indices 
    # d'éléments d'abscisses_tab et d'ordonnees_tab
    assert(indice1 >= 0 and indice1 < len(abscisses_tab))
    assert(indice2 >= 0 and indice2 < len(abscisses_tab))
    
    # On vérifie que indice1 < indice2
    assert(indice1 < indice2)
    
    # On vérifie que le nombre de sommets entre indice1 et indice2
    # est bien trois.
    assert((indice2 - indice1 - 1) == 3)
    
    # On utilisera les notations de la figure ci-dessus.   
    MILIEU = 2
    HAUTEUR = sin(angle)/rapport
    
    # Récupération des coordonnées connues
    Ax = abscisses_tab[indice1]
    Ay = ordonnees_tab[indice1]
    Ex = abscisses_tab[indice2]
    Ey = ordonnees_tab[indice2]
    
    # "hauteur" du point C sur l'axe (AE)
    Hx=(Ex+Ax)/MILIEU
    Hy=(Ey+Ay)/MILIEU
    
    # Calcul du vecteur AB
    AEx_reduit = (Ex-Ax) / rapport
    AEy_reduit = (Ey-Ay) / rapport

    # Calcul du vecteur C'C
    HCx =(Ey-Ay) * HAUTEUR
    HCy =(Ex-Ax) * HAUTEUR

    # Coordonnées de B
    Bx = Ax + AEx_reduit
    By = Ay + AEy_reduit

    # Coordonnées de C
    Cx = Hx - HCx
    Cy = Hy + HCy
    
    # Coordonnées de D
    Dx = Ex - AEx_reduit
    Dy = Ey - AEy_reduit

    # Pour l'instant d vaut 1 
    d = int((indice2 - indice1) / 4)
    
    # Assignation des coordonnées calculées dans le tableau

    abscisses_tab[indice1 + 1 * d] = Bx
    ordonnees_tab[indice1 + 1 * d] = By

    abscisses_tab[indice1 + 2 * d] = Cx
    ordonnees_tab[indice1 + 2 * d] = Cy

    abscisses_tab[indice1 + 3 * d] = Dx
    ordonnees_tab[indice1 + 3 * d] = Dy
```

Testez votre fonction.

```python
indice1 = 0
indice2 = 4
abscisses_tab[indice1] = 0
ordonnees_tab[indice1] = 0
abscisses_tab[indice2] = 10
ordonnees_tab[indice2] = 0
angle = pi / 3
rapport = 3


placer_sommets(indice1, indice2, angle, rapport)
print("abscisses : ", abscisses_tab)
print("ordonnees :", ordonnees_tab)
```

Il vous faut maintenant prendre en compte les autres cas de figure.
Copier ce que vous avez fait dans la cellule ci-dessous et modifier si besoin est votre code pour que la fonction effectue une itération même si |indice2 - indice1| > 3. Lorsqu'elle effectue une itération, elle ne renseigne que les coordonnées des trois sommets calculés.

```python
def placer_sommets(indice1, indice2, angle, rapport) :
    """
    Prérequis :
    indice1 et indice2 sont des entiers positifs strictements inférieurs à
    len(abscisses_tab)=len(ordonnees_tab). indice1 < indice2 et 
    |indice2 - indice1| - 1 est une puissance de trois. Cette 
    dernière condition assure qu'il reste bien à placer un nombre de 
    sommets cohérent avec la courbe de Von Koch.
    angle est un flottant et est exprimé en radians.
    rapport est un entier indiquant le rapport d'omothétie entre deux itérations
    successives (pour la courbe de Von Koch il vaut 3).

    
    Remplit les coordonnées des sommets d'indice compris entre indice1 et indice2
    dans les tableaux abscisses_tab et ordonnees_tab en respectant l'angle angle.
    
    Ne renvoie rien.
    """
    # On vérifie que indice1 et indice2 sont bien des indices 
    # d'éléments d'abscisses_tab et d'ordonnees_tab
    assert(indice1 >= 0 and indice1 < len(abscisses_tab))
    assert(indice2 >= 0 and indice2 < len(abscisses_tab))
    
    # On vérifie que indice1 < indice2
    assert(indice1 < indice2)
    
    # On vérifie que le nombre de sommets entre indice1 et indice2
    # est bien trois.
    assert((indice2 - indice1 - 1) % 3 == 0)
    
    # On utilisera les notations de la figure ci-dessus.   
    MILIEU = 2
    HAUTEUR = sin(angle)/rapport
    # Récupération des coordonnées connues
    Ax = abscisses_tab[indice1]
    Ay = ordonnees_tab[indice1]
    Ex = abscisses_tab[indice2]
    Ey = ordonnees_tab[indice2]
    
    # "hauteur" du point C sur l'axe (AE)
    Hx=(Ex+Ax) / MILIEU
    Hy=(Ey+Ay) / MILIEU
    
    # Calcul du vecteur AB
    AEx_reduit = (Ex-Ax) / rapport
    AEy_reduit = (Ey-Ay) / rapport

    # distance du point C à l'axe (AE)
    HCx =(Ey-Ay) * HAUTEUR
    HCy =(Ex-Ax) * HAUTEUR

    # Coordonnées de B
    Bx = Ax + AEx_reduit
    By = Ay + AEy_reduit

    # Coordonnées de C
    Cx = Hx - HCx
    Cy = Hy + HCy
    
    # Coordonnées de D
    Dx = Ex - AEx_reduit
    Dy = Ey - AEy_reduit

    d = int((indice2 - indice1) / 4)
    
    # Assignation des coordonnées calculées dans le tableau

    abscisses_tab[indice1 + 1 * d] = Bx
    ordonnees_tab[indice1 + 1 * d] = By

    abscisses_tab[indice1 + 2 * d] = Cx
    ordonnees_tab[indice1 + 2 * d] = Cy

    abscisses_tab[indice1 + 3 * d] = Dx
    ordonnees_tab[indice1 + 3 * d] = Dy
```

Tester de nouveau votre fonction dans le cas où indice2 - indice1 > 4 et dans le cas où indice2 - indice1 = 4. N'oubliez pas que abscisses_tab et ordonnees_tab ont déjà été modifié. Si besoin, vous pouvez les réinitialiser.

```python
#-------Initialisation des variables-------
abscisses_tab = [float('inf')] * nombre_sommets
ordonnees_tab = [float('inf')] * nombre_sommets
# première extrémité
abscisses_tab[0] = 0
ordonnees_tab[0] = 0
# deuxième extrémité
abscisses_tab[-1] = 100
ordonnees_tab[-1] = 0

# indices
indice1 = 0
indice2 = 4
abscisses_tab[indice1] = 0
ordonnees_tab[indice1] = 0
abscisses_tab[indice2] = 10
ordonnees_tab[indice2] = 0

indice1bis = 4
indice2bis = 4 + 4**2 
abscisses_tab[indice1bis] = 10
ordonnees_tab[indice1bis] = 0
abscisses_tab[indice2bis] = 50
ordonnees_tab[indice2bis] = 0

#-----------------Calculs--------------
placer_sommets(indice1, indice2, angle, rapport)
print("abscisses premier test : ", abscisses_tab)
print("ordonnees premier test :", ordonnees_tab)
placer_sommets(indice1bis, indice2bis, angle, rapport)
print("abscisses premier test : ", abscisses_tab)
print("ordonnees premier test :", ordonnees_tab)
```

Il vous faut maintenant traiter les sommets éventuels situés entre indice1 et indice2. Il vous faudra pour cela appeler placer_sommets au sein de cette même fonction. C'est ce que l'on appelle la récursivité.

Plus précisément, vous aurez besoin d'appeler placer_sommets uniquement si |indice2 - indice1| > 4 car sinon c'est que le nombre maximal d'itérations a été atteint.

Si vous devez faire appel à placer_sommets, combien de couples de sommets sont concernés ?

Recopier ce que vous avez déjà codé pour finir de remplir la fontion placer_sommets. 

ATTENTION : ne donner à placer_sommets que des arguments de type int. Pour transformer un flottant en entier, il vous faut utiliser la fonction int qui prend en argument un flottant et renvoie un entier. Pour plus de précision utiliser la fonction help.

```python
def placer_sommets(indice1, indice2, angle, rapport) :
    """
    Prérequis :
    indice1 et indice2 sont des entiers positifs strictements inférieurs à
    len(abscisses_tab)=len(ordonnees_tab). indice1 < indice2 et 
    |indice2 - indice1| - 1 est une puissance de trois. Cette 
    dernière condition assure qu'il reste bien à placer un nombre de 
    sommets cohérent avec la courbe de Von Koch.
    
    Remplit les coordonnées des sommets d'indice compris entre indice1 et indice2
    dans les tableaux abscisses_tab et ordonnees_tab.
    
    Ne renvoie rien.
    """
    # On vérifie que indice1 et indice2 sont bien des indices 
    # d'éléments d'abscisses_tab et d'ordonnees_tab
    assert(indice1 >= 0 and indice1 < len(abscisses_tab))
    assert(indice2 >= 0 and indice2 < len(abscisses_tab))
    
    # On vérifie que indice1 < indice2
    assert(indice1 < indice2)
    
    # On vérifie que le nombre de sommets entre indice1 et indice2
    # est bien un multiple de trois. Il doit en réalité être une
    # puissance de trois, cependant, ce test simple peut permettre
    # de détecter facilement d'éventuelles erreurs.
    assert((indice2 - indice1 - 1) % 3 == 0)
    
       
    # On vérifie que le nombre de sommets entre indice1 et indice2
    # est bien trois.
    assert((indice2 - indice1 - 1) % 3 == 0)
    
    # On utilisera les notations de la figure ci-dessus.   
    MILIEU = 2
    HAUTEUR = sin(angle)/rapport
    # Récupération des coordonnées connues
    Ax = abscisses_tab[indice1]
    Ay = ordonnees_tab[indice1]
    Ex = abscisses_tab[indice2]
    Ey = ordonnees_tab[indice2]
    
    # "hauteur" du point C sur l'axe (AE)
    Hx=(Ex+Ax)/MILIEU
    Hy=(Ey+Ay)/MILIEU
    
    # Calcul du vecteur AB
    AEx_reduit = (Ex-Ax)/rapport
    AEy_reduit = (Ey-Ay)/rapport

    # distance du point C à l'axe (AE)
    HCx =(Ey-Ay) * HAUTEUR
    HCy =(Ex-Ax) * HAUTEUR

    # Coordonnées de B
    Bx = Ax + AEx_reduit
    By = Ay + AEy_reduit

    # Coordonnées de C
    Cx = Hx - HCx
    Cy = Hy + HCy
    
    # Coordonnées de D
    Dx = Ex - AEx_reduit
    Dy = Ey - AEy_reduit

    d = int((indice2 - indice1) / 4)
    
    # Assignation des coordonnées calculées dans le tableau

    abscisses_tab[indice1 + 1 * d] = Bx
    ordonnees_tab[indice1 + 1 * d] = By

    abscisses_tab[indice1 + 2 * d] = Cx
    ordonnees_tab[indice1 + 2 * d] = Cy

    abscisses_tab[indice1 + 3 * d] = Dx
    ordonnees_tab[indice1 + 3 * d] = Dy
    
    if indice2 - indice1 > 4 :
        placer_sommets(indice1, indice1 + d, angle, rapport)
        placer_sommets(indice1 + d, indice1 + 2 * d, angle, rapport)
        placer_sommets(indice1 + 2 * d, indice2 - d, angle, rapport)
        placer_sommets(indice2 - d, indice2, angle, rapport)
```

Testez de nouveau votre fonction dans des cas différents.

```python
#-------Initialisation des variables-------
abscisses_tab = [float('inf')] * nombre_sommets
ordonnees_tab = [float('inf')] * nombre_sommets
# première extrémité
abscisses_tab[0] = 0
ordonnees_tab[0] = 0
# deuxième extrémité
abscisses_tab[-1] = 100
ordonnees_tab[-1] = 0

#-----------------Calculs--------------
placer_sommets(0, len(abscisses_tab) - 1, angle, rapport)
print("abscisses : ", abscisses_tab)
print("ordonnees :", ordonnees_tab)
```

## 3- Affichage

Il vous faut désormais afficher le résultat obtenu ! Vous utiliserez pour cela la fonction plot de matplotlib.pyplot.

```python
#-------Initialisation des variables-------
abscisses_tab = [float('inf')] * nombre_sommets
ordonnees_tab = [float('inf')] * nombre_sommets
# première extrémité
abscisses_tab[0] = 0
ordonnees_tab[0] = 0
# deuxième extrémité
abscisses_tab[-1] = 100
ordonnees_tab[-1] = 0

#-----------------Calculs--------------
placer_sommets(0, len(abscisses_tab) - 1, angle, rapport)


#-----------------Affichage----------------
# Nettoyage d'un tracé éventuel
plt.clf()
# tracé
plt.plot(abscisses_tab, ordonnees_tab)
# paramétrisation des axes 
plt.axis('equal')
# titre
plt.title("Courbe de Von Koch")
# affichage de la figure obtenue
plt.show()
```

## 4- Évaluation du temps de calcul

Avant de partir, penchez-vous sur le temps nécessaire pour calculer cette courbe. Nous avons ajouté de quoi mesurer et afficher le temps de calcul mis pour placer les sommets de la courbe.

Changez le nombre d'itérations et regardez comment le temps varie en fonction de celui-ci. À votre avis, cette évolution est-elle linéaire ? quadratique (c’est-à-dire proportionnelle au carré du nombre d’itérations) ? exponentielle ? 

```python
#-------Initialisation des variables-------

# On fixe le nombre d'itérations que l'on souhaite obtenir. 
# Ce nombre d'itérations doit être positif
nombre_iterations = 5

# On en déduit le nombre de segments de la fractale 
nombre_segments = 4**nombre_iterations

# On en déduit le nombre de sommets 
nombre_sommets = nombre_segments + 1
abscisses_tab = [float('inf')] * nombre_sommets
ordonnees_tab = [float('inf')] * nombre_sommets

# première extrémité
abscisses_tab[0] = 0
ordonnees_tab[0] = 0

# deuxième extrémité
abscisses_tab[-1] = 100
ordonnees_tab[-1] = 0

#-----------------Calculs------------------
debut = time.time()
placer_sommets(0, len(abscisses_tab) - 1, angle, rapport)
fin = time.time()
print("temps de calcul :", fin - debut)

#-----------------Affichage----------------
# Nettoyage d'un tracé éventuel
plt.clf()
# tracé
plt.plot(abscisses_tab, ordonnees_tab)
# paramétrisation des axes 
plt.axis('equal')
# titre
plt.title("Courbe de Von Koch")
# affichage de la figure obtenue
plt.show()
```

#### Réponse
Le temps mis par l’algorithme est exponentiel en fonction du nombre d’itérations (la fonction exponentielle est très fortement croissante.). En effet, chaque itération ajoute 3 avancées du curseur de Turtle ou 3 appels à l’itération suivante et trois rotation du curseur par étape. On peut donc considérer que chaque itération se fait à temps constant + les éventuels appels aux itérations suivantes. Le temps de calcul est donc à peu de chose près proportionnel au nombre d’appels à la fonction placer_sommets. Regardons ce qu’il se passe si n = 3.

![media/math/Schema_iterations.png](attachment:Schema_iterations.png)

La première ligne correspond à l’itération 1 qui représente le premier appel à la fonction. Cette ligne compte donc 4⁰ appel et génère 4 appels, un pour chaque segments. La ligne suivante compte donc 4¹ appels. Chacun de ces appels engendre 4 appels, la ligne suivante compte donc 4² appels. Il y a donc au total $4⁰ + 4¹ + 4² = \frac{1 - 4³}{1 - 4}= 21 $ appels (cette formule est celle de la somme d’une suite géométrique). À une constante près, le nombre d’appels est donc proportionnel à 4³ pour 3 itérations… On pourrait refaire le même calcul quel que soit le nombre d’itérations et on trouverait toujours un nombre d’appels proportionnel à 4 puissance le nombre d’itération. Comme on considère que le temps passé est proportionnel au nombre d’appels, on dit alors que le temps de calcul est exponentiel en fonction du nombre d’itérations.
