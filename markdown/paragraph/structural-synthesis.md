# Structural Synthesis

## Number and Type Synthesis

An existing designs of a mechanism structure can be divided into two parts: "Number synthesis" and "type synthesis".

Number synthesis is use to search the types of mechanism that are maintained at the same number of joints and links. The time costs of the same number of links will be more similar to the original design.

Type synthesis can be labeled an atlas that contains all the possibilities of connecting configuration by number synthesis condition. The flow chart of process shows in {@fig:creative-process}.

![Flow chart of Creative Design Method](images/creative-process.png){#fig:creative-process height=50%}

### Generalized Chain

The mechanism expression used in this research already has a "ground" link and "links" attribute, so this part is more simple than changing with normal mechanism graphs. For example, Watt I (a) and Watt II (b) can be turned into a six bar generalized chain (c) in {@fig:generalized-chain}.

![Generalized chain of Watt chain](images/generalized-chain.png){#fig:generalized-chain}

Generalization is the first step of Creative Design flow. The number of joints and links will be more clear in the graph. Also, the multiple joints will be separated into single joints at this step in order to identify the connection expression.

If a joint is a two DOF joint, it can not be conducted with structural synthesis directly, but there are two options; transforming into two joints and a link, which can change it as an equivalence structure that keep with original DOF, or assuming it as an one DOF joint, then specifying its role after type synthesis (the part of "specialization"). The concept is also accepted in a 3D mechanism.

A generalized chain is represented by an undirected graph in Graph Theory, where the links are connected to multiple different links with the joints, so the links will be treated as vertices in the graph and joints are treated as edges. For example, an eight bar linkage generalized chain shown in {@fig:link-node-mapping}, the labeled link 1 to 8 are mapped to the vertices (nodes).

![Links-vertices mapping of generalized chain](images/link-node-mapping.png){#fig:link-node-mapping width=60%}

### Number and Type Synthesis

Numbers of joints and links are features that we don't want to adjust in Creative Design flow, because of the cost increment. These two feature can also be written as $(N_L, N_J)$, like $(4, 4)$, $(6, 7)$, $(8, 10)$, etc. The step of number and type synthesis is to find out link assortment and contracted link assortment. Link assortment will define the quantities of each binary links and multiple links. Contracted link assortment will define the quantities of each series of binary links. All values in this section should be non-negative integers $\{0, \mathbb{Z}^+\}$ and the correction and improvement are based on Yan and Chiu's number synthesis thesis [@yan-number-synthesis].

Following equations {@eq:nl-syn1} and {@eq:nl-syn2}, number synthesis can find out the link assortment, and the number of joints can be assign to each different type of links.

$$
\sum_{m=2}^{m_{\max}} N_{L_m} = N_L
$${#eq:nl-syn1}

$$
\sum_{m=2}^{m_{\max}} mN_{L_m} = 2N_J
$${#eq:nl-syn2}

Where $N_L$ and $N_J$ represents number of links and joints, $m$ is the number of joints in each link, $N_{L_m}$ represents the number of $m$-joints links, $m_{\max}$ is the max number of joints that the links can support. The maximum can be determine by {@eq:link-max}. There are two possible cases, while others are invalid structures.

$$m_{\max} = \begin{cases}
N_J - N_L + 2 & \text{if } N_L \leq N_J \leq 2N_L - 3
\\
N_L - 1 & \text{if } 2N_L - 3 \leq N_J \leq \frac{1}{2} N_L(N_L - 1)
\end{cases}$${#eq:link-max}

Then, using trial and error looping program to find the amount of each type of links. After all, they will be represented as $[N_{L_2}, N_{L_3}, N_4, ...]$.

According to connections between binary links and multiple links, a series of binary links has only two external joints, same as a single binary link. So the quantities of different types of binary link series can be defined as contracted link assortment.

The number of a $n$-series binary links can be written as $N_{C_n}$, and the summation of all types of series binary links can be written as $N_C$. In {@eq:nc-syn1} and {@eq:nc-syn2}, the numbers of a contracted link assortment should meets the conditions of $N_C$ and $N_{L_2}$.

$$
\sum_{i=1}^{i_{\max}} N_{C_i} = N_C
$${#eq:nc-syn1}

$$
\sum_{i=1}^{i_{\max}} iN_{C_i} = N_{L_2}
$${#eq:nc-syn2}

Where $i_{\max}$ can be defined as {@eq:i-max-origin}, and corrected as {@eq:i-max-fixed} because the max number of contracted link should not exceed the number of binary links $N_{L_2}$.

$$
i_{\max} = N_{L_2} - N_C + 2
$${#eq:i-max-origin}

$$
i_{\max} = \min\{N_{L_2}, N_{L_2} - N_C + 2\}
$${#eq:i-max-fixed}

The range of $N_C$ is between two values, as shown in {@eq:nc-range}.

$$
\max\{1, J_M - J_M'\} \leq N_C \leq \min\{N_{L_2}, J_M\}
$${#eq:nc-range}

![Connections between multiple links](images/multiple-link-connections.png){#fig:multiple-link-connections width=60%}

In {@eq:nc-range}, the symbol $N_C$ is related with number of multiple links $N_M$, where $J_M$ ({@eq:Jm}) is the number of joint pairs between all multiple links, and $J_M'$ ({@eq:Jmp}) is joint quantity between two multiple links, shown as contracted graph in {@fig:multiple-link-connections}. Where assuming the nodes is links, number of connections $J_M$ that between three nodes is 5, and maximum connections $J_M'$ is 2.

$$
J_M = \frac{1}{2} \sum_{m=3}^{m_{\max}} mN_{L_m}
$${#eq:Jm}

$$J_M' = \begin{cases}
0 & \text{if } N_M \leq 1
\\
\frac{1}{2}[3(N_M - 1) - 1] & \text{if } N_M \in 2\mathbb{Z}^+
\\
\frac{1}{2}[3(N_M - 1) - 2] & \text{if } N_M \in 2\mathbb{Z}^+ + 3
\end{cases}$${#eq:Jmp}

For the rigid chains or degenerated kinematic chains, since the degenerated kinematic chains has three links loop, and the contracted graph is a multigraph, a planar graph with maximum direct connections will be assumed as $J_M'$. Even if the number of $J_M - J_M'$ is less than 1, the lower bound of $N_C$ is still as 1.

$$J_M' = \begin{cases}
0 & \text{if } N_M \leq 1
\\
1 & \text{if } N_M = 2
\\
3(N_M - 2) & \text{if } N_M \geq 3
\end{cases}$${#eq:Jmp-improved}

![A planar max connected graph with 6 vertex](images/planar-max-connected-graph.png){#fig:planar-max-connected-graph width=30%}

According to a simplified Euler's formula $E \leq 3(V - 2)$ of Kuratowski's Theorem, the maximum number of planar connections can be obtained. For the case of 6 vertices ({@fig:planar-max-connected-graph}), the max number of edges is 12. So $J_M'$ can be redefined as the max number of direct connections between multiple links; and $J_M$ is the joint number of multiple links. The maximum number of contracted links $N_C$ can be assumed as the total number of connections $J_M$, but it will be assumed as the number of binary links $N_{L_2}$ if $J_M$ is lower than $N_{L_2}$. The minimum number of contracted links $N_C$ can be assumed as the duplicate number of connections ($J_M - J_M'$), if no it will be assumed to be 1.

The previous equations {@eq:nc-syn1} and {@eq:nc-range} can be written as {@eq:new-nc-syn}.

$$
\max\{1, J_M - J_M'\}\leq \sum_{i=1}^{i_{\max}} N_{C_i} \leq \min\{N_{L_2}, J_M\}
$${#eq:new-nc-syn}

The contracted link assortment can be represent as $[N_{C_1}/N_{C_2}/N_{C_3}/...]$.

For example, $(8, 10)$ is an one DOF mechanism, whose $m_{\max}$ is 4. The equation is shown in {@eq:num-8-10}.

$$\begin{cases}
N_{L_2} + N_{L_3} + N_{L_4} = 8
\\
2N_{L_2} + 3N_{L_3} + 4N_{L_4} = 2 \times 10
\end{cases}$${#eq:num-8-10}

At last, three answers $[4, 4, 0]$, $[5, 2, 1]$ and $[6, 0, 2]$ can be found out. For the case $[6, 0, 2]$, $N_C$ is between 3 to 4, as shown in {@eq:j-8-10}.

$$\begin{cases}
J_m = \frac{1}{2} (0\times 3 + 2 \times 4) = 4
\\
J_m' = \frac{1}{2} [3(2 - 1) - 1] = 1
\\
\max\{1, 4 - 1\} \leq N_C \leq \min\{6, 4\}
\end{cases}$${#eq:j-8-10}

And then, $i_{\max}$ can be obtained as 5. Furthermore, the condition equations can be defined as {@eq:nc-8-10}.

$$\begin{cases}
i_{\max} = \min\{6, 6 - 3 + 2\} = 5
\\
3 \leq N_{C_1} + N_{C_2} + N_{C_3} + N_{C_4} + N_{C_5} \leq 4
\\
N_{C_1} + 2N_{C_2} + 3N_{C_3} + 4N_{C_4} + 5N_{C_5} = 6
\end{cases}$${#eq:nc-8-10}

For the above conditions, the contracted link assortment can be find out: $[0/3/0/0/0]$, $[1/1/1/0/0]$, $[2/0/0/1/0]$, $[2/2/0/0/0]$ and $[3/0/1/0/0]$.

## Graph Enumeration

The graph representation used by Yan is Adjacency matrix. In the programming implementation of this research is adopted from Python module "NetworkX" [@networkx]. NetworkX uses edge list, a 2D array with pairs of nodes that represent as two end points of each edge, which saves more space for closed loop graph.

### Generalized Chain Enumeration

The process methodology of structural synthesis is using Edge Set of undirected graph. Contrary to the generalized kinematic chains, each link are turned into node, and each joint will turn into edges. An Edge Set contains only edges but an adjacency table will also be applied in this calculation. The flow chart of structural synthesis algorithm as shown in {@fig:structural-synthesis}, which consists the enumeration process of the "contracted graphs" and "conventional graphs".

![Flow chart of structural synthesis algorithm](images/structural-synthesis.png){#fig:structural-synthesis width=50%}

During the process, link assortment (LA) and contracted link assortment (CLA) is required, and the algorithm will check if the assortment is valid or not. "Contracted graph" is a type of multigraph, which allows multiple edges connecting to the same pair of the start and end nodes. Assuming that $n_{ij} = n_{ji}$ is the number of edges between node $i$ and $j$, and $n_{ii} = n_{jj} = 0$ because there is no self-loops. With the equations of labeled vertices ({@eq:contracted-graph-equations}) transformed to the matrix $M$ ({@eq:contracted-graph-matrix}) and the sum of joints ({@eq:contracted-graph-all-joints}). The number of variables are $\frac{n(n - 1)}{2}$, where $n$ is equal to the number of multiple links $N_m$.

$$\begin{cases}
\sum_{j = 0}^{N_m} n_{0j} = n_0
\\
\sum_{j = 0}^{N_m} n_{1j} = n_1
\\
\sum_{j = 0}^{N_m} n_{2j} = n_2
\\
\dots
\\
\sum_{j = 0}^{N_m} n_{(N_m)j} = n_{(N_m)}
\end{cases}$${#eq:contracted-graph-equations}

$$M = \begin{bmatrix}
n_{01} & n_{02} & n_{03} & 0 & 0 & 0 & \dots & n_0
\\
n_{10} & 0 & 0 & n_{12} & n_{13} & 0 & \dots & n_1
\\
0 & n_{20} & 0 & n_{21} & 0 & n_{23} & \dots & n_2
\\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \ddots & \vdots
\\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \dots & n_{(N_m)}
\end{bmatrix}$${#eq:contracted-graph-matrix}

$$
\sum_{k = 0}^{N_m} n_k = 2N_j
$${#eq:contracted-graph-all-joints}

If the number of variables is less or equal than the number of equations (same as $N_m$), the matrix $M$ can be solved by Gaussian Elimination, if it's not, the solution will be enumerated by Cartesian product, because the equations are Diophantine Equations. The product enumeration is carried out through the equations one by one, if the first equation is passed, then the answers will be applied to the next equation, until all the verification are successful. Then, the enumeration algorithm will generate planar contracted graphs such as link assortment $[x, 2, 1]$, where $x$ is the number of binary links, as shown in {@fig:contracted-graph-example}.

![A contracted graph example of $[x, 2, 1]$](images/contracted-graph-example.png){#fig:contracted-graph-example width=35%}

After the picked edges are combined, then permuted the picked edges to match the contracted link assortment. The combination of edges needs to pre-select the multiple edges first, because the link can't be directly connected to another link with twice or more. For example, when contracted link assortment $[2, 1]$ is applied to link assortment $[x, 2, 1]$, where $x$ is equal to 4, the atlas can be generated with degenerate check and isomorphism check as shown in {@fig:contracted-graph-enumeration}.

![Atlas enumerated from $[2, 1]$ applied to $[4, 2, 1]$ contracted graph](images/contracted-graph-enumeration.png){#fig:contracted-graph-enumeration width=70%}

For each link assortment and contracted link assortment, the atlas will be transformed back as the visualized outfits shown in {@fig:example-atlas}, called "conventional graphs".

![Example atlas of (8, 10)](images/example-atlas.png){#fig:example-atlas width=40%}

### Cut-links Check

This cut-links check process is to check if there is any link in the "bridge" of the generalized chain. When the bridge link is the ground link (frame), the mechanism can be divided into multiple parts.

The operation is known as "Cut-vertices Check" or "Articulation Point Check" in Graph Theory. Each vertex will be removed to check the adjacency of the graph then placed it back. If the graph is divided, the removed vertex will become a cut-vertex.

### Planar Check

The planar graph testing process is adapted from the algorithm of NetworkX, which is based on Ulrik Brandes's Left-Right Planarity Test [@LR-planar]. The algorithm will attempt to establish a counterexample or an embedding by using vertex mapping [@planar-graph] to test the graph including a Kuratowski subgraph that is above $K5$.

As for Left-Right Planarity Test, the concept is to let the graph drawn as a tree-like sketch, all of edges are divided into left and right sides in this representation. The bottom node is called "root", the other nodes will be browsed by Deep First Searching method, and the path will became central trunk. The Edges are treated as branch relative to central trunk if it does not belong to a part of trunk, then place it according to the order of its start node and end node.

![$E1$ and $E2$ are T-alike; $E1$ and $E3$ are T-opposite](images/t-alike.png){#fig:t-alike height=30%}

![$E1$ and $E2$ are T-opposite](images/t-opposite.png){#fig:t-opposite height=30%}

There are two states when placing edges. "T-alike" describes two edges that can be placed to the same side, "T-opposite" is the counter case, as shown in {@fig:t-alike} and {@fig:t-opposite}. Finally, if all of edges can be placed without crossings, then the graph can be proved as a planar graph.

### Degenerate Check

The degenerate check process needs to check whether a degenerate chain is a subgraph or not, which is referenced from Hwang Wen-Miin and Hwang Yii-Wen's algorithm [@degenerate-check], but the implementation in this research uses Edge Set instead of Adjacency matrix in the algorithm. A fast triangle test can quickly detect degenerate chain usually, then the second process will undertake next.

During the process, the connected two or more series binary link will be removed one by one. If a ternary link connects with a binary link that was removed, the link will became binary link, and so on. In the end, the subgraph will be left as an smaller graph, when its degrees of freedom is zero, the graph become a degenerated chain.

As for another case, the subgraph will be possibly turned into a simple loop graph, if the loop graph is triangle, the generalized chain is the degenerated chain, while it's quadrilateral and above, the graph is not a degenerated chain.

### Isomorphic Check and Labeled Enumeration

The isomorphic check is adapted from the algorithm of NetworkX, which implements VF2 algorithm [@vf-two].

The time complexity of VF2 algorithm is $\Theta(N^2)$ with best case and $\Theta(N!N)$ with worst case, which is lower then $\Theta(N^3)$ of Ullmann's algorithm [@ullmann] with best case and $\Theta(N!N^2)$ with worst case. The spatial complexity of VF2 algorithm ($\Theta(N)$) is also lower then Ullmann's algorithm ($\Theta(N^3)$). Another improved version of VF2 algorithm that varies from 10 to 1000 times, has been proposed in 2017, called VF3 [@vf-three].

![Flow chart of VF2 algorithm](images/vf2.png){#fig:vf2-algorithm height=65%}

The flow chart of VF2 algorithm in this research shown as {@fig:vf2-algorithm}. The main method will attempt to map the nodes of graph $G2$ to $G1$, which create a $G2$ coverage "state". The VF2 algorithm can be used as graph or subgraph isomorphism test. If the test target is to compare the graph $G1$ with $G2$, the coverage state must cover all of the graphs; if the test target is to compare a graph $G1$ with its subgraph $G2$, the coverage state must be covered with $G2$.

In the flow of creating coverage state, the algorithm will test if the candidate pair can be applied to two graph $G1$ and $G2$. It is called semantic feasibility and syntactic feasibility. At last, the incompatible situations will return earlier, and the completely matching situation will return later.

If the checking mechanism applied to labeled nodes, the nodes will be temporarily ignored as a subgraph during process. A vertices marked as special usage one by one such like grounded enumeration before turn a generalized chain into a mechanism.

### Backward Analysis Method of Generalized Chain

If a generalized chain is known, to analysis its link assortment and contracted link assortment is possible. The algorithms shown in this section can get the above mentioned information to provide the attributes of graph enumeration algorithm.

Link assortment is the number of each $n$-ary link, sorted by the number of their connections. To check the connection number of a node (link), use Adjacency Lists of the graph can find the degree of the node (link).

Contracted link assortment is the number of each $n$ series binary links, sorted by the number of their connected. Continuously connection of binary links can be found out by deep-first searching method with Adjacency Lists of the graph. Assuming the start node is in the middle part of a series, two terminal nodes needs to be found out with forward and backward tracking recursively. If searching method can't complete forward or backward tracking with the node, it might be the one of terminal nodes or it is a single binary link.

### Outer Loop Layout

The process of graph layout just present the graphs to the designer. Transmission and comparison operation given by the designer are still based on Edge Sets to avoid misunderstanding.

There is a lot of graph layout mechanism of generalized chain. In the implementation of this research, the greedy method of outer loop is used, and the nodes inside the loop will be placed as the linear layout. This layout method will have better performance with smaller graphs. The process of the algorithm is shown in {@fig:outer-loop-layout}.

![Flow chart of outer loop layout algorithm](images/outer-loop-layout.png){#fig:outer-loop-layout}

The "outer loop" is found by the cycle basis of the graph with merge all of them. The closer cycle basis will be merged first. Cycles $C1$ and $C2$ need to be different direction when assembled, and the target node series will the longest of its cycle. If the remaining cycle basis have no intersection, there will have two or more "bridge" connections between the cycles, get the longer node series to merge them.

After get the outer loop of the graph, the inner nodes will be placed as Bezier curve across the outer loop, the longest of line path has higher priority. If two or more paths have the same start node and end nodes. The curve angle will be changed with equal division. For example with the $(8, 10)$ atlas of kinematic chains, the linkage transformation between polygons ({@fig:layout-example-atlas}) and nodes ({@fig:layout-example-atlas-node}) can be decided by middle points.

![Linkage layout as polygons](images/layout-example-atlas.png){#fig:layout-example-atlas width=40%}

![Linkage layout as nodes](images/layout-example-atlas-node.png){#fig:layout-example-atlas-node width=40%}

## Summary

The implementation of the graph enumeration algorithm is to convert the graph expression as Edge Set instead of Adjacency matrix and other contracted matrix, which caused the retrieval and comparison of data formats to be redesigned. But the Edge Set expression with Adjacency Lists has more readable and no size limitation on generalized chain.

"Specialization" process has been discarded in this research, because it can be fully replaced by isomorphism checking of labeled vertices. And the picking step of grounded enumeration can still be manipulated by designer(s).

On the other hand, the DOF of generalized joints are assumed as 1, this can cause differences in the design process for other types of joints. The other types of joints will probably decided from another graph with lower degrees and caused the misunderstanding of the same structure. For performance, the smaller graphs are faster than larger graphs, so the different types of joints needs to be configured with labeled enumeration before dimensional synthesis.
