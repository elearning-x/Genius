---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.6
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# <span style = "color :  #ca6f1e ">Activité Python : Dynamique des populations - Les lapins de Fibonacci, Partie I </span>


# Calcul des différents termes de la suite de Fibonacci.

Je te propose à travers cette activité d'utiliser le langage Python pour calculer les différentes valeurs de la suite de Fibonacci, soit à l'aide de la relation de récurrence soit à l'aide de la formule explicite.


## Exercice 1 : Calcul du nombre de couples de lapins à l'aide de la relation de récurrence

Je te propose dans cet exercice, de créer une fonction python Fibonacci_1() qui prend en entrée le mois $n$ et qui renvoie le nombre de couples de lapins présents au mois $n$. L'algorithme s'appuiera sur la relation que tu as découverte dans la vidéo qui lie les termes de la suite des mois $n$, $n-1$ et $n-2$. 

Ensuite, tu peux vérifier que ton programme fonctionne pour le 20ème mois, mois pour lequel nous avons déjà calculé le nombre de lapins présents. 
J'ai commencé à créer la structure du programme, je te laisse le compléter comme il se doit. Au travail !

```python
def Fibonacci_1(n): # n correspond à la n-ième génération dont on veut calculer le nombre de couples de lapins
    F0 = 0 # F0 corespond à l'initialisation, il n'y a pas de couples de lapin
    F1 = 1 # F1 correspond au premier mois, il y a le premier mois 1 couple de lapins
    
    for k in range(2,n+1) : # Cela signifie que k va de 2 à n+1 non inclus (ie. de 2 à n inclus)
        
        
        
        
    
```

Vérifie ci-dessous que ton programme fonctionne bien (n'oublie pas d'exécuter ton programme précédent au préalable). \
Pour rappel, nous cherchons le **nombre de lapins** présents le 20ème mois qui est égal à *13530*.


```python

```

## Exercice 2 : Calcul du nombre de couples de lapins à l'aide à l'aide de la formule explicite

Je te propose dans cet exercice de découvrir une formule qui nous permettra de calculer directement le nombre de couples de lapins présents à la génération $n$ sans devoir calculer toutes les valeurs intermédiaires comme précédement. A la fin de l'exercice, nous allons utiliser cette formule pour créer une fonction python Fibonacci_2() qui prend en entrée le mois $n$ et qui renvoie le nombre de couples de lapins présents au mois $n$. 
Tu pourras vérifier que ton programme fonctionne pour le 20ème mois, mois pour lequel nous avons déjà calculé le nombre de lapins présents. Courage !

<!-- #region -->
On considère l'équation $r^2 = r + 1$.

### 1) Trouver les racines de cette équation. {-}


On admet le résultat suivant : 

On consdière une suite $(u_n)_{n \in \mathbb{N}}$ définie par récurrence par $u_{n+2} = u_{n+1} + u_n$. Alors la formule explicite de la suite $(u_n)_{n \in \mathbb{N}}$ est : \begin{equation*} u_n = \alpha \cdot \phi^n + \beta \cdot \phi'^n \end{equation*} où $\phi$ et $\phi'$ sont les deux racines réelles de l'équation $r^2 = r + 1$, et $\alpha$ et $\beta$ sont des constantes.

### 2) Calculer le terme général $F_n$ de la suite de Fibonacci en vous aidant de ce résultat {-}

### 3) Ecrire un algorithme permettant de calculer le n-ième terme de la suite de Fibonacci en s'aidant de la formule trouvée dans la question précédente {-}
<!-- #endregion -->

```python
## Pour la question 3

from math import *

def Fibonacci_2(n) : 

    phi = (1+sqrt(5)) / 2
    phiprime = (1-sqrt(5)) / 2

    
    

```

Vérifie ci-dessous que ton programme fonctionne bien (n'oublie pas d'exécuter ton programme précédent au préalable). \
Pour rappel, nous cherchons le **nombre de lapins** présents le 20ème mois qui est égal à *13530*.

```python


```

### 4) Tester le programme avec plusieurs valeurs de n et comparer avec les résultats donnés par le programme de l'exercice 1} {-}
N'hésite pas à tester avec plusieurs valeurs de $n$, sans toutefois dépasser $n=1500$. D'ailleurs, question bonus : que se passe-t-il pour des valeurs de $n$ supérieures à 1500 par exemple ?

```python
# Test ci-dessous le programme avec différentes valeurs de n


```
