---
layout: post
title:  "Fibonacci Sequences"
date:   2016-06-21 21:58:02
categories: sequences algorithms
---

This is a short (ish) blog post on the different techniques of finding the value of the nth number in the Fibonacci Sequence.
The simplest method will be explained first, then some improvements will be proposed in order to speed up the calculation.

The simplest method is shown and explained first, please see the code snippet below.

{% highlight java %}
public int fibonacci(int n) {
  if (n <= 2) return 1;
  return fibonacci(n-1) + fibonacci(n-2);
}
{% endhighlight %}

As you can see, it's quite a simple method and quick to write. This is a recursive method as it calls itself. It stops because if n is less than or equal to 2, the value is 1.
How long will this take for a large n? Let's define the operation which is being counted as the '+' operation.
 The base case (the value of n for which fibonacci(n) doesn't enter any more recursive calls) is where n = 2, or n = 1 as at that point 1 is returned.
  If we look at the fibonacci method, we see that it makes 2 recursive calls, and n is decreased by at least 1 for each of those calls.
  As n was decreased by 1 for both of those recursive calls, then if we were to trace each of those recursive calls and their recursive calls it would mean we have a balanced binary tree type of structure (where each node is a call to the fibonacci method). See the image below for what a balanced binary tree looks like: ![Balanced Binary Tree]({{ site.url }}/assets/balanced-binary-tree.gif) The number of the nodes in a balanced binary tree is known to be at most 2^(h+1) - 1 where h is the height of the tree. Going back to the number of operations used by this, the height of the tree is going to be the value of n (for values of n over 2). That means that the number of recursive calls (and operations) is going to be at most 2^(n+1) - 1. When looking at time complexity, we can ignore the -1. As 2^(n+1) is really just (2^n)*(2^1) we can also ignore the 2^1. So that means the time complexity is O(2^n) for this method.

This method is very slow however, and there is a simple optimization to speed this up. As it is a sequence, the previous terms are used to construct the next terms. We can make use of this fact and store them as temporary variables to calculate the nth term we are looking for. Please see some code below.

{% highlight java %}
public int fibonacci(int N) {
  if (N <= 2) return 1;
  int fibOfNMinusOne = 1;
  int fibOfNMinusTwo = 1;
  for (int n = 2; n < N; n++) {
    int fibOfn = fibOfNMinusOne + fibOfNMinusTwo;
    if (n == N) {
      return fibOfn;
    }
    int fibOfNMinusTwo = fibOfNMinusOne;
    fibOfNMinusOne = fibOfn;
  }
}
{% endhighlight %}
This reduces the complexity of it to O(n) because the loop repeats n-2 times.
It's possible to speed up this further by using matrix multiplication. Please see the image below for an explanation on how this works.
I may come back and edit this with an actual explanation on how this works. For now just see the code snippet.


{% highlight java %}
  public static int solveUsingMatrices(int n) {
    if (n <= 2) {
      return 1;
    }
    final int[][] T = new int[2][2];
    T[0] = new int[]{1, 1};
    T[1] = new int[]{1, 0};
    return raise(T, n - 1)[0][0];
  }

  private static int[][] raise(int[][] matrix, int power) {
    if (power == 1) {
      return matrix;
    } else if (power % 2 == 1) {
      return matrixMul(matrix, raise(matrix, power - 1));
    } else {
      int[][] halfRaised = raise(matrix, power / 2);
      return matrixMul(halfRaised, halfRaised);
    }
  }

  private static int[][] matrixMul(int[][] A, int[][] B) {
    int[][] result = new int[2][2];
    for (int row = 0; row < A.length; row++) {
      for (int col = 0; col < A[0].length; col++) {
        // calculate this thing.
        int res = 0;
        for (int i = 0; i < 2; i++) {
          res += A[row][i] * B[i][col];
        }
        result[row][col] = res;
      }
    }
    return result;
  }
{% endhighlight %}

That's it, my first real post.
The code shown here can be found [at my github page](https://github.com/willb611/algorithms/tree/master/src/main/java/algorithms/sequences).
Thanks for reading!
