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

3.4 Linked Data Structures     

For the next exercise I provide a partial implementation of the List interface that uses a linked list to store the elements. If you are not familiar with linkedlists, you can read about them at http://thinkdast.com/linkedlist, but this section provides a brief introduction.        

A data structure is “linked” if it is made up of objects, often called “nodes”, that contain references to other nodes. In a linked list, each node contains a reference to the next node in the list. Other linked structures include trees and graphs, in which nodes can contain references to more than one other node.      

Here’s a class definition for a simple node:      
