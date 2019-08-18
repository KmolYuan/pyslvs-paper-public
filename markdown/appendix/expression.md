# Grammar of Mechanism Expression

The grammar is defined with lark expression, which is based on Extended Backus-Naur Form and supported with Regular Expression.

```javascript
// Number
DIGIT: "0".."9"
INT: DIGIT+
SIGNED_INT: ["+" | "-"] INT
DECIMAL: INT "." INT? | "." INT
_EXP: ("e" | "E") SIGNED_INT
FLOAT: INT _EXP | DECIMAL _EXP?
NUMBER: FLOAT | INT
SIGNED_NUMBER: ["+" | "-"] NUMBER

// Letters
LCASE_LETTER: "a".."z"
UCASE_LETTER: "A".."Z"
LETTER: UCASE_LETTER | LCASE_LETTER | "_"
CNAME: LETTER (LETTER | DIGIT)*

// White space and new line
WS: /\s+/
CR: /\r/
LF: /\n/
NEWLINE: (CR? LF)+
%ignore WS
%ignore NEWLINE

// Comment
LINE_COMMENT: /#[^\n]*/
MULTILINE_COMMENT: /#\[[\s\S]*#\][^\n]*/
%ignore LINE_COMMENT
%ignore MULTILINE_COMMENT

// Custom data type
JOINT_TYPE: "RP" | "R" | "P"
COLOR: "Red" | ...  // All supported color names
type: JOINT_TYPE
name: CNAME
number: SIGNED_NUMBER
color_value: INT

// Main grammar
joint: "J[" type ["," angle] ["," color] "," point "," link "]"
link: "L[" [name ("," name)* ","?] "]"
point: "P[" number "," number "]"
angle: "A[" number "]"
color: "color[" (("(" color_value ("," color_value) ~ 2 ")") | COLOR) "]"
mechanism: "M[" [joint ("," joint)* ","?] "]"
?start: mechanism
```
