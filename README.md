# Operator-Series-Calculator
The Ludic Numbers are a sequence described by another sequence of real values between 0 and 1, which I call the operator sequence. This Python program finds that operator sequence for arbitrary strictly increasing integer sequences (abbreviated to SIIS).

NOTATION:
Let $\lfloor n \rfloor$ denote the floor function on $n$, $\lceil n \rceil$ denote the ceiling function on $n$, and $\omega$ a number that satisfies
```math 
\frac{n}{n+1} \leq w < 1
```
for all positive integers $n$. Let $a(n)$ denote a strictly increasing integer sequence. This is defined as $a(n)$ being entirely composed of integers and having the property that for all positive integers $n$, $a(n) < a(n+1)$.

THEORY:
Given $a(n)$, an SIIS, we wish to find a real value $\beta$ such that $\lfloor \beta a(n) \rfloor$ is injective in terms of $n$ (i.e. $\lfloor \beta a(n) \rfloor \not = \lfloor \beta a(n+1) \rfloor$ for any $n$). Specifically in this context, we also request that $\lceil \frac{1}{\beta} \lfloor \beta a(n) \rfloor \rceil$ for all $n$, that is that we can reverse the floor function. The injective condition demands that if $\lfloor \beta a(n) \rfloor =b(n)$ then
```math
\frac{b(n)}{a(n)} \leq \beta < \frac{b(n)+1}{a(n)}
```
while the reversable condition demands that
```math
\frac{b(n)}{a(n)} \leq \beta < \frac{b(n)}{a(n)-1}.
```
Since for any $b(n)$, we have $b(n) < a(n)$ and so $b(n)/(a(n)-1) \leq (b(n)+1)/a(n)$, we can have both properties if we simply satisfy the reversable condition. All that remains is to determine the values we map $a(n)$ to. This is the purpose of the program.

ALGORITHM:
We are given a(n), a finite list from an SIIS.
1. Store a(0)-1 as $\lambda$. Subtract this value from each element in a(n). This results in the first element of a(n) now being equal to 1.
2. For any $\beta$ we choose, 1 maps to 0 so we can safely say that the bounds for b are 0 and 1, noninclusive. Store as currentLower and currentUpper respectively. We also designate the current_index = 0. Append 0 to the list of mappings of a(n). Call this list MAP.
3. (WHILE LOOP STARTED) Iterate the current_index and multiply currentLower and currentUpper by a(current_index). Round up both of these products and subtract 1 from the modified upper bound.
4. a(current_index) must map to a positive integer in this range (inclusive) in order for the current bounds for b to be satisfied. Append the least integer to MAP decided by the currentLower rounding.
5. If this integer is greater than the currentUpper rounding, pop the last element of MAP and iterate the new last element of MAP. Deiterate current_index.
6. Find the bounds given by this new mapping and call them newLower and newUpper.
7. If currentLower < newLower, replace. If currentUpper > newUpper, replace. If the length of MAP is equal to the length of a(n), then we end the algorithm. Otherwise, go to step 3.

This algorithm gives an SIIS list with the first element being a 0. To continue to find $\beta$ values, remove this 0 and start again. Don't forget to store the $\lambda$'s somewhere.

NOTES:
Of note is that in order for _every_ SIIS to be expressed in terms of an operator sequence and the $\lambda$'s obtained, we need to consider the situation where no $\beta$ in the range of 0 and 1 is reversable for all $a(n)$. A prime example is when $a(n)=n$. Specifically, in this case, we are forced to find a $\beta$ such that
```math
\frac{n}{n+1} \leq \beta < 1
```
for all integers $n$. Since $n/(n+1)$ converges on 1 as $n$ tends to infinity, we are stuck unless we can get a bit creative. We can increase the current $\lambda$ so that we subtract one extra from $a(n)$ and get the 0 we need (see ALGORITHM) or, my preference, we define a number $\omega$ that has this exact property (this convention is used in the Python program). Either way, these "trivial" values are usually noticed by the upper bound being 1 in the program. This is not a perfect test however.
Given either convention, there are plenty of SIIS's which can only be described with an operator sequence that, after a certain point, is strictly $\omega$. These "trivial" sequences are surprisingly common (I conjecture that given an arbitrary SIIS, there is a 50/50 chance that that sequence is Trivial). Currently, I am looking for operations between Trivial sequences that preserves triviality. For instance, $a(n)=n^k$ for any positive integer $k$ is trivial since $a(n)=n$ is trivial. However, multiplication in general does not preserve triviality.
