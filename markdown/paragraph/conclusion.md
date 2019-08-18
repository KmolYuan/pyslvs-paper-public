# Summary and Future Works

## Contributions and Conclusion

The Creative Design flow of planar linkage mechanism is presented in this thesis. The following are the major contributions:

1. Implemented the wrapper that can verify the kinematic analysis with two types of CAD kernel, the Newton-Raphson method and BFGS algorithm.

1. Developed an Automatic Configuration Algorithm to configure the position analysis method for dimensional synthesis.

1. Developed a generic dimensional synthesis tool based on PMKS expression.

1. Developed a generalization algorithm used to transform the mechanism expression into a generalized chain.

1. Developed a graph enumeration algorithm based on number, type and structural synthesis theory.

1. Developed an analysis method to get link assortment from generalized chain.

1. Developed an analysis method to get contracted link assortment from generalized chain.

The purpose of this research is to combine the design flow of structural synthesis and dimensional synthesis with a type of mechanism expression. Which includes a generic dimensional synthesis design flow that suitable for the planar linkage mechanism input of the rotate joints.

Mechanism expression in this research is based on the grammar of PMKS expression. Through the generalization algorithm and information picking in the configuration of dimensional synthesis, existing mechanism can be redesigned the partial features instead of the whole operations of the Creative Design flow.

In the dimensional synthesis process, Automatic Configuration Algorithm is an opportunity to bring a new type of generic path solving mechanism instead of specific position analysis method. The auto parsing mechanism can find the solution step by building an expression stack, which is more general than manually redesigning analytical method. The design flow achievement of this research is shown in {@fig:design-flow}.

![The design flow achievement of this research](images/design-flow.png){#fig:design-flow}

## Future Works

The implementation of the design flow in this research is an open source program. This makes the contributions of the algorithms and data format is more easier. The improvement or extension work in the future about the design flow are listed in this section.

1. Web platform: The design flow needs to share the information in the web framework to support collaboration.

1. Support more types of joints in mechanism expression: The mechanism expression has no gear joints from the future work of PMKS. Other types of joints can support more complex path matching.

1. Merge and simplify process of generalized chain: After actual testing, the larger graph can be easily enumerated by merge multiple subgraphs. This process can be added into the graph enumeration algorithm of Creative Design Method.

1. Atlas database: A labeled generalized chain database can store and provide the usage experience of different graphs. These big data can use on artificial intelligence to improve the selection of generalized chains.

1. Topological description: The description of mechanical structure were able applied to the explanatory text of product documentation or patent description, which can be generated automatically.

1. Dependency dimensional synthesis: Each length of links are independent variables in the synthesis algorithms. Dependent variable option will be able to generate specific appearance for the target mechanism.

1. Aided assembly of planar linkage mechanism: In spatial applications, the aided layers configuration can help for the obstacle avoidance of other link parts.
