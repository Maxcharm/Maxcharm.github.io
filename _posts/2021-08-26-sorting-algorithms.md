---

layout: post
title:  "Sorting Algorithms and Analyses"
date:   2021-08-26 14:05:21 +0800
tags: basic algorithms, theoretical analysis
color: rgb(255,90,90)
cover: '../assets/test.png'
subtitle: 'Insertion sort, merge sort and quick sort'

---

Sorting is one of the most fundamental problems in the study of algorithms. Many complicated methods consists of sorting as an important subprocess, which encourages the research on various sorting algorithms, as well as the analyses of them.  Besides, there is a theoretical lower bounds for all sorting techniques asymptotically, which also enables us to prove the lower bounds for other problems which take sorting as a subroutine.

Before reading the passage, it is recommended that you give a thought to the following problems:

- What are some common sorting algorithms? How can we apply these algorithms to different applications?
- What is sorting exactly? Namely, what it the **nature** of sorting?
- How can we find a lower bound for all sorting algorithms based on the nature of sorting?

It would be wonderful if you have some vague answers for the questions above, but it is also all right if you have no idea what they mean, for you would probably know the answers to all of them after reading this passage.

Here is a formal definition of sorting problem before we move on to the first question. 

**Input**:  A sequence of numbers $S = <a_1, a_2,...,a_n>$

**Output**: An ordered sequence $S' = <a_1',a_2',...,a_n'>$, where the elements are a permutation of the input sequence $S$, and $a_1'\leq a_2'\leq...\leq a_n'$. 

For example, given an input of $3, 4, -2, 0,7,1$, the algorithm will sort the sequence into $-2, 0, 1,3,4,7$, in an ascending order.



### I. Important Sorting Algorithms



**Insertion Sort**

First, let's start with insertion sort, an easy algorithm which we often use in sorting poker cards unconsciously. Suppose that we are given a hand of pokers, what we normally do to rearrange them is to find **a suitable place for each card from the leftmost to the rightmost one**.

This simple strategy we adopt here actually contains two iterations, one nested in the other. The inner iteration is the process of finding the right place for a single card, and the outer iteration is the repetition of the nested inner iteration for every card. Below is the pseudocode for this algorithm:

```python
Insertion Sort (S)
1. for i = 2 to S.len: //repeat this inner circle for every number
2. 			key = S[i] //select the current number as the key
3. 			j = i - 1
4. 			while j > 0 and S[j] > key:
5. 					S[j + 1] = S[j] //swap the greater number to its next place
6. 					j = j - 1 //proceed with the number on the left until it is in place
7. 			S[j + 1] = key //put the key number in the correct position
```

Starting with the second number in the sequence, we first select the value $S[i]$ of the current position $i$ as the key (line 2), and try to rearrange the subsequence $S[1, 2,..., i]$ by inserting $S[i]$ into the already sorted sequence $S[1,2,...,i-1]$. In order to do so, we scan every number $S[j]$ on the left side of position $i$ (line 4), if the number is greater than the key, we move it one place to the right (line 5), and then proceed with one number to the left (line 6) until the value is no larger than the key. Then we insert the key value to the position $j+1$ (line 7) to ensure that it's in the right place. That's how we rearrange the subsequence $S[1, 2,..., i]$. We implement this method for every number iteratively until the whole sequence is sorted.

![Screenshot 2021-08-27 at 2.34.50 AM](/Users/maxcharm/Desktop/Screenshot 2021-08-27 at 2.34.50 AM.png)

(Here is the illustration of the process of sorting the sequence $S=<5,2,4,6,1,3>$ with insertion sort. The rectangle highlighted in black indicates the current key value $S[i]$. Each picture shows how we rearrange the subsequence ended with the numbers in black rectangle by inserting the key value in place.)

So how do we ensure the correctness of this algorithm? To do so, we need to introduce a commonly used concept: **loop variant**. Loop variant is an assertion on a loop which remains true both at the beginning and the end of each iteration and yields a useful property about the correctness of the algorithm when the final iteration terminates. The loop variant for insertion sort is that: **at the start of each iteration (line 2 - 7), the subsequence $S'[1,2,...,i-1]$ will be a sorted permutation of the original elements in $S[1,2,...,i-1]$. **There are three important elements which constitute a loop variant, and we will use the insertion sort as an example to describe these elements:

- **Initialisation**: Before the first iteration, we take $i = 2$, and the subsequence $S'[1]$ contains only a single element, which is the original first number $S[1]$, and is sorted apparently.
- **Maintenance**: Suppose the subsequence $S'[1,2,...,i-1]$ is a sorted permutation of the correspoding original subsequence, we should prove that after the iteration with $S[i]$ as the key, the subsequence $S'[1,2,...,i]$ will be the sorted permutation of its corresponding original subsequence. For each inner iteration (line 4-6), we compares the key value to the value $S[j]$ and moves $S[j]$ one place to the right if it is greater than the key, that would make sure that the subsequence $S'[j + 1,...,i]$ is sorted after each iteration. The inner iteration only terminates when $j = 0$ or the value $S[j]$ is smaller than the key after filling the current position with the key value. The first case ensures that the subsequence $S[1,2,...,i]$ is sorted. As with the second case, we can see that $S'[1,2,...,j-1]$ would be identical to the original sorted sequence $S'[1,2,...,j-1]$ before this iteration, followed by the key value $S[j]$, which is greater than the element $S'[j-1]$ and smaller than the element $S[j+1]$, followed by a sorted sequence $S'[j+1,...,i]$, so the whole subsequence $S'[1,...,j,...i]$ is now sorted after the iteration.
- **Termination**: The iteration terminates when $i=S.len + 1$, according to the second principle, we will have that the subsequence $S[1,2,...,i-1]$, which is currently $S[1,2,...,S.len]$ to be a sorted permutation of the original sequence $S[1,2,...,S.len]$, which is exactly the complete input sequence. We can thus conclude that the correctness of insertion sort stands based on this property.

Similar to the mathematical induction, we prove the correctness of insertion sort by the three steps of the loop invariant. Before we move on to the next algorithm, we can conclude several other important properties about insertion sort.

1. **The average and worst case running time of insertion sort is $\theta(n^2)$.**

   The running time of insertion sort can be viewed as the sum of the execution time of each iteration. 

   In the worst case scenario, for each loop (line 2 - 7), we have to make $i$ comparisons and $i-1$ swaps. So the total execution time can be viewed as $\sum_{i=2}^{n}(c_1\cdot i+c_2\cdot(i-1)) = c_1\cdot\frac{(2+n)(n-1)}{2}+c_2\cdot\frac{n(n-1)}{2} = \theta(n^2)$

   The average case is a little bit harder to analyse in that we have to figure out the expected times of swapping for each key we select. Suppose that the input sequence is randomly selected from all the permutations of the sequence, then the expected number of swapping (that is to say, how many pairs of **inversions** exist in the first place) when we selected $S[i]$ as the key is actually $\theta(i)$, as we shall prove in last section of the passage. Hence we can draw the conclusion that the average running time for insertion sort is $\sum_{i=2}^{n}\theta(i) = \theta(n^2)$. 

2. **The insertion sort is an in-place sorting algorithm.**

   At any moment during the execution of the algorithm, only the current key is stored other than the input sequence itself, which makes it an in-place sorting algorithm.

Insertion sort is often used when the input list is small and half sorted, for the running time is acceptable when the number of inversions are small and the extra storage needed is almost none. However, for input larger in size, we often choose merge sorts or quick sorts for their lower complexity.



**Merge Sort**

If you are familiar with divide-and-conquer methods, then it'll be easy to understand merge sort. As the name implies, in merge sort we divide the sequence into two parts, sort them separately before merging them into a complete sorted sequence. But how do we sort the two subsequences? We don't. Instead we divide them further into four subsequences, and then divide those subsequences again... Until it cannot be divided again.

The pseudocode for merge sort looks like this:

```python
Merge Sort (S, p, r)
1. if p < r:
2.		q = (p + r) / 2
3. Merge Sort (S, p, q)
4. Merge Sort (S, q, r)
5. Merge (S, p, q, r)
```

![Screenshot 2021-08-28 at 2.11.33 AM](/Users/maxcharm/Desktop/Screenshot 2021-08-28 at 2.11.33 AM.png)

(Here is the illustration of the process of sorting the sequence $S=<5,2,4,7,1,3,2,6>$ with merge sort. We recursively divide the sequence into two parts same in length until it is not dividable. The we merge the sequences in pairs in bottom-up fashion.)

The basic logic of the pseudocode is to decompose the sequence into two parts until it is not dividable (the start and the end converges to the same element), and the important part, other than dividing, is how to merge the two sorted subsequence into one sorted sequence by the subprocess **Merge**:

```python
Merge (S, p, q, r)
1. h = q - p + 1
2. t = r - q
3. let L[1,...,h, h + 1] = S[p,...,q] + [INF]
4. let R[1,...,t, t + 1] = S[q + 1,...,r] + [INF]
5. let i = j = 1
6. for k from p to q:
7. 		i, j, S[k] = (L[i] > R[j]) ? i, j + 1, R[j] : i + 1, j, L[i]

```

In the merge process, we created two temporary lists $L$ and $R$ to store the left and right half of the sequence. Since $L$ and $R$ are already sorted, we just compare the head of the two lists and insert the smaller one into the original sequence $S$, if we pick one number from $L$, we just move the head pointer of $L$ one position to the right. We do the same when we pick one number from $R$ (line 6-7). In order to avoid the case where one of the pointers is pointed to the end of the list and there will be no number in the next position to compare to, we append a number which is large enough (which is **INF** in line 3-4) to the end of each list.

![Screenshot 2021-08-28 at 9.15.54 PM](/Users/maxcharm/Desktop/Screenshot 2021-08-28 at 9.15.54 PM.png)

![Screenshot 2021-08-28 at 7.33.53 PM](/Users/maxcharm/Desktop/Screenshot 2021-08-28 at 7.33.53 PM.png)

(Figures(a) - Figures(i) show the process of merging two lists $[2,4,5,7,\infin]$ and $[1,2,3,6,\infin]$.  First the $i$, $j$ pointer is at the head of two lists, then $L[i]$ and $R[j]$ are compared and the smaller one, $R[j]$ is inserted to the sequence, then the pointer $j$ is moved one position to the right, and the same process continues in figure (c) to (i).)

While the correctness of merge sort is self-evident (you can try to prove it with a loop variant yourself), how do we find its running time? Like the logic of the process, we can find the running time of sorting a sequence with $n$ numbers recursively by this expression:

​										     					$T(n) = 2T(\frac{n}{2})+\theta(n)$

Noted that the time needed to sort a sequence of $n$ numbers is equal to the time needed to sort two subsequences with $\frac{n}{2}$ numbers plus the time to merge them together. Suppose that the time for one comparisons is $c_1$ and one insertion is $c_2$, the total time for merging two lists with $n\over 2 $ should be $c_1\cdot \frac n 2+c_2\cdot n = \theta(n)$. 

There are two ways to solve the recurrence, namely the master theorem and the recursion tree method. We only introduce the master theorem here since it is methodical once you remember the formula.

The master theorem states that for the recurrence $T(n) = aT(\frac n b) + O(n^d)$, for a > 0, b > 1 and $d\geq0$, the solution is:
$$
T(n)=\left\{
\begin{aligned}
  O(n^d) &,& d>\log_b a \\
 O(n^d\log n)&,& d = \log _b a \\
O(n^{\log_b a}) &,& d < \log _b a
\end{aligned}
\right.
$$
In this case, $a=b=2$, $d = 1$, so $d = \log _b a$, thus $T(n) = O(n\ln n)$.



**Quick Sort**

Another classic recursive sorting algorithm is quick sort. Compared to merge sort, which is also a recursive function, quick sort is constructed in a top-down fashion instead of a bottom-up one. The idea is to recursively pick a pivot in the input sequence before putting all numbers smaller than the pivot to its front while all numbers greater to its back. The pseudocode looks like this:

```python
Quick Sort (S, p, r)
1. if p < r:
2.		 q = Partition (S, p, r)
3. 		 Quick Sort (s, p, q - 1)
4. 		 Quick Sort (s, q + 1, r)
```

The subprocess **partition** 

### II. The Nature of Sorting

We can conclude from the algorithms above that the nature of sorting is quite simple - find the right index for each number that is "misplaced" in the original sequence. 



### III.Lower Bound