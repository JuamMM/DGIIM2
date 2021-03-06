# Solución al examen de Topología I del 21 de noviembre de 2016

## Ejercicio 1: 4 puntos
**Un espacio topológico $(X, \tau)$ se llama Hausdorff si para cualquier par de puntos distintos $x \neq y$ existen entornos $V \in \mathcal{U}^x$ y $W \in \mathcal{U}^y$ cumpliendo $V \cap W = \emptyset$.**

**(1) Probar que si $(X, \tau)$ es un espacio topológico Hausdorff, entonces cualquier punto suyo es un subconjunto cerrado.**

> **Proposición 1.1.1:**
$C \subseteq X$ cerrado $\Leftrightarrow \forall y \notin C \quad \exists V \in \mathcal{U}^y: V \cap C = \emptyset$

Sea $C = \{x\} \subset X$, veamos que es cerrado.
Tomo $y \notin C \Rightarrow y \in X - \{x\}$. Es claro que $x \neq y$.
Como $(X, \tau)$ es Hausdorff, $\exists V \in \mathcal{U}^y, \exists W \in \mathcal{U}^x: V \cap W = \emptyset$.
Al ser $C = \{x\} \subset W$, se tiene que $V \cap C = \emptyset$, luego C es cerrado por la proposición 1.1.1.


**(2) Probar que cualquier subespacio topológico de un espacio Hausdorff es Hausdorff** ("La propiedad Hausdorff es hereditaria")

Dado $Y \subset X$ veamos que $(Y, \tau_y)$ es Hausdorff.
Sean $y_1, y_2 \in Y, y_1 \neq y_2$. Como también pertenecen a X y éste es Hausdorff, $\exists O_1, O_2$ tales que $y_1 \in O_1, y_2 \in O_2$ y $O_1 \cap O_2 = \emptyset$.
También se sabe que
$$
y_1 \in O_1 \cap Y \in \tau_y \\
y_2 \in O_2 \cap Y \in \tau_y
$$
Por lo que $O_1 \cap Y$ y $O_2 \cap Y$ son entornos de $y_1$ e $y_2$, respectivamente, en $(Y, \tau_y)$.
Si su intersección es vacía, habremos acabado, ya que tendremos dos entornos en la topología inducida con intersección vacía, por lo que ésta será Hausdorff.
$$
(O_1 \cap Y) \cap (O_2 \cap Y) = (O_1 \cap O_2) \cap Y = \emptyset \cap Y = \emptyset
$$
QED.


**(3) Probar que el producto topológico de dos espacios Hausdorff es Hausdorff**
Sean $(X_1, \tau_1)$ y $(X_2, \tau_2)$ dos espacios Hausdorff. Veamos que $(X_1 \times X_2, \tau_1 \times \tau_2)$ es un espacio Hausdorff.

> **Proposición 1.3.1:**
Sean $(X_1, \tau_1)$ y $(X_2, \tau_2)$ dos espacios topológicos y $(x_1, x_2) \in X_1 \times X_2$.
Se verifica que $\mathcal{B}^{(x_1, x_2)} = \{V_1 \times V_2: V_1 \in \mathcal{U}^{x_1}, V_2 \in \mathcal{U}^{x_2}\}$ es una base de entornos de $(x_1, x_2)$.

Sean $(y_1, y_2), (z_1, z_2) \in X_1 \times X_2$ tales que $(y_1, y_2) \neq (z_1, z_2)$. Buscamos un entorno de cada pareja de puntos cuya intersección sea vacía.  

Supongamos $y_1 \neq z_1$. En caso contrario, se tendría $y_2 \neq z_2$ y razonaríamos de forma análoga usando que $X_2$ también es Hausdorff.   

Si $y_1 \neq z_1$, por ser $X_1$ Hausdorff, $\exists V \in \mathcal{U}^{y_1}$ y $\exists W \in \mathcal{U}^{z_1}$ tales que $V \cap W = \emptyset$.

Por la proposición 1.3.1:  

- Como $V \in \mathcal{U}^{y_1}$ y $X_2 \in \mathcal{U}^{y_2}$, entonces $V \times X_2 \in \mathcal{U}^{(y_1, y_2)}$.  

- Como $W \in \mathcal{U}^{z_1}$ y $X_2 \in \mathcal{U}^{z_2}$, entonces $W \times X_2 \in \mathcal{U}^{(z_1, z_2)}$.

(Por ser $X_2 \in \tau_2$, es entorno de todos sus puntos.)

Calculemos, entonces, la intersección de los dos entornos:
$$
(V \times X_2) \cap (W \times X_2) = (V \cap W) \times (X_2 \cap X_2) = \emptyset \times X_2 = \emptyset
$$

Hemos obtenido así $V \times X_2$ y $W \times X_2$, entornos de $(y_1, y_2)$ y $(z_1, z_2)$ respectivamente, cuya intersección es vacía, luego $X_1 \times X_2$ es Hausdorff.


**(4) Probar que un espacio topológico $(X, \tau)$ es Hausdorff si y sólo si la diagonal $\Delta = \{(x, x) \in X \times X \vert \ x \in X\}$ es un subconjunto cerrado de $(X \times X, \tau \times \tau)$**

<u> Condición necesaria: </u>

<u> Condición suficiente: </u>


---
## Ejercicio 2: 3 puntos
**Sea $X$ un conjunto y $A$ y $B$ subconjuntos no vacíos de $X$ cumpliendo $A \subset B$. Se define:**
$$
\tau = \{O \subset X \vert \ O \subset X - A\} \cup \{O \subset X \vert \ B \subset O\}
$$

**(1) Probar que $\tau$ es una topología en $X$**
1.  - $\emptyset \in \tau$:
      $\emptyset \in X - A \Rightarrow \emptyset \in \tau$
    - $X \in \tau$:
      $X$ verifica: $X \subset X$ y $B \subset X \Rightarrow X \in \tau$

2. $\{O_\lambda \vert \ \lambda \in \Lambda \} \subset \tau \Rightarrow \cup_{\lambda \in \Lambda}{O_\lambda} \in \tau$:
    - $\forall O_\lambda \in \{O \subset X \vert \ O \subset X - A\} \Rightarrow \cup_{\lambda \in \Lambda}{O_\lambda} \subset X - A \Rightarrow \cup_{\lambda \in \Lambda}{O_\lambda} \in \tau$
    - $\exists \lambda_0 \in \Lambda: B \subset O_{\lambda_0} \Rightarrow B \subset \cup_{\lambda \in \Lambda}{O_\lambda} \Rightarrow \cup_{\lambda \in \Lambda}{O_\lambda} \in \tau$

3.  $\{O_i \vert \ i = 1, ..., n\} \subset \tau \Rightarrow \cap_{i = 1}^{n}{O_i} \in \tau$
    - $\forall O_i \in \{O \subset X \vert \ B \subset O\} \Rightarrow B \subset \cap_{i = 1}^{n}{O_i} \in \tau$
    - $\exists i \in \{1, ..., n\}: O_i \in \{O \subset X \vert \ O \subset X - A\} \Rightarrow \cap_{i = 1}^{n}{O_i} \subset X - A \Rightarrow \cap_{i = 1}^{n}{O_i} \in \tau$

**(2) Calcular el interior, la adherencia y la frontera de los subconjuntos A y B**
SIN HACER

> **Definición 2.2.1:**
$Aº = \{x \in X \vert \ \exists V \in \mathcal{U}^x: V \subset A\}$
$\bar{A} = \{x \in X \vert \ \forall V \in \mathcal{U}^x \quad V \cap A \neq \emptyset\}$
$Fr(A) = \{x \in X \vert \ \forall V \in \mathcal{U}^x \quad V \cap A \neq \emptyset \ \wedge \ V \cap (X - A) \neq \emptyset\}$


**(3) Hallar una base de la topología $\tau$**
SIN HACER

> **Definición: base de una topología**
Sea $(X, \tau)$ un espacio topológico. Una base de la topología $\tau$ es una familia $\mathcal{B} \subset \tau$ tal que para todo abierto $O \in   \tau$ existe $\{B_i: i \in I\} \subset \mathcal{B}$ tal que
$$
O = \cup_{i \in I}{B_i}
$$
Es decir, la topología se genera de la base haciendo las posibles uniones de elementos de la misma.

> **Proposición: caracterización de la base de una topología**
Sea $(X, \tau)$ un espacio topológico. Una familia $\mathcal{B} \in \tau$ es una base de la topología $\tau$ si y sólo si para todo abierto $O \in \tau$ y todo punto $x \in O$ existe un $B \in \mathcal{B}$ con $x \in B \subset O$

**(4) Hallar una base de entornos y el sistema de entornos de un punto $x \in A$**
SIN HACER

> **Definición: entorno de un punto**
Sea $(X, \tau)$ un espacio topológico y $x\in X$. Un entorno de $x$ es un subconjunto $V \subset X$ cumpleniendo:
existe un abierto $O \in \tau$ tal que $x \in O \subset \tau$
Denotaremos $\mathcal{U}^x$ al sistema de entornos de $x$, esto es, al conjunto de los entornos de $x$.

> **Definición: base de entornos de un punto**
Sea $(X, \tau)$ un espacio topológico y $x\in X$. Una base de entornos del punto $x$ es es una familia $\mathcal{B}^x \subset \mathcal{U}^x$ con la propiedad:
para todo $V \in \mathcal{U}^x$ existe $W \in \mathcal{B}^x$ tal que $W \subset V$
Es claro que los entornos se construyen a partir de una base de la siguiente manera:
$\mathcal{U}^x = \{{V \subset X \vert \ \exists W \in \mathcal{B}^x: W \subset V\}}$
