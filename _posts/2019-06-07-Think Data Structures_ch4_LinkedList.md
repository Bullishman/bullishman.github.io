---
layout: post
title:  "Think Data Structures_ch4_LinkedList"
date:   2019-06-07 21:25:28 +0900
categories: [Java, Japanese, Algorithms, Data Structure]
---

*Chapter 4*        

*LinkedList*        

*4.1 Classifying MyLinkedList methods*       

My implementation of indexOf is below. Read through it and see if you can identify its order of growth before you read the explanation.            

```java
	@Override
	public int indexOf(Object target) { // Solution code
		// 初めでノードを使う。
		Node node = head;
		for (int i=0; i<size; i++) { // リストの最後までforloopを回る。
			if (equals(target, node.data)) {//　targetとデータを比較する。
				return i;
			}
			node = node.next;
		}
		return -1;
	}
```        

Initially node gets a copy of head, so they both refer to the same Node. The loop variable, i, counts from 0 to size-1. Each time through the loop, we use equals to see if we’ve found the target. If so, we return i immediately. Otherwise we advance to the next Node in the list.      

Normally we would check to make sure the next Node is not null, but in this case it is safe because the loop ends when we get to the end of the list (assuming size is consistent with the actual number of nodes in the list).       

1. Each time through the loop we invoke equals, which is constant time (it might depend on the size of target or data, but it doesn’t depend on the size of the list). The other operations in the loop are also constant time.      

2. The loop might run n times, because in the worse case, we might have to traverse the whole list.      

So the run time of this method is proportional to the length of the list.         

Next, here is my implementation of the two-parameter add method. Again, you should try to classify it before you read the explanation.         

```java
	@Override
	public void add(int index, E element) { // Solution code
		// no need to check bounds; getNode does it.
		// 範囲をチェックしなくてもいい;　getNodeがその役割をします。

		//　インデックスが0の場合は、element入れます。
		//　他の場合はインデックス-1で移動して、次のインデックスにelement入れてsizeを1増加します。
		if (index == 0) {
			head = new Node(element, head);
		} else {
			Node node = getNode(index-1);
			node.next = new Node(element, node.next);
		}
		size++;
	}
```         

If index==0, we’re adding the new Node at the beginning, so we handle that as a special case. Otherwise, we have to traverse the list to find the element at index-1. We use the helper method getNode:        

```java
	private Node getNode(int index) {

		// if文の条件を満足させなければ例外を投げます。
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}

		//　入力されたインデックスにノードを入ります。
		Node node = head;
		for (int i=0; i<index; i++) {
			node = node.next;
		}
		return node;
	}
```       

Jumping back to add, once we find the right Node, we create the new Node and put it between node and node.next. You might find it helpful to draw a diagram of this operation to make sure you understand it.         

So, what’s the order of growth for add?       

	1. getNode is similar to indexOf, and it is linear for the same reason.   

	2. In add, everything before and after getNode is constant time.      

So all together, add is linear.       

Finally, let’s look at remove:      

```java
	@Override
	public E remove(int index) { // Solution code

		//　インデックスにあるelementを呼び出します。
		E element = get(index);

		//　インデックスが0の場合は次の値を入れます。
		//　以外の場合はインデックス-1の値を呼び出して、再来値をインデックスに入れます。
		if (index == 0) {
			head = head.next;
		} else {
			Node node = getNode(index-1);
			node.next = node.next.next;
		}

		//　そしてsize減らします。
		size--;
		return element;
	}
```       

remove uses get to find and store the element at index. Then it removes the Node that containedit.          

If index==0, we handle that as a special case again. Otherwise we find the node at index-1 and modify it to skip over node.next and link directly to node.next.next.    
This effectively removes node.next from the list, and it can be garbage collected.   

Finally, we decrement size and return the element we retrieved at the beginning.    

When people see two linear operations, they sometimes think the result is quadratic, but that only applies if one operation is nested inside the other. If you invoke one operation after the other, the run times add. If they are both in O(n), the sum is also in O(n).    

*4.2 Comparing MyArrayList and MyLinkedList*   

The following table summarizes the differences between MyLinkedList and MyArrayList, where 1 means O(1) or constant time and n means O(n) or linear.    

*4.3 Profiling*

For the next exercise I provide a class called Profiler that contains code that runs a method with a range of problem sizes, measures run times, and plots the results.   

You will use Profiler to classify the performance of the add method for the Java implementations of ArrayList and LinkedList.   

Here’s an example that shows how to use the profiler:    

```java
	public static void profileArrayListAddEnd() {
		Timeable timeable = new Timeable() {
			List<String> list;

			public void setup(int n) {
				list = new ArrayList<String>();
			}

			public void timeMe(int n) {
				for (int i=0; i<n; i++) {
					list.add("a string");
				}
			}
		};
		int startN = 4000;
		int endMillis = 1000;
		runProfiler("ArrayList add end", timeable, startN, endMillis);
	}
```

This method measures the time it takes to run **add** on an **ArrayList**, which adds the new element at the end. I’ll explain the code and then show the results.   

In order to use **Profiler**, we need to create a **Timeable** object that provides two methods: **setup and timeMe**. The **setup** method does whatever needs to be done before we start the clock; in this case it creates an empty list. Then **timeMe** does whatever operation we are trying to measure; in this case it adds n elements to the list.   

The code that creates **timeable** is an **anonymous class** that defines a new implementation of the **Timeable** interface and creates an instance of the new class at the same time.    

The next step is to create the **Profiler** object, passing the **Timeable** object and a title as parameters.    

The **Profiler** provides **timingLoop** which uses the **Timeable** object stored as an instance variable. It invokes the **timeMe** method on the **Timeable** object several times with a range of values of n. **timingLoop** takes two parameters:     

- **startN** is the value of n the timing loop should start at.     

- **endMillis** is a threshold in milliseconds. As **timingLoop** increases the problem size, the run time increases; when the run time exceeds this threshold, **timingLoop** stops.      

This code is in ProfileListAdd.java, which you’ll run in the next exercise. When I ran it, I got this output:      

		4000, 3       
		8000, 0      
		16000, 1      
		32000, 2      
		64000, 3
		128000, 6      
		256000, 18      
		512000, 30      
		1024000, 88      
		2048000, 185            
		4096000, 242      
		8192000, 544      
		16384000, 1325      

The first column is problem size, n;       
the second column is run time in milliseconds.       
The first few measurements are pretty noisy; it might have been better to set startN around 64000.      

The result from timingLoop is an XYSeries that contains this data. If you pass this series to plotResults, it generates a plot like the one in Figure 4.1.      

The next section explains how to interpret it.      

4.4 Interpreting results    

Based on our understanding of how ArrayList works, we expect the add method to take constant time when we add elements to the end. So the total time to add n elements should be linear.        

To test that theory, we could plot total run time versus problem size, and we should see a straight line, at least for problem sizes that are big enough to measure accurately. Mathematically, we can write the function for that line:       

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_1.png "Screenshot broadcast")      

Figure 4.1: Profiling results: run time versus problem size for adding n elements to the end of an **ArrayList**.       

runtime = a + b^n       

On the other hand, if **add** is linear, the total time for n adds would be quadratic. If we plot run time versus problem size, we expect to see a parabola. Or mathematically, something like:       

runtime = a + bn + cn^2          

With perfect data, we might be able to tell the difference between a straight line and a parabola, but if the measurements are noisy, it can be hard to tell. A better way to interpret noisy measurements is to plot run time and problem size on a **log-log** scale.   

Why? Let’s suppose that run time is proportional to n k , but we don’t know what the exponent k is. We can write the relationship like this:        

runtime = a + bn + . . . + cn^k       

For large values of n, the term with the largest exponent is the most important, so:     

runtime ≈ cn^k (最後次項)       

where ≈ means “approximately equal”. Now, if we take the logarithm of both sides of this equation:      

log(runtime) ≈ log(c) + k log(n)       

This equation implies that if we plot runtime versus n on a log-log scale, we expect to see a straight line with intercept log(c) and slope k. We don’t care much about the intercept, but the slope indicates the order of growth: if k = 1, the algorithm is linear; if k = 2, it’s quadratic.      
(切片についてはあまり気にしないですが、傾きは増加基準を指すため重要だ。)      

Looking at the figure in the previous section, you can estimate the slope by eye. But when you call plotResults it computes a least squares fit to the data and prints the estimated slope. In this example:       
(plotResultsメソッドは最小二乗法を使って当たる値を出力します。)       

Estimated slope = 1.06194352346708     

which is close to 1; and that suggests that the total time for n adds is linear, so each add is constant time, as expected.     

One important point: if you see a straight line on a graph like this, that does not mean that the algorithm is linear. If the run time is proportional to n^k for any exponent k, we expect to see a straight line with slope k. If the slope is close to 1, that suggests the algorithm is linear. If it is close to 2, it’s probably quadratic.     
(上のような形のグラフを見ても無条件に線形だとは言えない。    
もし,実行時間はn^kで指数k に比例すれば,私たちは傾きk の直線グラフを予測することができる。)     

###4.5 Exercise 4      

In the repository for this book you’ll find the source files you need for this exercise:    

1. Profiler.java contains the implementation of the Profiler class described above. You will use this class, but you don’t have to know how it works. But feel free to read the source.      

2. ProfileListAdd.java contains starter code for this exercise, including the example, above, which profiles ArrayList.add. You will modify this file to profile a few other methods.      

Run ant ProfileListAdd to run ProfileListAdd.java. You should get results similar to Figure 4.1, but you might have to adjust startN or endMillis. The estimated slope should be close to 1, indicating that performing n add operations takes time proportional to n raised to the exponent 1; that is, it is in O(n).      

In ProfileListAdd.java, you’ll find an empty method named profileArrayListAddBeginning. Fill in the body of this method with code that tests ArrayList.add, always putting the new element at the beginning. If you start with a copy of profileArrayListAddEnd, you should only have to make a few changes. Add a line in main to invoke this method.      

Run ant ProfileListAdd again and interpret the results. Based on our understanding of how ArrayList works, we expect each add operation to be linear, so the total time for n adds should be quadratic. If so, the estimated slope of the line, on a log-log scale, should be near 2. Is it?      

Now let’s compare that to the performance of LinkedList. Fill in the body of profileLinkedListAddBeginning and use it to classify LinkedList.add when we put the new element at the beginning. What performance do you expect? Are the results consistent with your expectations?      

Finally, fill in the body of profileLinkedListAddEnd and use it to classify LinkedList.add when we put the new element at the end. What performance do you expect? Are the results consistent with your expectations?      

I’ll present results and answer these questions in the next chapter.      

```java
	/**
	 * Characterize the run time of adding to the end of an ArrayList
	 */
	public static void profileArrayListAddEnd() {
		Timeable timeable = new Timeable() {
			List<String> list;

			public void setup(int n) {
				list = new ArrayList<String>();
			}

			public void timeMe(int n) {
				for (int i=0; i<n; i++) {
					list.add("a string");
				}
			}
		};
		int startN = 4000;
		int endMillis = 1000;
		runProfiler("ArrayList add end", timeable, startN, endMillis);
	}
```

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_2.png "Screenshot broadcast")      
&nbsp;
![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_3.png "Screenshot broadcast")      

```java
	/**
	 * Characterize the run time of adding to the beginning of an ArrayList
	 */
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

	/**
	 * Characterize the run time of adding to the beginning of an ArrayList
	 */
	public static void profileArrayListAddBeginning1() { // My Code
		Timeable timeable = new Timeable() {
			List<String> list;

			public void setup(int n) {
				list = new ArrayList<String>();
			}
			/*
			i = 0 add
			i = 1 add
			i = 2 add


			i = 0 add
			i = 0 add
			i = 0 add
			*/
			public void timeMe(int n) {
				int i = 0;
				for (i = 0; i<n; i++) {
					if (i == 0) {
						list.add("a string");

					}
				}
			}
		};
		int startN = 4000;
		int endMillis = 1000;
		runProfiler("ArrayList add end", timeable, startN, endMillis);
	}

```
![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_4.png "Screenshot broadcast")    
&nbsp;
![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_5.png "Screenshot broadcast")    

```java
	/**
	 * Characterize the run time of adding to the beginning of a LinkedList
	 */
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

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_6.png "Screenshot broadcast")    

&nbsp;

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_7.png "Screenshot broadcast")    


```java
	/**
	 * Characterize the run time of adding to the end of a LinkedList
	 */
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
![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_8.png "Screenshot broadcast")   

&nbsp;

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch4_9.png "Screenshot broadcast")   
