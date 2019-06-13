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
---
&nbsp;

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
---
&nbsp;

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
---
&nbsp;

6.3 Using jsoup    

&nbsp;

jsoup makes it easy to download and parse web pages, and to navigate the DOM tree. Here’s an example:    

String url = "http://en.wikipedia.org/wiki/Java_(programming_language)";    

//1.Jsoup.connect()はウェブサーバとのつながりを作ってStringの形のURLを取得します。    
//2.get()メソッドはHTMLをダウンロードして文章をプログラミング文法的に分析します、そしてDOMを提供してDocumentオブジェクトに返します。    

// download and parse the document    
Connection conn = Jsoup.connect(url);     
Document doc = conn.get();    

//3.getElementByIdはStringを持ってきてtreeの形態であるidフィールド確認機能を持ったelementを探索します。 そして,ウィキペディアページを確認するために<div id='mw-content-text' lang>を選択します。
また,getElementByIdのリターン値は<div>を表し<div>の子孫と子孫の孫は...を含んだElement オブジェクトです。    
//4.selectはStringを持ってきて、treeを探索します、そしてStringとマッチするすべてのelements with tagsを返還します。 この場合, <p> タグの表わすcontentのすべてのparagraph tagsを返却します。    

// select the content text and pull out the paragraphs.     
Element content = doc.getElementById("mw-content-text");     
Elements paragraphs = content.select("p");     

&nbsp;
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
		org.jsoup.Connection conn = Jsoup.connect(url);
		Document doc = conn.get();
		// select the content text and pull out the paragraphs.
		Element content = doc.getElementById("mw-content-text");
		Elements paragraphs = content.select("p");
		System.out.println(paragraphs);
	}
}
```

&nbsp;
&nbsp;

 ![Screenshot broadcast](https://raw.githubusercontent.com/Bullishman/bullishman.github.io/master/static/img/_posts/Think%20Data%20Structures/ch6_3.png "Screenshot broadcast")  
