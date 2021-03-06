# TopoRobin

**Bienvenue sur le Git de Robin Grapin**

top99 et Top88 sont les retranscriptions en Julia des codes matlab d'optimisatin topologique du même nom de Ole Sigmund.
top88_AD et topFiniteDiff sont leurs versions adaptées aux cas où l'on n'a pas d'expression explicite fonction objectif. Le premier code calcule cette dernière à chaque itération en utilisant la différentiation automatique, tandis que le second utilise la méthode des différences finies.
Quant au code topggp, il implémente la méthode Generalized Geometry Projection, inspiré du code de Simone Coniglio.
( voir https://topggp.github.io/blog/ )
Cec codes rentrent dans le cadre de mon projet PIR de deuxième année à l'ISAE-Supaéro, sous la tutelle de Joseph Morlier.

article overleaf : https://www.overleaf.com/project/5ee1fa32ac09c1000117c8ed

Pour voir quelques exemples autres que la poutre MBB, jetez un oeuil au dossier "examples".

# Imports nécessaires pour faire fonctionner les codes

1. top99
  + SparseArrays
  + Plots
2. top88
  + SparseArrays
  + LinearAlgebra
  + Plots  ( jai choisi ce type d'affichage, plus confortable que ImageView)
  + Statistics (fonction mean() )
  + SuiteSparse
3. top88_AD
  + SparseArrays
  + LinearAlgebra
  + Plots  
  + Statistics 
  + SuiteSparse
  + ForwardDiff
4. topFiniteDiff
  + SparseArrays
  + LinearAlgebra
  + Plots 
  + Statistics 
  + SuiteSparse
  + FiniteDiff
5. topggp
+ SparseArrays
+ LinearAlgebra
+ Plots
+ Statistics
+ SuiteSparse
+ VectorizedRoutines

# Tutorial

Each of the codes on the folder \emph{TopoRobin} are touching on the MBB problem. Some other basic examples of use on different load cases are in the \emph{examples} subfolder. To use and extend these algorithms for other problems, the following steps and modifications should be done.

1. top99
 + Import the packages : Plots, SparseArrays ;
+ Modify the material properties E and nu ;
+ Modify the applied stresses line 6 of the top function ;
+ Modify the constraints on the fixed nodes line 9 of the top function ;
+ Modify the ending criterion lines 18 and 37 ;
+ An other possibility (also for top88 and its extensions) is to add the non-linear condition to have no material somewhere. To do so, passive elements must be defined before the optimization while creating an array of booleans of the size of the design space. If an element is defined as passive, at the end of each step of the optimization loop, this element's density must be set to zero. See the examples folder for such an example ;
+ Run the function with the parameters $nelx, nely$ for the number of element along the x and y axis, volfrac is the wished proportion of material in the design space of the part, the penalty penal must be chosen in function of the Poisson coefficient nu, usually penal = 3 for nu = 0.3 and rmin is the filter size divided by the element size ;
+ Plot the resulting density distribution (output x) and the evolution of the objective function (output c) in function of the number of iterations (output l).

2. top88

+ Import the packages : Plots, SparseArrays, LinearAlgebra, Statistics, SuiteSparse ;
+ Modify the material properties E0, Emin and nu ;
+ Modify the applied stresses line 22 of the top88 function ;
+ Modify the constraints on the fixed nodes line 24 of the top88 function ;
+ Modify the ending criterion lines 54 and 104 ;
+ In addition to top99 parameters, ft allows to chose between the sensitivity filter (ft = 1) or the density filter (ft = 2).

3. top88_AD


In addition to top88 steps, it is necessary to :
+ Import $ForwardDiff$ package ;
+ Define the objective function as the $objectif$ parameter. It must be a function of the density x and the elementary sensitivity $ce$.

4. top88-FD

To use finite differences instead of automatic differentiation on a same problem, the steps to follow are the same, but instead of using ForwardDiff package, it will be FiniteDiff package.

The difference of efficiency between the version using the automatic differentiation and the version with an explicit expression for the derivative, as the SIMP codes are used, can be compared. Once the modifications in both algorithms have been made, the Julia command @ time will be useful, returning the execution time, the memory allocated during the execution and the proportion of time used for the garbage collection.

5. topGGP

The GGP code is much more complicated, as the amount of possibilities is more important as SIMP methods for a similar design space. It also allows to chose different methods to solve the problem. To adapt the code to an other problem than the one by default (MBB, solver GP), the obligations and possibilities are :
  + Import SparseArrays, LinearAlgebra, Plots, Statistics, SuiteSparse and VectorizedRoutines packages ;
  + Chose the design space through the inputs nelx and nely ;
  + Select the problem through the input problem, or define a new one writing from the line 114 of the main function ;
  + Change the end conditions through the input maxoutit or the parameters kkttol and changetol lines 223 and 224.
