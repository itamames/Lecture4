# Lecture 4 Algorythm analysis continued
(Adapted from Data Structures and Algorithm & Data Structures And Problem Solving Using Java)

The github for this page is:

https://github.com/itamames/Lecture4

## Practice Big O

When calculating Big O, you have to consider that the upper limit is what counts when addition but multiplication is a different matter:

### Addition

* O(log N) + O(N) = O(N)
* O(N log N) + O(N) = O(N log N)
* O(N log N) + O(N<sup>2</sup>) = O(N<sup>2</sup>)
* O(2<sup>N</sup>) + O(N<sup>2</sup>) = O(2<sup>N</sup>)

### Multiplication 
* O(log N) * O(N) = O(N log N)
* O(N log N) * O(N) = O(N<sup>2</sup> log N)
* O(N) * O(1) = O(N)


## Big-O
O(x) - Less than

Big-O notation gives you a rough indication of the worst running time of an algorithm.

"Our algorithm runs at most O(n<sup>2</sup>)"

## Big-Omega

&Omega;(x) - Greater than

Just as the big-Oh notation provides an asymptotic way of saying that a algorithm is “less than or equal to” another algorithm, the following notations provide an asymptotic way of saying that a algorithm grows at a rate that is “greater than or equal to”
that of another.

Is the opposite of Big-O

"Our lower bound shows &Omega;(n<sup>2</sup>)"

## Big-Theta

&Theta;(x) - Equal to

In addition, there is a notation that allows us to say that two algorithms grow at the same rate, up to constant factors.

"Furthermore, our bounds are tight at &Theta;(n<sup>2</sup>)"

### Example 
For T(n) = 73n<sup>3</sup> + 22n<sup>2</sup>+ 58, all of the following are generally acceptable, but tighter bounds (such as numbers 2 and 3 below) are usually strongly preferred over looser bounds (such as number 1 below).

T(n) = O(n<sup>100</sup>)

T(n) = O(n<sup>3</sup>)

T(n) = Θ(n<sup>3</sup>)

The equivalent English statements are respectively:

"T(n) grows asymptotically no faster than n<sup>100</sup>."

"T(n) grows asymptotically no faster than n<sup>3</sup>."

"T(n) grows asymptotically as fast as n<sup>3</sup>."


## Calculating the speed *vroom* *vroom* by time spent

One way to study the efficiency of an algorithm is to implement it and experiment by running the program on various test inputs while recording the time spent during each execution. A simple mechanism for collecting such running times in Java is based on use of the currentTimeMillis method of the System class.

```java
long startTime = System.currentTimeMillis( ); // record the starting time
 /* (run the algorithm) */
long endTime = System.currentTimeMillis( ); // record the ending time
long elapsed = endTime − startTime;
```

For example, lets take our trivial example of function that takes O(n!) time.

```java
public class CheckTime {

    static void SuckyFunction(int n) {
        if (n == 0) {
            return;
        }
        for (int i = 0; i < n; i++) {
            // do constant time stuff
            System.out.println(n + ": " + n);

        }
        SuckyFunction(n - 1);
    }

    // Driver code
    public static void main(String args[]) {
        long startTime = System.currentTimeMillis(); // record the starting time

        int N = 30;
        SuckyFunction(N);
        long endTime = System.currentTimeMillis(); // record the ending time
        System.out.println("Time elapsed:" + (endTime - startTime));

    }
}

```

### Uses repeated concatenation to compose a String with n copies of character c. 
```java
 public static String repeat1(char c, int n) { String answer = "";
 for (int j=0; j < n; j++)
 answer += c;
 return answer;
} 
```
 ### Uses StringBuilder to compose a String with n copies of character c. 
```java
 public static String repeat2(char c, int n) { StringBuilder sb = new StringBuilder( );
 for (int j=0; j < n; j++)
 sb.append(c);
 return sb.toString( );
 }
```


The “logarithm” – some on most confusing aspect of analysis of algorithms. 
Logarithm General Rule: 
An algorithm is O(logN) if it takes constant time, O(1), to cut the problem size by a fraction (which is usually ½)
Only special kinds of problems can be O(logN). 
If the input is a list of N numbers then an algorithm must take (N) merely to read the input in. Thus, when we talk about O(logN) algorithms we usually presume that the input is pre-read. 


## Algorithm: Binary Search O(logN) 

The Classic Search Problem:

Given an integer X and integers A<sub>0</sub>, A<sub>1</sub>, . . . , A<sub>N-1</sub>, which are pre-sorted and already in memory, find the index i such that A<sub>i</sub> = X, or return i = −1 if X is not in the input.

### Obvious solution:

 O(N) algorithm - Scan the list from left to right and find i. 
This algorithm does not take advantage of the fact that the list is pre-sorted.

### Better strategy (Binary Search):

* Compare X with middle item A[mid], 

* Go to left half	if X < A[mid] 

* Go to right half	if X > A[mid] 

* Repeat

![Steps](images/Steps.jpg)

![Steps](images/Tree.png)

```java
/**
 * Performs the standard binary search.
 * @return index where item is found, or -1 if not found.
 */
int NOT_FOUND=-1; // NOT_FOUND is defined as -1
public static <AnyType extends Comparable<? super AnyType>>
int binarySearch( AnyType [ ] a, AnyType x )
{
 	int low = 0, high = a.length - 1;
 	while( low <= high )
 	{
 	    int mid = ( low + high ) / 2;
 	    if( a[ mid ].compareTo( x ) < 0 )  // x > a[mid]
 	        low = mid + 1;
 	    else if( a[ mid ].compareTo( x ) > 0 )  // x < a[mid]
 	            high = mid - 1;
 	         else
 	            return mid; // Found
 	}
 	return NOT_FOUND; 
}

```

### Worst case? 

When X is not found.

How many iterations are executed before low > high?

* After first iteration: N/2 items remaining
* After 2nd iteration: (N/2)/2 = N/4 remaining
* After k-th iteration? 
    * N/2<sup>K</sup> remaining

* Last iteration occurs when N/2<sup>K</sup> ≥ 1 and N/2<sup>K+1</sup> < 1 item remaining
* 2<sup>K</sup> ≤ N and 2<sup>K+1</sup> > N [take log of both sides]
* Number of iterations is k ≤ logN and k > logN – 1
	* k = logN

**Binary Search is O(logN)**

### Our first data structure implementation. 

* The “contains” operation is O(logN) time in worst-case (no algorithm can do better).
* Is O(1) in the best-case (item is in the middle).
* All other operations (in particular insert-in-sorted-array) require O(N) time. 


## Euclid’s Algorithm for GCD 

Definition: The greatest common divisor (gcd) of two integers is the largest integer that divides both. 

Examples: gcd(50,15) = 5. 

### The problem:

Find the greatest common divisor of two positive integers, M and N.

### A good solution: The Euclid’s Algorithm

The Euclid’s Algorithm is based on the following two observations:

* gcd(M,N) = gcd(N, M mod N), if N != 0
* gcd(N,0) = N


Example: 
```text
	gcd(3084,1424) = ?
	3084 = 1424*2 + 236	    gcd(3084,1424)= gcd(1424,236)
	1424 = 236*6 + 8	    gcd(1424,236) = gcd(236,8)
	236 = 8*29 + 4	        gcd(236,8) = gcd(8,4)
	8 = 4*2 + 0		        gcd(8,4) = gcd(4,0)
				            then gcd(3084,1424) = 4 
```

### Formal description of the Euclid’s Algorithm 

* Input: Two positive integers, M and N.
* Output: The gcd of M and N.
* Internal computation:
    * If M < N, exchange M and N.
    * Divide M by N and get the remainder, r. 
	    * If r = 0, report N as the gcd of M and N.
    * Replace M by N and replace N by r. Return to the previous step.

```java
public static long gcd( long m, long n )
2 	{
3	      while( n != 0 )
4 	      {
5 	           long rem = m % n;
6 	           m = n;
7	           n = rem;
8 	       }
9 	       return m;
10 	}	
```
Analysis:

* The running time of the algorithm depends on determining how long the sequence of remainders is. 

Whats the O(n) of this algorithm?
```.
.
.
.
```

* After two iterations of the loop while, the remainder is at most half of its original value. 
* Then the number of iterations is at most 2logN = O(logN) and establish the running time. 

**Euclid’s Algorithm is O(logN)**

### Interesting fact:

The constant can be improved to approximately 1.44logN, in the worst case (the case when M and N are consecutive Fibonacci numbers).

## Why Recursion? 

* If recursion is less efficient (in some cases), why use it?

It leads to elegant solutions and the code can be clearer and simpler.

* Some problems with ADTs require recursion. Examples:
    * Tree traversals
    * Graph traversals
    * Search problems
* In some cases, an algorithm with a recursive solution has a lesser computational complexity. Example: Insertion Sort vs. Merge Sort. 



### Summary of the Algorithm Analysis 

* We can analyze the problem or the algorithm - usually algorithm
* We can consider Time or Space - usually time (running time)
* We can analyze the Best, Worst, or Average-case - usually worst-case.
* We can analyze the Upper, Lower, or Tight-bound - usually upper or tight-bound.
* Asymptotic complexity (Big-Oh) focuses on behavior for large N and is independent of the computer, programming language, coding, etc.

* Simple programs usually have simple analyses,  but this is not always the case. Example: sorting algorithm Shellsort.
* An interesting kind of analysis is the lower-bound analysis. We will see an example of later, where it is proved that any algorithm that sorts by using only comparisons requires &Omega;(NlogN) comparisons.
* Some of the algorithms described in this topic have real-life applications. The gcd algorithm is used in cryptography.

## Practice Problem

### The Channel Guide

Clone this practice folder, and solve the problem in the Readme.md file. Please start working with group members for this example if possible, remember there is a group project soon. The answers will be posted Tuesday 24th.

For this example we will write a program to read the television channel data from a data set. Each television channel in the set consists of a number followed by a name on the same line.  Please start working with group members for this example if possible, remember there is a group project soon.

### For example:
101 National Geographic Channel

The first line in the file contains the number of channels stored in the file.

NOTE: The channels in the file are already in alphabetical order based on channel name.

Guide.txt contains the dataset.

Channel.java represents a class for a single TV channel with two fields, a number and a name, along with the usual accessors and mutators.

CableSystem.java loads the guide and searches for channels in it.

### EXERCISES

1) In the class named CableSystem, the constructor has one parameter specifying the name of the data file that contains the television channel data. The constructor opens the file and loads the data into an array leaving no empty cells. If the file is not there, it reports an error and exits the program. In the constructor, write additional comments to explain why the two indicated statements are needed.

2) In the CableSystem class, complete the method search1 that performs a linear search of the array for the channel number given the name in the parameter. Examine only those channels that are necessary to determine your answer. The method should output the number of array cells that are examined during the search and then return the number of the channel corresponding to that name (if found) or -1 if not found. Use the main method in this class to test your solution before you move on.

3) In the CableSystem class, complete the method search2 that performs a binary search of the array for the channel number given the name in the parameter. In binary search, examine the middle element for the target. If it is not there, examine the middle element of whichever half the target might be in. If that element is not the target, examine the middle element of whichever half of the halves the target might be in, ... etc. 

(See the course notes for an algorithm for binary search.) 

   * The method should output the number of array cells that are examined during the search and then return the number of the channel corresponding to that name (if found) or -1 if not found. Use the main method in this class to test your solution.
   * Use the compareTo method of the String class to compare strings for lexicographical ordering. Lexicographical ordering is like alphabetical ordering, but includes uppercase, lowercase, digits, punctuation, etc. 
       * If s1 and s2 are Strings,
s1.compareTo(s2) returns a negative integer if s1 comes before s2 lexicographically
       * s1.compareTo(s2) returns a positive integer if s1 comes after s2 lexicographically
       * s1.compareTo(s2) returns 0 if s1 is equal to s2


4) Determine the maximum number of array cells that need to be examined in the search1 and search2 methods as a function of the number of channels in the array (call this n), and use the program to try to verify your solution. Complete the comment at the beginning of the CableSystem class that gives your answers with an explanation.




