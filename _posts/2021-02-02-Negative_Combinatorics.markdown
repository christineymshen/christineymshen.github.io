---
layout: post
title:  Negative Numbers in Combinatorics
date:   2021-02-02 14:54
description: explorations
---

I came across *Negative Numbers in Combinatorics* while working on STA 732 HW1. 

---

Suppose a set S has n elements, number of subsets of S of size k is:

\begin{equation\*}
  {n \choose k}  = \frac{n(n-1) \dots (n-k+1)}{k!}.
\end{equation\*}

Now if there were a set with -2 number of element, how many subsets of size 3 would it have? We can try naively applying the same formula:

\begin{equation\*}
  {-2 \choose 3}  = \frac{(-2)(-3)(-4)}{3!} = -4.
\end{equation\*}

This gives us a number, but how to understand it?

Note that a *set*, by definition can only consist of unique elements. here we introduce the notion of multisets, with unbounded multiplicity. For example, the following are three different and yet valid multisets: {x,x}, {x,y}, and {y,y}.

Now coming back to the question above, let S = {x,y}, where both x and y are "negative elements", i.e., they can be selected more than once into a multiset. Then the unique subsets of S of size 3 are: {x,x,x}, {y,y,y}, {x,x,y}, {x,y,y}, a total of 4, which matches with the number 4 we got above. But what about the negative sign？ Here let's say that a multiset with k negative elements has weight of $$ (-1)^k $$. And hence we have 

**Theorem**: For a set S with m negative elements, $$ {-m \choose k}$$ is the sum of the weights of all the k-element multiset-subsets of S.

**Proof**:

$$
\begin{aligned}
  {-m \choose k}  &= \frac{(-m)(-m-1) \dots (-m-k+1)}{k!} \\
  &= (-1)^k {m+k-1 \choose k}.
\end{aligned}
$$

Now we need to show that the number of unique multisets by choosing k from m elements, is indeed $$ {m+k-1 \choose k} $$. We can use a counting method called "stars and bars". This might be rather basic, but it's the first time I come across this idea. I find it quite interesting. 

Now think about choosing k from 2 negative elements. As the order doesn't matter here, this is equivalent to ... imagine we have k stars, lying in one line

$$ * * * \dots * * * $$

We want to put one bar in between to separate these stars into two groups, for example this is one possibility:

$$ * | * * \dots * * * $$

How many different ways are there to place this bar? This is exactly equivalent to the problem above. Here there are a total of (k+1) places for me to place this bar, and hence it's $$ {k+1 \choose 1} = {2+k-1 \choose 2-1}$$ number of different possibilities. And if we generalize it an (m choose k) problem, obviously the answer is

$$
  {m+k-1 \choose m-1} = {m+k-1 \choose k}.
$$

This completes the proof. 

This also works for hybrid sets, i.e., sets with both positive and negative elements:
+ a positive element can only be chosen once, with weight 1
+ a negative element can be chosen any number of times, with weight (-1)

For example, consider a set S = {a, x, y} where a is a positive element, while x and y are negative elements. In total S has (-1) elements. So how many subsets (potentially multisets) of size 3 does S have?

1. For subsets with only negative elements, each subset/ multiset has weight of $$ (-1)^3 = (-1) $$ and there are a total of 4 subsets
2. For subsets with one positive element a, and two negative elements, the weight is 1, and there are a total of 3 subsets.
3. All together, the sum of the weights of all the unique subsets is 3-4=-1. This is same as $$ -1 \choose 3 $$.

*This is where I decided to stop. I did not try to further prove the case for hybrid sets.*


References:
+ For general understanding of this concept: http://faculty.uml.edu/jpropp/msri-up12.pdf
+ For the "stars and bars" technique: https://brilliant.org/wiki/integer-equations-star-and-bars/