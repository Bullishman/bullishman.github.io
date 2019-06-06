---
layout: post
title:  "Think Data Structures_ch3_ArrayList"
date:   2019-05-30 23:57:28 +0900
categories: [Java, Japanese, Algorithms, Data Structure]
---

**Chapter 3**

**ArrayList**

**3.1 Classifying MyArrayList methods**

For many methods, we can identify the order of growth by examining the code. For example, here’s the implementation of get from MyArrayList:        

```java
	public E get(int index) {
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}
		return array[index];
	}
```         

Everything in get is constant time, so get is constant time. No problem.          

Now that we’ve classified get, we can classify set, which uses it. Here is our implementation of set from the previous exercise:            

```java
	public E set(int index, E element) {
		E old = get(index);
		array[index] = element;
		return old;
	}
```         

One slightly clever part of this solution is that it does not check the bounds of the array explicitly; it takes advantage of get, which raises an exception if the index is invalid.   

Everything in set, including the invocation of get, is constant time, so set is also constant time.     

Next we’ll look at some linear methods. For example, here’s my implementation of indexOf:   

```java
	public int indexOf(Object target) {
		for (int i = 0; i<size; i++) {
			if (equals(target, array[i])) {
				return i;
			}
		}
		return -1;
	}
```        

Each time through the loop, indexOf invokes equals, so we have to classify equals first. Here it is:        

```java
	private boolean equals(Object target, Object element) {
		if (target == null) {
			return element == null;
		} else {
			return target.equals(element);
		}
	}
```        

&nbsp;

  多くのメソッドで,私たちはコードを例にして作動順序を把握することができる
  たとえば,次のようにMyArrayListのgetメソッドの作動がある。    

```java
  public E get(int index) {
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}
		return array[index];
	}
```       

  全ての場合,get は定数時間である。したがって,getメソッドは定数時間である。
  今回は我々がgetを分類したように,我々はgetメソッドを使用するsetメソッドを分類することができる。
  ここ,以前の例示でsetメソッドの実行コードがある。      

```java
  public E set(int index, E element) {
		E old = get(index);
		array[index] = element;
		return old;
	}
```      

  この解決法において,かなり賢明な部分は明快に配列の範囲をチェックする必要がないということだ。
  これはインデックスが有効でなければ,例外を発生させるという長所がある。       

  次に,私たちは船型メソッドを見てみよう。 一例として,次は著者のインデックスOfの実行文である。   

```java
  public int indexOf(Object target) {
		for (int i = 0; i<size; i++) {
			if (equals(target, array[i])) {
				return i;
			}
		}
		return -1;
	}
```    

  ループ文が作動するたびに,インデックスOf はequals を呼び出す,だから私たちはequals を先に分類する。 次のように:      

```java
  private boolean equals(Object target, Object element) {
		if (target == null) {
			return element == null;
		} else {
			return target.equals(element);
		}
	}
```       

---

This method invokes target.equals; the run time of this method might depend on the size of target or element, but it probably doesn’t depend on the size of the array, so we consider it constant time for purposes of analyzing indexOf.     

Getting back to indexOf, everything inside the loop is constant time, so the next question we have to consider is: how many times does the loop execute?       

If we get lucky, we might find the target object right away and return after testing only one element. If we are unlucky, we might have to test all of the elements. On average, we expect to test half of the elements, so this method is considered linear (except in the unlikely case that we know the target element is at the beginning of the array).     

The analysis of remove is similar. Here’s my implementation:      

```java
	public T remove(int index) {
		T element = get(index);
		for (int i=index; i<size-1; i++) {
			array[i] = array[i+1];
		}
		size--;
		return element;
	}
```

It uses get, which is constant time, and then loops through the array, starting from index. If we remove the element at the end of the list, the loop never runs and this method is constant time. If we remove the first element, we loop through all of the remaining elements, which is linear. So, again, this method is considered linear (except in the special case where we know the element is at the end or a constant distance from the end).  

&nbsp;

  このメソッドは,terget.equals を呼び出す; このメソッドの実行時間は,target やelement の大きさに影響されるが,配列の大きさに影響されないだろう,だから私たちは,indexOf メサードを分析する目的は定数時間であるからであるといえる。   

  indexOfに戻って,ループのすべてのコードは定数時間である,だからこそ私たちが考えるべき次の質問は:何度もループ文が実行されたかである。   

  もし,私たちが幸運だったら,私たちは一つの要素をテストした後,目標オブジェクトをすぐ探して返還することもできる。 運が悪かったら,私たちはすべての要素を試してみなければならないかもしれない。
  平均的に,私たちはすべての要素の半分をテストすればよいと予測でき,そのためこのメソッドは線形時間とみなされる。 配列の一番目に目標要素が存在することを知る,起こることがほとんどない場合を除いて,   

  removeメソッドの分析もほぼ似ている。 次は著者の実行コードだ    

  ```java
  	public T remove(int index) {
  		T element = get(index);
  		for (int i=index; i<size-1; i++) {
  			array[i] = array[i+1];
  		}
  		size--;
  		return element;
  	}
  ```

  上のメソッドは定数時間である getメソッドを使って indexから始める配列を繰り返す。
  もし私たちがリストの最後の要素を削除すると,ループは作動せず,このメサードは定数時間になる。 私たちが第一の要素を削除すると,私たちは,線形時間の残りの要素を繰り返す。
  そう,再び,このメソッドは,線形時間とみなされる(最後のインデックスに要素が存在することを知ったり, 配列の最後の部分からの一定距離を知る特別場合を除く)      

---

**3.2 Classifying add**

Here’s a version of add that takes an index and an element as parameters:    

```java
	public void add(int index, T element) {
		if (index < 0 || index > size) {
			throw new IndexOutOfBoundsException();
		}
		// add the element to get the resizing
		add(element);

		// shift the elements
		for (int i=size-1; i>index; i--) {
			array[i] = array[i-1];
		}
		// put the new one in the right place
		array[index] = element;
	}
```    

This two-parameter version, called add(int, E), uses the one-parameter version, called add(E), which puts the new element at the end. Then it shifts the other elements to the right, and puts the new element in the correct place.   

Before we can classify the two-parameter add(int, E), we have to classify the one-parameter add(E):     

```java
	public boolean add(T element) {
		if (size >= array.length) {
			// make a bigger array and copy over the elements
			@SuppressWarnings("unchecked")
			T[] bigger = (T[]) new Object[array.length * 2];
			System.arraycopy(array, 0, bigger, 0, array.length);
			array = bigger;
		}
		array[size] = element;
		size++;
		return true;
	}
```     

&nbsp;

3.2 Classifying add

次のインデックスとパラメータ要素をもたらすaddメソッドバージョンがある     

```java
	public void add(int index, T element) {
		if (index < 0 || index > size) {
			throw new IndexOutOfBoundsException();
		}
		// add the element to get the resizing
		add(element);

		// shift the elements
		for (int i=size-1; i>index; i--) {
			array[i] = array[i-1];
		}
		// put the new one in the right place
		array[index] = element;
	}
```    

これらの複数パラメータバージョンは,add(int,E)と呼ばれ,単数パラメータバージョンはadd(E)と呼ばれる。
また,二つのメソッドは新しい要素を入れる。 そして,二つのメソッドは他の要素を右に移動させ,新しい要素を適した場所に入れる。     

我々が複数パラメータバージョンであるadd(int, E)を分類するために,単数パラメータバージョンであるadd(E)を分類しなければならない。      

```java
	public boolean add(T element) {
		if (size >= array.length) {
			// make a bigger array and copy over the elements
			@SuppressWarnings("unchecked")
			T[] bigger = (T[]) new Object[array.length * 2];
			System.arraycopy(array, 0, bigger, 0, array.length);
			array = bigger;
		}
		array[size] = element;
		size++;
		return true;
	}
```

---

The one-parameter version turns out to be hard to analyze. If there is an unused space in the array, it is constant time, but if we have to resize the array, it’s linear because System.arraycopy takes time proportional to the size of the array.     

So is add constant time or linear? We can classify this method by thinking about the average number of operations per add over a series of n adds. For simplicity, assume we start with an array that has room for 2 elements.         

1. The first time we call add, it finds unused space in the array, so it stores 1 element.   

2. The second time, it finds unused space in the array, so it stores 1 element.    

3. The third time, we have to resize the array, copy 2 elements, and store 1 element. Now the size of the array is 4.      

4. The fourth time stores 1 element.       

5. The fifth time resizes the array, copies 4 elements, and stores 1 element. Now the size of the array is 8.        

6. The next 3 adds store 3 elements.        

7. The next add copies 8 and stores 1. Now the size is 16.        

8. The next 7 adds store 7 elements.      

And so on. Adding things up:        

- After 4 adds, we’ve stored 4 elements and copied 2.       

- After 8 adds, we’ve stored 8 elements and copied 6.       

- After 16 adds, we’ve stored 16 elements and copied 14.       

To get the average number of operations per add, we divide the total by n; the result is 2 − 2/n. As n gets big, the second term, 2/n, gets small. Invoking the principle that we only care about the largest exponent of n, we can think of add as constant time.       

It might seem strange that an algorithm that is sometimes linear can be constant time on average. The key is that we double the length of the array each time it gets resized. That limits the number of times each element gets copied. Otherwise — if we add a fixed amount to the length of the array, rather than multiplying by a fixed amount — the analysis doesn’t work.        

This way of classifying an algorithm, by computing the average time in a series of invocations, is called amortized analysis. You can read more about it at http://thinkdast.com/amort. The key idea is that the extra cost of copying the array is spread, or “amortized”, over a series of invocations.         

Now, if add(E) is constant time, what about add(int, E)? After calling add(E), it loops through part of the array and shifts elements. This loop is linear, except in the special case where we are adding at the end of the list. So add(int, E) is linear.         

&nbsp;

1. addを呼び出すと、未使用空間を配列で訪れた後、1つの要素を保存します(1/2)        

2. 配列で未使用空間を探して、1つの要素を保存します(2/2)         

3. の配列をリサイズしなければなりません。 既存の2つの要素をコピーして新しい1つの要素を保存します
配列の大きさを4で伸ばします(3/4)       

4. 1つの要素を保存します(4/4)      

5. 配列の大きさを調整して、既存の4つの要素をコピーして、1つの要素を保存します。
現在、配列の大きさは8です(5/8)        

6. 3度のadd要サード・実行から3つの要素を保存します (8/8)        

7. 従来の8つの要素をコピーした後、1つの要素を保存します。 もう大きさは16です(9/16)      

8. 7度のadd要サード・実行から7つの要素を保存します(16/16)        

これはパターンを見なければなりません:nを追加するにはn個の要素を保存してn-2をコピーしなければなりません。 従って、総演算数はn+n-2であり、2n-2です。
add一つに就いて平均作業数を得るために合計をn に分けます。 結果は2-2/nです。 nが大きくなるほど、二番目の用語だった2/nは小さくなります。 私たちがn の最も大きな指数に対してのみ気を使う原理を呼び出せば,足し算を一定時間として考えることができます。         

時には線形であるアルゴリズムが平均的に上数時間になることがあることがおかしく見えることがあります。 核心は、大きさが調整されるたびに配列の長さを二倍に増やすことです。 これは各要素がコピーされる回数を制限します。
そうでなければ,-固定された量を掛け合わせるよりも,配列の長さに固定した量を加えれば分析が作動しません。           

一連の呼び出しで平均時間を計算し,アルゴリズムを分類するこれらの方式を償却分析といいます。 詳しい内容は http://thinkdast.com/amortで確認できます。 核心概念は一連の呼び出しに対し配列をコピーするのにかかる追加費用が分散されたり,"償却されます"です。        

これからadd (E)が一定時間なら add (int, E)はどうでしょうか。 電話後
アドバンスを実行すると,配列の一部を繰り返し,要素を移動します。 このループは,リストの最後に追加する特別な場合を除いては,線形です。         
それでadd (int, E)は線形です。         

---

3.3 Problem Size        

```java
	public boolean removeAll(Collection<?> collection) {
		boolean flag = true;
		for (Object obj: collection) {
			flag &= remove(obj);
		}
		return flag;
	}
```

Each time through the loop, removeAll invokes remove, which is linear. So it is tempting to think that removeAll is quadratic. But that’s not necessarily the case.      

In this method, the loop runs once for each element in collection. If collection contains m elements and the list we are removing from contains n elements, this method is in O(nm). If the size of collection can be considered constant, removeAll is linear with respect to n. But if the size of the collection is proportional to n, removeAll is quadratic. For example, if collection always contains 100 or fewer elements, removeAll is linear. But if collection generally contains 1% of the elements in the list, removeAll is quadratic.   

When we talk about problem size, we have to be careful about which size, or sizes, we are talking about. This example demonstrates a pitfall of algorithm analysis: the tempting shortcut of counting loops. If there is one loop, the algorithm is often linear. If there are two loops (one nested inside the other), the algorithm is often quadratic. But be careful! You have to think about how many times each loop runs. If the number of iterations is proportional to n for all loops, you can get away with just counting the loops. But if, as in this example, the number of iterations is not always proportional to n, you have to give it more thought.   

&nbsp;
&nbsp;

3.3 Problem Size        

```java
	public boolean removeAll(Collection<?> collection) {
		boolean flag = true;
		for (Object obj: collection) {
			flag &= remove(obj);
		}
		return flag;
	}
```     

removeAllが実行されるたびに線形時間のremoveを呼び出す。 そのためにremoveAllが二次時間と考えられる可能性もある。 でも,いつもそうではない。        

もし,collectionがm 要素を含み,削除しようとするlist がn 要素を含めるなら,このメソッドはO(nm)である。         
そしてcollectionのサイズが定数時間であれば,removeAllは線形時間である。        
しかし,collectionのサイズがnに比例すると,removeAllは二次時間である。        

上記例は,アルゴリズム分析の潜在的危険を示す:繰り返し実行文を数える時,最も簡単な方法を試みることを。 一度の繰り返しの時は概ねアルゴリズムは線形の形であり、二度の繰り返しである時は普通二次形式である。 でも,上のようにいつもそうなのではないので,気をつけなければならない。        

---

3.4 Linked Data Structures     

For the next exercise I provide a partial implementation of the List interface that uses a linked list to store the elements. If you are not familiar with linkedlists, you can read about them at http://thinkdast.com/linkedlist, but this section provides a brief introduction.        

A data structure is “linked” if it is made up of objects, often called “nodes”, that contain references to other nodes. In a linked list, each node contains a reference to the next node in the list. Other linked structures include trees and graphs, in which nodes can contain references to more than one other node.      

Here’s a class definition for a simple node:      

```java
public class ListNode {

	public Object data;
	public ListNode next;

	public ListNode() {
		this.data = null;
		this.next = null;
	}

	public ListNode(Object data) {
		this.data = data;
		this.next = null;
	}

	public ListNode(Object data, ListNode next) {
		this.data = data;
		this.next = next;
	}

	public String toString() {
		return "ListNode(" + data.toString() + ")";
	}
}
```

The ListNode object has two instance variables: data is a reference to some kind of Object, and next is a reference to the next node in the list. In the last node in the list, by convention, next is null.      

You can think of each ListNode as a list with a single element, but more generally, a list can contain any number of nodes. There are several ways to make a new list. A simple option is to create a set of ListNode objects, like this:        

ListNode node1 = new ListNode(1);         
ListNode node2 = new ListNode(2);         
ListNode node3 = new ListNode(3);        

And then link them up, like this:        

node1.next = node2;       
node2.next = node3;        
node3.next = null;          


Alternatively, you can create a node and link it at the same time. For example, if you want to add a new node at the beginning of a list, you can do it like this:       

ListNode node0 = new ListNode(0, node1);      

After this sequence of instructions, we have four nodes containing the Integers 0, 1, 2, and 3 as data, linked up in increasing order. In the last node, the next field is null.    

&nbsp;
&nbsp;

The ListNode オブジェクトは,二つのインスタンスの変数を有します:dataはObjectの一部に対する参照であり,nextはリスト内の次のノードに対する参照です。

各リストNodeを単一の要素があるリストとすることができますが,一般的に,リストには任意の数のノードが含まれることがあります。 新しい目録を作る方法にはいろいろあります。 簡単なオプションは,以下のように一連のListNodeオブジェクトを作るものです。   

ListNode node1 = new ListNode(1);    
ListNode node2 = new ListNode(2);    
ListNode node3 = new ListNode(3);     

そして,次のように縛れる:     

node1.next = node2;      
node2.next = node3;      
node3.next = null;     

またはノードを作って同時に繋げることができます。 たとえば,リストスタート部分に新しいノードを追加するには,以下のようにすることができます。     

ListNode node0=new ListNode(0、node1);     

この命令シーケンス次に浄水0、1、2及び3をデータで含む4つのノードが増加順に連結されます。 最後のノードで次のフィールドはnullです。     

---

3.5 Exercise 3

In the repository for this book, you’ll find the source files you need for this exercise:

- MyLinkedList.java contains a partial implementation of the List interface using a linked list to store the elements.

- MyLinkedListTest.java contains JUnit tests for MyLinkedList

Run ant MyArrayList to run MyArrayList.java, which contains a few simple tests.

Then you can run ant MyArrayListTest to run the JUnit tests. Several of them should fail. If you examine the source code, you’ll find three TODO comments indicating the methods you should fill in.

Before you start, let’s walk through some of the code. Here are the instance variables and the constructor for MyLinkedList:

```java
	public class MyLinkedList<E> implements List<E> {

		private int size; // keeps track of the number of elements
		private Node head; // reference to the first node

		public MyLinkedList() {
		head = null;
		size = 0;
		}
	}
```

&nbsp;
&nbsp;

この本のレポジットで,貴方は次の例に必要なソースファイルを探せます。

- MyLinkedList.javaは,要素を保存するためのa linked listを使用するリストインタフェースの部分的実行を含みます。
- MyLinkedListTest.javaは,MyLinkedListのためのJunittestsを含みます。

いくつかのテストを含むMyArrayList.javaを実行してください。 何人かのメソッドは失敗するでしょう。あなたがソースコードを例にとれば,あなたは,三つのTODO commentsのある貴方が満たしなければならないメソッドを見つけます。

詰め込む前に,コードの一部分を見てみましょう。 ここにMyLinkedListのインスタンス変数と生成者がいます。

```java
public class MyLinkedList<E> implements List<E> {

	private int size; // 要素の数量を記します。
	private Node head; //　初めのノードを参照します。

	public MyLinkedList() {
	head = null;
	size = 0;
	}
}
```

---

But if we store size explicitly, we can implement the size method in constant time; otherwise, we would have to traverse the list and count the elements, which requires linear time.

Because we store size explicitly, we have to update it each time we add or remove an element, so that slows down those methods a little, but it doesn’t change their order of growth, so it’s probably worth it.

The constructor sets head to null, which indicates an empty list, and sets size to 0.

The type parameter also appears in the definition of Node, which is nested inside MyLinkedList:

```java
private class Node {
		public E data;
		public Node next;

		public Node(E data, Node next) {
			this.data = data;
			this.next = next;
		}
	}
```

Finally, here’s my implementation of add:

```java
	public boolean add1(E element) {
		if (head == null) {
			head = new Node(element);
		} else {
			Node node = head;
			// loop until the last node
			for ( ; node.next != null ; node = node.next) {}
			node.next = new Node(element);
		}

		return true;
	}
```

This example demonstrates two patterns you’ll need for your solutions:

1. For many methods, we have to handle the first element of the list as a special case. In this example, if we are adding the first element of a list, we have to modify head. Otherwise, we traverse the list, find the end, and add the new node.

2. This method shows how to use a for loop to traverse the nodes in a list. In your solutions, you will probably write several variations on this loop. Notice that we have to declare node before the loop so we can access it after the loop.

Now it’s your turn. Fill in the body of indexOf.
In particular, notice how it’s supposed to handle null.

I provide a helper method called equals and it handles null correctly.
This method is private because it is used inside this class but it is not part of the List interface.

Next, you should fill in the two-parameter version of add, which takes an index and stores the new value at the given index.

Last one: fill in the body of remove.

&nbsp;
&nbsp;

そして,彼らがサイズを明示的に保存するなら,私たちは,定数時間にサイズを実行することができます。
その意味は,私たちは要求される線形時間に要素の個数を数えてリストを旋回しなければならないということです。

大きさを明示的に保存するため,要素を追加したり除去するたびにアップデートしなければならないため,このような方法の速度は多少遅くなるが,入力手順は変更されないのでそれなりの価値があります。

生成者はheadをnullに設定して空いているリストを示し、大きさを0に設定します。

また,タイプ媒介変数はMyLinkedList内部に重畳されたNode定義に示されます。

```java
private class Node
		public E data;
		public Node next;

		public Node(E data, Node next) {
			this.data=data;
			this.next=next;
		}
	}
```

最後に,次は著者のアドドメソッドである。

```java
	public boolean add1(E element){
		if (head=null) {
			head=new Node(element);
		else
			Node node = head;
			//　最後のノードまで繰り返します。
			for (; node.next!= null; node= node.next) {}
			node.next=new Node(element);
		}

		return true;
	}
```

一例として、ソリューションに必要な二つのパターンを見せます。

1.多くのメソッドに対して、私は目録の最初の要素を特別ケースとして扱わなければならない。 この例では,リストの最初の要素を追加する場合は,head を修正する必要があります。 そうでなければ,リストを探索して終りを見出し,新しいノードを追加します。

2.このメソッドはどうforループを使用してリストのノードを探索しているかを示してくれます。 ソリューションでこのループにさまざまな変数を作成することができます。 ループ前にノードを宣言するとループにアクセスできます。

では,あなたの順番です。インデックスOfの本文を埋めてみてください。
特にnullを処理する方法に注意してください。

私はequalsというヘルパーメソッドを提供し,このメソドは正しくnullを処理します。
このメソッドは,このクラスの内部で使われるため非公開ですが,リストインターフェースの一部ではありません。

次に,索引をとり,与えられた索引に新しい値を保存するaddの二枚変数バージョンのメソッドを埋めなければなりません。

最後:removeメソッドの胴体を満たしてください。

**3.6 A note on garbage collection**

In MyArrayList from the previous exercise, the array grows if necessary, but it
never shrinks. The array never gets garbage collected, and the elements don’t
get garbage collected until the list itself is destroyed.      

One advantage of the linked list implementation is that it shrinks when elements are removed, and the unused nodes can get garbage collected immediately.      

Here is my implementation of the clear method:

```java
public void clear() {
	head = null;
	size = 0;
}
```

When we set head to null, we remove a reference to the first Node. If there are
no other references to that Node (and there shouldn’t be), it will get garbage
collected. At that point, the reference to the second Node is removed, so it gets
garbage collected, too. This process continues until all nodes are collected.   

So how should we classify clear? The method itself contains two constant
time operations, so it sure looks like it’s constant time. But when you invoke
it, you make the garbage collector do work that’s proportional to the number
of elements. So maybe we should consider it linear!   

This is an example of what is sometimes called a performance bug: a program that is correct in the sense that it does the right thing, but it doesn’t
belong to the order of growth we expected. In languages like Java that do a
lot of work, like garbage collection, behind the scenes, this kind of bug can be
hard to find.   

```java
@Override
public void add(int index, E element) { // Solution code
	// no need to check bounds; getNode does it.
	if (index == 0) {
		head = new Node(element, head);
	} else {
		Node node = getNode(index-1);
		node.next = new Node(element, node.next);
	}
	size++;
}

public void add1(int index, E element) { // My code
	if (index < 0 || index > size) {
	} else {
		size++;
		Node node = head;
		for ( ; index < size; index++) {
			if (node.next != null) {
				node.next = node;
			}
		}
		node.next = new Node(element);
	}
}
```          

```java
@Override
public int indexOf(Object target) { // Solution code
	Node node = head;
	for (int i=0; i<size; i++) {
		if (equals(target, node.data)) {
			return i;
		}
		node = node.next;
	}
	return -1;
}

public int indexOf1(Object target) { // My code
	Node node = head;
	for (int i=0; i<size; i++) {
		if (target.equals(node.data)) {
			return i;
		}
	}
	return -1;
}
```

```java
@Override
public E remove(int index) { // Solution code
	E element = get(index);
	if (index == 0) {
		head = head.next;
	} else {
		Node node = getNode(index-1);
		node.next = node.next.next;
	}
	size--;
	return element;
}

public E remove1(int index) { // My code
	if (index == 0) {
		head = head.next;
	} else {
		Node node = getNode(index);
		node.data = null;
	}
	size--;
	return null;
}
```
