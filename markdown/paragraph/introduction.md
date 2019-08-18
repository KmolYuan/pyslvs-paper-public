# Introduction

## Motivation

According to the overview of computer-based design synthesis researches [@computer-design-overview], the synthesis design flow is divided into the following categories: Function-based, Grammar-based and Analogy-based. Function-based synthesis is to redesign the decomposed partial solutions due to the functionality of product, as shown in {@fig:synthesis-design-categories}. Grammar-based synthesis is a type of conceptual design by using generative grammars. Analogy-based synthesis design is drawing inspiration from the previous design knowledge.

![The design categories of computer-aided synthesis](images/synthesis-design-categories.png){#fig:synthesis-design-categories width=30%}

The major concept of recreating an existing mechanism is to synthesize the mechanism through the Creative Design Flow of Mechanism (abbreviated as Creative Design Method). A mechanism has two properties according to its functional appearance, which are the structure and dimension, where the structure is related to the adjacency between the joints and links, and the dimension means the distance and orientation of the joints in space. The design flow, including structural synthesis and dimensional synthesis is also related to the Graph Theory and kinematics simulation.

Planar linkage mechanisms have a very wide range of applications, such as windshield wipers, pure gear system, cushion mechanism, etc. Some of the spatial linkage mechanism is composed of planar components, for example, a parallel manipulator can be composed by multiple four bar linkage components. All those designs of applications need to organize the manufacture of the products then verify their comprehensiveness, and that is indispensable with the assistance of computer.

With the Metaheuristic Random Algorithms, Numerical Methods or Enumeration Algorithms, the design problems can be solved by various convergence strategies under realistic constraints. In this case, Python programming language is a tool for high-speed abstract logic development, cross platform environment and concise expression, will be used for the implementation of these concepts.

The synthesis process and applications of planar mechanism are used in different environments. The structure and dimension constraints can be designed by computer aided process with generic design grammar in this research.

## Statement of Research

"Creative Design", "Structural Synthesis", "Dimensional Synthesis" (path generation) and "Mechanism Expression" are the major composition of this research. The related theories and methodologies will be collected, composed and implemented into this thesis. The flow chart is shown in {@fig:computer-aid-design-method}.

![Conceived computer-aided design method of this research](images/computer-aid-design-method.png){#fig:computer-aid-design-method width=70%}

+ Forward Synthesis: Derived from "Creative Design Method", it uses "Structure Synthesis" to find new types of mechanism. The generalized chain will be "Dimensional Synthesized", then output as "Mechanism Expressions".

+ Backward Analysis: Convert "Mechanism Expressions" into generalized chains and get their atlas classification.

Among them, the data conversion processes between different steps are needed to be designed. The details such like the variables of Dimensional Synthesis are needed to be decided, as shown in {@fig:design-flow-do}. And the reference of the design flow will be introduced in next chapter.

![Contemplated design flow of this research](images/design-flow-do.png){#fig:design-flow-do}

The purpose of this research is to reproduce the graph enumeration algorithm of generalized chain with numerical dimensional synthesis method, and verify the results with kinematics simulation, which is also reproduced from the mechanism expression of PMKS and included backward analysis process supplied by several topological algorithms.

## Project Architecture

When binding with C++ libraries, the Interface Description Language (IDL) can be used to combine the implementation of multiple algorithms written in different programming languages. In this research, Python is the major language used to integrate all the algorithms, and develop the graphical user interface efficiently. There are many Python interfaces for C++ language, such as Protocol Buffers (Protobuf) of Google, Simplified Wrapper and Interface Generator (SWIG) [@swig], AutoWIG [@autowig], Cython language [@cython], SIP for PyQt [@sip-site], Shiboken for PySide, etc. For the wrapper of CAD libraries, SWIG and Cython techniques were used as the interfaces for Solvespace and Sketch Solve respectively.

Although Cython can be used as IDL between C / C++ and Python, itself also is a superset language of Python with part of C / C++ features. The software architecture in this research is designed to create an algorithm prototype written in Python, which can speed up software performance by translating into Cython language, rather than being written in C / C++ directly. Especially, when handle with lots of low-ordered data types and frequently accessed them in the operations, which can be benefitial from the memory management of Python, the libraries can be even faster than non-fully optimized pure C / C++ programs. Moreover, the algorithms can be combined with the main trunk directly.

![The software architecture of Pyslvs](images/pyslvs-architecture.png){#fig:pyslvs-architecture}

The programming implementation of this research is called "Pyslvs"[@pyslvs-github][@pyslvs-readthedocs][@pyslvs-blog], which is named after its programming language "Python" and all kinds of constraint "solvers". The software architecture is shown in {@fig:pyslvs-architecture}. The main program is the planar linkage mechanism simulator under the mechanism expression with several IO features, such like YAML format (text-based), DXF format (text-based), SQL database (binary-based), even Solvespace format (text-based). On the other hand, the functions of synthesis can generate and analyze the mechanism with the algorithms in this research, which is placed on the functional panels of the user interface.

## Organization of Thesis

The chapters in this research are organized according to technical requirements. Thus the related works and terminologies will be mentioned before the implementation of them. The chapters are as follows:

+ Chapter 2: The brief review of the Creative Design methods and the recent developments of the algorithms used in computer-aided design.

+ Chapter 3: The related terminologies will be explained in this section.

+ Chapter 4: The Number and type synthesis, link assortment, graph enumeration algorithms and checking conditions of generalized chain will be discussed in this chapter.

+ Chapter 5: Describe the C++ wrapper implementation of two geometric constraint solvers corresponded to two types of numerical methods.

+ Chapter 6: Present the Definitions of the mechanism expression and its topological analysis algorithms.

+ Chapter 7: Automatic Configuration Algorithm of position analysis method will be included in the methodology to synthesize the dimension.

+ Chapter 8: At last, this chapter will make a technical summary of its contributions and future works.

All the chapters have a short summary at last.
