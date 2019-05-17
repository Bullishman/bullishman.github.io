---
layout: post
title:  "Think Data Structures_ch1_Interfaces"
date:   2019-05-18 00:08:28 +0900
categories: [Java, Japanese, Algorithms, Data Structure]
---

Think Data Structures Algorithms and Information Retrieval in Java       

Chapter 1        
Interfaces     

- Data structures: Starting with the structures in the Java Collections Framework(JCF)          
- Analysis of algorithms: I present techniques for analyzing code and predicting how fast it will run and how much space (memory) it will require            
- Information retrieval: To motivate the first two topics, and to make the exercises more interesting, we will use data structures and algorithms to build a simple web search engine             

- データ構造:ジャワ·コレクション·フレームワークで資料構造について学ぶ。   
- アルゴリズムの分析:いかに速く作動するか,そしてどれほど十分な空間が必要かを予測し,分析するコードについてテクニックを提供する。    
- 情報(データ)検索:一番目と二番目のテーマの同期を付与し,さらに興味をそそる例題で練習するために,私たちは簡単なウェブ検索エンジンを具現するためにデータ構造とアルゴリズムを使う。  

- we’ll compare your implementations with the Java classes ArrayList and LinkedList.    
- I’ll introduce tree-shaped data structures and you will work on the first application    
- We’ll learn about the Map interface and Java’s HashMap implementation. Then you’ll write classes that implement this interface using a hash table and a binary search tree.      
- Finally, you will use these classes (and a few others I’ll present along the way) to implement a web search engine, including: a crawler that finds and reads pages, an indexer that stores the contents of Web pages in a form that can be searched efficiently, and a retriever that takes queries from a user and returns relevant results          

- ジャバクラスのArrayList and LinkedListで作成して実行したあなたのコードを比較してみる。          
- ツリー形の資料構造を紹介し,初めてのアプリケーションを作動して見る。         
- 私たちはマップインターフェースとジャワのハッシュマップを実行する方法を学ぶつもりだ。 また,あなたはハッシュテーブルと二分探索トリを使ったインタフェースを含むクラスを作成してみるつもりだ.            
- 最後にあなたは次の機能を含むウェブ検索エンジンの実行のためのクラスを使ってみる。:ページを読んで探すためのcrawler,効率的に検索できる構成のウェブページの内容を保存するためのindexer,そしてリターン結果とユーザからのクエリー文を読み込めるretriever             

---

** 1.1 Why are there two kinds of List? **            
When people start working with the Java Collections Framework, they are sometimes confused about ArrayList and LinkedList.           
In the first few exercises, you’ll implement classes similar to ArrayList and LinkedList, so you’ll know how they work, and we’ll see that each of them has pros and cons.             

人々はジャワ·コレクション·フレームワークを作動させる際,時々ArrayListとLinkedListを混同する。              
一番目のいくつかの例題で,貴方は類似したArrayListとLinkedListクラスを実行し,そして,どのようにしてそれが作動するのかが分かるようになるし,二つのクラスはそれぞれの長所と短所があることが分かるでしょう。                 

---

** 1.2 Interfaces in Java **             
```java
public interfaces Comparable<T> {
	public int compareTo(T o);
}
```

This interface definition uses a type parameter, T, which makes Comparable a generic type. In order to implement this interface, a class has to           
- Specify the type T refers to, and               
- Provide a method named compareTo that takes an object as a parameter and returns an int.
For example, here’s the source code for java.lang.Integer:             

これらのインタフェースの定義はゼネリックタイプのComparableを作るためのタイプパラメータであるTを使用します。 このようなインターフェースを使うため,クラスは           
- タイプTが何を参照するか明示して            
- prameterのobjectを持ってきてintを返すcompareToというネームのメソッドを提供しなければなりません。           
例えば, 次のjava.lang.Integer ソースコードが例にあげられます。          

```java
public final class Integer extends Number implements Comparable<Integer> {
// Integer ゼネリックタイプのComparable インタフェースを具現され,Numberクラスの相続を受け,public タイプに変更不可でcompareToメソッドを相続するInteger クラス生成
	public int compareTo(Integer anotherInteger) {
		int thisVal = this.value;
		int anotherVal = anotherInteger.value;
		return (thisVal < anotherVal ? -1 : (thisVal==anotherVal ? 0 : 1));
	}
	//compareToメソッドはIntegerの入力を受け,int値をリターンするメソッドである。
	//タメソドル省略
}
```                

This class extends Number, so it inherits the methods and instance variables of Number; and it implements Comparable, so it provides a method named compareTo that takes an Integer and returns an int.          

---

** 1.3 The List interface **            
The Java Collections Framework (JCF) defines an interface called List and provides two implementations, ArrayList and LinkedList.            

ArrayList and LinkedList provide these methods, so they can be used interchangeably. A method written to work with a List will work with an ArrayList, LinkedList, or any other object that implements List.        

ArrayListとLinkedListは,次の相互交換可能なメソッドを提供する.        
Listが含めたメソッドはArrayList, LinkedList, 又は他のListを具現するobjectと作動する。        

```java
public class ListClientExample {
	private List list;

	public ListClientExample() {
		list = new LinkedList();
	}

	public List getList() {
		return list;
	}

	public static void main(String[] args) {
		ListClientExample Ice = new ListClientExample();
		List list = Ice.getList();
		System.out.println(list);
	}
}
```     

ListClientExample doesn’t do anything useful, but it has the essential elements of a class that encapsulates a List; that is, it contains a List as an instance variable. I’ll use this class to make a point, and then you’ll work with it in the first exercise.                

The ListClientExample constructor initializes list by instantiating (that is, creating) a new LinkedList; the getter method called getList returns a reference to the internal List object; and main contains a few lines of code to test these methods.            

The important thing about this example is that it uses List whenever possible and avoids specifying LinkedList or ArrayList unless it is necessary. For example, the instance variable is declared to be a List, and getList returns a List, but neither specifies which kind of list

ListClientExampleは,あまり役立たずに見える殻である。しかし,Listをカプセル化したクラスの必須要素を持っている。     

ListClientExample生成者は,new LinkedListをインスタンス化することによって,リストを初期化する; getList と呼ばれるgetterメソッドは,参照変数を内部のList object に返還する。なお,mainはこのようなメソッドをテストできるコードを含む。      

This style is called interface-based programming, or more casually, “programming to an interface” (see http://thinkdast.com/interbaseprog). Here we are talking about the general idea of an interface, not a Java interface.         

When you use a library, your code should only depend on the interface, like List. It should not depend on a specific implementation, like ArrayList. That way, if the implementation changes in the future, the code that uses it will still work

On the other hand, if the interface changes, the code that depends on it has to change, too. That’s why library developers avoid changing interfaces unless absolutely necessary

これはinterface-based programming と呼ばれる、そしてこれはJava interfaceではない。
ライブラリーを使うとき, あなたのコードはリストのようなインターフェースだけに頼らなければならない.

ArrayList のような特定の実装部に依存してはならない。 つまり, 即ち, 後日, 実装部の変化にも作成されたコードは依然として働くだろう.

反面,インターフェースが変化すると,これに依存するコードも変化する。
それがlibrary developersが絶対的に必要でないインターフェースの変化を避ける理由だ。

---

**1.4 Exercise 1**

Since this is the first exercise, we’ll keep it simple. You will take the code from the previous section and swap the implementation; that is, you will replace the LinkedList with an ArrayList. Because the code programs to an interface, you will be able to swap the implementation by changing a single line and adding an import statement        

今回は最初の例題だから,簡単だろう。 あなたは以前の部分と実装部の交換からコードを持ってくることができるでしょう。 それはあなたにLikedListをArrayListに代替できることを表す。 なぜかと言うとインターフェースのコードプログラムである。あなたは,一行を変え,import句文を追加することで,実装部の相互交換ができるだろう。
