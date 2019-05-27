---
layout: post
title:  "Think Data Structures_ch2_Analysis of Algorithms"
date:   2019-05-27 22:08:28 +0900
categories: [Java, Japanese, Algorithms, Data Structure]
---

Chapter 2      
Analysis of Algorithms      

To decide which one is better for a particular application, one approach is to try them both and see how long they take. This approach, which is called “profiling”, has a few problems:          

1. Before you can compare the algorithms, you have to implement them both.        

2. The results might depend on what kind of computer you use. One algorithm might be better on one machine; the other might be better on a different machine.         

3. The results might depend on the size of the problem or the data provided as input.       

特定のアプリケーションでどれが早いかを決定するための一つの方法で二つの方法を試みてからどれだけ時間がかかるのか見るのである。 この方法は,いわゆるprofilingと呼び,次の特徴を持つ。       

  1. アルゴリズムを比較する前二つを具現化してみなさい        

  2. 結果はあなたがどんな種類のコンピューター(どんなレベルの性能を持つコンピュータ)を使用するかによって変わるだろう。 一つのコンピュータで特定のアルゴリズムが考えられ, 他のコンピュータで他のアルゴリズムが良いこともある。       

  3. 結果は提供されるinputによるデータと問題の大きさによって変わるだろう。        


We can address some of these problems using analysis of algorithms. When it works, algorithm analysis makes it possible to compare algorithms without having to implement them. But we have to make some assumptions:   

1. To avoid dealing with the details of computer hardware, we usually identify the basic operations that make up an algorithm — like addition, multiplication, and comparison of numbers — and count the number of operations each algorithm requires.        

2. To avoid dealing with the details of the input data, the best option is to analyze the average performance for the inputs we expect. If that’s not possible, a common alternative is to analyze the worst case scenario.      

3. Finally, we have to deal with the possibility that one algorithm works best for small problems and another for big ones. In that case, we usually focus on the big ones, because for small problems the difference probably doesn’t matter, but for big problems the difference can be huge.        

我々はアルゴリズムを分析することで,これらの特長について調べることができる。             

  1. コンピューターのハードの細部事項を取り扱うことを避けるため、私たちは普通アルゴリズムを構成する基本演算を確認するー加算,乗算,数字の比較そして各アルゴリズムで要求される演算の数を数えることなど              

  2. input dataの細部事項を取り扱うことを避けるため、最高のオプションは予想されるinputsの平均性能を測定することだ。 それができないなら, 一般的な次善策は最悪のシナリオを分析することだ            

  3. 最後に私たちは大小の問題に対する最高のアルゴリズムの可能性について取り上げなければならない。
  このような場合,私たちは普通大きいことに焦点を合わせる。なぜなら,小さな問題に対しては小さな差が問題にならないこともあるし,しかし,大きな問題に対しては小さな差が大きい問題になるかもしれない。          

This kind of analysis lends itself to simple classification of algorithms. For example, if we know that the run time of Algorithm A tends to be proportional to the size of the input, n, and Algorithm B tends to be proportional to n 2 , we expect A to be faster than B, at least for large values of n.           

Most simple algorithms fall into just a few categories.             

- Constant time: An algorithm is “constant time” if the run time does not depend on the size of the input. For example, if you have an array of n elements and you use the bracket operator ([]) to access one of the elements, this operation takes the same number of operations regardless of how big the array is.       

- Linear: An algorithm is “linear” if the run time is proportional to the size of the input. For example, if you add up the elements of an array, you have to access n elements and perform n − 1 additions. The total number of operations (element accesses and additions) is 2n − 1, which is proportional to n.     

- Quadratic: An algorithm is “quadratic” if the run time is proportional to n 2 . For example, suppose you want to check whether any element in a list appears more than once. A simple algorithm is to compare each element to all of the others. If there are n elements and each is compared to n − 1 others, the total number of comparisons is n 2 − n, which is proportional to n 2 as n grows.       

これらの分析はアルゴリズムの分類に適している。 たとえばinput の大きさによってアルゴリズムの速度を比較して知ることができる。       

ほとんどの簡単なアルゴリズムは以下の分類に分かれる。          

- 固定時間:実行時間が入力値の大きさによって変わるのでなければ,次のアルゴリズムは固定時間である。 例えば,貴方が要素の配列を有しているならば,bracket operator ([])を要素中の一つに接近するのに用いる,この作業は配列がどれほど大きいかに関わらず,同じ数字をもたらす。     

- 線形:アルゴリズムが線形形態日頃の右は,入力値の大きさから実行時間が比例する。
例えば、あなたが配列に要素を追加すると、n要素に接近しなければばかりしてn-1の合算が形成される。
作業の回数(アクセス要素と合算)はnに比例する2n-1       

-二次:アルゴリズムが二次形である際に実行時間はn^2に比例する。 例えば,あなたが一回以上リスト要素を確認したいと仮定できる。 簡単なアルゴリズムは他の全ての要素に各要素を比較することである。 n要素と他のそれぞれの要素がn-1などと比較されるとき、比較の回数はnの増加に比例して上昇するn^2ほどn^2–nになる      

---

** 2.1 Selection sort **    

```java
package ch2;

import java.util.Arrays;

/**
 * @author downey
 *
 */
public class SelectionSort {

	/**
	 * Swaps the elements at indexes i and j.
	 *
	 * インデックスiとjの配列の要素を交換する。
	 */
	public static void swapElements(int[] array, int i, int j) {
		int temp = array[i];
		array[i] = array[j];
		array[j] = temp;
	}

	/**
	 * Finds the index of the lowest value
	 * between indices low and high (inclusive).
	 *
	 * ダイスの数字から一番小さいインデックスを探す。
	 */
	public static int indexLowest(int[] array, int start) {
		int lowIndex = start;
		for (int i = start; i < array.length; i++) {
			if (array[i] < array[lowIndex]) {
				lowIndex = i;
			}
		}
		return lowIndex;
	}

	/**
	 * Sorts the cards (in place) using selection sort.
	 *
	 * selectionSortを使って配列を昇順に整列
	 */
	public static void selectionSort(int[] array) {
		for (int i = 0; i < array.length; i++) {
			int j = indexLowest(array, i);
			swapElements(array, i, j);
//			swapElements(array, 0, 3);
//			swapElements(array, 1, 3);
//			swapElements(array, 2, 4);
//			swapElements(array, 3, 3);
//			swapElements(array, 4, 4);
		}
	}

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		int[] array = {2, 5, 6, 1, 3};
		selectionSort(array);
//		[2, 5, 6, 1, 3]
//		[1, 5, 6, 2, 3]
//		[1, 2, 6, 5, 3]
//		[1, 2, 3, 5, 6]

		System.out.println(Arrays.toString(array));
	}


}
```

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/1_ch2.png "Screenshot broadcast")   


The first method, swapElements, swaps two elements of the array        

The second method, indexLowest, finds the index of the smallest element of the array starting at a given index, start. Each time through the loop, it accesses two elements of the array and performs one comparison. Since these are all constant time operations, it doesn’t really matter which ones we count. To keep it simple, let’s count the number of comparisons.        

  1. If start is 0, indexLowest traverses the entire array, and the total number of comparisons is the length of the array, which I’ll call n.         

  2. If start is 1, the number of comparisons is n − 1.        

  3. In general, the number of comparisons is n - start, so indexLowest is linear.         

The third method, selectionSort, sorts the array. It loops from 0 to n−1, so the loop executes n times. Each time, it calls indexLowest and then performs a constant time operation, swapElements.      

The first time indexLowest is called, it performs n comparisons. The second time, it performs n − 1 comparisons, and so on. The total number of comparisons is   

n + n − 1 + n − 2 + ... + 1 + 0      

The sum of this series is n(n + 1)/2, which is proportional to n 2 ; and that means that selectionSort is quadratic       

To get to the same result a different way, we can think of indexLowest as a nested loop. Each time we call indexLowest, the number of operations is proportional to n. We call it n times, so the total number of operations is proportional to n 2 .          

最初のメソドゥインswapElementsは、配列の二つの要素を交換する。 私たちは配列の大きさと第一インデックスのスタート位置がわかれば,追加掛け算や合算を含む他の要素の位置が計算できるので,固定時間の作業であることが分かる。        

二番目メソドゥインindexLowestは与えられたインデックスのstartから始まり、配列の一番小さな要素のインデックスを探すことができる。 反復文の各視点から,配列の二つの要素に接近して一回の比較を行う。 これらはすべて固定時間作業であるため,どのような要素を計算するかは関係ない。          

1. 0から始めると、indexLowestはすべての配列を横断する高炉比較回数は、配列の長さ並。     
これはnとよばれる       

2. 1から始めると、比較回数はn-1      

3. 一般的に、比較回数がn–startには、indexLowestは、線形形態である      

第三のメソッドであるselectionSortは,配列をまとめる。 0からn-1までn得た施行される。       
各時点において,indexLowest と呼ばれ,固定時間作業であるswapElements を実行する。       

最初の時点でindexLowestは比較をn番施行し、二つ目の時点ではn-1藩施行する。        
総比較回数は        

n+n−1+n−2+... + 1 + 0         

つまりn(n+1)/2だ。 そしてn^2と比例してselectionSortは二次形態であることを意味する          

---

** 2.2 Big O notation **      

All constant time algorithms belong to a set called O(1). So another way to say that an algorithm is constant time is to say that it is in O(1). Similarly, all linear algorithms belong to O(n), and all quadratic algorithms belong to O(n^2). This way of classifying algorithms is called “big O notation”.   

This notation provides a convenient way to write general rules about how algorithms behave when we compose them. For example, if you perform a linear time algorithm followed by a constant algorithm, the total run time is linear. Using ∈ to mean “is a member of”:        
If f ∈ O(n) and g ∈ O(1), f + g ∈ O(n).       

If you perform two linear operations, the total is still linear:     
If f ∈ O(n) and g ∈ O(n), f + g ∈ O(n).       

In fact, if you perform a linear operation any number of times, k, the total is linear, as long as k is a constant that does not depend on n.         
If f ∈ O(n) and k is a constant, kf ∈ O(n).        

But if you perform a linear operation n times, the result is quadratic:       
If f ∈ O(n), nf ∈ O(n^2).          

In general, we only care about the largest exponent of n. So if the total number of operations is 2n + 1, it belongs to O(n). The leading constant, 2, and the additive term, 1, are not important for this kind of analysis. Similarly, n 2 + 100n + 1000 is in O(n 2 ). Don’t be distracted by the big numbers!        

“Order of growth” is another name for the same idea. An order of growth is a set of algorithms whose run times are in the same big O category; for example, all linear algorithms belong to the same order of growth because their run times are in O(n).        

In this context, an “order” is a group, like the Order of the Knights of the Round Table, which is a group of knights, not a way of lining them up. So you can imagine the Order of Linear Algorithms as a set of brave, chivalrous, and particularly efficient algorithms.             

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/2_ch2.png "Screenshot broadcast")   

Big-O Complexity Chart: http://bigocheatsheet.com/       

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/3_ch2.png "Screenshot broadcast")   

** 2.2 Big O notation **   

すべての定数時間アルゴリズムはO(1)に含まれる。  
同様に、すべての線形アルゴリズムはO(n)に含まれ、すべての2次アルゴリズムはO(n^2)に含まれる    
これらのアルゴリズムの分類方式は"big O notation"と言われる。      

このような表記法は,アルゴリズムが動作する時にどのように構成されるかに対する一般的な法則を実装するのに便利な方法である。 例えば,定数アルゴリズムが伴う線形アルゴリズムを作る時,総合実行時間は線形形態である。(a linear algorithm with a constant algorithm)      
If f ∈ O(n) と g ∈ O(1), f + g ∈ O(n).      

あなたが二つの線形アルゴリズムを具現化する際の、総合実行時間は依然として(two linear algorithm)       
If f ∈ O(n) と g ∈ O(n), f + g ∈ O(n).  

実際,線形演算を複数回行いますと,総示度回数kは線形形態でありnに依存しない定数形態です(a constant algorithm that does not depend on n and linear algorithm)      
If f ∈ O(n) と k は 定数, kf ∈ O(n).  

また、貴方が線形演算をn回の実行する場合、結果は2回目の形態である       
If f ∈ O(n), nf ∈ O(n^2).   

一般的に,私たちは最も大きな指数であるn に対してのみ気を使う。 また、総演算回数が2n+1度ある場合、これは、O(n)だ。    

---

** 2.3 Exercise 2 **    

The exercise for this chapter is to implement a List that uses a Java array to store the elements            

- MyArrayList.java contains a partial implementation of the List interface. Four of the methods are incomplete; your job is to fill them in.     

- MyArrayListTest.java contains JUnit tests you can use to check your work.      

Before you start filling in the missing methods, let’s walk through some of the code. Here are the class definition, instance variables, and constructor.     

```java
	public class MyArrayList<E> implements List<E> {
		int size; // keeps track of the number of elements
		private E[] array; // stores the elements

		public MyArrayList() {
		array = (E[]) new Object[10];
		size = 0;
		}
	}
```

今回のチャプターでの演習は,Java配列で要素を保存するためのa Listを実装するためのものである。       

- MyArrayList.javaは,List interface構弁部の一部を含む。 4つのメソッドが未完成状態だ。       
あなたの任務はそれを完成することだ       

- MyArrayListTest.javaはJunitテストを含めるのであなたはまともな完成状態かどうか確認できる。     

未完成メソッドを具現する前に,いくつかのコードをひっかかえよう。       
ここクラスの定義,インスタンスの変数,そして生成者がいる。  

```java
	public class MyArrayList<E> implements List<E> {
		int size; // keeps track of the number of elements
		private E[] array; // stores the elements

		public MyArrayList() {
		array = (E[]) new Object[10];
		size = 0;
		}
	}
```   

---

As the comments indicate, size keeps track of how many elements are in MyArrayList, and array is the array that actually contains the elements.

The constructor creates an array of 10 elements, which are initially null, and sets size to 0. Most of the time, the length of the array is bigger than size, so there are unused slots in the array.

One detail about Java: you can’t instantiate an array using a type parameter; for example, the following will not work:
```java
array = new E[10];
```

To work around this limitation, you have to instantiate an array of Object and then typecast it.

Next we’ll look at the method that adds elements to the list:

```java
	public boolean add(T element) {
		if (size >= array.length) {
			// make a bigger array and copy over the elements
			T[] bigger = (T[]) new Object[array.length * 2];
			System.arraycopy(array, 0, bigger, 0, array.length);
			array = bigger;
		}
		array[size] = element;
		size++;

		return true;
	}
```

コメントが示すように,sizeはどれだけの要素がMyArrayListに記録されているかを管理する。       
アレイは現実に要素を含む配列だ       

生成者が最初にnullだった10個の要素を持った配列を作ってサイズを0にセットします         
概して,配列の長さはサイズより大きいです。そこには未使用スロットがあるからです。        

ジャワについてさらに詳しいことは:あなたはタイプパラメータを使用する配列をインスタンス化できません。たとえば,次の例は作動しません。:    
```java
array=new E[10];       
```

このような限界に対する次善本は,貴方がオブジェクトタイプの配列をインスタンス化してタイプキャストするだけです。     

次に私たちはリストに要素を追加するメソッドを注視する必要があります.       

```java
	public boolean add(T element) {
		if (size >= array.length) {
			// make a bigger array and copy over the elements
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

If there are no unused spaces in the array, we have to create a bigger array and copy over the elements. Then we can store the element in the array and increment size.

It might not be obvious why this method returns a boolean, since it seems like it always returns true. As always, you can find the answer in the documentation: http://thinkdast.com/colladd. It’s also not obvious how to analyze the performance of this method. In the normal case, it’s constant time, but if we have to resize the array, it’s linear. I’ll explain how to handle this in Section 3.2.

```java
	public T get(int index) {
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}
		return array[index];
	}
```

Actually, get is pretty simple: if the index is out of bounds, it throws an exception; otherwise it reads and returns an element of the array.

In MyArrayList.java, you’ll find a stub for set that looks like this:

```java
public T set(int index, T element) {
			// TODO: fill in this method.
			return null;
		}
```

これ以上残った空間が配列に存在しなければ,私たちはもっと大きな配列を生成し,要素をコピーしなければなりません。 これにより,要素を配列に保存し,サイズを増やすことができます。      

このようなメソッドはいつもtrueを返却するののように見えますので,どうしてこのメソッドがa booleanを返却するか疑問に思うかもしれません。 いつものように,あなたは正解をthe documentationで見つけることができます。:      
http://thinkdast.com/colladd. このメソッドの性能をどのように評価するかは疑問です。         
一般的な場合なら,定数時間ですが,私たちはthearrayの大きさを調節しなければならないので,線形時間です。 これについてどう扱うべきかSection 3.2で説明します       

```java
	public T get(int index) {
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}
		return array[index];
	}
```

実際,getメソッドはかなり簡単です。もしインデックスが範囲外なら,例外を投げます。そうでなければ配列の要素を読んで返却します。
MyArrayList.javaで,あなたは次のようなsetメザードの一部分を発見できると思います。:

```java
public T set(int index, T element) {
			// TODO: fill in this method.
			return null;
		}
```

---

HINT: Try to avoid repeating the index-checking code.        

Your next mission is to fill in indexOf        

I’ve provided a helper method called equals that compares an element from the array to a target value and returns true if they are equal (and it handles null correctly).        

When you are done, run MyArrayListTest again; testIndexOf should pass now        

Only two more methods to go, and you’ll be done with this exercise. The next one is an overloaded version of add that takes an index and stores the new value at the given index, shifting the other elements to make room, if necessary.         

HINT: Avoid repeating the code that makes the array bigger.          

Last one: fill in the body of remove.          

Once you have your implementation working, compare it to mine, which you can read at http://thinkdast.com/myarraylist.       

ヒント: インデックスチェックコードが繰り返されるのを避けなさい        

あなたの次のミッションはインデックスOfメソッドを埋めるのです.       

私は配列における要素と目標値を比較し,それが一致してtrueを返還するequalsと呼ばれるa helper methodを提供してきました。(null値を正しく扱った場合)          

ミッションを完了したとき,MyArrayListTestを再起動させなさい; testIndexOfは,もう正常にパスするだろう。        

もう二つだけしか残っていません。あなたはこの例題と完了することができるでしょう。 次は,インデックス値を持ってきて,もし必要であれば,追加空間を作って他の要素を移動させ,与えられたインデックスに新しい値を保存するオーバーロードバージョンのaddメサードである。        

ヒントと大きさの配列を作るコードの繰り返しを避けよ        

最後:removeメソッドの"body"を満たせ         

あなたが実行結果が正常に作動するなら,  http://thinkdast.com/myarraylist で読める著者のものと比べてみなさい。    

---

```java
  /**
   *  @author KIM JONG DOO
   */
//	@Override
	public T set1(int index, T element) {
		if (index >= array.length-1 || index < 0) {
			return null;
		}
		//array[index] = element;
		return array[index] = element;
	}

	/**
	 *  @author downey
	 */
	@Override
	public T set(int index, T element) {
		// 	no need to check index; get will do it for us
		//　インデックスをチェックする必要はありません。 getメソッドが確認します。
		T old = get(index);
		array[index] = element;
		return old;
	}
```      


```java
	/**
	 *  @author KIM JONG DOO
	 */
//	@Override
	public int indexOf1(Object target) {
		// TODO: FILL THIS IN!
		array.equals(target);
		return -1;
	}

	/**
	 *  @author downey
	 */
	@Override
	public int indexOf(Object target) {
		for (int i = 0; i<size; i++) {
			if (equals(target, array[i])) {
				return i;
			}
		}
		return -1;
	}
```


```java
	/**
	 *  @author KIM JONG DOO
	 */
//	@Override
	public boolean add1(T element) {
		if (size >= array.length) {
			//	make a bigger array and copy over the elements
			//　既存の配列より大きな配列を作って要素をコピーする

			//　思いつかないのでソリューションコードを探してそのままコーディングしてみました。
			//	既存の配列の大きさを変更できないので、従来の配列より2倍の配列を作って2倍の大きさの配列を既存の配列に入力して使用する
			T[] bigger = (T[]) new Object[array.length * 2];
			System.arraycopy(array, 0, bigger, 0, array.length);
			array = bigger;
		}
		array[size] = element;
		size++;

		return true;
	}

	/**
	 *  @author downey
	 */
	@Override
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

```java
	/**
	 *  @author KIM JONG DOO
	 */
//	@Override
	public T remove1(int index) {
		//　私の考えにはindexにあるelementを削除する 例えば、index 5に存在するelement"five"を削除して
		//　index 6をindex 5で、index 7をindex 6に一間ずつ引いた次に最後のindexに不在ができ、index++している
		//　ところでindex 5にあるelementをどのように削除しなければならないか思い出せない!!
		if (index >= array.length-1 || index < 0) {
			return null;
		}
		array[index] = null;

		for (int i=index; i<size; i++) {
			array[i] = array[i+1];
			array[size] = null;
		}

		return null;
	}

	/**
	 *  @author downey
	 */
	@Override
	public T remove(int index) {
		T element = get(index);
		for (int i=index; i<size-1; i++) {
			array[i] = array[i+1];
		}
		size--;
		return element;
	}
```

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/4_ch2.png "Screenshot broadcast")   
