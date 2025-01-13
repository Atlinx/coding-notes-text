# Master's Theorem
#algorithm 

This is a method of analyzing time complexity of recurrence relations for branch problems like "divide and conquer" algorithms.

$$
\huge \begin{aligned} \\
T(n) &= a T\left( \frac{n}{b} \right) + O(n^k) \\
T(1) &= c \\ \\

T(n) &\in \Theta(n^k) &\text{if } \log_{b} a < k \\
T(n) &\in \Theta(n^k\log n) &\text{if } \log_{b} a = k \\
T(n) &\in \Theta(n^{\log_{b} a}) &\text{if } \log_{b} a > k \\
\end{aligned}
$$
- $a$ = recursive calls per iteration
	- **Ex.** 2 = we make 2 separate recursive calls every iteration
- $b$ = sub-problem shrinkage factor
	- **Ex.** 2 = our sub-problem shrinks by half every iteration
- $k$ = polynomial factor that represents the cost to combine the work done by the sub-problems
	- **Ex.** 1 = it takes $O(n)$ time to merge the sub-problems together
	- **Ex.** 2 = it takes $O(n^2)$ time to merge the sub-problems together

Explanation
- If $\log_{b} a < k$
	- The work required to merge sub-problems dominates, therefore runtime is $O(n^k)$
- If $\log_{b} a = k$
	- The work required to merge sub-problems is equal to the work of sub-problems, therefore both runtimes are equally valid
	- $n^k \log n$
- If $\log_{b} a > k$
	- The work of sub-problems dominates, therefore runtime is $O(n^{\log_{b} a})$

# Flashcards
#flashcards/algorithms 

Master's theorem
?
- $$\huge \begin{aligned} \\T(n) &= a T\left( \frac{n}{b} \right) + O(n^k) \\T(1) &= c \\ \\T(n) &\in \Theta(n^k) &\text{if } \log_{b} a < k \\T(n) &\in \Theta(n^k\log n) &\text{if } \log_{b} a = k \\T(n) &\in \Theta(n^{\log_{b} a}) &\text{if } \log_{b} a > k \\\end{aligned}$$
- Recurrence relation
- $a$ = recursive calls per iteration
- $b$ = sub-problem shrinkage factor
- $k$ = polynomial factor that represents the cost to combine the work done by the sub-problems
<!--SR:!2025-01-17,5,230-->