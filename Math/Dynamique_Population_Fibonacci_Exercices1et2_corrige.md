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

# <span style = "color :  #ca6f1e ">Activité Python : Dynamique des populations - Les lapins de Fibonacci, Partie I </span> {-}
# <span style = "color:red">CORRECTION</span> {-}

# Calcul des différents termes de la suite de Fibonacci.




## Exercice 1 : Calcul du nombre de couples de lapins à l'aide de la relation de récurrence

Je te propose dans cet exercice, de créer une fonction python Fibonacci_1() qui prend en entrée le mois $n$ et qui renvoie le nombre de couples de lapins présents au mois $n$. L'algorithme s'appuiera sur la relation que tu as découverte dans la vidéo qui lie les termes de la suite des mois $n$, $n-1$ et $n-2$. 

Ensuite, tu peux vérifier que ton programme fonctionne pour le 20ème mois, mois pour lequel nous avons déjà calculé le nombre de lapins présents. 
J'ai commencé à créer la structure du programme, je te laisse le compléter comme il se doit. Au travail !

```python
def Fibonacci_1(n): # n correspond à la n-ième génération dont on veut calculer le nombre de couples de lapins
    F0 = 0 # F0 corespond à l'initialisation, il n'y a pas de couples de lapin
    F1 = 1 # F1 correspond au premier mois, il y a le premier mois 1 couple de lapins
    
    for k in range(2,n+1) : # Cela signifie que k va de 2 à n+1 non inclus (ie. de 2 à n inclus)
        newF = F0 + F1 # On calcule le nombre de couples de lapins à la prochaine génération
        F0 = F1  
        F1 = newF
    
    return(newF)     
```

Vérifie ci-dessous que ton programme fonctionne bien (n'oublie pas d'exécuter ton programme précédent au préalable). \
Pour rappel, nous cherchons le **nombre de lapins** présents le 20ème mois qui est égal à *13530*.


```python
n = 20

n = 43

print('A la ', str(n), 'e génération, il y a ', str(Fibonacci_1(n)), ' couples de lapins.', 'Donc il y a au total', str(2*Fibonacci_1(n)), 'lapins')
```

```python
## Méthode 2

# On crée une liste F où on va stocker tous les résultats de la suite de Fibonacci jusqu'à la n-ième génération
# Ainsi, F[n] correspondra au nombre de couples de lapins de la n-ième génération. 

def Fibonacci_1_bis(n) :
    F0 = 0
    F1 = 1
    
    F = [0]*(n+1)  # On crée une liste F de zéros que l'on modifiera par la suite
    F[0] = F0
    F[1] = F1

    for k in range(2,n+1) : # Cela signifie que k va de 2 à n+1 non inclus (ie. de 2 à n inclus)
        F[k] = F[k-1] + F[k-2]   # Relation de récurrence de la suite de Fibonacci
        
    return(F[n])
```

```python
# Vérifie ici que ton programme fonctionne bien (n'oublie pas d'exécuter ton programme au préalable)

n = 7

print('A la ', str(n), 'e génération, il y a ', str(Fibonacci_1_bis(n)), ' couples de lapins.')
```

Tu as peut-être plutôt codé la deuxième méthode mais les deux méthodes sont correctes et donnent d'ailleurs les mêmes résultats si on prend le même n. En revanche, la méthode 1 prend moins de mémoire car elle ne stocke que les deux derniers résultats, tandis que la méthode 2 stocke tous les résultats de 0 à n dans une liste. Pour des n "petits", cela n'a pas d'importance, mais dans d'autres contextes, avec des n "plus grands", il faudra faire attention aux problèmes de stockage des données. 


## Exercice 2 : Calcul du nombre de couples de lapins à l'aide à l'aide de la formule explicite

Je te propose dans cet exercice de découvrir une formule qui nous permettra de calculer directement le nombre de couples de lapins présents à la génération $n$ sans devoir calculer toutes les valeurs intermédiaires comme précédement. A la fin de l'exercice, nous allons utiliser cette formule pour créer une fonction python Fibonacci_2() qui prend en entrée le mois $n$ et qui renvoie le nombre de couples de lapins présents au mois $n$. 
Tu pourras vérifier que ton programme fonctionne pour le 20ème mois, mois pour lequel nous avons déjà calculé le nombre de lapins présents. Courage !

<!-- #region -->
On considère l'équation $r^2 = r + 1$.

### 1) Trouver les racines de cette équation}. {-}


On admet le résultat suivant : 

On consdière une suite $(u_n)_{n \in \mathbb{N}}$ définie par récurrence par $u_{n+2} = u_{n+1} + u_n$. Alors la formule explicite de la suite $(u_n)_{n \in \mathbb{N}}$ est : \begin{equation*} u_n = \alpha \cdot \phi^n + \beta \cdot \phi'^n \end{equation*} où $\phi$ et $\phi'$ sont les deux racines réelles de l'équation $r^2 = r + 1$, et $\alpha$ et $\beta$ sont des constantes.

### 2) Calculer le terme général $F_n$ de la suite de Fibonacci en vous aidant de ce résultat {-}

### 3) Ecrire un algorithme permettant de calculer le n-ième terme de la suite de Fibonacci en s'aidant de la formule trouvée dans la question précédente}{-}
<!-- #endregion -->

1) Soit $\Delta$ le discrimnant de l'équation $r^2-r-1=0$.

Ainsi $\Delta = 1 - 4*(-1)*1 = 5 $. \\

On note $\phi$ et $\phi'$ les deux racines de l'équation.

$\begin{equation*}
\phi = \frac{1+\sqrt{5}}{2}
\qquad
\phi' = \frac{1-\sqrt{5}}{2}
\end{equation*}$

2) La suite de Fibonacci s'écrit $F_{n+2} = F_{n+1} + F_n$
Ainsi, d'après le résultat admis de l'énoncé, la formule du terme général s'écrit 
$\begin{equation*} F_n = \alpha \cdot \phi^n + \beta \cdot \phi'^n \end{equation*} $ où $\alpha$ et $\beta$ sont des constantes que l'on va déterminer grâce aux valeurs d'initialisation $F_0$ et $F_1$

Or, $F_0 = 0$ et $F_1 = 1$ car le premier mois, le premier couple de lapins ne peut pas faire d'enfants. Il y a donc toujours un seul couple de lapins le 1er mois. 

Donc on a :

$\begin{equation*} F_0 = \alpha \cdot \phi^0 + \beta \cdot \phi'^0 = 0 \qquad F_1 = \alpha \cdot \phi^1 + \beta \cdot \phi'^1 = 1 \end{equation*}$

Ce qui donne le système d'équations suivant :

$\begin{cases}
\alpha + \beta = 0
\\
\alpha \cdot \phi + \beta \cdot \phi' = 1
\end{cases}$

Soit : 

$\begin{cases}
\alpha =-\beta
\\
(-\beta) \cdot \phi + \beta \cdot \phi' = 1
\end{cases}$

Donc : 

$\begin{equation*}
\beta = \frac{1}{\phi' - \phi} = -\frac{1}{\sqrt{5}}
\end{equation*}$

et 
$\begin{equation*}
\alpha = - \beta = \frac{1}{\sqrt{5}}
\end{equation*}$

On obtient donc  : 

$\begin{equation*}
F_n = \frac{\phi^n - \phi'^n}{\sqrt{5}}
\end{equation*}$

```python
## Pour la question 3

from math import *

def Fibonacci_2(n) : 

    phi = (1+sqrt(5)) / 2
    phiprime = (1-sqrt(5)) / 2

    F = ((phi**n) - (phiprime**n)) / sqrt(5)
    
    return(F)

    
    

```

Vérifie ci-dessous que ton programme fonctionne bien (n'oublie pas d'exécuter ton programme précédent au préalable). \
Pour rappel, nous cherchons le **nombre de lapins** présents le 20ème mois qui est égal à *13530*.

```python
n = 7

print('A la ', str(n), 'e génération, il y a ', str(Fibonacci_2(n)), ' couples de lapins.')
```

### 4) Tester le programme avec plusieurs valeurs de n et comparer avec les résultats donnés par le programme de l'exercice 1 {-}
N'hésite pas à tester avec plusieurs valeurs de $n$, sans toutefois dépasser $n=1500$. D'ailleurs, question bonus : que se passe-t-il pour des valeurs de $n$ supérieures à 1500 par exemple ?

```python
print("Test pour n = 7, programme 2 : ",Fibonacci_2(7))
print("Test pour n = 7, programme 1 : ",Fibonacci_1(7))

print("Test pour n = 30, programme 2 : ",Fibonacci_2(30))
print("Test pour n = 30, programme 1 : ",Fibonacci_1(30))

print("Test pour n = 50, programme 2 : ",Fibonacci_2(50))
print("Test pour n = 50, programme 1 : ",Fibonacci_1(50))

print("Test pour n = 100, programme 2 : ",Fibonacci_2(100))
print("Test pour n = 100, programme 1 : ",Fibonacci_1(100))
```

On remarque que l'algorithme de la question 4 ne donne pas tout à fait la valeur exacte de $F_n$, mais un nombre approché. En effet, pour faire le calcul de $\phi^n$ et $\phi'^n$, la machine doit faire des arrondis car elle ne peut mémoriser qu'un nombre fini de chiffres après la virgule. Ainsi, le résultat final est en fait une valeur approchée qui reste néanmoins très précise à l'échelle où on travaille (je te laisse calculer l'erreur relative pour t'en convaincre). Lorsqu'on écrit un programme, il faut donc garder en tête que la valeur retournée par le programme peut être une valeur approchée de la valeur recherchée. Attention donc aux mauvaises surprises : tu en découvriras davantage lors des prochains modules. 
