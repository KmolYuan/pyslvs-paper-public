# Terminologies

## Mechanism

The theory in this research is based on the mechanism of planar linkage system, and the parts are treated as rigid body.

### Links and Joints

"Links" means the parts of mechanism. Such as: driver, follower or connector in a linkage system. Two particles will not change the distance between them if they are on the same rigid link.

"Joints" means the bonds between two or more links. For examples: revolution pair, block in slot pair or pin in slot pair. A joint must be a particle between two links that the relative speed is zero in planar space.

### Multiple Joints

A multiple joint will be connected to multiple links at once. Usually, the joint type must be consistent, so the joint can be treated as same type of several  single joints that in the same position on planar system.

For the dynamic or kinematic reason, the multiple joints will be more difficult to decided. In topological views, multiple joints will be treated as single joints, or treated as a virtual link which has been proposed by Ding, Yang, Huang and Kecskemethy [@structural-synthesis-multiple-joint], but in this research, the synthesis process will using former method.

### Generalized Chain

If we assume that placing all of grounded joints into a new link, the structure of mechanism called "generalized chain", and this step called "generalization". A generalized chain is just showing its connection characteristics, so its link dimension will be ignored. Generalized chain can be used to solve structural problems with Graph Theory such as patents.

## Graph Theory

Graph Theory is a branch of Combinatorial Mathematics. A "graph" is used to describe the status of adjacency between several objects, regardless of its form of expression. With some specific constraints and graph types, the algorithm methods based on Graph Theory can be used to solve problem.

### Nodes and Edges of Graphs

"Nodes" and "edges" are the base element of a graph, nodes also known as "vertices". An edge will connect two nodes together or just one node (it's be called self loop). A node can be connect to multiple nodes. There are several types of graph names:

1. "Connected graph" means the nodes of the graph is connected with each other one by one, instead of separating into different parts.

1. "Weighted graph" means the edges has specific weight values, such likes "time cost" or "straight line distance". This situation can make the considerations more complete.

1. "Directed graph" means the edges has specific direction, so the explorer must follow the direction of each edges. The graph can simply use negative weight value to represent the reverse concept. The opposite graph is called "undirected graph".

1. "Multigraph" or "pseudograph" means the number of edges that between same pair of nodes can be multiple.

1. "Hypergraph" means the edges of the graph can bind multiple nodes together, not only restricted in start node and end node.

1. "Subgraph" means the nodes and edges of the graph is subset of original graph. The adjacency of original graph must not have difference set to subgraph, otherwise it will not be established. The opposite graph of "Subgraph" is called "supergraph".

1. "Contracted graph" means the nodes of the graph must be connected to three or more nodes (including itself), and it will smaller than original graph, but the graph is not a subgraph. Contracted graph is a type of multigraph, usually used as homeomorphism detection.

"Generalized chain" is basically described by "undirected graph"; "contracted graph" will be used in graph enumeration; and the concept of "hypergraph" will be used in mechanism expression in subsequent chapters.

### Expression

There are many ways to express a graph by describe its nodes and edges. There is a 5 nodes graph example ({@fig:example-graph}).

![Example Graph](images/example-graph.png){#fig:example-graph height=35%}

Edge Set is the one of the commonly used representation, for an example: DOT language in GraphViz [@graphviz]. The data will be stored on each edge, for a simple graph, it is the most space-saving method. The graph {@fig:example-graph} can be expressed as {@eq:example-graph-edge-set}.

$$
\begin{aligned}
G = \{&(N_1 \leftrightarrow N_2, 10), \\
&(N_3 \rightarrow N_1, 40), \\
&(N_2 \rightarrow N_4, 5), \\
&(N_4 \rightarrow N_3, 20), \\
&(N_3 \leftrightarrow N_5, 30)\}
\end{aligned}
$${#eq:example-graph-edge-set}

Another way is using the Adjacency Matrix. The data will be stored as a $n$ times $n$ matrix, where $n$ is the number of nodes. The index of row and column represent the weight of edge that between two nodes, the index of row as the major. If the weight is zero, it means that two nodes are not connected. The one of the expressed graph {@fig:example-graph} can be build as {@eq:example-graph-matrix}. Therefore the weight value must be positive, and the sign of the value represent the direction.

$$
G = \begin{bmatrix}
0 & 10 & -40 & 0 & 0 \\
10 & 0 & 0 & 5 & 0 \\
40 & 0 & 0 & -20 & 30 \\
0 & -5 & 20 & 0 & 0 \\
0 & 0 & 30 & 0 & 0
\end{bmatrix}
$${#eq:example-graph-matrix}

Adjacency Lists will be more easier to retrieve when searching connected nodes. This representation can be created by a filter function from two methods that mentioned above. However, Adjacency Lists is more simple then Edge Set, so it can't store the weight value. The graph {@fig:example-graph} will be expressed as {@eq:example-graph-list}.

$$
\begin{aligned}
G = \{&(N_1 \rightarrow \{N_2\}), \\
&(N_2 \rightarrow \{N_1, N_4\}), \\
&(N_3 \rightarrow \{N_1, N_5\}), \\
&(N_4 \rightarrow \{N_3\}), \\
&(N_5 \rightarrow \{N_3\})\}
\end{aligned}
$${#eq:example-graph-list}

### Walks, Paths, Loops and Cycles

A sequence of nodes that follow the adjacency of a graph called the "walk" of the graph. It will be called "path" if the nodes is not repeated. Technologically, the path is a subgraph of the original graph, so it can do a adjacency test for the original graph.

"Loop" is a type of "walk" that start node and end node are the same, and the number of nodes should greater than three. The same concept is used with "cycle" for "path".

The number of loops $L$ in a graph can be counted with the number of edges $E$ and vertices $V$ (nodes) by {@eq:loop-count}.

$$
L = E - V + 1
$${#eq:loop-count}

A special case is "self loop", the start node and the end node are the same node for the edge, which is also applicable to the above formula.

### Isomorphism and Homeomorphism

"Isomorphism" means the adjacency of two graphs $G_1$ and $G_2$ are the same, no matter how the graphs are expressed, even though the labels of nodes are completely different.

Two edges can be smoothed out as one edge, if these two edges are the only two links on the intersection node. After smoothing all dual-connected nodes, a graph can be turned into a contracted graph.

The isomorphism between the contracted graphs of two graphs $G_1$ and $G_2$ is called "homeomorphism". A homeomorphic comparison can reduce the time of enumerating the combination of graph atlas; and a isomorphic testing can make sure the graphs is the only answer of specific conditions.

### Planar Graph

If and only if the edges in a graph can be expressed in disjoint forms, that is the simplest description of a "planar graph". For a non-planar graph, its edges can't be expressed as a disjoint form.

If all of nodes can connect to all of other nodes in a graph, that is called "complete graph". A complete graph has $n$ nodes is also called "$K_n$", which is shown in {@fig:complete-graph}. For $K_4$, it is still planar graph because the one of inner edge can be moved outside, but $K_5$ or higher $K_n$ can not.

![Complete graphs $K_2$ to $K_5$](images/complete-graph.png){#fig:complete-graph width=50%}

In another words, if a graph has a subgraph $K_n$, and $n$ equal or larger than 5, the graph must be a non-planar graph. The planar graph checking can make sure the mechanism is a planar system.

## Information Technology

Using computer to handle the complex mathematics calculation is the most effective way to perform mechanism designs. But certain data structures and algorithms need to be considered and applied to ensure a more efficient implementation. In this section, the related technologies mentioned in this research will be listed here.

### Environment and Dependencies

The configuration and installation for computer  usage generally called as "environment preparation", including setup of the specific libraries, driver programs, devices and operating systems.

When referring to some functions of a program, the project size can be reduced very well. The referred program is called "dependency", must be install or setup first during installation.

The installation of develop environment has been simplified in recent years. On Unix like operating systems, the package management software is very common. For continuous integration server or service, customizable configuration and parameter matrix can build a lot of virtual environments quickly by operating web pages, even deploying the release products directly.

### Desktop and Web Based Application

In a unfamiliar environment (such as client side), the computer is not the same as the development environment. Generally, the dependences of the software will be packed into installer program or portable virtual environment, called "desktop application". Another way, the program can be a server side web page or connect with a light weight client application, called "web based application" or "web service".

### Programming Languages

All programming languages need to be "compiled" into machine code before they are used. The interpretable language can be executed immediately when just input single expression. The tools used to compile or interpret is called "compiler" or "interpreter".

Another way to categorize a programming language is the relative level between hardware to developers. Languages with lower levels will have higher flexibility to configure the hardware, but the higher level will save the cumbersome annotations and make it easier to develop.

The programming language defines the scripting rules and creates syntactic sugar or syntactic salts to make developers to keep the features that the language pursues. For example, a high level language will use a lot of built-in functions instead of implementing functions by developers.

The interpreted languages may be lost performance, but it's good at debugging and testing. Also, there are a lot of ways to increase the speed of execution. Conversely, languages that can only be compiled lack these benefits.

### Markup Language

Markup Language is a type of data format that is based on text file instead of binary file. This data format type also supports embedded annotation text for developer identification.

Although the file size is not optimized enough, however, it is easy to view the data directly from the file without having to go through a special viewer. In addition, this data format will support to write line breaks, and usually has a single line syntax to facilitate data transfer.

### Expression Matching and Parsing

Regular Expression is a type of matching rules used as string capturing. The matching method will search whole text but ignore the non-matched characters. The feature of the matching method is to put the captured words into a list, commonly used in syntax highlighter and input mask. But overall it is not that easy to define a lot of grammar changes.

Context-free grammar is usually used as a definition of mathematical formulas or computer programming language, because the algebraic exchange is permanently equivalent. Extended Backus-Naur Form is one of the commonly used syntax parsing definition based on Backus-Naur Form, the latter is a type of context-free grammar invented by John Backus and Peter Naur.

A expression parser can analysis the sentence constructed from user defined grammar, even to create a searching tree. With different language behavior, the parser can execute the functional program during process to do "pretreatment" of the scripts.
