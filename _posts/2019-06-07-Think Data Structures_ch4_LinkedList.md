---
layout: post
title:  "Think Data Structures_ch3_ArrayList"
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
