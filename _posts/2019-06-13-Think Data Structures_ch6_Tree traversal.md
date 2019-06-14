---
layout: post
title:  "Think Data Structures_ch6_Tree traversal"
date:   2019-06-13 23:51:28 +0900
categories: [Java, Japanese, Algorithms, Data Structure]
---

Chapter 6    

Tree traversal    

This chapter introduces the application we will develop during the rest of the book, a web search engine. I describe the elements of a search engine and introduce the first application, a Web crawler that downloads and parses pages from Wikipedia. This chapter also presents a recursive implementation of depth-first search and an iterative implementation that uses a Java Deque to implement a “last in, first out” stack.    

&nbsp;
&nbsp;

---

6.1 Search engines    

&nbsp;

A web search engine, like Google Search or Bing, takes a set of “search terms” and returns a list of web pages that are relevant to those terms (I’ll discuss what “relevant” means later). You can read more at http://thinkdast. com/searcheng, but I’ll explain what you need as we go along.    

- Crawling: We’ll need a program that can download a web page, parse it, and extract the text and any links to other pages.    

- Indexing: We’ll need a data structure that makes it possible to look up a search term and find the pages that contain it.    

- Retrieval: And we’ll need a way to collect results from the Index and identify pages that are most relevant to the search terms.    

&nbsp;

We’ll start with the crawler. The goal of a crawler is to discover and download a set of web pages. For search engines like Google and Bing, the goal is to find all web pages, but often crawlers are limited to a smaller domain. In our case, we will only read pages from Wikipedia.    

&nbsp;

As a first step, we’ll build a crawler that reads a Wikipedia page, finds the first link, follows the link to another page, and repeats. We will use this crawler to test the “Getting to Philosophy” conjecture, which states:    

&nbsp;

Clicking on the first lowercase link in the main text of a Wikipedia article, and then repeating the process for subsequent articles, usually eventually gets one to the Philosophy article.    

&nbsp;

This conjecture is stated at http://thinkdast.com/getphil, and you can read its history there.    

&nbsp;

Testing the conjecture will allow us to build the basic pieces of a crawler without having to crawl the entire web, or even all of Wikipedia. And I think the exercise is kind of fun!

&nbsp;

In a few chapters, we’ll work on the indexer, and then we’ll get to the retriever.    

&nbsp;
&nbsp;

---

6.2 Parsing HTML    

&nbsp;

When you download a web page, the contents are written in HyperText Markup Language, aka HTML. For example, here is a minimal HTML document:    

&nbsp;

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch6_1.png "Screenshot broadcast")  
Figure 6.1: DOM tree for a simple HTML page.    

&nbsp;

The phrases “This is a title” and “Hello world!” are the text that actually appears on the page; the other elements are tags that indicate how the text should be displayed.    

&nbsp;

When our crawler downloads a page, it will need to parse the HTML in order to extract the text and find the links. To do that, we’ll use jsoup, which is an open-source Java library that downloads and parses HTML.    

&nbsp;

The result of parsing HTML is a Document Object Model tree, or DOM tree, that contains the elements of the document, including text and tags. The tree is a linked data structure made up of nodes; the nodes represent text, tags, and other document elements.    
The relationships between the nodes are determined by the structure of the document. In the example above, the first node, called the root, is the <html> tag, which contains links to the two nodes it contains, <head> and <body>; these nodes are the children of the root node.

&nbsp;

The <body> node has one child, <title>, and the <body> node has one child, <p> (which stands for “paragraph”). Figure 6.1 represents this tree graphically.    

&nbsp;

Each node contains links to its children; in addition, each node contains a link to its parent, so from any node it is possible to navigate up and down the tree. The DOM tree for real pages is usually more complicated than this example.    

&nbsp;

![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch6_2.png "Screenshot broadcast")  
Figure 6.2: Screenshot of the Chrome DOM Inspector.    

&nbsp;

Figure 6.2 shows a screenshot of the DOM for the Wikipedia page on Java, http://thinkdast.com/java. The element that’s highlighted is the first paragraph of the main text of the article, which is contained in a <div> element with id="mw-content-text". We’ll use this element id to identify the main text of each article we download.    

&nbsp;
&nbsp;

---

6.3 Using jsoup    

&nbsp;

```java
public class Test {
	public static void main(String[] args) {
		System.out.println(new Crawler());
	}
}
```

&nbsp;

```java
import java.io.IOException;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

public class Crawler {
	public Crawler() {
		try {
			GetHomePage();
		} catch (Exception e) {
			System.out.println("Error");
		}
	}

	public void GetHomePage() throws Exception {
		String url = "http://en.wikipedia.org/wiki/Java_(programming_language)";

		// 1.Jsoup.connect()はウェブサーバとのつながりを作ってStringの形のURLを取得します。    
		org.jsoup.Connection conn = Jsoup.connect(url);

		//2.get()メソッドはHTMLをダウンロードして文章をプログラミング文法的に分析します、そしてDOMを提供してDocumentオブジェクトに返します。
		Document doc = conn.get();

		// select the content text and pull out the paragraphs.

		//3.getElementByIdはStringを持ってきてtreeの形態であるidフィールド確認機能を持ったelementを探索します。 そして,ウィキペディアページを確認するために<div id='mw-content-text' lang>を選択します。また,getElementByIdのリターン値は<div>を表し<div>の子孫と子孫の孫は...を含んだElement オブジェクトです。    
		Element content = doc.getElementById("mw-content-text");    

		 //4.selectはStringを持ってきて、treeを探索します、そしてStringとマッチするすべてのelements with tagsを返還します。 この場合, <p> タグの表わすcontentのすべてのparagraph tagsを返却します。
		Elements paragraphs = content.select("p");
		System.out.println(paragraphs);
	}
}
```

&nbsp;
&nbsp;

 ![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch6_3.png "Screenshot broadcast")  

&nbsp;

 Figure 6.3 is a UML diagram showing the relationships among these classes. In a UML class diagram, a line with a hollow arrow head indicates that one class extends another. For example, this diagram indicates that Elements extends ArrayList. We’ll get back to UML diagrams in Section 11.6.    

&nbsp;

 ![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch6_4.png "Screenshot broadcast")  

Figure 6.3: UML diagram for selected classes provided by jsoup.          
Edit: http://yuml.me/edit/4bc1c919          

&nbsp;
&nbsp;

---

6.4 Iterating through the DOM      

&nbsp;

To make your life easier, I provide a class called WikiNodeIterable that lets you iterate through the nodes in a DOM tree. Here’s an example that shows how to use it:    

&nbsp;

```java
public class Test {
	public static void main(String[] args) {
		new Crawler();
	}
}
```

&nbsp;

```java

import java.io.IOException;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.nodes.Node;
import org.jsoup.nodes.TextNode;
import org.jsoup.select.Elements;

public class Crawler {
	public Crawler() {
		try {
			GetHomePage();
		} catch (Exception e) {
			System.out.println("Error");
		}
	}

	public void GetHomePage() throws Exception {
		String url = "http://en.wikipedia.org/wiki/Java_(programming_language)";
		org.jsoup.Connection conn = Jsoup.connect(url);
		Document doc = conn.get();
		// select the content text and pull out the paragraphs.
		Element content = doc.getElementById("mw-content-text");
		Elements paragraphs = content.select("p");
		Element firstPara = paragraphs.get(1);
//		System.out.println(firstPara);


		Iterable<Node> iter = new WikiNodeIterable(firstPara);
		for (Node node : iter) {
			if (node instanceof TextNode) {
				System.out.print(node);
			}
		}
	}
}
```

&nbsp;

```java
package ch6;

import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Deque;
import java.util.Iterator;
import java.util.List;
import java.util.NoSuchElementException;

import org.jsoup.nodes.Node;


/**
 * Performs a depth-first traversal of a jsoup Node.
 　　 jsoup Nodeのdepth-first traversalを使います。
 *
 * @author downey
 *
 */
public class WikiNodeIterable implements Iterable<Node> {

	private Node root;

	/**
	 * Creates an iterable starting with the given Node.
	 　　与えられたノードから始まるiterableを使います。
	 * @param root
	 */
	public WikiNodeIterable(Node root) {
	    this.root = root;
	}

	@Override
	public Iterator<Node> iterator() {
		return new WikiNodeIterator(root);
	}

	/**
	 * Inner class that implements the Iterator.
	 　　内部のクラスはIteratorを具現します。
	 * @author downey
	 *
	 */
	private class WikiNodeIterator implements Iterator<Node> {

		// this stack keeps track of the Nodes waiting to be visited
		//　このスタッフは待ちノードの記録を保存します。
		Deque<Node> stack;

		/**
		 * Initializes the Iterator with the root Node on the stack.
		 　　スタッフのルートノードを含むIteratorを初期設定します。
		 * @param node
		 */
		public WikiNodeIterator(Node node) {
			stack = new ArrayDeque<Node>();
		    stack.push(root);
		}

		@Override
		public boolean hasNext() {
			return !stack.isEmpty();
		}

		@Override
		public Node next() {
			// if the stack is empty, we're done
			if (stack.isEmpty()) {
				throw new NoSuchElementException();
			}

			// otherwise pop the next Node off the stack
			Node node = stack.pop();
			//System.out.println(node);

			// push the children onto the stack in reverse order
			List<Node> nodes = new ArrayList<Node>(node.childNodes());
			Collections.reverse(nodes);
			for (Node child: nodes) {
				stack.push(child);
			}
			return node;
		}

		@Override
		public void remove() {
			throw new UnsupportedOperationException();
		}
	}
}
```

&nbsp;
&nbsp;

 ![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch6_5.png "Screenshot broadcast")      

  ![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch6_6.png "Screenshot broadcast")      

&nbsp;
&nbsp;

---

6.5 Depth-first search    

&nbsp;

There are several ways you might reasonably traverse a tree, each with different applications. We’ll start with “depth-first search”, or DFS. DFS starts at the root of the tree and selects the first child. If the child has children, it selects the first child again. When it gets to a node with no children, it backtracks, moving up the tree to the parent node, where it selects the next child if there is one; otherwise it backtracks again. When it has explored the last child of the root, it’s done.      

&nbsp;

There are two common ways to implement DFS, recursively and iteratively. The recursive implementation is simple and elegant:      

```java
	private static void recursiveDFS(Node node) {
		if (node instanceof TextNode) {
			System.out.print(node);
		}
		for (Node child : node.childNodes()) {
			recursiveDFS(child);
		}
	}
```

&nbsp;

In this example, we print the contents of each TextNode before traversing the children, so this is an example of a “pre-order” traversal.      

&nbsp;

By making recursive calls, recursiveDFS uses the call stack (http://thinkdast. com/callstack) to keep track of the child nodes and process them in the right order. As an alternative, we can use a stack data structure to keep track of the nodes ourselves; if we do that, we can avoid the recursion and traverse the tree iteratively.      

&nbsp;
&nbsp;

---

6.6 Stacks in Java      

&nbsp;

Before I explain the iterative version of DFS, I’ll explain the stack data structure.
A stack is a data structure that is similar to a list: it is a collection that maintains the order of the elements.      

&nbsp;

- push: which adds an element to the top of the stack.       
- pop: which removes and returns the top-most element from the stack.       
- peek: which returns the top-most element without modifying the stack.       
- isEmpty: which indicates whether the stack is empty.      

&nbsp;

It might not be obvious why stacks and queues are useful: they don’t provide any capabilities that aren’t provided by lists; in fact, they provide fewer capabilities. So why not use lists for everything? There are two reasons:      
1. If you limit yourself to a small set of methods — that is, a small API — your code will be more readable and less error-prone. For example, if you use a list to represent a stack, you might accidentally remove an element in the wrong order. With the stack API, this kind of mistake is literally impossible. And the best way to avoid errors is to make them impossible.      

2. If a data structure provides a small API, it is easier to implement efficiently. For example, a simple way to implement a stack is a singly-linked list. When we push an element onto the stack, we add it to the beginning of the list; when we pop an element, we remove it from the beginning.      

&nbsp;

To implement a stack in Java, you have three options:      

1. Go ahead and use ArrayList or LinkedList. If you use ArrayList, be sure to add and remove from the end, which is a constant time operation. And be careful not to add elements in the wrong place or remove them in the wrong order.      

2. Java provides a class called Stack that provides the standard set of stack methods. But this class is an old part of Java: it is not consistent with the Java Collections Framework, which came later.      

3. Probably the best choice is to use one of the implementations of the Deque interface, like ArrayDeque.      

&nbsp;

“Deque” stands for “double-ended queue”; it’s supposed to be pronounced “deck”, but some people say “deek”. In Java, the Deque interface provides push, pop, peek, and isEmpty, so you can use a Deque as a stack.      

&nbsp;
&nbsp;

---

6.7 Iterative DFS

&nbsp;

Here is an iterative version of DFS that uses an ArrayDeque to represent a stack of Node objects:      

```java
	// パラメータを私たちが探索するツリーのrootです。 また,私たちはstackを生成し,rootを入れます。
	private static void iterativeDFS(Node root) {

		Deque<Node> stack = new ArrayDeque<Node>();
		stack.push(root);

		// loopは stackが 空いた状態になるまで繰り返されます。
		while (!stack.isEmpty()) {

			// 各繰り返し状態毎にNodeをスタックにポップします。
			Node node = stack.pop();

			// もし TextNode を探すならnodeをプリントします。
			if (node instanceof TextNode) {
				System.out.print(node);
			}

			// その後は,子供のノードをstackにpushします。
			List<Node> nodes = new ArrayList<Node>(node.childNodes());

			// 子供ノードを右の方向に進めるため,私たちは逆方向にスターバックにパスします。
			Collections.reverse(nodes);

			for (Node child : nodes) {
				stack.push(child);
			}
		}
	}
```

&nbsp;

One advantage of the iterative version of DFS is that it is easier to implement as a Java Iterator; you’ll see how in the next chapter.      

&nbsp;

But first, one last note about the Deque interface: in addition to ArrayDeque, Java provides another implementation of Deque, our old friend LinkedList. LinkedList implements both interfaces, List and Deque. Which interface you get depends on how you use it. For example, if you assign a LinkedList object to a Deque variable, like this:      
```java
Deqeue deque = new LinkedList();
```

&nbsp;

you can use the methods in the Deque interface, but not all methods in the List interface. If you assign it to a List variable, like this:      
```java
List deque = new LinkedList();
```

&nbsp;

you can use List methods but not all Deque methods. And if you assign it like this:      
```java
LinkedList deque = new LinkedList();
```

&nbsp;

you can use all the methods. But if you combine methods from different interfaces, your code will be less readable and more error-prone.      
