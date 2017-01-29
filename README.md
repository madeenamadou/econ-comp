# Essentiel de mes notes du cours d'economie computationelle appliquee

A. Système d’équations linéaire : Ax = b

* Méthodes directes : Décomposition LU, Cholesky
x = A\b
* Méthode itérative : forme Qx = b + (Q − A)x

Gauss-jacobi : gjacobi (A,b)

Gauss-seidel : gseidel (A,b)

B. Systèmes d’équation non linéaire : Points fixes : x0 tel que f(x0) = x0 / Solutions racines : x0 tel que f(x0) = 0

* Méthode bisection, sur un interval [a,b]
Pour une fonction f, 
bisect (‘f’,a,b)

* Méthode Newton : un ou plusieurs variables, avec des valeurs initiales , utilise le jacobien
Pour une fonction f à 2 variables, avec des valeurs initiales respectives x1 et x2
newton(’f’,[x1;x2])

* Méthode Quasi-Newton : utilise une approximation du jacobien
Secant Method : une variable
Broyden Method : plusieurs variables, utilise une valeur initiale pour la racine, et une autre pour le Jacobien
Pour une fonction f à deux variables, et pour les valeurs initiales x1et x2 des variables
broyden(’f’,[x1;x2])

Pour ces méthodes, on peut ajouter une backstepping routine, pour éviter les divergences

* Méthodes exclusives pour Point-fixes :
Méthode Itération de fonction, pour une valeur initiale x0
Pour une fonction g,
fixpoint(’g’,x0)

* Complementary Method : utilise le jacobien
Pour résoudre f(x) = 0, pour a < x < b ; a et b peuvent être Inf
- Méthode semismooth
Pour une fonction f, un intervalle [a,b], et une valeur initiale x0,
ncpsolve(’f’,a,b,x)
- Méthode minmax
Spécifier d’abord l’option “type”
optset(’ncpsolve’,’type’,’minmax’)

C. Problèmes d’optimisation (recherche du maximum et du minimum)

* Méthodes sans dérivées : 
- Method Golden Search : une variable, optimum local sur un interval [a,b]
Pour une fonction f, sur un interval [a,b]
golden(’f’,a,b)
- Méthode Algorithm Nelder-Mead : plusieurs variables, avec des valeurs initiales pour les variables
Pour une fonction f à deux variables, avec les valeurs initiales x1 et x2,
neldmead(’f’,[x1;x2])

D. Méthode d’intégration et de différentiation

* Méthode d’intégration

a) Calcul de l’aire
Si w(x)=1, on calcule l’aire sous la fonction
* Méthodes Newton-cotes : calculs de l’aire sous la fonction
Trapezoid rule : pour les fonctions discontinues et avec des points d’inflexion
Pour n trapezes, sur un intervalle [a,b], les nodes  et les weights w
[x,w] = qnwtrap(n,a,b)
Simpson rule : pour les fonctions plus lisses
[x,w] = qnwsimp(n,a,b)

* Méthodes Gaussian quadrature
Legendre quadrature, pour w(x) = 1
[x,w] = qnwlege(n,a,b)
b)	Calcul de l’espérance
Méthodes Gaussian quadrature
Si w(x)=prob. density function de x, on calcule l’espérance de la fonction
Pour x suivant une loi normale (mu, var), x les nodes gaussiens et w les weights gaussiens,
[x,w] = qnwnorm(n,mu, var)
L’espérance de la fonction est obtenue ensuite par Somme(w*f(x))
Méthodes Intégration Monte-Carlo
Il faut générer pseudoaléatoirement n nodes x d’après la distribution ; les weights w=1/n sont identiques
L’espérance de f est obtenue par  Somme(w*f(x))
Méthodes Quasi-Monte Carlo
Ici les n nodes x sont déterministes, sur une intervalle [a,b] ;  les weights 
w=(b-a)/n sont identiques
L’espérance de f est obtenue par Somme (w*f(x))

•	Méthode de différentiation

Pour une fonction f, une approximation O(h²) de sa dérivée, autour du point x0, est de la forme:

f’(x0) = a*f(x0) + b*f(x0 + h) + c*f(x0 + alpha*h) [ + O(h²)]

où (x0 + h), (x0 + alpha*h) sont deux autres points,
choisir, alpha = 2, et choisir h quelconque,
les paramètres a, b et c, s’obtiennent en résolvant le système suivant :

a + b + c = 0
b + cλ = 1/h
b + cλ2 = 0


E.	IVP

Prend la forme d’une equation différentielle à résoudre pour une function solution, connaissant les valeurs initiales pour la fonction.

e.g. à l’ordre 1  
ou à l’ordre 2 

il faut réécrire parfois le problème sous la forme d’une équation différentielle. une équation différentielle d’ordre 2 peut se ramener à un système d’équation 
différentielle d’ordre 1.

Le principe de résolution est de dériver une approximation de taylor de  à l’ordre 1 (Méthode de Euler), ou 2 (Méthode Runge-Kutta 2), ou 4 (Runge-Kutta 4). On choisit un pas h pour subdiviser la période de temps ; plus petit h, mieux c’est…en général.
Méthode de Euler : 

Méthode Runge-Kutta 2 : , avec :


Pour une bonne approximation, il faut que :

On choisit  puis on trouve ensuite les autres paramètres. Il existe trois méthodes communes pour évaluer les paramètres inconnus, connus comme: méthode de Heun, la méthode du point milieu et la méthode de Ralston.
Heun’s Method : 
Midpoint’s Method : 
Ralston’s Method :  
Paul L. Fackler & Mario J. Miranda : 
Méthode Runge-Kutta 4 : Avec: 




Pour un système d’équation différentiel « system », une période allant de t0 à tf, des valeurs initiales stockées dans la matrice « inits », la résolution numérique avec Runge-Kutta 4 :

[t,y] = ode45('system',[t0 tf],inits)
[t,y] = rk4('system',(t0:tf)',inits) 

Ou, avec un pas « h », des valeurs initiales stockées dans la matrice « Y0 », la période initiale « t0 », pour un système d’équation différentiel « f », la résolution numérique MuPAD avec Runge-Kutta 4 :

sol1 := numeric::odesolve2(f, t0, Y0, RK4, Stepsize=h)


F.	BVP
Il s’agit d’un boundary value problem quand on ne connait pas les valeurs initiales, pour la fonction. Il est souvent donné une boundary condition, comme par exemple la valeur terminale pour la fonction .
On cherche le guess  pour la valeur initiale pour la fonction tel que 

On réécrit ce problème sous la forme d’un problème de recherche de solution racine, à résoudre avec broyden.
Quand on trouve  on peut résoudre l’IVP

e.g. ici, on doit chercher la solution racine  pour la fonction « shooting » avec broyden. 





y0 = broyden('shooting',guess);  	


G.	Interpolation

Il s’agit d’obtenir une approximation  aussi correcte possible d’une fonction 

Pour un ensemble de  interpolation nodes  on veut que :
De façon générale,   est construit à partir d’une combinaisons linéaires de plusieurs fonctions connues . Pour  degrés d’interpolation ( fonctions  on a :

  

Les  sont inconnues et pour les obtenir, on résous simultanément les équations linéaires suivantes : 

Pour 


Les méthodes d’interpolations diffèrent dans le choix des fonctions . Les  sont généralement des polynômes. 
 
On peut des polynômes Chebychev.

Pour un degré d’interpolation , un intervalle de nodes   , on obtient les   : 
phi = fundefn('cheb', m, a, b);

Ensuite, on obtient les  pour une interpolation de 
c = funfitf(fhat, f);

puis on calcule  , pour “x” le vecteur-colonne des nodes 
fhat = funeval(c,phi,x)

On peut des polynômes splines. Le cubic splines est l’interpolation à l’ordre 3 avec cette famille de polynômes.
En utilisant cubic splines (), 

phi = fundefn('spli', m, a, b, k); 
phi(nodes) = funbas(basis);
c = funfitf(fhat, f); ou c = phi(nodes) \ v ; 
fhat = funeval(c,phi,x);


H.	Méthode collocation

Il s’agit d’une méthode pour la fonction  solution du problème du type : 

 sur un intervalle . Comme un système d’équation différentielle à l’ordre zéro. 
Le principe est d’utiliser une interpolation   de .
Pour un ensemble de  collocation nodes , le problème revient à résoudre simultanément les équations linéaires suivantes, pour  :
Pour 

On peut obtenir les collocations nodes à partir de nodes chebychev…

phi = fundefn(’cheb’, m, a, b);
x = funnode(phi);

Ensuite, on peut résoudre ce problème en utilisant broyden.

Pour un système d’équation « system », 
c = broyden(’system’,guess);



I.	Programmation dynamique

Sur  périodes,  . L’horizon  peut être fini ou infini.
On peut observer à chaque période,  plusieurs états  possibles. L’espace des états peut être discret ou continu.
 
Pour chaque état, il y a un ensemble d’actions disponibles  L’espace des actions peut aussi être discret ou continu.
 

Pour discrete state space et discrete action space,
Pour les horizons finis,

À chaque période t, l’action  dans l’état  produit le reward 
 est un processus soit déterministe, soit stochastique. 
Pour  stochastique, on suppose qu’il s’agit d’un markov decision process, donc, la probabilité d’un état  dépend seulement de l’état  et de l’action  à la période précédente t.
Quand il n’y a qu’une seule action disponible pour chaque état possible, on parle de processus chaine de markov.

La probabilité de transition 

L’objectif est de trouver un ensemble optimal d’action  pour chaque période 

Soit  le reward maximum que l’agent peut atteindre à partir de  jusqu’à la fin de période .  Pour chaque état  possible à , il y a une valeur 

Pour ,

C’est l’équation de Bellman.
Ensuite, on cherche  tel que : 


	
On impose une condition terminale, généralement . Avec cette condition pour  , on va backward, en cherchant  puis  et ainsi de suite jusqu’à .

Pour les horizons infinis,  ne dépend plus de  et le problème s’écrit : 







 
Pour ,




Et ainsi de suite….


On peut utiliser la méthode backward recursion,

Soit  le nombre fini d’états possibles,  le nombre fini d’actions disponibles
À la période , la matrice des probabilités de transition vers l’état 


Il y a  possibilités pour l’état  suivant :

Pour une fonction reward « f », un horizon « N », un facteur d’escompte « delta »,  les probabilités de transition « P » dans une matrice ,

On crée la structure « model »
model.reward    = f;
model.transprob = P;
model.horizon   = N;
model.discount  = delta;

Ensuite on trouve la solution 
[v,x] = ddpsolve(model);

Rechercher les informations suivantes:

State space
Action space
Reward function (based on Action space)
Transition probability matrix (m x n x n)


Pour continuous state space , le next state est une fonction continue du current state, du current action et d’un terme d’erreur (déterministe ou stochastique) : 

On travaille avec une approximation de la objective function (avec cubic spline)

, à partir de collocation nodes 

Pour chaque collocation nodes , on veut résoudre :



Si  est stochastique, on calcule l’espérance  avec une méthode d’intégration (comme gaussian quadrature). À partir de nodes (et de weights  l’équation devient




Concrètement…

Pour une fonction de transition d’état , avec  une v.a. 
On génère les nodes  et les weights  
[e,w] = qnwnorm(m,0,sigma^2);
Ensuite on trouve les 
basis = fundefn('spli', n, Smin, Smax);    
p     = funnode(basis);      
Puis on trouve               
phi   = funbas(basis); 

Ensuite, à partir d’un vecteur de valeurs initiales  on calcule 
c = zeros(n,1);

Et on recherche  tel que 
L’objectif est de trouver  qui vérifie 

for it=1:1000
    cold   = c;
    vprime = 0; 
    for k=1:m 
        vprime = vprime + w(k)*funeval(c,basis,beta*p+e(k));
    end
    v = max(exp(p)-K,delta*vprime);
    c = Phi\v;
    if norm(c-cold)<1.e-10, break, end
end

on devrait avoir au final, 
funeval(c,basis,p) = v

Pour continuous action space  

Action  est une function du state 

On a l’équation du problème



Action  peut être contrainte ou pas.
Sans contrainte, on trouve la solution à partir de la condition de premier ordre 





Donc 




