## Problems

### 15-1 Longest simple path in a directed acyclic graph

> Suppose that we are given a directed acyclic graph $$G = (V, E)$$ with real-valued edge weights and two distinguished vertices $$s$$ and $$t$$ . Describe a dynamic-programming approach for finding a longest weighted simple path from $$s$$ to $$t$$ . What does the subproblem graph look like? What is the efficiency of your algorithm?

Topological sort.

### 15-2 Longest palindrome subsequence

> A __*palindrome*__ is a nonempty string over some alphabet that reads the same forward and backward. Examples of palindromes are all strings of length 1, civic, racecar, and aibohphobia (fear of palindromes). 
> Give an efficient algorithm to find the longest palindrome that is a subsequence of a given input string. For example, given the input character, your algorithm should return carac. What is the running time of your algorithm?

LCS of the original string and the reversed string.

### 15-3 Bitonic euclidean traveling-salesman problem

> In the __*euclidean traveling-salesman problem*__, we are given a set of n points in the plane, and we wish to find the shortest closed tour that connects all n points. Figure 15.11(a) shows the solution to a 7-point problem. The general problem is NP-hard, and its solution is therefore believed to require more than polynomial time (see Chapter 34).
J. L. Bentley has suggested that we simplify the problem by restricting our attention to __*bitonic tours*__, that is, tours that start at the leftmost point, go strictly rightward to the rightmost point, and then go strictly leftward back to the starting point. Figure 15.11(b) shows the shortest bitonic tour of the same 7 points. In this case, a polynomial-time algorithm is possible.
Describe an $$O(n^2)$$-time algorithm for determining an optimal bitonic tour. You may assume that no two points have the same $$x$$-coordinate and that all operations on real numbers take unit time.

Sort the points by their $$x$$-coordinates, and suppose there are $$n$$ points. Let $$dp[i][j]$$ be the minimal distance from $$i$$ to the first point and from the first point to $$j$$. Since $$dp[i][j]$$ is symmetric, suppose that $$j < i$$, then $$\displaystyle \min_{j} dp[j][n]$$ is the shortest bitnoic tour. If $$j=i-1$$, $$\displaystyle dp[i][j]=\min_{k=1}^{i-2} dp[i-1][k] + dist(k, i)$$; if $$j<i-1$$, $$dp[i][j] = dp[i-1][j] + dist(i - 1, i)$$.

### 15-4 Printing neatly

> Consider the problem of neatly printing a paragraph with a monospaced font (all characters having the same width) on a printer. The input text is a sequence of $$n$$ words of lengths $$l_1, l_2, \dots , l_n$$, measured in characters. We want to print this paragraph neatly on a number of lines that hold a maximum of $$M$$ characters each. Our criterion of "neatness" is as follows. If a given line contains words $$i$$ through $$j$$, where $$i \le j$$ , and we leave exactly one space between words, the number of extra space characters at the end of the line is $$M - j + i - \sum_{k=i}^j l_k$$, which must be nonnegative so that the words fit on the line. We wish to minimize the sum, over all lines except the last, of the cubes of the numbers of extra space characters at the ends of lines. Give a dynamic-programming algorithm to print a paragraph of $$n$$ words neatly on a printer. Analyze the running time and space requirements of your algorithm.

Let $$dp[m]$$ be the minimal sum when we finshed printing $$m$$ words. 
$$
dp[m] = dp[m-k] + cost(m-k+1, m)
$$$$
cost(i, j) = \left \{ 
\begin{matrix}
\infty & \text{if}~M - j + i - \sum_{k=i}^j l_k < 0 \\
0 & \text{if}~j = n\\
\left ( M - j + i - \sum_{k=i}^j l_k \right ) ^ 3 & \text{otherwise}
\end{matrix}
\right .
$$
### 15-5 Edit distance

> In order to transform one source string of text $$x[1 \dots m]$$ to a target string $$y[1 \dots n]$$, we can perform various transformation operations. Our goal is, given $$x$$ and $$y$$, to produce a series of transformations that change $$x$$ to $$y$$. We use an array $$z$$—assumed to be large enough to hold all the characters it will need—to hold the intermediate results. Initially, $$z$$ is empty, and at termination, we should have $$z[j] = y[j]$$ for $$j = 1,2, \dots ,n$$. We maintain current indices $$i$$ into $$x$$ and $$j$$ into $$z$$, and the operations are allowed to alter $$z$$ and these indices. Initially, $$i = j = 1$$. We are required to examine every character in $$x$$ during the transformation, which means that at the end of the sequence of transformation operations, we must have $$i = m + 1$$.

> We may choose from among six transformation operations:

> __Copy__ a character from $$x$$ to $$z$$ by setting $$z[j]=x[i]$$ and then incrementing both $$i$$ and $$j$$. This operation examines $$x[i]$$.

> __Replace__ a character from $$x$$ by another character $$c$$, by setting $$z[j] = c$$, and then incrementing both $$i$$ and $$j$$. This operation examines $$x[i]$$.

> __Delete__ a character from $$x$$ by incrementing $$i$$ but leaving $$j$$ alone. This operation examines $$x[i]$$.

> __Insert__ the character $$c$$ into $$z$$ by setting $$z[j] = c$$ and then incrementing $$j$$, but leaving $$i$$ alone. This operation examines no characters of $$x$$.

> __Twiddle__ (i.e., exchange) the next two characters by copying them from $$x$$ to $$z$$ but in the opposite order; we do so by setting $$z[j] = x[i+1]$$ and $$z[j+1] = x[i]$$ and then setting $$i=i+2$$ and $$j=j+2$$. This operation examines $$x[i]$$ and $$x[i+1]$$.

> __Kill__ the remainder of $$x$$ by setting $$i=m+1$$. This operation examines all characters in $$x$$ that have not yet been examined. This operation, if performed, must be the final operation.

> __*a*__. Given two sequences $$x[1 \dots m]$$ and $$y[1 \dots n]$$ and set of transformation-operation costs, the __*edit distance*__ from $$x$$ to $$y$$ is the cost of the least expensive operatoin sequence that transforms $$x$$ to $$y$$. Describe a dynamic-programming algorithm that finds the edit distance from $$x[1 \dots m]$$ to $$y[1 \dots n]$$ and prints an optimal opeartion sequence. Analyze the running time and space requirements of your algorithm.

* __Copy__: $$dp[i][j] = dp[i - 1][j - 1] + cost(copy)$$ if $$x[i] = y[j]$$. 
* __Replace__: $$dp[i][j] = dp[i-1][j-1] + cost(replace)$$ if $$x[i] \ne y[j]$$.
* __Delete__: $$dp[i][j] = dp[i-1][j] + cost(delete)$$.
* __Insert__: $$dp[i][j] = dp[i][j-1] + cost(insert)$$.
* __Twiddle__: $$dp[i][j] = dp[i-2][j-2] + cost(twiddle)$$ if $$x[i-1] = y[j]$$ and $$x[i] = y[j-1]$$.
* __Kill__: $$dp[i][j] = \min_{k=1}^{i} dp[k][j] + cost(kill)$$ if $$j = n$$.

> The edit-distance problem generalizes the problem of aligning two DNA sequences (see, for example, Setubal and Meidanis [310, Section 3.2]). There are several methods for measuring the similarity of two DNA sequences by aligning them. One such method to align two sequences $$x$$ and $$y$$ consists of inserting spaces at arbitrary locations in the two sequences (including at either end) so that the resulting sequences $$x'$$ and $$y'$$ have the same length but do not have a space in the same position (i.e., for no position $$j$$ are both $$x'[j]$$ and $$y'[j]$$ a space). Then we assign a "score" to each position. Position $$j$$ receives a score as follows:
> * +1 if $$x'[j] = y'[j]$$ and neither is a space,
> * -1 if $$x'[j] \ne y'[j]$$ and neither is a space,
> * -2 if eigher $$x'[j]$$ or $$y'[j]$$ is a space.

> __*b*__. Explain how to cast the problem of finding an optimal alignment as an edit distance problem using a subset of the transformation operations copy, replace, delete, insert, twiddle, and kill.

$$cost(copy) = +1$$, $$cost(replace)=-1$$, $$cost(delete)=cost(insert)=1$$.