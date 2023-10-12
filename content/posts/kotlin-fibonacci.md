---
math: true
---

I'm absolutely sure you've heard about the *Fibonacci sequence* and tried to 
tackle this task during at least one interview. What if I tell you this is a 
popular interview task and an excellent tool for learning a *new programming* 
language? If you doubt so, continue reading.

On the one hand, the Fibonacci sequence is a really *beginner-friendly* task. 
It adds the two preceding numbers to generate the next one: 0, 1, 1, 2, 3, 5, 
8, 13, 21, and so on. That's why you can quickly start learning all the basic 
statements of your new language, such as *loops, if-statements, variables, CLI input/output, and recursion*.

On the other hand, you can dive deeper and learn more advanced techniques such as 
*binary pow, matrix multiplication, code optimization, and algorithmic complexity*.

I've decided to take this task and look at *Kotlin* and took *matrix approach*.
Let's start from a small theory
So, everybody know that we can calculate the n-th Fibonacci number as a sum of
two previous:

$
f_n = f_{n-1} + f_{n-2}
$

but we can also use the matrix form:

$
\begin{pmatrix}
f_{n-2} & f_{n-1}
\end{pmatrix} * \begin{pmatrix}
a_{11} & a_{21} \newline a_{21} & a_{22}
\end{pmatrix} = \begin{pmatrix}
f_{n-1} \newline f_n
\end{pmatrix}
$

$
f_{n-2} * a_{11} + f_{n-1} * a_{21} = f_{n-1}\newline
f_{n-2} * a_{12} + f_{n-1} * a_{22} = f_{n}
$

so

$
a_{11} =  0, a_{21} = 1, a_{12} = 1, a_{22} = 1
$

or just

$
\begin{pmatrix}
0 & 1 \newline
1 & 1
\end{pmatrix}
$

Let's define a function called 'fibonacci' that takes 'N' integer as input 
and returns a Long.

```
fun fibonacci(n: Int): Long {

}
```
