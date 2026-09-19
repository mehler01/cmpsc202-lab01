# Lab 01: Empirical Benchmarking and The Maximum Subarray Problem

## Overview

In this lab, you will explore how to benchmark algorithms. Given an algorithm that solves the Maximum Subarray Problem (described below), you will design a baseline algorithm, implement both the baseline and optimized algorithms in Python, and benchmark both to compare the difference in their running time.

For this lab, you will be working in your project teams. If you do not have a team, see the teaching staff to get assigned to one. Here is the [project list](https://docs.google.com/document/d/1pTKetb_EuZRGcA39BaDLchRSRRrMXAxWEI9UJK41JOU/edit?usp=sharing).

Note: fork this repository to your own GitHub account before starting the lab!

**The Problem:** The Maximum Subarray Problem.
Given a list of integers (containing both positive and negative numbers), find the contiguous sublist with the largest sum.
For example: 

```
Input: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Output: 6
Explanation: The contiguous sublist [4, -1, 2, 1] has the largest sum = 6.
```

## Part 1: Baseline Algorithm Design

Before looking at optimized solutions, your first task is to design a baseline or "naive" solution.

**Design:** Devise an algorithm that finds the maximum subarray by evaluating all possible contiguous subarrays. Write down the steps clearly in pseudocode before moving on to the implementation. 

*Hint:* see the pseudocode for Kadane's Algorithm in Part 2 for guidance on how to structure your approach.

```
Pseudocode baseline solution
Algorithm: BaselineSubarray(A)
Input: A which is a list of n integers
Output: The maximum contiguous subarray sum

1. max_so_far = negative infinity
2. for i from 0 to n-1:
3.    sum = 0
4.    for j from i to n-1:
5.       sum = A[j] + sum
6.       if sum > max_so_far: 
7.            max_so_far = sum
8. return max_so_far

```

## Part 2: The Optimized Algorithm (Kadane's Algorithm)

Computer scientist Jay Kadane developed an elegant, dynamic programming approach to this problem that runs in $O(N)$ time.

Review the pseudocode below. You will implement this alongside your baseline algorithm.

```
Algorithm: KadaneMaxSubarray(A)
Input: A list A of n integers
Output: The maximum contiguous subarray sum

1. max_so_far = negative infinity
2. current_max = 0
3. For i from 0 to n-1:
4.     current_max = current_max + A[i]
5.     If current_max > max_so_far:
6.         max_so_far = current_max
7.     If current_max < 0:
8.         current_max = 0
9. Return max_so_far

```

## Part 3: Python Implementation & Data Generation

You will now implement both algorithms in Python and set up an experimental framework to test them.

1. **Implementation:** Write two Python functions, `baseline_max_sublist(arr)` and `kadane_max_sublist(arr)`.

2. **Data Generation:** Write a helper function using `numpy.random.randint` to generate random integer lists of a given length. Ensure the lists contain both positive and negative numbers (e.g., range from -100 to 100).

3. **Benchmarking Setup:**

   * Use `time.perf_counter()` to measure execution time.

   * Test your algorithms on lists of the following lengths: $N \in \{100, 500, 1000, 2500, 5000, 10000\}$.

   * **Important:** System background processes can cause noisy data. For each array size $N$, run the algorithms 5 to 10 times on freshly generated lists and record the *average* execution time.

## Part 4: Visualization

A crucial part of empirical benchmarking is communicating your results clearly. Use `matplotlib` and/or `seaborn` to visualize your data.

1. **The Plot:** Create a line plot with Array Size ($n$) on the x-axis and Execution Time (seconds) on the y-axis.

2. **Requirements:** Plot the execution times for both algorithms on the same axes. Include a clear legend, title, and axis labels. Try to make the plot look nice and readable!

3. **Logarithmic Scale (Optional but Recommended):** You will likely notice that the baseline algorithm's curve grows so fast that Kadane's algorithm looks like a flat line at $y=0$. Try plotting the y-axis on a logarithmic scale (`plt.yscale('log')`) to better observe the scaling behavior of both algorithms simultaneously.

## Part 5: Teaching Staff Check-in

Discuss your work with a member of the teaching staff to get feedback on your implementation and analysis. In particular, discuss the following:
- Be able to clearly describe the problem
- How your baseline algorithm works
- How Kadane's algorithm works
- Your plot showing the performance comparison between the baseline and Kadane's algorithm

 If everything looks good, then proceed to Part 6 and complete the exercise.

## Part 6: Exercise

Answer the following questions in a file called `reflection.md`:

1. At what array size did your baseline algorithm become noticeably sluggish to execute?

```
While the baseline algorithm performed much slower in comparison to the kadane algorithm, I think that it became noticeably sluggish around the 2,500-5,000 element arrays. The baseline function's runtime eclipsed a tenth of a second for both of these arrays, while the kadane algorithm completed the same task in less than a thousanth of a second (.000108s and .000220s). At 10,000 integers, the baseline algorithm jumped up to over 1.8 seconds, which was noticeably more sluggish than its kadane counterpart, whose runtime still didn't eclipse a thousanth of a second (.000437s). 
```

2. Based on your empirical data and the shape of your graph, estimate how long (in seconds, minutes, or hours) your baseline algorithm would take to process an array of $1,000,000$ elements. Show your reasoning.

```
From the graph and empirical data, the baseline algorithm approximately quadruples in runtime between the 2,500, 5,000, and 10,000 length arrays (.116s -> .460s -> 1.83s). Note that all of these jumps double the amount of elements, so we can estimate that the runtime increases by 4x for every 2x increase to the number of elements. This estimation is somewhat supported by the fact that our graph approaches a linear relationship as k increases. Thus for one million elements, which is 100 times 10,000, the list size would increase by ~2^6-2^7 (64-128). We'll say it increases by 2^6.5 for estimation sake. Therefore, the runtime should increase by a factor of 4*6.5=26. Thus, I project the run speed would be around 1.83s x 26 ~ 47.5s for a 1,000,000 element array.
```

3. Based on your empirical data and the shape of your graph, estimate how long (in seconds, minutes, or hours) your Kadane's algorithm would take to process an array of $1,000,000$ elements. Show your reasoning.

```
From the graph and empirical data, the kadane algorithm approximatley doubles in runtime between the 2,500, 5,000, and 10,000 length arrays (.000108s -> .000220s -> .000437s). Note that all of these jumps double the amount of elements. Thus, the runtime increases by 2x for every 2x increase to the number of elements. The graph for kadane also approaches a linear relationship as k increases. Thus to get to 1,000,000 elements, we must increase the 10,000 element array by 100~2^6.5. So the run time will aproximately increase by a factor of 2x6.5=13. Thus, I project the run speed would be around .000437s x 13 ~ .00568s for a 1,000,000 element array.
```

Once you're finished, commit your changes and push them to your GitHub repository. Submit a link to your repo [here](https://forms.gle/5mFKZ9RtFJYVcPfJ6).