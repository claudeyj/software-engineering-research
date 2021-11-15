# ASE'18 GRT: An Automated Test Generator using Orchestrated Program Analysis

### Authors: Lei Ma, Cyrille Artho, Cheng Zhang, Hiroyuki Sato, Johannes Gmeiner and Rudolf Ramler
### Link: https://ieeexplore.ieee.org/document/7372077/

## Abstract
This study utilize both static analysis and runtime information to guide test generation, with the aim on improving code coverage and detecting more defects.

## Background
### Random Test Generators:
Creating input objects (most mainsteam random test generators): use method sequences whose returning objects can be used as inputs for further test generation.

*JCrasher*: use a parameter graph to analyze method dependencies and to create test sequences.

*Eclat* and *Randoop*: perform feedback-directed random test generation to incrementally construct complex objects from primitive types, in a bottom-up style.

*MSeqGen"* mine client code bases to extract frequently used sequence patterns to assist test generation.

*OCAT*: capture input object states by running developer-written test cases, and directly using these objects as input for random test generation.

GRT requires no additional sources of information like existing test cases and code bases. (Which could be useful indeed!)

### Systematic Test Generators
- Symbolic execution: represent input iput as symbolic values and uses abstract semantics for execution to collect the path constraints. Tools: PathFinder, DART, Cute, JCute, Pex, Dsc.
- Bounded exhaustive testing: exhaustively generate method sequence up to a small bound of sequence length. Tools: TestEra, Korat and Symstra.

- Evolutionary Test Generators: random test generation + evolutionary algorithms. Tools: Evosuite.

## Concerns of FRT (feedback-directed random testing)
### Selection of initial values
### Selection of MUT
### Selection of methods to generate input data (especially object input)
- Properties of methods: method signature, including thre reutrn type and parameter type. For exact subtype, dynamic type can be more accurate. 
- Dependencies of methods: considering all applicable methods (MUTs and their dependent methods) is infeasible while considering limited methods is not sufficient to create necessary objects.

### Composition of method sequences
- Several factors related to the cost-effectiveness can be considered: time to execute a sequence and the likelihood to cover more unexecuted code. (I don't think this can make big difference)

### Constant mining
- Use ASM interpreter framework to analyze Java bytecode of CUT and performs constant propagation and folding.
- Mine constants from global level and local level.

### Impurity
- Favoring methods that can change object states.
- Gaussian distribution is adopted for primitive numeric values.
- When a non-primitive object is selected as input and requires fuzzing, we use the corresponding impure methods to mutate the object before passing it to the MUT.

### Elephant Brain
- Use reflection to obtain type information.
- Store a method sequence into the object pool by using the exact type of the returned object as key.

### Detective
- Focus on objects not found in the main object pool.
- First randomly constructs an object with type T or subtypes of T. Then it analyzes all methods of T that can return the corresponding objects, and recursively finds method returning objects that are type compatible.

### Orienteering
- Estimate the execution cost of each method sequence (execution time and length of the sequence)

### Bloodhound
- Adapt modified **JaCoCo** to monitor code coverage.
- Tend to select MUTs that are not well covered.

## Usage Scenarios
- Defect Detection
- Regression Test Generation

## Result
- Detected 23 bugs in 30 open source projects
- Detected 147 (out of 224) real bugs from four subject programs in Defects4j