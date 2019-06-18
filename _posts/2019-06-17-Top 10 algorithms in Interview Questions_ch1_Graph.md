---
layout: post
title:  "Top 10 algorithms in Interview Questions_ch1_Graph"
date:   2019-06-17 20:51:28 +0900
categories: [Java, Japanese, Algorithms, Graph]
---

1. Breadth First Search or BFS for a Graph

```java
//This class represents a directed graph using adjacency list
//representation
class Graph
{
	private int V; // No. of vertices

	//　一例にadj[0]には近接ノードを数えられます。(0,1 / 0,2)等々
	private LinkedList<Integer> adj[]; //Adjacency Lists

	// Constructor
	Graph(int v)
	{
		//　定点の個数だけ LinkedListタイプのadjを生成
		V = v;
		adj = new LinkedList[v];
		for (int i=0; i<v; ++i)
			adj[i] = new LinkedList();
	}

	// Function to add an edge into the graph
	void addEdge(int v,int w)
	{
		// adj[v]に含まれたint w
		adj[v].add(w);
	}

	// prints BFS traversal from a given source s
	void BFS(int s)
	{
		// Mark all the vertices as not visited(By default
		// set as false)
		boolean visited[] = new boolean[V];

		// Create a queue for BFS
		LinkedList<Integer> queue = new LinkedList<Integer>();

		// Mark the current node as visited and enqueue it
		visited[s]=true;
		queue.add(s);

		while (queue.size() != 0)
		{
			// Dequeue a vertex from queue and print it
			s = queue.poll();
			System.out.print(s+" ");

			// Get all adjacent vertices of the dequeued vertex s
			// If a adjacent has not been visited, then mark it
			// visited and enqueue it
			// adj[s]に含まれたインデックスを呼び出します。
			Iterator<Integer> i = adj[s].listIterator();
			while (i.hasNext())
			{
				int n = i.next();
				if (!visited[n])
				{
					visited[n] = true;
					queue.add(n);
				}
			}
		}
	}

	// Driver method to
	public static void main(String args[])
	{
		Graph g = new Graph(4);

		g.addEdge(0, 1);
		g.addEdge(0, 2);
		g.addEdge(1, 2);
		g.addEdge(2, 0);
		g.addEdge(2, 3);
		g.addEdge(3, 3);

		System.out.println("Following is Breadth First Traversal "+
						"(starting from vertex 2)");

		g.BFS(2);
	}
}
//This code is contributed by Aakash Hasija
```

2. Depth First Search or DFS for a Graph

```java
package com.mystudy.graph;

import java.util.Iterator;
import java.util.LinkedList;

public class DFS { // MyCode

	private int V;
	private LinkedList<Integer> adj[];

	DFS(int v) {
		V = v;
		adj = new LinkedList[v];
		for(int i=0; i < v ; i++)
			adj[i] = new LinkedList();
	}

	void edges(int x, int y) {
		adj[x].add(y);
	}

	void solution(int s) {
//		boolean visit[] = null;
//		visit[s] = true;

		Iterator it = adj[s].listIterator();  
		while(it.hasNext()) {
			System.out.println(it.next());
		}
	}

	public static void main(String[] args) {
		DFS graph = new DFS(4);

		graph.edges(0, 1);
		graph.edges(0, 2);
		graph.edges(0, 3);
		graph.edges(2, 4);

		graph.solution(0);

	}
}

//Example:
//Input:
//5 4
//0 1 0 2 0 3 2 4
//4 3
//0 1 1 2 0 3
//
//Output:
//0 1 2 4 3    // dfs from node 0
//0 1 2 3

```

&nbsp;
&nbsp;

```java
package com.mystudy.graph;

//Java program to print DFS traversal from a given given graph
import java.io.*;
import java.util.*;

//This class represents a directed graph using adjacency list
//representation
class Graph2 // SolutionCode
{
 private int V;   // No. of vertices

 // Array  of lists for Adjacency List Representation
 private LinkedList<Integer> adj[];

 // Constructor
 Graph2(int v)
 {
     V = v;
     adj = new LinkedList[v];
     for (int i=0; i<v; ++i)
         adj[i] = new LinkedList();
 }

 //Function to add an edge into the graph
 void addEdge(int v, int w)
 {
     adj[v].add(w);  // Add w to v's list.
 }

 // A function used by DFS
 void DFSUtil(int v,boolean visited[])
 {
     // Mark the current node as visited and print it
     visited[v] = true;
     System.out.print(v+" ");

     // Recur for all the vertices adjacent to this vertex
     Iterator<Integer> i = adj[v].listIterator();
     while (i.hasNext())
     {
         int n = i.next();

         //　再帰関数を利用して訪問しないvertices をずっと探し続ける。
         if (!visited[n])
             DFSUtil(n, visited);
     }
 }

 // The function to do DFS traversal. It uses recursive DFSUtil()
 void DFS(int v)
 {
     // Mark all the vertices as not visited(set as
     // false by default in java)
     boolean visited[] = new boolean[V];

     // Call the recursive helper function to print DFS traversal
     DFSUtil(v, visited);
 }

 public static void main(String args[])
 {
     Graph2 g = new Graph2(4);

     g.addEdge(0, 1);
     g.addEdge(0, 2);
     g.addEdge(1, 2);
     g.addEdge(2, 0);
     g.addEdge(2, 3);
     g.addEdge(3, 3);

     System.out.println("Following is Depth First Traversal "+
                        "(starting from vertex 2)");

     g.DFS(2);
 }
}
//This code is contributed by Aakash Hasija
```
