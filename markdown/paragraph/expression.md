# Mechanism Expression

## Planar Mechanism Kinematic Simulator

Planar Mechanism Kinematic Simulator (PMKS) [@pmks-site] is a C# library that provide kinematic calculations as online software and Microsoft Office Excel plug-in. In their online interface, a new style expression has been used. That expression can clearly represent the dimension and adjacent of all links and joints for the mechanism.

### Attributes

In the expression of PMKS, the links are corresponded to nodes, and the joints are corresponded to edges, it is actually a hypergraph because edges can combine with multiple nodes, not only start node and end node.

![PMKS expression](images/pmks-expression.png){#fig:pmks-expr}

As {@fig:pmks-expr} shows, all the links will be given a name, except the framework will be named as "ground", the remaining links allow users to name by themself, such as "coupler", "follower", etc.

A joint has five attributes, they are: links, type of joint, x position, y position and the angle of slot.

1. "Links" attribute represents an orderly list of links that connected with this joint, these names will be called "labels". Base link is the first link in the "links" attribute and it will beome a slider slot when the joint type is prismatic joint, other links will be connecting on joint.

1. "Type" attribute is the method that the connecting links used of. There are three types of joint in PMKS: revolute joint (R), prismatic joint (P), revolute-prismatic joint (RP), gear joint (G). However, G joints are not supported in the simulation section and the program of this research, which still can be applied in the expression actually.

1. "X position" and "y position" attributes are the instantaneous position of this joint on the mechanism in cartesian coordinate.

1. "Current position" attribute are the simulated position of this joint on the mechanism that are using cartesian coordinate. For the slider joints, there are two coordinates with this attribute. The first is the position of slot center, the second is the pin position.

1. "Angle" attribute is the angle between slider slot and horizontal axis on the base link. However, this attribute will not cause any affect if the joint is not a slider.

The above definition can refer to Pradeep Radhakrishnan's dissertation [@automated-design-of-planar-mechanisms]. A benefit of expression is that users can using as uniform resource locator (URL) to put all of attributes to the address bar of browser, then parsing the URL easily by server program.

### Grammar

In the program side, the text based expression can be either table interface or URL string, that will be more readably and effortlessly to handle in grammar parser. The grammar parser used is "lark-parser", a high performance Python module parsing by JavaScript Object Notation (JSON) technology, based on Extended Backus–Naur Form (EBNF) with more supplementary grammar (such like Regular Expression).

In order to collocate the user interface, "color" attribute has been added. It means to present the joint as "red", "green", "dark-blue", "yellow" and other common colors that can be using case insensitive name string directly, or through a "(R, G, B)" string with using RGB color model.

The text based expression just like follows:

```python
# Single line annotation.
M[
    J[R, color[Green], P[0.0, 0.0], L[ground, link_0]],
    J[R, color[Green], P[12.92, 32.53], L[link_0, link_1]],
    J[R, color[Green], P[73.28, 67.97], L[link_1, link_2]],
    J[R, color[Green], P[33.3, 66.95], L[link_1]],
    J[R, color[Green], P[90.0, 0.0], L[ground, link_2]],
]
```

The syntax in pair of small brackets are optional, like the "angle" attribute can be ignored when the "type" is "R". And the ellipsis means the same type of syntax can be multiple, just separated by commas. Writing as multi-line, indentation and more than one blank character are also be allowed in the syntax.

"VPoint" is a class of data structure that can help us to carry on the attributes as we mentioned before. A list of "vpoints" are not sequential, because the ordering priority will not cause calculation more efficient.

### Input Pairs

The input options is called "input pairs" in this research, which is a table of data including the revolute and slider joints. They include"base point", "driver point" and "angle value". The angle value is relative to absolute coordinate system, even the joint is not on the frame. The "angle value" is only applied on the input of revolute joints.

For slider joints as input, the "base point" and "driver point" are assumed as the same point. And "angle value" is replaced by "offset". "Offset" value is defined by the distance between "slider pin" and "slot base" with positive and negative sign.

## Topological Analysis

The topological methods of undirected graphs can be used at hypergraphs, but needed to searching with edges instead of nodes (vertices). For convenience, an Adjacency List viewed from vertices are usually created to decrease the time costs.

Mechanism expression is based on Objective Programming instead of graph expression (Edge Set) in this research, and Adjacency List is implemented by Hash Set. The joints view and links view are named as "vpoints" and "vlinks" respectively, which are corresponded to edges and vertices in hypergraph.

### Degrees of Freedom

The degrees of freedom (DOF) can be decided from the expression by {@eq:pmks-dof}.

$$F = 3(N_L - 1) - 2 \times J_1 - J_2$${#eq:pmks-dof}

Where $J_1$ is stands for the number of joints that only have one degree of freedom, like revolute joints or prismatic joints; and $J_2$ means two degree of freedom, like revolute-prismatic joints or gear joints.

In the program side, counting function needs to ignore the joint that is not "connecting links" actually: "link" attribute will contain one or zero of name label. In other hand, when the number of name labels is greater or equal to three, it means the joint is a multiple joint and should be treated as two or more of joints in the same position.

The counting function will only scan "vpoints" once. First,  it will skip the joint that "link" attribute contain less than two of name label. Second, collecting name labels from "link" attribute, which is using to calculate the number of links. At last, the degrees of each joint will be counted to $J_1$ and $J_2$, and $J_1$ will be plus one for each extra link.

### Contract Joints Operation

When multiple joints are needed to decrease as one joint, the operation can be done as union operator of Set Theory. Assuming that the selected points $p_s = \{p_1, p_2, ..., p_i\}$ will merge to target point $p_m$, all of links $l_s = \{\{l_{11}, l_{12}, ...\}, \{l_{21}, l_{22}, ...\}, ..., l_i\}$ need to transfer to the superset links of target point $l_m$ ({@eq:contract-joints-links}), then remove all the selected points ({@eq:contract-joints-graph}).

$$
l_m \coloneqq l_m \cup (\bigcup_{n = 1}^i l_n)
$${#eq:contract-joints-links}

$$
G \coloneqq G - p_s
$${#eq:contract-joints-graph}

This operation is not considered for geometric constraints, so the dimensions of links are not kept during contracting the selected joints. Actually, the coordinates of them will be turned into base joint's.

### Mapping to Geometric Constraint Solver

In order to apply the constraint solvers mechanically, the expression of planar linkage mechanism needs to transfer to a generic constraints form. Since the planar mechanisms are 2D geometries, the constraints can be simplified from transform matrix without considering for a third dimension. The required geometric constraints are as follows:

1. Constant point position: A point must keep its coordinate as the highest order, avoid to change the values.

1. Distance between point to point: A point must keep distance to another point. The distance can be zero as coincident constraint.

1. Angle between line to line: A line must keep an inner or external angle to another line and absolute coordinate system. The angle can be $\{0^\circ, 90^\circ\}$ as parallel constraint or perpendicular constraint. Also, a control option can be decide the orientation of the angle value.

1. Point on line: A point must stay on a line or its extension line.

Since the position of grounded joints can't be changed, the position constraints will be applied at first. In the numerical methods, the values of coordinates won't be treated as variables. And the values are a type of known conditions, same as the lengths of links.

The dimension of polygon links is done by the distance constraints between its points. First, any two points are constrained with one side dimension, then constrain remaining points one by one as triangulation, as shown in {@fig:polygon-link-constraints}.

![The constraints of polygon link](images/polygon-link-constraints.png){#fig:polygon-link-constraints width=40%}

Moreover, input joints and its crank bar will be applied angle constraint with its input angle. The constraint is relative to absolute coordinate system, even the joint is not on the frame. The reference point is a specific joint on the same link with input pair.

![The constraints of slider](images/slider-constraints.png){#fig:slider-constraints width=50%}

As shown in {@fig:slider-constraints}, for the slider joints, prismatic joints will have an extra angular constraint than revolute-prismatic joints. The basic constraint is "point on line" between "slider pin" and "slot line", where "slot line" is consisted by "slot base" and "slot point" with an angular constraint between "slot line" and "slot link" will be applied to keep the slider angle same as its attribute, but if the "slot link" is the frame, the angular constraint won't be applied. Additionally, if the joint is an input point, there will be added a distance constraint between "slot base" and "slider pin" with the offset value, where the negative sign means they need to be inversed or not.

### Generalization Algorithm

A generalized chain can be represented as a graph that the links map to nodes and the joints map to edges. For the PMKS expression, the ground link label can be replaced with any other name to release the mechanism from the frame.

Some of other information can be parsed from the mechanism expression:

1. Grounded joints: A list of grounded joints.

1. Input list: A list of input angle acting on rotation joints. The number of input list is same as DOF.

1. Position data: The position of each joints.

1. Redundant points: A list of points that are not connected with more than two links so they are strictly not joints.

1. Multiple joints: A list of joints that are have same position with some other joints.

According to Cayley's formula [@cayley], the number of trees on $n$ labeled vertices is $n^{n - 2}$. It equals the number of multiple joints mapping to single joints when the mechanism expression transform into generalized chain. The generalization algorithm is sorting by the order of link labels to generate with same results. The flow chart of generalization algorithm is shown in {@fig:generalization}.

![Flow chart of generalization algorithm](images/generalization.png){#fig:generalization width=90%}

In the part of dimensional synthesis, these information can be used as turn the generalized chain back to the mechanism expression.

## Summary

In the field of information technology, designing expression is the most important thing to decide the data structure. In this section, two kind of expression are used to represented the adjacency of mechanism and the processing rule of directly solving algorithm.

One of the advantages is the expression will be simpler to present the multiple joints in graph adjacency. Another benefit is the joint type, placement and slider angle of slider joint will belong with single clause in grammar. But the curve elements can't be included in the grammar, like a curve slider slot or a cam profile.

Unified mechanism expression can be applied to various geometric constraint solvers. The position analysis of planar linkage mechanism can be simulated with the distance constraint and coincide constraint, including locked chains.
