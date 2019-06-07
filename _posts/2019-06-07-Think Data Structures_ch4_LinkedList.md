---
layout: post
title:  "Think Data Structures_ch3_ArrayList"
date:   2019-06-07 21:25:28 +0900
categories: [Java, Japanese, Algorithms, Data Structure]
---

Chapter 4        

LinkedList        

**4.1 Classifying MyLinkedList methods**       

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

**4.2 Comparing MyArrayList and MyLinkedList**    

The following table summarizes the differences between MyLinkedList and MyArrayList, where 1 means O(1) or constant time and n means O(n) or linear.    
