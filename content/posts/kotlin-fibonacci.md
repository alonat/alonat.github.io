---
title: "Fibonacci"
date: 2023-10-12T12:38:55+02:00
draft: false
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

but we can also use the matrix form which uses multiplication instead of addition:

$
\begin{bmatrix}
f_{n-1} \newline f_n
\end{bmatrix} =
\begin{bmatrix}
f_{n-2} \newline f_{n-1}
\end{bmatrix} * \begin{bmatrix}
0 & 1 \newline 1 & 1
\end{bmatrix}
$

so, using the associative property of the multiplication we get

$
\begin{bmatrix}
f_{n} \newline f_{n+1}
\end{bmatrix} =
\begin{bmatrix}
0 \newline 1
\end{bmatrix} * \begin{bmatrix}
0 & 1 \newline 1 & 1
\end{bmatrix} ^ {n}
$

and this gives us an ability to calculate Fibonacci number with $O(log n)$ complexity (using binpow algorithm)

Time to dive into coding! To store our matrices, Kotlin provides a nifty tool - data classes. According to the
documentation, data classes are designed specifically to hold data. This makes them a perfect fit for our needs.
```kotlin
data class Matrix (val matrix: Array<Array<Long>>) {
    private fun countN() = matrix.count()
    private fun countM() = matrix.first().count()

    fun raw() = matrix
}
```

And now, we're all set to initialize our starting matrix by declaring a variable of our new data class, Matrix
```kotlin
val initial = Matrix(arrayOf(arrayOf(0L, 1L)))
```

and the other matrix which we'll raise to the power of N
```kotlin
val complement = Matrix(
    arrayOf(
        arrayOf(0L, 1L),
        arrayOf(1L, 1L)
    )
)
```

Kotlin provides us with a powerful capability - operator overloading, and we can use this feature for our data class
Matrix. At its core, this entails defining a `times` function within our Matrix class. This `times` function essentially
acts as a custom handler for the multiplication operator `*`. In essence, when we write `A * B`, it is equivalent to
`A.times(B)` thanks to this operator overloading. Now, let's delve into the nitty-gritty of implementing this feature.
In the context of our Matrix class, this 'times' function serves as the workhorse, responsible for matrix multiplication.
It performs dimension checks to ensure compatibility and then employs a systematic process to compute the results
efficiently.
```kotlin
operator fun times(secondMatrix: Matrix): Matrix {
    val row1 = this.countN()
    val col1 = this.countM()
    val col2 = secondMatrix.countM()

    assert(row1 == col2)

    var resultArray = arrayOf<Array<Long>>()

    for (i in 0 until row1) {
        var lineArr = arrayOf<Long>()
        for (j in 0 until col2) {
            var item = 0L
            for (k in 0 until col1) {
                item += (matrix[i][k] % mod * secondMatrix.raw()[k][j] % mod) % mod
            }
            lineArr += item
        }
        resultArray += lineArr
    }

    return Matrix(resultArray)
}
```

Let's define a function called `fibonacci` that takes `n` integer as input 
and returns a Long.

```kotlin
fun fibonacci(n: Int): Long {

}
```
