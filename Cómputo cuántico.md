# Proyecto final

# Red Neuronal Cuántica (QNN)

**Eduardo Solano Jaime**

**Paulina Barba Mendoza**

## Cómputo cuántico

### Postulados

Tal como el cómputo clásico se describe y analliza con álgebra booleana clásica, el cómputo cuántico se razona con mecánica cuántica (qué es solo un lenguaje matemático). Y este tiene cuatro postulados:

1. La definición del bit cuántico (qubit)

2. La transformación (evolución) de qubits

3. Efecto de la medición

4. Sistemas de multiples qubits

#### Postulado 1: Qubit

> Todo sistema físico aislado tiene asociado a sí mismo un espacio vectorial complejo normalizado perteneciente al espacio de Hilbert conocido como *vector de estado*. 

Este vector estado se puede representar de dos formas que son un espacios vectorial dual:

- Ket: 
  
              $|\psi \rang = 
  \begin{pmatrix}
  C_1 \\
  C_2 \\
  \vdots
  \end{pmatrix} = 
  \begin{pmatrix}
  u_1 \langle \psi | \\
  u_2 \langle \psi | \\
  \vdots
  \end{pmatrix}$

- Bra: 
  
               $\lang \psi| = (C_1^*, C_2^*, \ldots) = (\lang \psi|u_1 \rang, \lang \psi|u_2 \rang, \ldots)$

***Nota*** -> *Estos espacios son solo descripciones, no formas concretras. Por lo que todos los kets de la forma $e^\theta|\psi \rang$ representan al mismo estado vectorial para todo $\theta$*

Por la definición del qubit, $\lang \psi|$ debe ser un vector unitario por lo que su producto escalar es la unidad $\braket{\psi|\psi} = 1 \therefore |a|^2+|b|^2=1$ dado ${a,b} \in \mathbb{C}$. (esto es relevante para comprender el poder de transformar problemas al espacio cuántico, pues decribe la **superposición**). Así elqubitt se define como un sistema de dos vectores estado: $\lang \psi| = a\lang \phi_1| + b\lang \phi_2|$ 

Y para simplificar $\lang \phi_1| = \lang 0|$ y $\lang \phi_2| = \lang 1|$ representando *upsin* y *downspin* respectivamente. Aunque realmente sea una abstraccion del fenómeno físico entre la propiedad del espín y la lógica de la mecánica cuántica (como pasa con el voltaje positivo y negativo en la lógica booleana).

De una manera más elegante, podemos represantar el qubit en coordenadas polares, transformando el espacio vectorial en una **esfera de Bloch**. Terminando con un vector 

               $|\psi\rang=cos\frac{\theta}{2}|0\rang+e^{i\phi}sin\frac{\theta}{2}|1\rang$

Donde $\theta$ es el ángulo entre el eje Z y Y, y $\phi$ es el ángulo entre X y Y. Esto facilita la interpretación de algunas compuertas lógicas.

          <img src="https://i.stack.imgur.com/9CpXB.png" title="" alt="Bloch Sphere" data-align="center">

#### Postulado 2: Evolución de qubits

> La transformación de un sistema cuánticop cerrado del tiempo $t_1$ al tiempo $t_2$ se describe con una transformación unitaria

Un estado $|\psi \rang$ del sistema en $t_1$ está relacionado con el estado $|\psi' \rang$ en el tiempo $t_2$ por un operador unitario $U$ que depende solo de $t_1$ y $t_2$. Se puede ver com una "función" para simplificar: $|\psi' \rang = U|\psi\rang$

La transformación $U$ (tristemente) ocurre independientemente del estado de $|\psi\rang$.

Ejemplo:

            $|\psi\rangle = 1|0\rang + 0|1\rang = |0\rang$

             $U = \frac{1}{\sqrt{2}} \begin{bmatrix} 1 & 1 \\ 1 & -1 \end{bmatrix}$
            $U|\psi\rang = \frac{1}{\sqrt{2}} \begin{bmatrix} 1 \\ 0 \end{bmatrix} + \frac{1}{\sqrt{2}} \begin{bmatrix} 1 \\ 1 \end{bmatrix} = \frac{|0\rang + |1\rang}{\sqrt {2}}$

Teniendo en cuenta que $U$ debe ser unitaria: $UU^†=I$

#### Posrtulado 3: Medición

> Las mediciones cuánticas se describen como la colección $\{M_m\}$ de operadores de medición operando en el estado de espacio del sistema. El subíndice $_m$ se refiere a los posibles resultados del experimento

Si el estado de un sistema cuántico es $|\psi\rang$ justo antes de ser medido, entonces la probabilidad de $m$ se da por $p(m)=\lang\psi|M_m^\dagger M_m|\psi\rang$ $^{[1]}$ y el sistema de estado después de la medición es 

             $\frac{M_m | \psi \rangle}{\sqrt{\langle \psi | M_m^\dagger M_m | \psi \rangle}}$ $^{[2]}$ 

Siempre y cuando los operadores de medición cumplan con

             $\sum_m \lang\psi|M_m^\dagger M_m|\psi\rang=I$ 

expresando asi que la suma de las probabilidades del sistema es la unidad.

Los operadores de medición más importantes son $M_0=|0\rang\lang0|$ y $M_1=|1\rang\lang1|$ , es decir 

            $M_0=\begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$ $ M_1=\begin{bmatrix} 0 & 0 \\ 0 & 1 \end{bmatrix}$

#### Postulado 4: Sistemas de multiples qubits

> El estado de espacio de un sistema físico cuántico compuesto es el producto tensorial de los sistemas independientes

Esta no es una prueba de la existencia del sistema, sino una descripción de este. El producto tensorial captura la esencia de la superposición en sistemas cuánticos.

Teniendo $|\psi_1\rang=a|0\rang+b|1\rang$ y $|\psi_2\rang=c|0\rang+d|1\rang$, el sistema compuesto se describe como:

        $|\psi_1\rang\otimes|\psi_2\rang=|\psi_1\psi_2\rang=a\cdot c|0\rang|0\rang + a\cdot d|0\rang|1\rang +b\cdot c|1\rang|0\rang + b\cdot d|1\rang|1\rang$

                               $= ac|00\rang + ad|010\rang + bc|10\rang + bd|11\rang$

### Compuertas lógicas cuánticas (1 qubit)

Ántes de continuar, es necesario mencionar las principales compuertas lógicas (operadores) fundamentales para construir circuitos cuánticos y realizar operaciones sobre qubits en computación cuántica. Estas permiten manipular estados de qubits de forma controlada y determinista. Algunas de las puertas más fundamentales son:

- **Identidad**: Mantiene el estado. Su matriz es la matriz identidad $\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$

- **Puerta Hadamard (H)**: Esta puerta coloca un qubit en una superposición de los estados |0⟩ y |1⟩. Su matriz es: $\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$

- **Compuerta X de Pauli (NOT):** Similar a la puerta clásica NOT, invierte el estado del qubit. Su matriz es: $\begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$

- **Compuerta Y de Pauli:** Una combinación de las puertas Z y X, aplicando una fase y una inversión. Su matriz es: $\begin{bmatrix} 0 & -i \\ i & 0 \end{bmatrix}$

- **Compuerta Z de Pauli:** Aplica uncambiio de fase al qubit. Su matriz es: $\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$

- **Puerta de fase (σ o S):** Agrega una fase de $\frac{\pi}{2}$. Su matriz es: $\begin{bmatrix} 1 & 0 \\ 0 & i \end{bmatrix}$ 

- **Puerta T:** Aplica una fase de $\frac{\pi}{4}$. Su matriz es: $\begin{bmatrix} 1 & 0 \\ 0 & e^{i\frac{\pi}{4}} \end{bmatrix}$

### Entrelazamiento

Es una propiedad intrínseca de los sistemas cuánticos compuestos y se usa en nuestro favor en el cómputo cuántico. Este afirma que los estados cuánticos del sistema deben de ser descritos mediante un solo estado, aun cuando físicamente los estados no esten juntos. La propiedad, en resuumen, nos impiode descomponer un sistema compuesto en los sistemas de los componentes.

Se explica mejor con un ejemplo al crear y destruir un EPR (un par de qubits en nombre a Einstein-Podolsky-Rosen):

Teniendo un qubit $|\psi_1\rang$ en estado $|0\rang$ y un operador de Hadamard: 

            $U=H=\frac{1}{\sqrt2}\begin{bmatrix}1 & 1 \\ 1 & -1 \end{bmatrix}$ 

Tenemos entonces $|\psi_1'\rang = \frac{1}{\sqrt2}(|0\rang+|1\rang)$. 

Y un espacio estado conjunto con otro qubit inicial idéntico sería 

            $|\psi_1'\rang \otimes|\psi_2\rang=\frac{|00\rang+|10\rang}{\sqrt2}+0|01\rang+0|11\rang$

Ahora definimos la transformación unitaria (importante **compuerta lógica** cuántica): 

            $CNot = \begin{bmatrix}1&0&0&0 \\ 0&1&0&0 \\ 0&0&0&1 \\0&0&1&0 \end{bmatrix}$

Así, aplicando el **CNot** a nuestro sistema de dos qubits (y omitiendo el desarrollo) tenemos que podemos describir el sistema entrelazado de la forma 

            $CNot|\psi_1'\psi_2\rang=\frac{1}{\sqrt2}(|00\rang+|11\rang)$

Lo extraño de este fenómeno es que la superposición de los qubits entrelazados queda entrelazada de igualmente. Es decir, que al medir un qubit del sistema despues de aplicar el CNot, se afecta la amplitud de probabilidad del otroqubitt. Esto se puede demostrar de forma mátemática (*Oskin, M. (2023). Quantum Mechanics*) aplicando las fórmulas $^{[1]}$ y $^{[2]}$ ántes y después de aplicar el CNot.

### Operadores comunes para un sistema de 2 qubits

Para un sistema de 2 qubits, además de las compuertas cuánticas individuales, existen operadores importantes que actúan sobre ambos qubits a la vez. Algunos de los operadores más importantes son:

- **Operador de identidad (I):** Este operador deja los estados de los qubits sin cambios. Para un sistema de 2 qubits, la matriz del operador de identidad es
  
          $I= \begin{bmatrix}1&0&0&0 \\ 0&1&0&0 \\ 0&0&1&0 \\0&0&0&1 \end{bmatrix}$

- **Operador de control-NOT (CNOT):** Este previamente mencionado operador aplica una compuerta NOT controlada por otro qubit. La matriz del operador CNOT es:
  
          $CNot = \begin{bmatrix}1&0&0&0 \\ 0&1&0&0 \\ 0&0&0&1 \\0&0&1&0 \end{bmatrix}$

- **Operador de intercambio (SWAP):** Este operador intercambia los estados de dos qubits. Para un sistema de 2 qubits, la matriz del operador SWAP es:
  
          $Swap= \begin{bmatrix}1&0&0&0 \\ 0&0&1&0 \\ 0&1&0&0 \\0&0&0&1 \end{bmatrix}$

- **Operador de superposición (S):** Este operador se utiliza para crear superposiciones de estados de qubits. Para un sistema de 2 qubits, la matriz del operador S es:
  
          $S= \frac{1}{\sqrt2}\begin{bmatrix}1&0&0&1 \\ 0&1&-1&0 \\ 0&1&1&0 \\1&0&0&-1 \end{bmatrix}$

### Otras propiedades

Existen otras propipedades intrínsecas de la mecánica cuántica que son tanto útiles como perjudiciales para el rendimiento de los circuitos cuánticos. Tales son la teleportación y la encodificación super densa (que ayudan a este avance tecnológico), y la decoherencia (que brinda pérdida de reversivilidad en los circuitos).

Si bien, todos ellos son temas fascinantes e increiblemente complejos, estám fuera del alcance de esta pequeña introducción y de las necesidades del proyecto.

## Circuitos Cuánticos

Conociendo las bases de la mecánica cuántica podemos ver el elemento fundamental del cómputo cuántico: los circuítos cuánticos.

Podemos definir los circuitos como rutinas de operaciones cuánticas coherentes en información cuántica (qubits). Se compone de compuertas, mediciones, y resets acomodados de forma secuancial. 

En el siguiente ejemplo podemos ver el estado $|\psi\rang=\frac{|000\rang+|111\rang}{\sqrt2}$, como un sistema de 3 qubits donde el primero es puesto en superposición por la compuerda de Hadamard y es posteriormente entrelazado a los otros dos qubits con compuertas CNot:

<img src="file:///D:/Documentos/ECID/Deep%20Learning/Quantum/circuit-1.png" title="" alt="psi" data-align="center">

Para colapsar las probabilidades de los circuitos, usamos la propiedad de medición, que se representa de la siguiente manera ($q_0$ es un qubit y $c_0$ un bit clásico):              <img src="file:///D:/Documentos/ECID/Deep%20Learning/Quantum/circuit-2.png" title="" alt="Medir" data-align="center">

## Algoritmos variacionales

Para mejorar los dispositivos NISQ$^1$ se lelvo a la creacion de los VQA que adoptan el modelo de las redes neuronales para encontrar los parámetros ideales para minimizar/maximizar la función objetivo del circuito cuántico y poder así competir con computadoras clásicas. Funcionan con 4 puntos clave:

1. Estructura de circuito cuántico: el comportamiento del algoritmo se describe con la construcción del circuito cuántico usando las compuertas previamente mencionadas.

2. Parametrizacion de compuertas: Se usan parámetros numéricos para controlar el comportaiento de las compuertas. Un circuito parametrizado se conoce como **Anzats**.

3. Optimización: Para maximizar/minimizar la función objetivo se trata el circuito variacional como una función clásica y se usa oprimización clásica para ajustar los parámetros.

4. Retroalimentación cuántica-clásica: La computadora clásica evalua la funcion (la medición al final del circuito) y actualiza los parámetros. 

> $^1$: Cuántica  de escala intermedia ruidosa son los dispositivos no tan avanzados para resistir fallos ni tan grandes como para alcanzar la supremacia cuántica (usar computo cuántico para solucionar problemas que las computadoras clásicas no pueden)

#### Ansatz

Un circuito parametrizado se conoce como **Anzats**. Es decir, circuitos donde sus compuertas son definidas mediante parámetros tuneables. La forma más sencilla de parametrizar es rotando los qubits en el eje Z de la esfera de Bloch usando la compuerta de rotacion Z

            $R_z(\theta)=\begin{bmatrix} e^{-i\frac{\theta}{2}} &&0 \\ 0 && e^{i\frac{\theta}{2}} \end{bmatrix}$

Donde puede haber una $\theta$ por cada compuerta a parametrizar, es decir, un vector de valores de rotación ($\theta$) con los cuales el algoritmo de optimización puede realizar operaciones de ML o NN clásicas para resolver el circuito cuántico. Se parametriza, por ejemplo, de la siguiente manera. Aunque hay más formas y más complejas.

<img src="file:///D:/Documentos/ECID/Deep%20Learning/Quantum/circuit-3.png" title="" alt="Rz" data-align="center">

##### Métodos de parametrización

Generalmente se usa un solo método de parametrización: el circuito de expresión de Pauli. Este método usa las compuertas de Pauli y el entrelazamiento de todos los qubits del sistema. Este método se usa en su forma básica:

               $U_{\phi(x)} = \exp \left( i \sum_{s \in I} \phi_s(x) \prod_{i \in S} P_i \right)$

Su desarrollo matemático y explicación a profundidad no entra en el alcance de esta investigación, sin embargo, es importante recalcar que este método puede ser utilizado forzando solo el uso de compuertas Y o Z de Pauli, causando efectos conocidos como Interacciones Y, Z, y ZZ.

#### Codificación de información clásica

A menos que nuestro problema sea cuántico por naturaleza, generalmente tenemos que transformar infromación clásica (bits) a informacion cuántica (qubits). Esto se realiza con la codificacion de información o mejor conocieda como **Feature Mapping**. 

Teniendo $N$ features y $M$ registros en nuestro dataset $X=\{x^1, \dots, x^M\}$ dónde $ x\in\mathbb{N}^N$, existen numerosas formas de *traducir* la información a qubit:

- Básico: asociar un bitstring a un estado cuántico de longitud $N$ a cada feature. Es decir, si un registro $x=6$, podemos mapear el dato a su representacion binaria $110_2$ y convertirlo en un estado $|110\rang$.

- De ángulo: codificar los valores de un registro como rotaciones de n qubits (siempre que $N\leq n$). Por lo que $|X\rang = \bigoplus_{i=1}^{N} \cos(x_i)|0\rang + \sin(x_i)|1\rang$

- Otro método popular es Pauli Two Design, que alterna capas de rotación y de entrelazamiento con una capa inicial $\sqrt{H}=R_Y(\frac{\pi}{4})$ y las capas de rotacion consisten en compuertas de Pauli elegidas de forma aleatoria uniformemente.

## Bibliografía

> Oskin, M. (2023). Quantum Computing - Lecture Notes. Department of Computer Science and Engineering, University of Washington.

> Ekert, P. (2021). Introduction to Quantum Information Science [Playlist de YouTube]. Recuperado de https://www.youtube.com/playlist?list=PLkespgaZN4gmu0nWNmfMflVRqw0VPkCGH. Visitado el 1$^{ro}$ de abril de 2024.

> IBM Quantum. (n.d.). Qiskit: Open-source quantum computing software development framework for quantum physicists and researchers. Recuperado de https://docs.quantum.ibm.com/api/qiskit/circuit el 3 de abril de 2024.

> Quantum Computing Group, IIT Roorkee. (2023). Variational Quantum Algorithms. Recuperado de https://medium.com/@qcgiitr/variational-quantum-algorithms-66367053a2f3 el 3 de abril de 2024.

> Qiskit Ecosystem. Qiskit Machine Learning 0.7.2. IBM 2017 (2024).  Recuperado de https://qiskit-community.github.io/qiskit-machine-learning/index.html el 5 de abril del 2024.
