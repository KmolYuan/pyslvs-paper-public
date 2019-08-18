# Application of Open Source CAD Libraries

## Solvespace

Solvespace is a free and open source computer aided design software developed by Jonathan Westhues, written in C++ language. The open source version was released in 2013, licensed under the GNU General Public License version 3.0. This program is very fast and takes small size (around 6 megabyte) that user can create 2D or 3D geometries easily in three different operating systems: Windows, Mac OS and Linux.

There is a main window and a sub window of Solvespace shown in {@fig:solvespace-main}. The sub window called "property browser", which can be used to look through or edit the geometric configuration data of the current file.

![Main window and sub window of Solvespace](images/solvespace-main.png){#fig:solvespace-main}

### Features Design

The user interface of Solvespace is written with GTK minus minus (gtkmm, an offical C++ interface for the GTK+ GUI library). User can use the 2D "entities" and "constraints" to create geometries, then transform 2D geometries to 3D geometries by simple "extruding" and "rotating" commands.

When the 3D parts has been created, they can be copied by "repeat rotating" or "repeat translating". Every blocks of geometries will be placed in different groups, and the groups can be switching to edit, rename, change appearance. In addition, Solvespace also has command line interface.

In its IO functions, the formats supported include drawing interchange format (DXF), scalable vector graphics (SVG), stereolithography (STL), etc. Solvespace has a text-based file format using "slvs" as name suffix.

Moreover, Solvespace has two more advantage for mechanical engineering: an external application programming interface for C++ and Virtual Basic language, and the file format are also readable and able-assembly.

### Geometric Constraint Solver

Solvespace separates the geometric constraint solver into two parts. One is the "entities", which is used to represent geometric data for each graph unit; and the other part is the "constraints", which is used to represent the relationships between graph units. The description of "entities" types are enumerated in {@tbl:solvespace-3d-entities} and {@tbl:solvespace-2d-entities}.

| Entity name | Description | Requirements |
|:------------|:------------|:-------------|
| Point 3D | A spatial point. | A three-valued coordinate. |
| Normal 3D | A spatial vector. | A quaternion or a three-valued unit vector. |
| Face | A spatial mesh. | A 3D normal and a point. |
| Work plane | A spatial plane. | A 3D normal and a 3D point. |

Table: 3D entities types of Solvespace {#tbl:solvespace-3d-entities}

| Entity name | Description | Requirements |
|:------------|:------------|:-------------|
| Point 2D | A planar point. | A plane and a two-valued coordinate. |
| Normal 2D | A planar vector. | A plane and a two-valued unit vector. |

Table: 2D entities types of Solvespace {#tbl:solvespace-2d-entities}

Some entities have dependencies between 3D and 2D geometric data, its requirements can be accepted from two types of geometries that is shown above. As enumerated in {@tbl:solvespace-other-entities}.

| Entity name | Description | Requirements |
|:------------|:------------|:-------------|
| Distance | A value use to be radius. | A value. |
| Line segment | A line between two points. | A pair of points. |
| Cubic | A Bezier curve. | Four points. |
| Circle | A circle perpendicular to a normal. | A point, a normal, a distance. |
| Arc | An arc defined by three points. | Three points, a normal, a distance. |

Table: Other entities types of Solvespace {#tbl:solvespace-other-entities}

Entities has a "group code" requirement that is not listed in the tables. A "group code" is used to distinguish the region of solving steps, 2D and 3D geometries will be placed in different groups usually. Different groups have different degrees of freedom.

The description of "constraint" types are enumerated in {@tbl:solvespace-constraints}.

| Constraint name | Description | Requirements |
|:----------------|:------------|:-------------|
| Points coincident | Coincidence between points. | Two points. |
| Distance | Distance constraint between two entities. | Two entities. |
| On | Coincidence between a point and an entities. | A point and an entities. |
| Equal | Equal value of lengths, angles or radius. | Two entities. |
| Symmetrical | Symmetry between two entities. | Two entities and a basis. |
| Midpoint | A point become midpoint of a line segment. | A point and a line. |
| Horizontal | A line or vector become horizontal. | A line or normal. |
| Vertical | A line or vector become vertical. | A line or a normal. |
| Diameter | Defined diameter of a circle. | A circle or an arc. |
| Angle | Defined angle between lines. | Two lines or work planes. |
| Parallel | Angle constraint become zero. | Two lines or work planes. |
| Perpendicular | Angle constraint become 90 degrees. | Two lines or work planes. |

Table: Constraint types of Solvespace {#tbl:solvespace-constraints}

Constraints also has a "group code" requirement too, top level group will be more preferred if the constraint is crossing group. The constraint will additionally has a "work plane" requirement when the objects contain at least a 2D entities.

The constraints are also be able to corelate different conditions of objects, which use vectors to solving non-unique solution.

### Scripting Rules

In the wrapper redesigned in this research, there is a solver "SolverSystem" class to represent data set of Solvespace solver. In other words, remove the "system" object can clear the data set and be able calculate again. There also have "Params" and "Entity" classes to represent all parameters and entities in Solvespace solver.

An entity or a constraint can be added to systems by its object methods, which is also pointed to the current system. Constraints can be added at any time with current entities, even they might caused over constraint. The length limitation of memory has been replaced as a dynamic allocate strategy before solve the system. The solving process in a Python script is shown in {@fig:python-solvespace-rules}.

![The process of Python-Solvespace solver](images/python-solvespace-rules.png){#fig:python-solvespace-rules height=50%}

After solved, Python script can use a determine statement to comparing with result flag. If solved ok, the data can be collected with the attributes in fake class, or return by system method with number code. If caused an error, there are some error flags to show the error type, including "inconsistent", "didn't converge" and "too many unknowns".

## Sketch Solve

Sketch Solve is a light 2D geometric solver library written in C++, under New BSD License. The solver of FreeCAD [@freecad] and HeeksCAD [@heekscad] are also based from Sketch Solve. Because it is a 2D solver, Sketch Solve will more faster then Solvespace solver, and the structure of solver will also friendly to be handled by a non-maintainer.

Sketch Solve is contributed by multiple developers, and its functions are also been adjusted with our demand.

### Geometric Constraint Solver

The source code of Sketch Solve only use expressions and functions to get the answer. There is a Broyden-Fletcher-Goldfarb-Shanno (BFGS, which is named after Charles George Broyden, Roger Fletcher, Donald Goldfarb and David Shanno) algorithm update with Newton-Raphson method, which is used as optimization routine.

The expression of Sketch Solve as shown in {@tbl:sketch-solve-expr}.

| Symbol | Attribute |
|:------:|:----------|
| Point | Two float numbers to represent a coordinate of a point. |
| Line | Two points to represent a start point and an end point. |
| Arc | A radius, a start angle, an end angle and a center point. |
| Circle | A radius and a center point. |
| Constraint | Data set of a constraint object. |

Table: Expression of Sketch Solve {#tbl:sketch-solve-expr}

Sketch Solve provides 39 constraint types, but some of implementations are deprecated caused they are too similar and unuseful, so we take 36. The principle of algorithm is to get an error from each times of constraint solving, and each error well be configure with different weights. For example, a "Point to point distance" constraint is calculate the error by a expected distance and a distance between two points, then multiply the error by 100 after squaring.

The types of constraint are similar with Solvespace, but some are more convenient for CAD developers. There is an idea to create "constraint" objects by function inputs, which is referenced from Solvespace. The functions are simply shown below.

| Constraint types | Description |
|:---------|:------------|
| PointOnPoint | Two points are coincided. |
| P2PDistance | The distance between two points. |
| P2PDistanceVert | The vertical distance between two points. |
| P2PDistanceHorz | The horizontal distance between two points. |
| PointOnLine | A point is on a line. |
| P2LDistance | The distance between a point and a line. |
| P2LDistanceVert | The vertical distance between a point and a line. |
| P2LDistanceHorz | The horizontal distance between a point and a line. |
| Vertical | A line should be vertical. |
| Horizontal | A line should be horizontal. |
| TangentToCircle | A line should be tangent with a circle. |
| TangentToArc | A line should be tangent with an arc. |
| ArcRules | Apply arc rules to an arc. |
| LineLength | The length of a line. |
| EqualLegnth | Two lines should be the same length. |
| ArcRadius | The radius of an arc. |
| EqualRadiusArcs | Two arcs should be the same radius. |
| EqualRadiusCircles | Two circles should be the same radius. |
| EqualRadiusCircArc | An arc and a circle should be the same radius. |
| ConcentricArcs | Two arcs should be the same center point. |
| ConcentricCircles | Two circles should be the same center point. |
| ConcentricCircArc | An arc and a circle should be the same center point. |
| InternalAngle | The internal angle between two lines. |
| ExternalAngle | The external angle between two lines. |
| Perpendicular | Two lines should be perpendicular. |
| Parallel | Two lines should be parallel. |
| Colinear | Two lines should be colinear. |
| PointOnCircle | A point is on a circle. |
| PointOnArc | A point is on an arc. |
| PointOnLineMidpoint | A point is the midpoint of a line. |
| PointOnArcMidpoint | A point is the midpoint of an arc. |
| PointOnCircleQuad | A point is on quartile of a circle by number 0~3. |
| SymmetricPoints | Two points is symmetric with a line. |
| SymmetricLines | Two lines is symmetric with a line. |
| SymmetricCircles | Two circles is symmetric with a line. |
| SymmetricArcs | Two arcs is symmetric with a line. |

Table: Constraint types of Sketch Solve {#tbl:sketch-solve-constraint}

The solving function is require a list of parameters, a list of constraint data and step accuracy. The parameters are projected to each instance, so the value will be changed. If there is a value not in the list, the value will not be changed, like an anchor point.

### Scripting Rules

Because C or C++ objects can't used with Python directly, the solver system and function calling should stay in Cython level. In C++ version of Sketch Solve API, developer need to handle with a list of expression objects by arrays or other higher level container types. In Cython, the expression objects can be processed with a pointer that has been given a dynamically divided memory.

Cython can handle pointer object like C / C++ does and also can indexing them, including multiple level pointer. The difference between an instance and a pointer is that the former is on the stack and the latter is on the heap. Be aware that the objects that on the stack will be deleted when leaving the scope, the objects that in the heap need to be deleted manually, otherwise they will stay in memory until end of program.

Rely on convenient grammar and built-in functions of Python language, commonly used methods in C / C++ can be supported. So, Sketch Solve solver can be use as a normal C++ program. The solving process in a Cython script is shown in {@fig:sketch-solve-rules}.

![The process of Sketch Solve solver](images/sketch-solve-rules.png){#fig:sketch-solve-rules height=50%}

BFGS algorithm will reflect the result by priority or the highest weight of constraint. The result of "redundant constraint" will not be warning since this algorithm is a type of approximation method, unless the result is not convergent.

## Binding Method of Python Programming Language

Python is an efficient development language that provides more clearly syntax and supports more abstract programming concept.

### Simplified Wrapper and Interface Generator

Simplified Wrapper and Interface Generator (SWIG) [@swig-site] is a cross-platform development tool that can connect libraries written in C or C++ programming language with variety of high-level programming languages. The main process of SWIG is to write an "interface file" then compile it into a wrapper of definition file of specified language. The "interface file" is similar with header file of C language, the functions that developer selected can be exposed to high-level languages, or they can only used for C libraries.

In 2013, a developer "BBBSnowball" make a Python 2 wrapper for Solvespace using SWIG [@python-solvespace-origin], called it "python-solvespace". Its interface file is objectization the solver functions into Python style classes. Unfortunately, the source code are not maintained after several corrections, so it is not compatible with Python 3 and even new version of Solvespace.

In this research, "python-solvespace" has been improved and upgrade with new version dependences, and changed its name into "Python-Solvespace". Some of missing functions are been properly connected, compilation process has improved to more efficiently way, and some grammatical errors in the Python paradigm have also been fixed. Now the Python version is support to 3 and Solvespace solver kernel version is upgraded into 2.3. The compilation process is shown in {@fig:swig}.

![The compilation process of SWIG](images/swig.png){#fig:swig width=40%}

The original libraries can be worked as normal C libraries, but the wrapper library should be placed with original libraries. In case of Python language, there also has a extend script that is generated by SWIG to guide the wrapper library.

Because of garbage collection mechanism in Python, some of the pointer objects in C or C++ language will be deleted when in outer scope, so the creation of C objects will be done by function method or fake class in Python, to avoid triggering de-constructor in a wrong time.

This compilation process is built with GNU develop tool kits, and it is cross platform in Windows and Linux. The process script is a "Makefile" with two operating systems detection, and there has a good handle with binary files, source code and temporary storage.

### Cython

Cython [@cython-site] is a superset of the Python programming language based on Pyrex [@pyrex], it is as a module provided in Python. Cython has a very well support between Python, C and C++, even can be scripting them in the same scope. Cython is better than SWIG if developer just want scripting in Python language.

Known that Python has its own memory system, so the objects between shared libraries and Python will base on two environments. The shared libraries is deploy by C / C++ itself; Python scripts are be depend on dynamic planning of interpreter. The values between them are just copied, and only basic number type and a wrapped pointer can be passed.

The scripts in Cython are same as wrapper definitions in SWIG. And more than that, developer can also just write Cython script only --- it is a C / C++ code with complex checking mechanism that Python does.

In Cython scripts, developer can get object names from C and C++ header files, without doing any declaration in header files. But the C / C++ objects can only used in Cython scripts. If callback to Python environment is needed, we have to provide a wrapper like fake class or Python style function declaration. With same concepts, Python object should copied before take to C / C++ types, it can be simply done by assigning to C / C++ variables.

Also, Cython scripts can share declarations for each others, which is similar with header files in C or C++, but need to follow some rules. Ordinary Cython definition files is using "pyx" suffix, declaration files is using "pxd" suffix, but the "pxd" files can not write any definition, because Cython confirm that the "pxd" files will be included multiple times, which will cause C / C++ compiler error. Furthermore, the "pyx" file should be same file name with its "pxd" file, and no need to explicit include its "pxd" file in the script. The compilation process is shown in {@fig:cython}.

![The compilation process of Cython](images/cython.png){#fig:cython height=40%}

Cython uses default compiler that Python does. Before compiling, "pyx" files need to "cythonize" --- a process to translate Cython scripts into C or C++ code, which will truly implement functions without Python interpreter. In this step, developer can manually enable some optimization options --- it means some complex checking mechanism can be turn off if the Cython scripts is very rigorous as C / C++ code, like boundary checking of arrays.

At last, the original libraries will be contained into wrapper libraries and they can be used directly with Python script.

## Summary

This section shows the open source libraries can also be used as a third-party verification tools in the computer-aided design flow.

Python-Solvespace is the initial project of extending CAD library in this research. Although Python-Solvespace has been re-written in Cython for the better environment of maintenance, the previous project of "python-solvespace" enabled us the possibility of customization with third-party verification tools, and also can complement the deficiency of each tool that we have. Sketch Solve as an ancestor of CAD libraries, it has a fairly straightforward architecture. It was introduced to verify the error between the different methods.

CAD libraries must be scriptable that can be customize by our demand. For example, auto-assemble and auto-sketch for specific product is very efficient than manually executing. For higher-level software, program can help designer to verify the feasibility and consistency when go through each step.

On the other hand, open source libraries can provide more flexiblility to developers, and it is also easier to compare between two libraries. Since the ways of C or C++ binding can be fulfilled, the implementation of sketching and geometric solving now can be convert from expression and solve them.

Cython is a fast and efficient way to binding a C / C++ library to Python scripting, because the wrapper structure has been well-configured and the scripts are more error-proof during executing and testing functions. By comparison, SWIG must consider the versatility of different programming languages in configuration and interface writing, which is more cumbersome and unnecessary.
