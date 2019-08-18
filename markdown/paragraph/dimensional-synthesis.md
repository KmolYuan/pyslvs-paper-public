# Dimensional Synthesis

## Position Analysis Methodology

Since all the objects of simulation are planar mechanisms, the position analysis methodology of this research applies scalar geometric method to solving three types of joint. The formulas will be scriptable and can solve each joint step by step.

This methodology will be faster than approximation method in dimensional synthesis, because this is a directly solution through numberical calculation.

### PLAP Function

As {@fig:plap-preview} shows, point $A$, length $L_0$ and angle $\beta$ and point $B$ are known, attempt to find out the coordinate of point $C$.

![Geometric graph of PLAP function](images/PLAP.png){#fig:plap-preview width=60%}

The angle $\alpha$ can be determined by inverse tangent function like {@eq:plap-alpha}.

$$\alpha=\tan^{-1}(\frac{B_y-A_y}{B_x-A_x})$${#eq:plap-alpha}

To apply circular formula {@eq:plap-c1}: 

$$C_1(C_x, C_y) = (A_x + L_0 \times \cos(\alpha + \beta), A_y + L_0 \times \sin(\alpha + \beta))$${#eq:plap-c1}

By the clockwise case (in order as point A, $L_{0}$, $\beta$, point B), the answer is $C_1$. In the program function, a parameter can switch the result as trun the angle $\beta$ into negative.

$$C_2(C_x, C_y) = (A_x + L_0 \times \cos(\alpha - \beta), A_y + L_0 \times \sin(\alpha - \beta))$${#eq:plap-c1}

Angle $\alpha$ will be zero if the point $B$ is not given, the baseline can be considered as parallel with the horizontal axis.

### PLLP Function

As {@fig:pllp-preview} shows, point $A$, length $L_0$, length $L_1$ and point $B$ are known, attempt to find out the coordinate of point $C$.

![Geometric graph of PLLP function](images/PLLP.png){#fig:pllp-preview width=60%}

The equation is same as to calculate the intersections between two circles, with two radius as $L_0$ and $L_1$. Assuming that $d$ is the distance of two points, then the following statuses are not allowed, because there is no any intersection or the circles has been coincide.

$$\begin{cases}d < |L_1 - L_0| \\ d > L_0 + L_1 \\ (d = 0)\ &(L_1 = L_0)\end{cases}$${#eq:pllp-exceptions}

When the case has not been exclude, find out area and height of the triangle surrounded by circle centers and the intersection as $a$ ({@eq:pllp-a}) and $h$ ({@eq:pllp-h}).

$$a=\frac{L_0^2-L_1^2+d^2}{2d}$${#eq:pllp-a}

$$h=\sqrt{L_0^2-a^2}$${#eq:pllp-h}

The differences of x axis and y axis.

$$\begin{cases} \text{d}x = B_x - A_x \\ \text{d}y = B_y - A_y \end{cases}$${#eq:pllp-dx-dy}

Find out the middle point on the line of centers and the line between two intersections.

$$m(m_x, m_y)=(A_x + a \times \frac{\text{d}x}{d}, A_y + a \times \frac{\text{d}y}{d})$${#eq:pllp-xm-ym}

At last, the coordinates of two intersections as shown in {@eq:pllp-c}.

$$\begin{cases}
C_1(C_x, C_y) = (m_x - h \times \frac{\text{d}y}{d}, m_y + h\times\frac{\text{d}x}{d})
\\
C_2(C_x, C_y) = (m_x + h \times \frac{\text{d}y}{d}, m_y - h\times\frac{\text{d}x}{d})
\end{cases}$${#eq:pllp-c}

By the clockwise case (in order as point $A$, $L_0$, $L_1$, point $B$), the answer is $C_1$. In the program function, a parameter can switch the result of two intersections, as same as swap the parameter of point A and point B.

### PLPP Function

As {@fig:plpp-preview} shows, point $A$, length $L_0$, point $B$ and point $C$ are known, attempt to find out the coordinate of point D that on the line between point $B$ and point $C$.

![Geometric graph of PLPP function](images/PLPP.png){#fig:plpp-preview width=60%}

The equation is same as to claculate the intersections between a circle and a line, with radius $L_0$. When the distance of point $A$ and line $\overline{BC}$ is less then $L_0$, because there is no any intersection.

The differences of x axis and y axis as shown in {@eq:plpp-dx-dy}.

$$\begin{cases} \text{d}x = C_x - B_x \\ \text{d}y = C_y - B_y \end{cases}$${#eq:plpp-dx-dy}

The intersection of vertical line between point A and line $\overline{BC}$ can be determine by unit value as shown in {@eq:plpp-u}.

$$u=\frac{\text{d}x (A_x - B_x) + \text{d}y (A_y - B_y)}{d_{BC}^2}$${#eq:plpp-u}

Then determine the coordinate of intersection point.

$$I(I_x,I_y)=(B_x+u\times\text{d}x,B_y+u\times\text{d}y)$${#eq:plpp-i}

If the distance of point $A$ and line $\overline{BC}$ is same as $L_0$, the intersection has only one point, is exactly point $I$. If not, the distance between point $I$ and two intersections as shows as {@eq:plpp-di}.

$$d_i=\frac{\sqrt{L_0^2 - d_{AI}^2}}{d_{BC}}$${#eq:plpp-di}

Two coordinates of intersections $D_1$ and $D_2$ can finally found by {@eq:plpp-d}.

$$\begin{cases}
D_1(D_x, D_y) = (I_x + d_i \times \text{d}_x, I_y + d_i\times\text{d}_y)
\\
D_2(D_x, D_y) = (I_x - d_i \times \text{d}_x, I_y - d_i\times\text{d}_y)
\end{cases}$${#eq:plpp-d}

By the clockwise case (in order as point $A$, $L_0$, point $I$), the answer is $D_1$.  In the program function, a parameter can switch the result of two intersections.

### PXY Function

This simple function can determine a point translate on a planar system referenced from another point by Manhattan distance.

![Geometric graph of PXY function](images/PXY.png){#fig:pxy-preview width=60%}

Which $x$ and $y$ represent the coordinate difference between two points $A$ and $B$. Note that the length of these parameters can be negative.

$$B(B_x, B_y) = (A_x + x, A_y + y)$${#eq:pxy-a}

### Scripting

To be able to solving different mechanisms, all of formulas can be scriptable to determine the final solution. Known data will be packing into a map container at first, and the parser will use the data through with the given scripts.

Grammar of the script shows as below:

```python
Formula[Parameter1, Parameter2, ...](Target); ...
```

The ellipsis means the same type of syntax can be multiple. This expression is programming friendly to make parser as simple as that can be just get the string from square brackets and parentheses, then split with semicolon and comma. Because of that, the expression string will be white space ignored, can not contain new line symbol, and should be case sensitive.

Parameters and targets will be named as specified rules as follows:

1. Start with upper case letter "P": A type of data structure, can be use to contain planar coordinate, and has an operator to calculate the distance between two coordinates.

1. Start with upper case letter "L": A digital data is represented as a length value.

1. Start with lower case letter "a": A digital data is represented as a radian angle value.

1. Start with upper case letter "S": Same as "P" symbol, but the coordinate is a temporary coordinate use to ditermine the second point on the slot (the first is slot origin).

1. After the first letter, a integer will follow behind, on behalf of its order.

An expression example like follows:

```python
PLAP[P0, L0, a0](P1); PLLP[P1, L1, L2, P3](P2)
```

This is an expression of four bar linkage mechanism. The points "P0" and "P3" are grounded, and link lengths "L0", "L1", "L2" are known; there also has a input angle between link "P0-P1" and horizontal axis. It means there are two known points, and another two point coordinates should be find out.

In the first section of expression, using PLAP formula to determine the point "P1" by three parameters "P0", "L0" and "a0". Then, using PLLP formula and point "P1" to determine the coordinate of "P2".

## Examples of Expression Stack

In this section, there will enumerate some examples of planar mechanisms that represent with the mechanism expression. Such as three different types of joints, multiple joints, and the mechanism with two DOF. The solution scripts of examples are also generated by automatic configuration algorithm. All of example data will be stored in the GUI program as default.

### Crank Rocker

Crank Rocker example ({@fig:example-crank-rocker}, {@tbl:example-crank-rocker}) is a simple four-bar linkage mechanism. Where P3 is a extension point of the functional uses.

!["Crank rocker" example](images/crank-rocker.png){#fig:example-crank-rocker}

| Point | Type | Angle | Coordinate | Links |
|:-----:|:----:|:------|:-----------|:------|
| P0 | R |   | (0.0, 0.0) | ground, L1 |
| P1 | R |   | (12.92, 32.53) | L1, L2 |
| P2 | R |   | (73.28, 67.97) | L2, L3 |
| P3 | R |   | (33.3, 66.95) | L2 |
| P4 | R |   | (90.0, 0.0) | ground, L3 |

Table: Expression data sheet of "Crank Rocker" example {#tbl:example-crank-rocker}

Where the known position of grounded joints are P0 and P4; the known link length are L0 to L4; input angle is a0. Shown in {@tbl:conditions-crank-rocker} and {@tbl:script-crank-rocker}.

| Parameter | Value | Parameter | Value |
|:---------:|:------|:---------:|:------|
| P0 | (0.0, 0.0) | L2 | 70.0 |
| P4 | (90.0, 0.0) | L3 | 40.0 |
| L0 | 35.0 | L4 | 39.99 |
| L1 | 70.0 | a0 | Input angle |

Table: Known conditions of "Crank Rocker" example {#tbl:conditions-crank-rocker}

| Formula | Parameter | Target |
|:-------:|:----------|:------:|
| PLAP | P0, L0, a0 | P1 |
|PLLP | P1, L1, L2, P4 | P2 |
| PLLP | P1, L3, L4, P2 | P3 |

Table: Solution script of "Crank Rocker" example {#tbl:script-crank-rocker}

### Jansen's Linkage

Jansen's Linkage example ({@fig:example-jansen-linkage}, {@tbl:example-jansen-linkage}) is a type of walking mechanism shows the attributes of two multiple revolution joints, where point P7 is the terminal point of ankle joint. Usually, this type of walking mechanism is in pairs.

!["Jansen's linkage" example](images/jansen-linkage.png){#fig:example-jansen-linkage}

| Point | Type | Angle | Coordinate | Links |
|:-----:|:----:|:------|:-----------|:------|
| P0 | R |   | (0.0, 0.0) | ground, L1 |
| P1 | R |   | (9.61, 11.52) | L1, L2, L4 |
| P2 | R |   | (-38.0, -7.8) | ground, L3, L5 |
| P3 | R |   | (-35.24, 33.61) | L2, L3 |
| P4 | R |   | (-77.75, -2.54) | L3, L6 |
| P5 | R |   | (-20.1, -42.79) | L4, L5, L7 |
| P6 | R |   | (-56.05, -35.42) | L6, L7 |
| P7 | R |   | (-22.22, -91.74) | L7 |

Table: Expression data sheet of "Jansen's Linkage" example {#tbl:example-jansen-linkage}

Where the known position of grounded joints are P0 and P2; the known link length are L0 to L4; input angle is a0. Shown in {@tbl:conditions-jansen-linkage} and {@tbl:script-jansen-linkage}.

| Parameter | Value | Parameter | Value |
|:---------:|:------|:---------:|:------|
| P0 | (0.0, 0.0) | L5 | 61.91 |
| P2 | (-38.0, -7.8) | L6 | 39.3 |
| L0 | 15.0 | L7 | 36.7 |
| L1 | 41.5 | L8 | 39.4 |
| L2 | 49.99 | L9 | 49.0 |
| L3 | 40.1 | L10 | 65.7 |
| L4 | 55.8 | a0 | Input angle |

Table: Known conditions of "Jansen's Linkage" example {#tbl:conditions-jansen-linkage}

| Formula | Parameter | Target |
|:-------:|:----------|:------:|
| PLAP | P0, L0, a0 | P1 |
| PLLP | P2, L1, L2, P1 | P3 |
| PLLP | P2, L3, L4, P3 | P4 |
| PLLP | P1, L5, L6, P2 | P5 |
| PLLP | P5, L7, L8, P4 | P6 |
| PLLP | P5, L9, L10, P6 | P7 |

Table: Solution script of "Jansen's Linkage" example {#tbl:script-jansen-linkage}

### Triple Ball Lifter

Triple Ball Lifter example ({@fig:example-triple-ball-lifter}, {@tbl:example-triple-ball-lifter}) is about three Stephenson III (six-bar linkage) combined to a one DOF mechanism. Where points P7, P13 and P19 are the holding point of carrier.

!["Triple ball lifter" example](images/triple-ball-lifter.png){#fig:example-triple-ball-lifter}

| Point | Type | Angle | Coordinate | Links |
|:-----:|:----:|:------|:-----------|:------|
| P0 | R |   | (10.2, 10.4) | ground, L1 |
| P1 | R |   | (7.44, 20.01) | L1, L2, L6, L10 |
| P2 | R |   | (-10.52, 11.21) | L2, L3 |
| P3 | R |   | (-28.48, 2.42) | L2, L4 |
| P4 | R |   | (-6.6, 0.0) | ground, L3 |
| P5 | R |   | (-12.8, 0.0) | ground, L5 |
| P6 | R |   | (-19.11, 32.24) | L4, L5 |
| P7 | R |   | (-64.12, 4.61) | L4 |
| P8 | R |   | (43.78, 20.17) | ground, L7 |
| P9 | R |   | (13.9, 38.46) | L6, L7 |
| P10 | R |   | (21.05, 62.93) | L6, L8 |
| P11 | R |   | (23.8, 0.0) | ground, L9 |
| P12 | R |   | (39.15, 41.95) | L8, L9 |
| P13 | R |   | (10.29, 92.17) | L8 |
| P14 | R |   | (73.44, 61.74) | L10, L11 |
| P15 | R |   | (63.67, 0.0) | ground, L11 |
| P16 | R |   | (101.58, 72.34) | L10, L12 |
| P17 | R |   | (92.3, -1.63) | L13, ground |
| P18 | R |   | (111.74, 70.43) | L12, L13 |
| P19 | R |   | (7.5, 93.44) | L12 |

Table: Expression data sheet of "Triple Ball Lifter" example {#tbl:example-triple-ball-lifter}

Where the known position of grounded joints are P0, P4, P5, P8, P11, P15 and P17; the known link length are L0 to L24; input angle is a0. Shown in {@tbl:conditions-triple-ball-lifter} and {@tbl:script-triple-ball-lifter}.

| Parameter | Value | Parameter | Value | Parameter | Value | Parameter | Value |
|:---------:|:------|:---------:|:------|:---------:|:------|:---------:|:------|
| P0 | (10.2, 10.4) | L1 | 11.8 | L9 | 19.55 | L17 | 78.09 |
| P4 | (-6.6, 0.0) | L2 | 20.0 | L10 | 35.03 | L18 | 62.51 |
| P5 | (-12.8, 0.0) | L3 | 20.0 | L11 | 45.03 | L19 | 30.07 |
| P8 | (43.78, 20.17) | L4 | 40.0 | L12 | 25.49 | L20 | 107.71 |
| P11 | (23.8, 0.0) | L5 | 31.26 | L13 | 27.71 | L21 | 10.34 |
| P15 | (63.67, 0.0) | L6 | 32.85 | L14 | 44.67 | L22 | 74.64 |
| P17 | (92.3, -1.63) | L7 | 35.71 | L15 | 31.16 | L23 | 96.42 |
| L0 | 10.0 | L8 | 52.81 | L16 | 57.92 | L24 | 106.75 |
| a0 | Input angle |   |   |   |   |

Table: Known conditions of "Triple Ball Lifter" example {#tbl:conditions-triple-ball-lifter}

| Formula | Parameter | Target |
|:-------:|:----------|:------:|
| PLAP | P0, L0, a0 | P1 |
| PLLP | P4, L1, L2, P1 | P2 |
| PLLP | P2, L3, L4, P1 | P3 |
| PLLP | P3, L5, L6, P5 | P6 |
| PLLP | P3, L7, L8, P6 | P7 |
| PLLP | P1, L9, L10, P8 | P9 |
| PLLP | P1, L11, L12, P9 | P10 |
| PLLP | P10, L13, L14, P11 | P12 |
| PLLP | P10, L15, L16, P12 | P13 |
| PLLP | P1, L17, L18, P15 | P14 |
| PLLP | P14, L19, L20, P1 | P16 |
| PLLP | P16, L21, L22, P17 | P18 |
| PLLP | P16, L23, L24, P18 | P19 |

Table: Solution script of "Triple Ball Lifter" example {#tbl:script-triple-ball-lifter}

### Crank Slider (RP Joint)

Crank Slider (RP Joint) example ({@fig:example-crank-slider-rp}, {@tbl:example-crank-slider-rp}) is about a mechanism compose with a multiple revolute-prismatic joint. Where point P5 is a extension point of the functional uses.

!["Crank slider (RP joint)" example](images/crank-slider-rp.png){#fig:example-crank-slider-rp width=60%}

| Point | Type | Angle | Coordinate | Links |
|:-----:|:----:|:------|:-----------|:------|
| P0 | R |   | (-67.38, 36.13) | ground, L1 |
| P1 | R |   | (-68.925, 55.925) | L1, L2 |
| P2 | RP | 0.0 | (11.88, 0.0) | ground, L2, L3 |
| P3 | R |   | (50.775, 24.791) | L3, L4 |
| P4 | R |   | (74.375, 7.625) | ground, L4 |
| P5 | R |   | (95.197, 52.881) | L3 |

Table: Expression data sheet of "Crank Slider (RP Joint)" example {#tbl:example-crank-slider-rp}

Where the known position of grounded joints are P0, P2 and P4; the known link length are L0 to L7; input angle is a0. Shown in {@tbl:conditions-crank-slider-rp} and {@tbl:script-crank-slider-rp}.

| Parameter | Value | Parameter | Value |
|:---------:|:------|:---------:|:------|
| P0 | (-67.38, 36.13) | L3 | 98.27 |
| P2 | (11.88, 0.0) | L4 | 46.12 |
| P4 | (74.375, 7.625) | L5 | 29.18 |
| L0 | 19.86 | L6 | 52.56 |
| L1 | 88.02 | L7 | 98.68 |
| L2 | 1.0 | a0 | Input angle |

Table: Known conditions of "Crank Slider (RP Joint)" example {#tbl:conditions-crank-slider-rp}

| Formula | Parameter | Target |
|:-------:|:----------|:------:|
| PLAP | P0, L0, a0 | P1 |
| PLLP | P0, L1, L2, P2 | S2 |
| PLPP | P1, L3, P2, S2, F | P2 |
| PLLP | P2, L4, L5, P4 | P3 |
| PLLP | P3, L6, L7, P2 | P5 |

Table: Solution script of "Crank Slider (RP Joint)" example {#tbl:script-crank-slider-rp}

### Crank Slider (P Joint)

Crank Slider (P Joint) example ({@fig:example-crank-slider-p}, {@tbl:example-crank-slider-p}) is about a mechanism compose with a multiple prismatic joint. Where points P2, P3 and P4 are on a slider block, which has parallel movement.

!["Crank slider (P joint)" example](images/crank-slider-p.png){#fig:example-crank-slider-p width=60%}

| Point | Type | Angle | Coordinate | Links |
|:-----:|:----:|:------|:-----------|:------|
| P0 | R |   | (-33.625, -19.625) | ground, L1 |
| P1 | R |   | (-48.375, 12.125) | L1, L3 |
| P2 | R |   | (17.125, 33.875) | L2, L3 |
| P3 | P | 30.0 | (51.38, -12.63) | ground, L2 |
| P4 | R |   | (50.35, 53.117) | L2, L5 |
| P5 | R |   | (143.455, 65.967) | L4, L5 |
| P6 | R |   | (99.244, 20.447) | ground, L4 |

Table: Expression data sheet of "Crank Slider (RP Joint)" example {#tbl:example-crank-slider-p}

Where the known position of grounded joints are P0, P2 and P6; the known link length are L0 to L9; input angle is a0. Shown in {@tbl:conditions-crank-slider-p} and {@tbl:script-crank-slider-p}.

| Parameter | Value | Parameter | Value |
|:---------:|:------|:---------:|:------|
| P0 | (-33.625, -19.625) | L4 | 34.26 |
| P2 | (17.125, 33.875) | L5 | -46.51 |
| P6 | (99.244, 20.447) | L6 | -1.03 |
| L0 | 35.01 | L7 | 65.75 |
| L1 | 1.0 | L8 | 93.99 |
| L2 | 74.7 | L9 | 63.46 |
| L3 | 69.02 | a0 | Input angle |

Table: Known conditions of "Crank Slider (P Joint)" example {#tbl:conditions-crank-slider-p}

| Formula | Parameter | Target |
|:-------:|:----------|:------:|
| PLAP | P0, L0, a0 | P1 |
| PLLP | P1, L3, L2, P0 | S2 |
| PLPP | P1, L3, P2, S2, F | P2 |
| PXY | P2, L4, L5 | P3 |
| PXY | P3, L6, L7 | P4 |
| PLLP | P4, L8, L9, P6 | P5 |

Table: Solution script of "Crank Slider (P Joint)" example {#tbl:script-crank-slider-p}

### Two DOF Arm

Two DOF Arm example ({@fig:example-two-dof-arm}, {@tbl:example-two-dof-arm}) is a multiple linkage system, which has two angular inputs. Where point P9 is the terminal point of jaws. Usually, the frame is rotatable in spatial coordinate as three DOF robot arm.

!["Two DOF Arm" example](images/two-dof-arm.png){#fig:example-two-dof-arm width=50%}

| Point | Type | Angle | Coordinate | Links |
|:-----:|:----:|:------|:-----------|:------|
| P0 | R |   | (-34.25, -20.625) | ground, L1, L2 |
| P1 | R |   | (29.75, 77.375) | L2, L5, L6 |
| P2 | R |   | (-54.0, 10.875) | L1, L3 |
| P3 | R |   | (-86.25, -3.125) | ground, L4 |
| P4 | R |   | (-7.25, 94.625) | L4, L5 |
| P5 | R |   | (57.0, 110.875) | L5, link_8 |
| P6 | R |   | (126.5, 56.125) | L7, L8 |
| P7 | R |   | (114.5, 35.625) | L6, L7 |
| P8 | R |   | (7.0, 131.125) | L3, L6 |
| P9 | R |   | (163.5, 47.875) | L7 |

Table: Expression data sheet of "Two DOF Arm" example {#tbl:example-two-dof-arm}

Where the known position of grounded joints are P0 and P3; the known link length are L0 to L13; input angles are a0 and a1. Shown in {@tbl:conditions-two-dof-arm} and {@tbl:script-two-dof-arm}.

| Parameter | Value | Parameter | Value |
|:---------:|:------|:---------:|:------|
| P0 | (-34.25, -20.625) | L7 | 58.37 |
| P3 | (-86.25, -3.125) | L8 | 143.79 |
| L0 | 37.18 | L9 | 94.48 |
| L1 | 117.05 | L10 | 88.47 |
| L2 | 125.68 | L11 | 23.75 |
| L3 | 40.82 | L12 | 37.91 |
| L4 | 66.27 | L13 | 50.51 |
| L5 | 43.18 | a0 | Input angle |
| L6 | 134.84 | a1 | Input angle |

Table: Known conditions of "Two DOF Arm" example {#tbl:conditions-two-dof-arm}

| Formula | Parameter | Target |
|:-------:|:----------|:------:|
| PLAP | P0, L0, a0 | P2 |
| PLAP | P0, L1, a1 | P1 |
| PLLP | P3, L2, L3, P1 | P4 |
| PLLP | P4, L4, L5, P1 | P5 |
| PLLP | P2, L6, L7, P1 | P8 |
| PLLP | P8, L8, L9, P1 | P7 |
| PLLP | P5, L10, L11, P7 | P6 |
| PLLP | P6, L12, L13, P7 | P9 |

Table: Solution script of "Two DOF Arm" example {#tbl:script-two-dof-arm}

## Automatic Configuration Algorithm

All those four formulas mentioned in previous section can use to determine the position of mechanism joints, correspond to three types of joint from mechanism expression. An greedy method algorithm ({@fig:auto-configuration}) can be use as automatic generate functional script for topology of the expression.

![Flow chart of Automatic Configuration Algorithm](images/auto-configuration.png){#fig:auto-configuration}

Note that the algorithm can only be used in moveable mechanisms, where DOF greater than zero. In the special cases, when the input joint(s) is on a five bar loop or above, the patch process will transfer to geometric constraint solver.

### Main Flow

First, create a list of "vlink" that can help us to find the relationship just like Adjacency matrix. "VLink" is a class of map data that can be "stand in shoes of links" to find the other points which is on the same link. The names of links will be the key of map data, a set of joint numbers will be the value. A list of "vlinks" are also not sequential just same as "vpoint".

There is only translational movement in a link if it contains a P joint or above. So all the R joints of the link should be change into RP joints, exclude P joint.

Then pick the position data of all joints. A "vpoint" has two coordinate if it is a P joint or RP joint, they are represent the coordinate of slot origin and pin. The positions data are use to detect the solution case is clockwise or not.

The input joints will has a angle data, use the "PLAP" solution, they are a pair of points, one of they is "base point", another is "driver point". But the joint must be grounded because it is no movement, and others will be configured in algorithm.

The algorithm has some of parameters for the program loop:

1. Parameter "status" will record each joint whether is configured or not, all grounded R joints will be "true" at begin.

1. Parameter "skip times" counts the times when current joint has been skipped.

1. Parameter "around" is the amount of all joint, use to interrupt if "skip times" is greater than joints amount.

While searching for all the points until reaching the expected solutions, each joint will be classified by types:

1. For the case R joint: The role of input joint will using PLAP function to find the coordinate of "driver point" since its "base point" is known. A normal joint can be configure with PLLP function since two neighbor points were also be known.

1. For the case P joint: Each of link will be only contains maximum of a P joint, so the P joint position can be determined from its neighbor with PXY function. After get the solution for this joint, all of joints that connect with it and can also be configured with PXY function.

1. For the case RP joint: If a slot of RP joint is not on the ground, current angle of the slot should calculate again, because the "angle" attribute is using absolute angle of main coordinate system. Then create a virtual point coordinate named with upper case letter "S", it means the second point of slot line.

1. If the current point can't find the solution yet, in other words, there is no any other point has solution siding with current point, the configuration will be skip at this time.

When the number of input angle is equal as degrees of freedom, all of coordinates can be determine by the scripts that has been generated from this algorithm.

### Special Cases

Some special cases can not using automatic configuration algorithm. Although the algorithm will more faster than other geometric solution, it still can not support reverse solving. An normal case with a P joint is shown in {@fig:algorithm-normal-case}, and its solution script generated with the algorithm is shown in {@tbl:algorithm-normal-case}.

| Formula | Parameter | Target |
|:-------:|:----------|:------:|
| PLAP | P0, L0, a0 | P1 |
| PLLP | P2, L1, L2, P1 | P3 |
| PLLP | P2, L3, L4, P3 | P4 |
| PLLP | P1, L5, L6, P2 | P5 |
| PLLP | P5, L7, L8, P4 | P6 |
| PLLP | P5, L9, L10, P6 | P7 |

Table: Solution script of "Normal case" example {#tbl:algorithm-normal-case}

![Normal case of the algorithm](images/algorithm-normal-case.png){#fig:algorithm-normal-case width=40%}

Where all of lengths and coordinates of "P0" and "P6" are known. Both the joints "P2" and "P4" will be transform into grounded RP joints, which is caused by "P3" is a grounded P joint. The greedy method can not be use to solving the problem that DOF is zero. In other words, the positions of locked chains (some examples are shown in {@fig:locked-chain}) is not a general solution. Additionally, if the mechanism left a multi-link locked chain subgraph when using algorithm, the subgraph will be ignored.

![Multi-link locked chain examples](images/locked-chain.png){#fig:locked-chain width=80%}

A special case belong with Stephenson III is shown in {@fig:algorithm-special-case}. If the input joint is "P6", the rules of the algorithm will ignore all points except "P5" and "P6". Because the conditions can't be done with "P1", "P2", "P3" and "P4", which is need to use other method than scalar geometric method.

![Special case of the algorithm](images/algorithm-special-case.png){#fig:algorithm-special-case width=40%}

For special cases, the patch process is using Sketch Solve kernel to handle the remaining simultaneous equations. But the numerical methods will much slower than the solution of formulas, the time costs comparison of examples "Normal case" and "Special case" shown in {@tbl:time-costs-comparison}. In the table, the example is executed on same environment and used the same settings of Differential Evolution with 8 target points, and their time costs have great differences.

| Result | **Fully configured** 8 bar linkage | **Partial configured** 6 bar linkage |
|:------:|:-----------------:|:------------------:|
| Number of nodes | 6 | 1 + 3 (Numerical Method) |
| Time costs | 22.11s | 110.62s |

Table: Time costs comparison of configured cases {#tbl:time-costs-comparison}

## Metaheuristic Random Algorithms

Dimensional synthesis is the process after structural synthesis. This is a opposite process of graph generalization. The synthesis algorithms is referenced from Real-coded Genetic Algorithm, Firefly Algorithm and Differential Evolution [@django]. In Lee's thesis, a type of verification mechanism can be substitute as fitness function, the target status will cause fitness value as zero, and the result will evolve through lowest fitness value.

The verification function actually is a non-linear problem and usually solved by random algorithms instead of numerical methods. These searching method can be stopped by following limitation to avoid suffering from stagnation: Generation limitation, time limitation and threshold of fitness value.

In another part, the initial populations option will affect the searching range of the algorithm. Increasing populations may increase the chances of searching, but it will also decrease the performance of each generation.

The generic options of three algorithms shown in {@tbl:algorithm-generic-option}. After the synthesis, a valid position of the latest result will be generated as a mechanism expression string mentioned in previous chapter.

| Option | Value Type |
|:-------|:-----------|
| Initial populations | Integer |
| Generation limitation (Optional) | Positive integer |
| Time limitation (Optional) | Positive real value (second) |
| Threshold of fitness value (Optional) | Positive real value |

Table: Generic options of algorithms {#tbl:algorithm-generic-option}

### Configuration of Generalized Chain

The graph generalization will lost several information, such as multiple joints, grounded joints, the dimension of links and the joint positions. In the part of data pretreatment, the above information must be added by designing constraints and requirements.

The synthesis algorithm can decide the position and dimension with random values, and the values will input to verification function, which is the expression stack created with Automatic Configuration Algorithm. The random values constitute the "gene" components of an individual called "chromosome", the value type can be boolean (binary value), real value (floating point value) or integer, and the value of natural number should be generated in a specific range.

When a generalized chain input, the following informations will be specified by designer:

1. Grounded link: The coordinate system will be placed on this link.

1. Input list: The variables of the mechanism. An angle value and a pair of points on the same link will using for rotate joints.

1. Custom points: A list of points that represent the particle on specified link. Usually, the custom points will only be used as kinematic analysis, because they actually are not the joints that affect with adjacency of the generalized chain.

1. Multiple joints: A list of multiple joints, each joint will map to a non-multiple joint. The joint that in this list will have the same position with its partner.

1. Target points: The points used as kinematic analysis. The grounded joints can't be used as target points.

1. Initial positions: The initial positions of all points. This information is used at geometric constraints solver.

1. Upper and lower bounds: The limitation used to generate the random values. The bounds should be at the same length as the length of chromosome.

The data format of the chromosome is using real value with three types of algorithm. The data has be divided into three parts:

1. Grounded joints: The position of grounded joints.

1. Length of links: The relative position of remaining points.

1. Input list: The list of variable values.

The length of input list "$\text{len}(I)$" will decided by the length of target paths as shown in {@eq:input-list-length}. Where "$\text{len}(I_q)$" is the number of input variables, and "$\text{len}(T_i)$" is the length of $i$-th target path relative to target paths $T$. Since the length of each target path must be equal to each other, the parameter length of each variable can directly decided by multiply the number of target paths "$\text{len}(T)$".

$$
\text{len}(I) = \text{len}(I_q) \times \text{len}(T_i) = \text{len}(I_q) \times \text{len}(T) \times \text{len}(T_0)
$${#eq:input-list-length}

Though the position analysis method, each individual will obtain the fitness calculated by verification function, which is the absolute error between target paths and actual paths as shown in {@eq:fitness-error}. Where fitness $f$ equals the sum of all the errors of each target path, and the errors are obtained by distance formula between $j$-th target point $T_{ij}$ and actual point $P_{ij}$. Reorganization and filter condition mechanism are varies in different algorithm.

$$
f =
\sum_{i=0}^{\text{len}(T)}
\sum_{j=0}^{\text{len}(T_0)}
\text{hypot}((T_{ij})_x - (P_{ij})_x, (T_{ij})_y - (P_{ij})_y)
$${#eq:fitness-error}

### Real-coded Genetic Algorithm

Genetic Algorithm is proposed by John Holland [@Genetic-Algorithm] based on Darwin's theory of evolution. This random Global Search Method can obtain the better results in many unknown solutions. But it may have poor performance compared with using numerical methods when doing convergence searching. If the value type of chromosome is real value (floating point value), the algorithm is called "real-coded".

The process of Genetic Algorithm has following steps:

1. Reproduction: Select two individual to combine their chromosome until meet the populations of the next generation. In this research, the select method is using Roulette Wheel Selection, which is selecting the couples randomly. Another way is Tournament Selection, it will filter with each random group by fitness value.

1. Crossover: Each pair will generate two children with a symmetrical combination. The proportion of allocation is decided by crossover rate.

1. Mutation: Randomly select a chromosome and reset it to another values. This execution is decided by mutation rate and mutation gain value.

1. Replacement: New populations will replace the old populations with better individual, they will also preserve the previous generation of elites. Each comparison still has the probability of failure, decided by win rate.

The mutation gain value will change the chromosome value by {@eq:mutation}, decided randomly. Where $V_g$ is the chromosome value of the individual; $s$ value is a random indicator of chromosome; $V_U$ and $V_L$ is the upper bound and lower bound of chromosome; $\delta$ is the mutation gain value; $g$ and $g_{\max}$ is the generation of current individual and the target generation. If the $g_{\max}$ is not exist, it will became $g$.

$$
(V_g)_s \coloneqq \begin{cases}
(V_g)_s + \text{random}\{0, 1\}[(V_U)_s - (V_g)_s](1 - \frac{g}{g_{\max}})^\delta
\\
(V_g)_s - \text{random}\{0, 1\}[(V_g)_s - (V_L)_s](1 - \frac{g}{g_{\max}})^\delta
\end{cases}
$${#eq:mutation}

The flow chart of Real-coded Genetic Algorithm implemented in this research is shown in {@fig:rga}, and the options shown in {@tbl:rga-option}.

![Flow chart of Real-coded Genetic Algorithm](images/rga.png){#fig:rga height=60%}

| Option | Value Type |
|:-------|:-----------|
| Crossover rate | Positive real value (0 ~ 1) |
| Mutation rate | Positive real value (0 ~ 1) |
| Mutation gain value | Positive real value |
| Win rate | Positive real value (0 ~ 1) |

Table: The options of Real-coded Genetic Algorithm {#tbl:rga-option}

### Differential Evolution

Differential Evolution is a type of algorithm based on Direct Search Method with parameter vectors. Although the overall architecture is quite similar, the main difference between Genetic Algorithm and Differential Evolution is that former relies on crossover and the latter relies on mutation operation [@Differential-Evolution].

First step is to generate random "vectors", which are the indicators in the recombination step. There are 10 different strategies in Differential Evolution, each of these has different mutation and recombination step. The number of random vectors will decided by strategies.

All strategies are labeled with a different number 0 to 9. And there have different mutation formulas with random vectors. Where $V_g$ is an copy of current chromosome; $n$ is an indicator of chromosome starts randomly, $V_{\text{best}}$ is the best chromosome in history; $F$ is weight factor; $V_k$ is $k$-th random vector generate before, which is point to a random chromosome excluding current one.

The strategies are combination with two types of loop flow and five assignment equations ({@eq:strategy-1} ~ {@eq:strategy-5}) shown in {@tbl:de-strategies}.

| Type | {@eq:strategy-1} | {@eq:strategy-2} | {@eq:strategy-3} | {@eq:strategy-4} | {@eq:strategy-5} |
|:----:|:----:|:----:|:----:|:----:|:----:|
| Type 1 | 1 | 2 | 3 | 4 | 5 |
| Type 2 | 6 | 7 | 8 | 9 | 0 |

Table: The labeled strategies of Differential Evolution {#tbl:de-strategies}

1. Type 1: Repeat assignment until all chromosome values has been replaced and extra mutation decided by recombination rate.

1. Type 2: Repeat assignment until all chromosome values has been through a round. All indexes excluding last one have once mutation decided by recombination rate, no extra chance. The last one index will be triggered for sure.

$$
(V_g)_n = (V_{\text{best}})_n + F[(V_1)_n - (V_2)_n]
$${#eq:strategy-1}

$$
(V_g)_n = (V_1)_n + F[(V_2)_n - (V_3)_n]
$${#eq:strategy-2}

$$
(V_g)_n \coloneqq (V_g)_n + F[(V_{\text{best}})_n - (V_g)_n] + F[(V_1)_n - (V_2)_n]
$${#eq:strategy-3}

$$
(V_g)_n = (V_{\text{best}})_n + F[(V_1)_n + (V_2)_n - (V_3)_n - (V_4)_n]
$${#eq:strategy-4}

$$
(V_g)_n = (V_5)_n + F[(V_1)_n + (V_2)_n - (V_3)_n - (V_4)_n]
$${#eq:strategy-5}

The flow chart of Differential Evolution implemented in this research is shown in {@fig:de}, and the options shown in {@tbl:de-option}.

![Flow chart of Differential Evolution](images/de.png){#fig:de height=50%}

| Option | Value Type |
|:-------|:-----------|
| Strategy | Positive integer (0 ~ 9) |
| Weight factor | Positive real value (0.5 ~ 1) |
| Recombination rate | Positive real value (0 ~ 1) |

Table: The options of Differential Evolution {#tbl:de-option}

### Firefly Algorithm

Firefly Algorithm is a Metaheuristic Random Algorithm proposed by Xin-She Yang[@Firefly-Algorithm-1][@Firefly-Algorithm-2][@Firefly-Algorithm-3]. The algorithm implemented in this research is formulated from Yang's metaphor: all fireflies are unisexual; attractiveness is proportional to their brightness, but intensity decrease as mutual distance increases; a given firefly will move randomly if there are no fireflies brighter than it.

Attractiveness is inversely proportional to distance, which is calculated by the Euclidean distance formula, as shown in {@eq:firefly-distance}. Where $r_{i, j}$ is the distance between the fireflies pointed by $i$ and $j$ indicator; $N$ is the dimension of the chromosome; $(x_k)_i$ and $(x_k)_j$ is actually the $k$-th chromosome value of $i$ and $j$ firefly.

$$
r_{i, j} = \sqrt{\sum_{p=0}^N((x_k)_i - (x_k)_j)^2}
$${#eq:firefly-distance}

Brightness is a composite value of fitness value and distance in Firefly Algorithm, as shown in {@eq:firefly-brightness}. Where $I$ is brightness and $r$ is the distance; $I_0$ is the original brightness equals fitness value. Updating the brightness is to re-calculate the fitness values of all fireflies.

$$
I = \frac{I_0}{r^2}
$${#eq:firefly-brightness}

Beta value $\beta$ is the attractiveness between two fireflies as shown in {@eq:firefly-beta}. Where $\beta_0$, $\beta_{\min}$ and $\gamma$ is the constant values from algorithm options.

$$
\beta_{i, j} = (\beta_0 - \beta_{\min})e^{-\gamma r_{i, j}^2}
$${#eq:firefly-beta}

The moving function is work as {@eq:firefly-move}, but it will triggered only when the fitness of $i$ firefly is lower than $j$ firefly. Where $V_i$ and $V_j$ is the chromosome value of $i$ and $j$ firefly; $V_U$ and $V_L$ is the upper and lower bounds of the chromosome value; $n$ is an indicator of chromosome starts from zero; $\alpha$ is the step value given by options.

$$
(V_i)_n \coloneqq (V_i)_n + \beta_{i, j}[(V_j)_n - (V_i)_n] + \text{random}\{-0.5, 0.5\}[(V_U)_n - (V_L)_n]\alpha
$${#eq:firefly-move}

The flow chart of Firefly Algorithm implemented in this research is shown in {@fig:firefly}, and the options shown in {@tbl:firefly-option}.

![Flow chart of Firefly Algorithm](images/firefly.png){#fig:firefly height=50%}

| Option | Value Type |
|:-------|:-----------|
| Gamma value ($\gamma$) | Positive real value |
| Alpha value ($\alpha$) | Positive real value |
| Initial beta value ($\beta_0$) | Positive real value |
| Minimum beta value ($\beta_{\min}$) | Positive real value |

Table: The options of Firefly Algorithm {#tbl:firefly-option}

## Example of Dimensional Synthesis

In "Six bar synthesis" example, the mechanism expression is as {@fig:dimensional-synthesis-example} and {@tbl:example-dimensional-synthesis}.

!["Six bar synthesis" example](images/dimensional-synthesis-example.png){#fig:dimensional-synthesis-example width=60%}

| Point | Type | Angle | Coordinate | Links |
|:-----:|:----:|:------|:-----------|:------|
| P0 | R |   | (47.5648, -97.5182) | L0, ground |
| P1 | R |   | (21.7793, -76.6328) | L0, L3 |
| P2 | R |   | (-7.34294, -52.8188) | L0, L4 |
| P3 | R |   | (135.4498, 4.6885) | L1, ground |
| P4 | R |   | (21.9578, 8.0311) | L1, L3 |
| P5 | R |   | (-3.4841, 42.9895) | L1, L5 |
| P6 | R |   | (-68.9723, -17.6489) | L4, L5 |

Table: Expression data sheet of "Six bar synthesis" example {#tbl:example-dimensional-synthesis}

The Metaheuristic Random Algorithm is using Differential Evolution, the algorithm option shown in {@tbl:example-dimensional-synthesis-option}. The range of grounded joints position and link length are $\pm 25$ and $\pm 50$ respectively. But the position of P1 is changed to $(135.45, -25.31)$.

| Option | Value |
|:-------|:------|
| Initial population | 400 |
| Max generation | 1000 |
| Strategy | 1 |
| Weight factor | 0.6 |
| Recombination rate | 0.9 |

Table: Algorithm option of "Six bar synthesis" example {#tbl:example-dimensional-synthesis-option}

After 51.42 seconds on Windows 10 64-bit with 16 GB memory and Intel Core i7-6700 CPU 3.4 GHz, the result is generated as {@fig:dimensional-synthesis-example-after} and {@tbl:example-dimensional-synthesis-after}.

![Synthesis result of "Six bar synthesis" example](images/dimensional-synthesis-example-after.png){#fig:dimensional-synthesis-example-after width=60%}

| Point | Type | Angle | Coordinate | Links |
|:-----:|:----:|:------|:-----------|:------|
| P0 | R |   | (38.5244, -114.1034) | ground, L1 |
| P1 | R |   | (111.7201, -2.3228) | ground, L4 |
| P2 | R |   | (-1.6693, -114.7059) | L1, L2 |
| P3 | R |   | (-29.8988, -81.9561) | L1, L3 |
| P4 | R |   | (21.7447, -18.3762) | L2, L4 |
| P5 | R |   | (-74.7529, -14.0358) | L3, L5 |
| P6 | R |   | (14.4965, 12.956) | L4, L5 |

Table: Expression data sheet of "Six bar synthesis" example after synthesis {#tbl:example-dimensional-synthesis-after}

## Summary

The geometric solutions of Automatic Configuration Algorithm is extended from Lee's triangle formulas configuration. Through the stack of configured module, the verification function can obtain more performance during dimensional synthesis. The algorithm is also an application of topological analysis for mechanism expression in this research. All of the design flow steps can be generalized back to previous step or go to the next step, benefit from the expression of mechanism in the thesis.

The position analysis method is also generated as one of the kinematics simulation kernels. In the normal simulation state such as constant velocity motion, this method does not gain much performance than others, but takes great differences in the process of dimensional synthesis flow.

In this research, the unified configuration and verification function of Metaheuristic Random Algorithms are planned to do path generation. Unlike numerical methods, the algorithms are generated chromosome from random numbers by specific ranges without using initial parameters. Because of this, the results are always different unless the random seeds are same.
