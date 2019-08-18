# Related Works

## Creative Design Methodologies of Mechanism

Creative Design Flow of Mechanism was proposed by Yan [@creative-design][@creative-mechanism-design][@yan-permutation-group]. The process has been mentioned with structural synthesis, dimensional synthesis and dynamics design. On the other hand, a type of mechanical conceptual design was proposed, inspired by enumerating major functions just like TRIZ methodology [@backward-design].

Nowadays, computer-aided Creative Design Methods with generalized atlas can be used to design elementary components, such as prismatic joints [@martin-type-synthesis], ratchet [@cad-creative-mechanism-design], magnetic gears [@cad-magnetic-gears], CAD-based expert system [@opsyn], etc. The design flow in the thesis uses several algorithms and numerical methods to handle with enumeration and analysis, including graph enumeration algorithms, evolution algorithms and geometric constraint solver.

Many different graph enumeration methods of generalized chain for respective usage, including automatic sketching methods [@auto-sketching]. Among them, the method of contracted graph enumeration with contracted link assortment is used to improve the performance. An enumeration method of contracted graph with labeled atlas was proposed [@automatic-structural-synthesis].

The structural synthesis of design flow is referenced from Hong-Sen Yan and Yu-Ting Chiu's dissertation [@yan-number-synthesis], the comparison between the methodology and this research is shown in {@tbl:comparison-creative}.

| Method | Reference [@yan-number-synthesis] | This Research |
|:----------------------|:------------|:------------|
| Graph Expression | Adjacency Matrix | Edge Set |
| Generalization Chains | Undirected Graph | Same |
| Contracted Graphs | Multigraph | Same |
| Link Assortment | Diophantine Equations with Cartesian product | Same |
| Contracted Link Assortment | Same as above | Equations corrections |
| Enumeration of Contracted Graphs | Same as above | Same |
| Enumeration of Conventional Graphs | Permutation and Combination | Same |
| Specialization | Not included | Replaced by Labeled Enumeration |
| Cut-link Checking | Cut-link Checking | Same |
| Degenerate Checking | Degenerate Checking [@degenerate-check] | Same |
| Planar Graph Checking | Kuratowski's Theorem | Ulrik Brandes's Left-Right Planarity Test [@LR-planar] |
| Isomorphism Checking | Franke's Condensed Notation [@franke-notation] | VF2 Algorithm [@vf-two] |
| Graph Sketching | Hyper Graph and Line Graph | External Loop Algorithm |
| Programming Implementation | C# | Python / Cython |
| GUI | Stand-alone (Microsoft Windows Forms) | Stand-alone (PyQt) |
| Open Source | No | Yes (Pyslvs) |

Table: The comparison of Creative Design Methodologies {#tbl:comparison-creative}

## Dimensional Synthesis Methodologies

There are several types of dimensional synthesis methods: geometric method, analytical method, numerical method and even Neural Networks [@neural-network-synthesis] to solve the tasks like Motion Generation, Path Generation, Function Generation and dead-center problems [@analysis-synthesis]. According to Acharyya and Mandal's thesis [@algorithm-performance], when using Differential Evolution , compared with Particle Swarm Optimization and Genetic Algorithm, on four bar linkage synthesis, the former has the best performance. The algorithm can synthesize four bar or six bar linkage mechanism with auto-adaptive parameters [@de-adaptive-parameters]. On the other hand, some of the examples that integrated structural and dimensional synthesis was used on the exoskeleton [@integrated-type-dimensional-synthesis], or a generic problem with topology decomposition [@modular-dimensional-synthesis].

One of verification mechanisms is using numerical methods, such as with geometric constraint solver kernels. The solvers are using Newton-Raphson method and Broyden-Fletcher-Goldfarb-Shanno algorithm (BFGS algorithm) implemented by two CAD application libraries, Solvespace [@solvespace-site] and Sketch Solve [@sketchsolve-site] respectively.

Another verification mechanism is following with Lee's thesis [@django], which uses two geometric formulas PLAP and PLLP. The geometric formulas is faster than numerical methods in comparison with computer performance. The comparison between the methodology and this research is shown in {@tbl:comparison-dimensional}.

| Method | Reference [@django] | This Research |
|:----------------|:------------|:------------|
| Algorithms | Metaheuristic Random Algorithm | Same |
| Type of Mechanisms | Planar Four Bar Linkage Mechanisms | Planar Linkage Mechanisms |
| Verification Formulas | PLAP, PLLP | PLAP, PLLP, PLPP, PXY |
| Formulas Configuration | Direct Formulas | Automatic Configuration Algorithm |
| Programming Implementation | Python / Cython | Python / Cython |
| GUI | Server based (Django) | Stand-alone (PyQt) |
| Open Source | Yes | Yes (Pyslvs) |

Table: The comparison of Dimensional Synthesis Methodologies {#tbl:comparison-dimensional}

## Design Grammar and Mechanism Expression

To simplify the topological structure during design process, the design grammars of the research are basically extended from the expression of Graph Theory.

For the synthesized results, the mechanism expression is used as representation of the mechanism. The expression is referenced from Radhakrishnan's dissertation [@automated-design-of-planar-mechanisms], which is a read and store mechanism used in a web application. The comparison between the methodology and this research is shown in {@tbl:comparison-expression}.

| Method | Reference [@automated-design-of-planar-mechanisms] | This Research |
|:----------------|:------------|:------------|
| Graph | Hypergraph | Same |
| Dimension | Planar | Planar |
| Implemented Joint Types | Revolute (R), Prismatic (P), Revolute-Prismatic (RP) | Same |
| Literal String | Coded String, URL | Literal String, YAML |
| Simplified Attributes | No | Redefined Default Values, Name of Links |
| Input Joint Types | R | R, P, RP |
| Programming Implementation | C# | Python / Cython |
| GUI | Server based (Microsoft Silverlight) | Stand-alone (PyQt) |
| Open Source | Yes (PMKS) | Yes (Pyslvs) |

Table: The comparison of Mechanism Expression Methodologies {#tbl:comparison-expression}

## Summary

![Major composition of the research](images/major-composition.png){#fig:major-composition width=45%}

With three different phases of methodologies [@yan-number-synthesis][@django][@automated-design-of-planar-mechanisms], the design process of planar linkage mechanisms can be categorized as the "Creative Design", "Dimensional Synthesis" and "Mechanism Expression" respectively.

The relation of the major composition as shown in {@fig:major-composition}, where graph expression is the design grammar; "Mechanism Expression" is the result grammar of "Structural Synthesis" and "Dimensional Synthesis" under the Creative Design Method. And the design flow is a type of Function-based and Analogy-based synthesis mentioned in the overview of computer-based design synthesis researches [@computer-design-overview].
