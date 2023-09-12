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

<!-- #region id="hoNzFAff9R4O" -->
# <span style = "color :  #ca6f1e ">Activité Python : Dynamique des populations - Modèle d'Euler et Malthus</span>
<!-- #endregion -->

<!-- #region id="P9sr5TmgJwZy" -->
Tu trouveras dans ce module python les fonctions qui nous ont permis de simuler des épidémies, puis des activités complémentaires qui permettent d'aller un peu plus loin que ce qu'on a vu en vidéo.

Tu peux naviguer librement dans ce notebook en suivant les instructions. N'hésite pas à te familiariser avec le code et les différentes notions.
<!-- #endregion -->

<!-- #region id="zzkDAekqAwdh" -->
## I. Croissance géométrique d'une population
<!-- #endregion -->

<!-- #region id="okGM6EP6J8Sf" -->
On commence simplement par reprendre une croissance géométrique de la population. Tout ce qui est précédé ou entouré d'un # dans le code est un commentaire qui est là pour t'aider à comprendre l'algorithme.

Commence par lire le code ci-dessous et essaie de bien le comprendre. Une fois que c'est bon, tu peux exécuter la cellule et jouer avec les différents paramètres.
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 306} id="SmCMHlImAwdn" outputId="7f3bfe6a-d27d-472d-8206-412b02e069a4"
import matplotlib.pyplot as plt

#On définit une fonction "croissance_geometrique" qui prend en entrée les paramètres de la population
#et qui affiche la croissance au cours du temps

def croissance_geometrique(p0,q,N):
    #P est une liste contenant toutes les tailles successives de la population, p_n=P[n]
    P=[p0]

    ##############
    #   Calcul   #
    ##############

    #On crée une boucle for qui permet de calculer les termes successifs de p_n selon la relation p_k=q*p_(k-1)
    for k in range (1,N+1):
        p_k=P[-1]*q
        P.append(p_k)
        #On aurait aussi pu faire P.append(p0*q**k)

    ##############
    # Affichage  # 
    ##############

    #On peut ensuite afficher les valeurs de p_n.
    plt.plot(P, linewidth=3,color='black',label='population')
    plt.ticklabel_format(style='plain')
    plt.xlabel("Années")
    plt.ylabel("Taille de la population")
    plt.title("Evolution de la population pour p0="+str(p0)+" et q="+str(q),y=1.05)
    plt.show() 


##############
# Paramètres #
##############

#p0 représente la population initiale
p0=10
#q représente la raison de la suite géométrique
q=0.7
#N représente le nombre d'années maximal qu'on considère, si N=50, on regarde l'évolution sur 50 ans
N=9

#On appelle la fonction définie précédemment avec les paramètres choisis
croissance_geometrique(p0,q,N)

#Exécute la cellule une première fois puis change les paramètres p0, q et N pour voir leurs effets respectifs.

```

<!-- #region id="Wu7QEi7mAwdp" -->
####  <span style="color:blue">Module ipwidgets</span>
Comme tu le remarques, il est assez fastidieux de modifier les paramètres systématiquement dans le code pour obtenir de nouvelles simulations. Pour plus de simplicité, on utilise un module de Python *ipywidgets* qui permet de changer les paramètres avec un curseur plutôt que dans le code. Il suffit de choisir les paramètres puis de cliquer sur "Run Interact". On peut alors voir la simulation et changer à nouveau les paramètres en actualisant à chaque fois via le bouton "Run Interact".
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 452, "referenced_widgets": ["77de350191fb4dc7b590cd6f93ced9cf", "0d889642be2a40d98ccfe83db3398196", "831550fd328546cf9804e6039b95cefa", "d7138a1fe4df47f9a21d18787debf4e2", "5fd7abe06fff4c45bee972659221bfc9", "be18e1570a554cb2831286cc9470357d", "e267c4e6eb9d4dc4995f96a51a829872", "f5fdd963b3f947e0a199fb904dcb49b0", "a88666276d5f4fa8adcc2fff03c8226c", "0a55f869643a4ae88295490aa6c8c6d3", "221135f5c21e4a7dbf78b02b400bc38e", "9235f99f6f5d4c6aba1ac1d1ae7f4fcb", "d493529c52d74440961b587019f51d29", "8afa3fe43b434355990be2d8ec389f2d", "88bdd8c0c3ff49039a29d95a68f58701", "73ac63d934f1454b863c55fa92a6e441", "0a2c4b5a675f4f42b621b7d758630639"]} id="Dy2cqo2jAwdq" outputId="be8979be-326f-459e-8ea8-49ac601d65fa"
import numpy as np
import ipywidgets as widgets
from IPython.display import display

p0 = widgets.IntSlider(min=0, max=100, value=10, description='p0')
q = widgets.FloatSlider(min=0, max=2, value=1.05, description='q')
N = widgets.IntSlider(min=1, max=1000, value=50, description='N')

widgets.interact_manual(croissance_geometrique, p0=p0, q=q, N=N)
```

<!-- #region id="SH-uNONCAwdq" -->
## II. Croissance géométrique limitée par la croissance arithmétique des ressources
<!-- #endregion -->

<!-- #region id="VJXZhZhemwyw" -->
On définit désormais une fonction "croissance_ressources" qui prend en entrée les paramètres de la population et qui affiche la croissance au cours du temps en tenant compte des limitations des ressource. Essaie de comprendre le code et joue ensuite avec les paramètres.
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 306} id="WdF6oJO7Awdq" outputId="a90d47c8-0c2d-4d6d-92f4-165723a55c06"


def croissance_ressources(p0,q,r0,a,N):
    #P est une liste contenant toutes les tailles successives de la population, p_n=P[n]
    P=[p0]
    
    #Pour une meilleure compréhension, on affiche aussi la suite géométrique classique et la croissance des ressources
    # en pointillés
    
    #R contient les valeurs de la suite arithmétique des ressources
    R=[r0]
    #Q contient les valeurs de la suite géométrique
    Q=[p0]

    ##############
    #   Calcul   #
    ##############

    #On crée une boucle for qui permet de calculer les termes successifs de p_n en prenant le minimum
    # entre q*p_(n-1) et r_(n)=n*a+r0
    for k in range (1,N+1):
        p_k=min(P[-1]*q,k*a+r0)
        P.append(p_k)
        
        R.append(r0+a*k)
        Q.append(p0*q**k)

    ##############
    # Affichage  #
    ##############

    #On peut ensuite afficher les valeurs de p_n.
    plt.plot(P, linewidth=3,color='black',label='population')
    plt.plot(R,'--',color='orange',label='croissance des ressources')
    plt.plot(Q,'--',color='blue',label='croissance géométrique')
    plt.xlabel("Années")
    plt.legend()
    plt.ticklabel_format(style='plain')
    plt.ylabel("Taille de la population")
    plt.ylim(0,10*N+100)
    plt.title("Evolution de la population pour p0="+str(p0)+", q="+str(q)+", r0="+str(r0)+" et a="+str(a),y=1.05)
    plt.show()


##############
# Paramètres #
##############

#p0 représente la population initiale
p0=10
#q représente la raison de la suite géométrique
q=1.05
#a représente la raison de la suite arithmétique des ressources
a=10
#r0 représente la valeur initiale des ressources
r0=500
#N représente le nombre d'année maximal qu'on considère, si N=50, on regarde l'évolution sur 50 ans
N=200

#On appelle la fonction définit précédemment avec les paramètres qu'on a choisit
croissance_ressources(p0,q,r0,a,N)

#Tu peux exécuter la cellule plusieurs fois en changeant les paramètres à chaque fois pour voir leur effet respectif.

```

<!-- #region id="5GeMlrjOnByN" -->
Encore une fois, on ajoute un module qui permet de changer les paramètres directement sans avoir à exécuter la cellule à chaque fois.
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 484, "referenced_widgets": ["14d861610016478b962253509b8b0055", "5cdbe3d027fb4331baafc0e96e4143b3", "ab49c7e395b84ef99a718eebf9573258", "45e72de56cb54dab80e8a503e69380f6", "7d2d2c688dac435b919fa46bfa0f3cfd", "0dd04a1311014406ad726de39136b3d4", "eb53fccff9da4edeb76f3caeb9b46d34", "c2c0b24db78444089e69564ef8f11bbe", "a0a18c8b5f9047d59baeafc429417c42", "2298c35aa3954d43a716468f569b22d9", "5e66344478bd4ef29ec6365be47aa645", "d753ad82f76746ccba7340f3224c89b3", "8267e769fd5a4138ae762ab83bbc9ab8", "5c87388b85814f65a245372012c67ddd", "491cfa98772742478353e18d90c8f0ae", "07922fe32e8c4d61b4f3fe3ee993e14a", "0b2a09710fde4427a16922dbcbf15e7b", "ebe57509d8574812bd162955d33d1907", "978d8b70a38b4fd58aa42094a99c7a48", "0f9e8f5c941741a29c52ed1679a463be"]} id="SRgM-dDVAwdr" outputId="3359e37c-8be7-4195-942e-4f8c47d3378f"
p0 = widgets.IntSlider(min=0, max=100, value=10, description='p0')
q = widgets.FloatSlider(min=0, max=2, value=1.1, description='q')
r0 = widgets.FloatSlider(min=1, max=100, value=50, description='r0')
a = widgets.FloatSlider(min=1, max=10, value=5, description='a')
N = widgets.IntSlider(min=1, max=1000, value=50, description='N')

widgets.interact(croissance_ressources, p0=p0, q=q, r0=r0, a=a, N=N)
```

<!-- #region id="IZ4KoyVnAwds" -->
## III. Exercice pratique

On peut tester notre nouveau modèle sur des données réelles, en l'occurrence la démographie française entre 1945 et 2015. Le but de l'exercice suivant est de trouver les paramètres $c, a_0$ et $r_0$ qui permettent de simuler le plus précisement possible l'évolution de la population française. Essayez de modifier ces paramètres via les curseurs pour obtenir une courbe qui colle aux points de données rouges. On précise que $c$ est le taux de croissance de la population exprimé en pourcentage. Ainsi, $q=1+\frac{c}{100}$. \
<span style="color:green">Remarque : le code ci-dessous est beaucoup plus avancé. Tu peux ne pas tout comprendre, ce n'est pas grave. L'important ici est de pouvoir utiliser la simulation pour estimer les paramètres d'évolution d'une population. Tu pourras relire ce code un peu plus tard lorsque tu auras progressé en Python !</span>
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 452, "referenced_widgets": ["c5d6b68bf952414cb9277c9a2d82a9d9", "ae3e35e595224bf6bfb7cd21b09014ba", "3fa86589b39d40dc8994c46478fe2922", "5cb8b20fdfbd4b51862eacc3cbc77058", "5b4692e7c0614df4ac9e086194b454b7", "d4775db658594ad189d5720efcea7351", "2384bcb75bbd4196a56e3de790123e54", "ec2a4304c57f4a0d96e317fc000f3388", "020d354cd9a64321aa19067dd4058748", "92f1d96f10444e4d803d9ae64b65ba1d", "811c6e71f51e4e89b65d50b465d2331c", "56e3a86eb5ca4691a98f9a0ea19953df", "5b8c15bb0d9648cea0f7c7d77644e279", "b55439aa2048482186a40c02aea9fb2e", "1f194e225c654402a61264d48f5fd587", "dc344b9bed0e4781b9801e30801c04b9", "cd6f7dd88fdd4e39b44b809dba918b3a"]} id="N7uumhz1Awds" outputId="6d9ad948-b57e-4da0-d388-c7b7c4977aea"
population_fr=[39660000, 41429000, 43428000, 45684000, 47758000, 50772000, 52699000, 
               53880000, 55284000, 56709000, 57844000, 59549000, 61182000, 62765000, 64300000]
annees=[1945+k*5 for k in range(0,15)]


def exercice(c,r0,a):
    q=1+c/100
    p0=39660000
    P=[p0]
    R=np.array([r0+a*k for k in range(71)])
    Q=np.array([p0*q**k for k in range(71)])
    P=np.zeros(71)
    n=np.argmax(R<Q)
    if Q[-1]<R[-1]:
        n=71
    P[:n]=Q[:n]
    P[n:]=R[n:]
    absc=[1945+k for k in range(71)]
    plt.plot(absc,P, linewidth=3,color='black',label='population')
    plt.plot(absc,R,'--',color='orange',label='croissance des ressources')
    plt.plot(absc,Q,'--',color='blue',label='croissance géométrique')
    plt.xlabel("Années")
    plt.ylim(39660000,68000000)
    plt.ticklabel_format(style='plain')
    plt.ylabel("Taille de la population")
    plt.title("Evolution de la population pour p0="+str(p0)+", q="+str(q)+", r0="+str(r0)+" et a="+str(a),y=1.05)
    plt.scatter(annees,population_fr,color='red',label='données')
    plt.ticklabel_format(style='plain')
    plt.legend()
    plt.show()


c = widgets.FloatSlider(min=0, max=2, value=1.4, description='c')
r0 = widgets.IntSlider(min=39660000, max=68000000, value=50000000, description='r0')
a = widgets.IntSlider(min=100000, max=500000, value=200000, description='a')

widgets.interact_manual(exercice, c=c, r0=r0, a=a)


```

<!-- #region id="HTMIiUg7nNaU" -->
### <span style="color : green">Exercice 1</span> 

Joue avec les paramètres jusqu'à ce que les 2 courbes soient à peu près superposées. Les "bons" paramètres à trouver se trouvent ci-dessous, je te conseille donc de ne pas regarder la suite avant d'avoir essayé de trouver seul les "bons" paramètres.
<!-- #endregion -->

<!-- #region id="NsjbPW9kAwdt" -->
## IV. Variante du modèle de Malthus : ressources non périssables

Jusqu'à maintenant, on a implicitement supposé que les ressources non utilisées sur une année n'étaient pas utilisables pour l'année suivante, mais on peut tout à fait faire l'hypothèse que les ressources sont non périssables et qu'elles peuvent se conserver ! 

Avant de regarder la suite, quel va être à ton avis le nouveau comportement de la population ? Qu'est-ce que cela change ? 

Il faut tout d'abord traduire notre hypothèse en langage mathématique. 
On a toujours $p_n=\min (q*p_{n-1},r_n)$, mais cette fois, $r_n=(na+r_0)+(r_{n-1}-p_{n-1})$. Le premier terme correspond à la production de ressource annuelle comme tout à l'heure qui croît de manière arithmétique. Le deuxième terme correspond aux ressources de l'année dernière qui n'ont pas été consommée par la population. Avec ces formules, on peut lancer des simulations pour observer ce qu'il se passe.
<!-- #endregion -->

```python colab={"base_uri": "https://localhost:8080/", "height": 227, "referenced_widgets": ["f4b4b537c387485eb86d61a7e0fd210f", "1c8091a3b0044dc7bff184977aa10f3a", "eba18099dfab415c909a3a03eef64735", "cd44bcc0e28c45e996f4f67afafd5e90", "1897283b226c4c87a65526307d99cd96", "14c81c7adc6944d4ad18a09b5135ec81", "a5fb93f0ab6c466883b6db728f5e9f1a", "a7e19eb17e2a45afadc3e5aa66aff138", "b7ad546437c9425696115935b1e32118", "98c18e1ad0984f1bb32a8d7f3605ec2f", "6c62bb2fdd8f45da93194049452f8778", "45287d6dde9f450f8cdf5e3822f1b536", "002d28e5868e4098b7752b96ec79ce5a", "fdbc557ac2634c3fba8efd6ffe5d91a5", "e7f63647077e408283ea5f8308262f67", "0b5204ea23ac4e6bb8d96bc924c08c10", "ecbb0085074c41ca815fd3684f682657", "2f53878b2d2c416c93eb953f972880ce", "54af184326af49299bb445b389bb7838", "5b81be1cdb0446d199d41177bc2a2eda", "eb3da8b52c0b48b4847612079d466564", "cec7517e3e18485fac0f5955640fe80d", "be90d4eed18e4599b3a5011946326e36"]} id="bQ8JUIqGAwdt" outputId="768991f1-0a28-4d53-8946-0313ed4b4dc2"

#On définit une fonction "croissance_ressources" qui prend en entrée les paramètres de la population
#et qui affiche la croissance au cours du temps en tenant compte de la croissance des ressources

def variante_malthus(p0,q,r0,a,N):
    #P est une liste contenant toutes les tailles successives de la population, p_n=P[n]
    P=[p0]
    #R contient les valeurs de la suite arithmétique des ressources
    R=[r0]
    #Q contient les valeurs de la suite géométrique
    Q=[p0]
    for n in range (1,N+1):
        nouveau_terme=(R[-1]-P[-1]) 
        r_n=n*a+r0+nouveau_terme
        p_n=min(P[-1]*q,r_n)
        P.append(p_n)
        R.append(r_n)
        Q.append(p0*q**n)
        
    ##############
    # Affichage  #
    ##############
    plt.plot(P, linewidth=3,color='black',label='population')
    plt.plot(R,'--',color='orange',label='ressources disponibles')
    plt.plot(Q,'--',color='blue',label='croissance géométrique')
    plt.xlabel("Années")
    plt.ylim(0,max(R)+100)
    plt.legend()
    plt.ticklabel_format(style='plain')
    plt.ylabel("Taille de la population")
    plt.title("Evolution de la population pour p0="+str(p0)+", q="+str(q)+", r0="+str(r0)+" et a="+str(a),y=1.05)
    plt.show()
    

p0 = widgets.IntSlider(min=0, max=100, value=10, description='p0')
q = widgets.FloatSlider(min=0, max=2, value=1.2, description='q')
r0 = widgets.FloatSlider(min=1, max=100, value=50, description='r0')
a = widgets.FloatSlider(min=1, max=10, value=5, description='a')
N = widgets.IntSlider(min=1, max=1000, value=50, description='N')

widgets.interact_manual(variante_malthus, p0=p0, q=q, r0=r0, a=a, N=N)

```

<!-- #region id="ZU_oR4dWAwdu" -->
Le comportement devient alors très intéressant. Comme précédemment, on a initialement une croissance géométrique jusqu'à ce que la population atteigne ses capacités en ressources. Cette fois ci, comme les ressources ont été économisées au fil des années, on a une décroissance de la population sur un certain temps car en fait cette dernière vit au-delà de ses moyens ! Elle ne fait que profiter des ressources mises de côté, mais cela ne peut pas durer éternellement ! La population décroît alors jusqu'à pouvoir se satisfaire de sa production annuelle de ressources. A partir de ce moment, la croissance se fait comme précédemment, au même rythme que la croissance arithmétique des ressources.
<!-- #endregion -->

<!-- #region id="H1fo7VjtAwdu" -->
### Variante du modèle de Malthus : des meurtres dans la population !

Pour terminer, on peut regarder ce qu'il se passe si on fait l'hypothèse qu'une partie de la population s'entretue... On oublie les ressources et on suppose que la population croît toujours de manière géométrique, à ceci près que chaque année, une part de la population meurt (soit parce-qu'il y a des meurtres ou que trop d'humains induisent une pollution toxique par exemple). En language mathématique, on va par exemple supposer que :
$$ p_{n+1}=q \ p_n-\frac{m}{1000} \ p_{n}^2$$
avec $m$ un paramètre qui contrôle la mortalité.
Le terme négatif $-\frac{m}{1000} \ p_{n}^2$ correspond donc à ce phénomène de mortalité qui croît comme le carré de la population (c'est une hypothèse, il y a d'autres façons d'introduire un terme de mortalité).

### <span style="color : green">Exercice 2</span>

Qu'est-ce que cela peut donner en termes de simulation à ton avis ? Est-ce que la population peut tendre vers l'infinie ? Peut-elle s'éteindre ?

Essaie de programmer ta propre fonction qui modélise cette évolution de population afin de répondre à cette question !
<!-- #endregion -->

```python id="8_Ovx2suAwdv" outputId="aa2a978d-3c69-4b70-b035-267c6ffa3178"
def variante_mortalite_exercice(p0,q,m,N):
    #P est une liste contenant toutes les tailles successives de la population, p_n=P[n]
    P=[p0]
    #On veut remplir les N premières valeurs de P ! (Indice : il faudra une boucle for)
    
    
    
    
    # à toi de remplir ces lignes #
    
    
    
    #On a préparé ci-dessous le code permettant l'affichage graphique de la simulation    
    plt.plot(P, linewidth=3,color='black',label='population')
    plt.xlabel("Années")
    plt.legend()
    plt.ticklabel_format(style='plain')
    plt.ylabel("Taille de la population")
    plt.title("Evolution de la population pour p0="+str(p0)+", q="+str(q)+" et m="+str(m),y=1.05)
    plt.show()
    
variante_mortalite_exercice(10,1.2,1,50)
```

<!-- #region id="kah46M-gnbTD" -->
## <span style="color : red">Correction Exercice 1</span>


Pour l'exercice sur la démographie francaise, tu devrais trouver à peu près ces paramètres : c=1, r0=45 000 000, a=270 000. Evidemment, il n'y a pas de valeur exacte, donc pas de soucis si tu n'as pas exactement les mêmes nombres.
<!-- #endregion -->

<!-- #region id="NUSSMqwnnw6V" -->
## <span style="color : red">Correction Exercice 2</span>

Le code qui suit est la correction. Ne la regarde pas avant de chercher toi-même, mais tu peux t'en inspirer si jamais tu bloques après plusieurs minutes de recherche. Il y a une infinité de façons de coder un même algorithme, ne t'inquiètes pas si tu n'as pas exactement la même chose. Si tu penses avoir trouvé un algorithme qui fonctionne, choisis un jeu de paramètre et lance une simulation sur ton algorithme puis sur l'algorithme de la correction. Si le résultat est le même dans les deux cas, c'est que ton algorithme fonctionne bien ! 
<!-- #endregion -->

```python colab={"referenced_widgets": ["084a8a0828df4070a7c3fa57ea8dfacc", "d0f3462147b8491997739f1f604b3df1"]} id="W_xw8ojSAwdv" outputId="12a994ab-a20b-4909-e3d8-38f522f2398d"
def variante_mortalite(p0,q,m,N):
    #P est une liste contenant toutes les tailles successives de la population, p_n=P[n]
    P=[p0]
    
    for n in range (1,N+1):
        p_n=q*P[-1]-m/1000*P[-1]**2
        P.append(p_n)
        
    plt.plot(P, linewidth=3,color='black',label='population')
    plt.xlabel("Années")
    plt.legend()
    plt.ticklabel_format(style='plain')
    plt.ylabel("Taille de la population")
    plt.title("Evolution de la population pour p0="+str(p0)+", q="+str(q)+" et m="+str(m),y=1.05)
    plt.show()

p0 = widgets.IntSlider(min=0, max=100, value=10, description='p0')
q = widgets.FloatSlider(min=0, max=2, value=1.2, description='q')
m = widgets.FloatSlider(min=0, max=5, value=1, description='M')
N = widgets.IntSlider(min=1, max=100, value=50, description='N')

widgets.interact_manual(variante_mortalite, p0=p0, q=q, m=m, N=N)
        
```

<!-- #region id="kt76tGP0Awdw" -->
On remarque qu'avec ce modèle, la population atteint un niveau d'équilibre ! Cela permet donc de rendre compte des démographies qui ont commencé par grossir jusqu'à atteindre un plateau. On a d'ailleurs posé les bases du modèle de Verhulst qu'on étudie plus en détails dans le prochain module !

Question bonus : en supposant vrai le fait que la suite $p_n$ converge vers une limite $l$ et qu'on a la relation $ p_{n+1}=q \ p_n-\frac{m}{1000} \ p_{n}^2$, peut-on trouver une formule pour $l$ en fonction de $q$ et de $m$ ?
<!-- #endregion -->

```python

```
