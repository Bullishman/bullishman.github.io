---
layout: post
title:  "Think Data Structures_ch5_Doubly-linked list"
date:   2019-06-11 23:51:28 +0900
categories: [Java, Japanese, Algorithms, Data Structure]
---
Chapter 5      

Doubly-linked list       

5.1 Performance profiling results       

&nbsp;

In the previous exercise, we used Profiler.java to run various ArrayList and LinkedList operations with a range of problem sizes. We plotted run time versus problem size on a log-log scale and estimated the slope of the resulting curve, which indicates the leading exponent of the relationship between run time and problem size.    

&nbsp;

For example, when we used the add method to add elements to the end of an ArrayList, we found that the total time to perform n adds was proportional to n; that is, the estimated slope was close to 1. We concluded that performing n adds is in O(n), so on average the time for a single add is constant time, or O(1), which is what we expect based on algorithm analysis.      

&nbsp;

The exercise asks you to fill in the body of profileArrayListAddBeginning, which tests the performance of adding new elements at the beginning of an ArrayList. Based on our analysis, we expect each add to be linear, because it has to shift the other elements to the right; so we expect n adds to be quadratic.      

&nbsp;

Here’s a solution, which you can find in the solution directory of the repository.    

```java
	public static void profileArrayListAddBeginning() { // Solution Code
		Timeable timeable = new Timeable() {
			List<String> list;

			public void setup(int n) {
				list = new ArrayList<String>();
			}

			public void timeMe(int n) {
				for (int i=0; i<n; i++) {
					list.add(0, "a string");
				}
			}
		};
		int startN = 4000;
		int endMillis = 10000;
		runProfiler("ArrayList add beginning", timeable, startN, endMillis);
	}
```      

&nbsp;
&nbsp;

This method is almost identical to profileArrayListAddEnd. The only difference is in timeMe, which uses the two-parameter version of add to put the new element at index 0. Also, we increased endMillis to get one additional data point.
(上のメソッドと profileArrayListAddEndメソッドの違いはインデックス0を明示してパラメータを一つ増加しだけです。)       

Here are the timing results (problem size on the left, run time in milliseconds on the right):         

		4000, 14     
		8000, 35      
		16000, 150      
		32000, 604      
		64000, 2518      
		128000, 11555      

Remember that a straight line on this graph does not mean that the algorithm is linear. Rather, if the run time is proportional to n^k for any exponent, k      
(グラフの一直線はアルゴリズムが線形を表しのはないです。      
むしろ、実行時間は n^kの指数に比例します。)      

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch5_1.png "Screenshot broadcast")  

Figure 5.1: Profiling results: run time versus problem size for adding n elements at the beginning of an ArrayList.      

&nbsp;

expect to see a straight line with slope k. In this case, we expect the total time for n adds to be proportional to n 2 , so we expect a straight line with slope 2. In fact, the estimated slope is 1.992, which is so close I would be afraid to fake data this good.      

---

5.2 Profiling LinkedList methods      

In the previous exercise you also profiled the performance of adding new elements at the beginning of a LinkedList. Based on our analysis, we expect each add to take constant time, because in a linked list, we don’t have to shift the existing elements; we can just add a new node at the beginning. So we expect the total time for n adds to be linear.   

&nbsp;   

Here’s a solution:      

```java
	public static void profileLinkedListAddBeginning() {

		Timeable timeable = new Timeable() {
			List<String> list;

			public void setup(int n) {
				list = new LinkedList<String>();
			}

			public void timeMe(int n) {
				for (int i=0; i<n; i++) {
					list.add(0, "a string");
				}
			}
		};
		int startN = 4000;
		int endMillis = 1000;
		runProfiler("LinkedList add beginning", timeable, startN, endMillis);

	}
```      

&nbsp;
&nbsp;

We only had a make a few changes, replacing ArrayList with LinkedList and adjusting startN and endMillis to get a good range of data. The measurements were noisier than the previous batch; here are the results:      

		128000, 16      
		256000, 19      
		512000, 28      
		1024000, 77      
		2048000, 330      
		4096000, 892      
		8192000, 1047      
		16384000, 4755      

Figure 5.2 shows the graph of these results.      

&nbsp;

It’s not a very straight line, and the slope is not exactly 1; the slope of the least squares fit is 1.23. But these results indicate that the total time for n adds is at least approximately O(n), so each add is constant time.      

---

5.3 Adding to the end of a LinkedList      

Adding elements at the beginning is one of the operations where we expect LinkedList to be faster than ArrayList. But for adding elements at the end, we expect LinkedList to be slower.      

&nbsp;

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch5_2.png "Screenshot broadcast")  

Figure 5.2: Profiling results: run time versus problem size for adding n elements at the beginning of a LinkedList.      

&nbsp;

In my implementation, we have to traverse the entire list to add an element to the end, which is linear. So we expect the total time for n adds to be quadratic.      

```java
	public static void profileLinkedListAddEnd() {

		Timeable timeable = new Timeable() {
			List<String> list;

			public void setup(int n) {
				list = new LinkedList<String>();
			}

			public void timeMe(int n) {
				for (int i=0; i<n; i++) {
					list.add("a string");
				}
			}
		};
		int startN = 4000;
		int endMillis = 1000;
		runProfiler("LinkedList add beginning", timeable, startN, endMillis);

	}
```

&nbsp;
&nbsp;

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch5_3.png "Screenshot broadcast")  

Figure 5.3: Profiling results: runtime versus problem size for adding n elements at the end of a LinkedList.      

Here are the results:      

		64000, 9      
		128000, 9      
		256000, 21      
		512000, 24      
		1024000, 78      
		2048000, 235      
		4096000, 851      
		8192000, 950      
		16384000, 6160      

Figure 5.3 shows the graph of these results.      

&nbsp;

Again, the measurements are noisy and the line is not perfectly straight, but the estimated slope is 1.19, which is close to what we got adding elements at the beginning, and not very close to 2, which is what we expected based on our analysis. In fact, it is closer to 1, which suggests that adding elements at the end is constant time. What’s going on?      

---

5.4 Doubly-linked list      

&nbsp;

My implementation of a linked list, MyLinkedList, uses a singly-linked list; that is, each element contains a link to the next, and the MyArrayList object itself has a link to the first node.      

&nbsp;

But if you read the documentation of LinkedList at http://thinkdast.com/ linked, it says:   

Doubly-linked list implementation of the List and Deque interfaces. [. . . ] All of the operations perform as could be expected for a doubly-linked list. Operations that index into the list will traverse the list from the beginning or the end, whichever is closer to the specified index.      

&nbsp;

If you are not familiar with doubly-linked lists, you can read more about them at http://thinkdast.com/doublelist, but the short version is:      

- Each node contains a link to the next node and a link to the previous node.      

- The LinkedList object contains links to the first and last elements of the list.      

&nbsp;

So we can start at either end of the list and traverse it in either direction. As a result, we can add and remove elements from the beginning and the end of the list in constant time!

&nbsp;

The following table summarizes the performance we expect from ArrayList, MyLinkedList (singly-linked), and LinkedList (doubly-linked):      

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch5_4.png "Screenshot broadcast")  

---

5.5 Choosing a Structure      


&nbsp;
The doubly-linked implementation is better than ArrayList for adding and removing at the beginning, and just as good as ArrayList for adding and removing at the end. So the only advantage of ArrayList is for get and set, which require linear time in a linked list, even if it is doubly-linked.      

&nbsp;

If you know that the run time of your application depends on the time it takes to get and set elements, an ArrayList might be the better choice. If the run time depends on adding and removing elements near the beginning or the end, LinkedList might be better.      

&nbsp;

But remember that these recommendations are based on the order of growth for large problems. There are other factors to consider:      

- If these operations don’t take up a substantial fraction of the run time for your application — that is, if your applications spends most of its time doing other things — then your choice of a List implementation won’t matter very much.      
(万一、こんな実行がアプリケーションの実行時間の一部分を占めないならば―取りも直さず、貴方のアプリケーションが他の作業に暇に飽かせばListを使うのがいいです。)      

- If the lists you are working with are not very big, you might not get the performance you expect. For small problems, a quadratic algorithm might be faster than a linear algorithm, or linear might be faster than constant time. And for small problems, the difference probably doesn’t matter.      
(今、貴方が使うlistが小さい場合には思ったよりも性能が悪い時もあります。)      

 - Also, don’t forget about space. So far we have focused on run time, but different implementations require different amounts of space. In an ArrayList, the elements are stored side-by-side in a single chunk of memory, so there is little wasted space, and computer hardware is often faster with contiguous chunks. In a linked list, each element requires a node with one or two links. The links take up space (sometimes more than the data!), and with nodes scattered around in memory, the hardware might be less efficient.   

&nbsp;

1. In summary, analysis of algorithms provides some guidance for choosing data structures, but only if      
2. The run time of your application depends on your choice of data structure, and      
3. The problem size is large enough that the order of growth actually predicts which data structure is better.      

&nbsp;

You could have a long career as a software engineer without ever finding yourself in this situation.      
