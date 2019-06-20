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

3. Dijkstra’s shortest path algorithm

Dijkstra’s algorithm is very similar to Prim’s algorithm for minimum spanning tree. Like Prim’s MST, we generate a SPT (shortest path tree) with given source as root. We maintain two sets, one set contains vertices included in shortest path tree, other set includes vertices not yet included in shortest path tree. At every step of the algorithm, we find a vertex which is in the other set (set of not yet included) and has a minimum distance from the source.      

```java
package com.mystudy.graph;

import java.util.LinkedList;

public class Dijkstra { // MyCode
	LinkedList<Integer> adj[][];
	int V;

	Dijkstra(int v) {
		V = v;
		adj = new LinkedList[v][v];
		for(int i=0; i<v; i++) {
			adj[i][i] = new LinkedList();
		}

	}

	void edges(int x, int y, int z) {
		adj[x][y].add(z);
		System.out.println(adj[x][y]);
	}

	void solution() {
		boolean flag[] = new boolean[V];
	}

	public static void main(String[] args) {

		Dijkstra di = new Dijkstra(2);
		di.edges(1,1,3);

	}
}

```

&nbsp;
&nbsp;

```java
// A Java program for Dijkstra's single source shortest path algorithm.
// The program is for adjacency matrix representation of the graph
import java.util.*;
import java.lang.*;
import java.io.*;

class ShortestPath // SolutionCode
{
	// A utility function to find the vertex with minimum distance value,
	// from the set of vertices not yet included in shortest path tree
	static final int V=9;
	int minDistance(int dist[], Boolean sptSet[])
	{
		// Initialize min value
		int min = Integer.MAX_VALUE, min_index=-1;

		for (int v = 0; v < V; v++)
			if (sptSet[v] == false && dist[v] <= min)
			{
				min = dist[v];
				min_index = v;
			}

		return min_index;
	}

	// A utility function to print the constructed distance array
	void printSolution(int dist[], int n)
	{
		System.out.println("Vertex Distance from Source");
		for (int i = 0; i < V; i++)
			System.out.println(i+" tt "+dist[i]);
	}

	// Funtion that implements Dijkstra's single source shortest path
	// algorithm for a graph represented using adjacency matrix
	// representation
	void dijkstra(int graph[][], int src)
	{
		int dist[] = new int[V]; // The output array. dist[i] will hold
								// the shortest distance from src to i

		// sptSet[i] will true if vertex i is included in shortest
		// path tree or shortest distance from src to i is finalized
		Boolean sptSet[] = new Boolean[V];

		// Initialize all distances as INFINITE and stpSet[] as false
		for (int i = 0; i < V; i++)
		{
			dist[i] = Integer.MAX_VALUE;
			sptSet[i] = false;
		}

		// Distance of source vertex from itself is always 0
		dist[src] = 0;

		// Find shortest path for all vertices
		for (int count = 0; count < V-1; count++)
		{
			// Pick the minimum distance vertex from the set of vertices
			// not yet processed. u is always equal to src in first
			// iteration.
			int u = minDistance(dist, sptSet);

			// Mark the picked vertex as processed
			sptSet[u] = true;

			// Update dist value of the adjacent vertices of the
			// picked vertex.
			for (int v = 0; v < V; v++)

				// Update dist[v] only if is not in sptSet, there is an
				// edge from u to v, and total weight of path from src to
				// v through u is smaller than current value of dist[v]
				if (!sptSet[v] && graph[u][v]!=0 &&
						dist[u] != Integer.MAX_VALUE &&
						dist[u]+graph[u][v] < dist[v])
					dist[v] = dist[u] + graph[u][v];
		}

		// print the constructed distance array
		printSolution(dist, V);
	}

	// Driver method
	public static void main (String[] args)
	{
		/* Let us create the example graph discussed above */
	int graph[][] = new int[][]
{% raw %}
								{{0, 4, 0, 0, 0, 0, 0, 8, 0},
								{4, 0, 8, 0, 0, 0, 0, 11, 0},
								{0, 8, 0, 7, 0, 4, 0, 0, 2},
								{0, 0, 7, 0, 9, 14, 0, 0, 0},
								{0, 0, 0, 9, 0, 10, 0, 0, 0},
								{0, 0, 4, 14, 10, 0, 2, 0, 0},
								{0, 0, 0, 0, 0, 2, 0, 1, 6},
								{8, 11, 0, 0, 0, 0, 1, 0, 7},
								{0, 0, 2, 0, 0, 0, 6, 7, 0}
								};
{% endraw %}
		ShortestPath t = new ShortestPath();
		t.dijkstra(graph, 0);
	}
}
//This code is contributed by Aakash Hasija
```
