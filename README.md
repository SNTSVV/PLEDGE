# PLEDGE (Version 1.2)
PLEDGE: PracticaL and Efficient Data GEnerator for UML. 

# Story behind it: 

The ability to generate test data is often a necessary prerequisite for automated software testing. For the generated data to be fit for its intended purpose, the data usually has to satisfy various logical constraints derived from specifications. When testing is performed at a system level, these constraints tend to be complex and are typically captured in expressive formalisms based on first-order logic. Using UML and its constraint language, OCL, is a standard way for capturing such constraints at a system level. However, common solvers, such as SAT or SMT, are not sufficient to address such complex constraints.

Motivated by improving the feasibility and scalability of data generation for system testing, we present a novel tool named PLEDGE (PracticaL and Efficient  Data GEnerator). PLEDGE hybridizes metaheuristic search and Satisfiability Modulo Theories (SMT) for generating UML instance models subject to complex OCL constraints.  This hybridization is done in such a way as to take advantage of the complementary strengths of metaheuristic search and SMT.

In a nutshell, PLEDGE works by first transforming all the constraints ascribed to a UML data model into a single constraint. This single constraint is represented in the Negation Normal Form (NNF). The tool then distributes the responsibility of solving the different subformulas in this normal form over (metaheuristic) search and SMT. Intuitively, search handles subformulas whose satisfaction involves structural tweaks to the instance model, i.e., additions and deletions of objects and links. SMT handles subformulas involving only primitive attributes, i.e., attributes with primitive types. These subformulas, if satisfiable, can be solved without structural tweaks. Many such subformulas, e.g., those whose symbols are within linear arithmetic, can be efficiently handled by background SMT theories. Finally, search and SMT are both tasked with handling subformulas whose satisfaction may require a combination of structural tweaks and value assignments to primitive attributes.

# System requirements:
- [Java Development Kit 1.8.0](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) (or higher)
- [Z3](https://drive.google.com/file/d/1igvUmfkWWQ811Fs_RInuyz34SJj2ELQ0/view)  (version 4.5.1); Installation instructions for Z3 can be found at [link](https://github.com/Z3Prover/z3).

# Usage instructions:

1.	Download and Unzip PLEDGE into your system. In the remainder, we will refer to the folder containing PLEDGE as **main_directory**. 

2.	The inputs for PLEDGE can be set via the solver.properties file. The file is under **main_directory/config/**. The solver file includes the following properties: 
    
- **modelFolder**: specifies the location of the UML class diagram (.uml file)
- **OCLconstraintsPath**: points to the file containing the OCL constraints to solve (.ocl file)
- **searchMaxIteration**: defines the maximum number of iterations that search can execute 
- **nonEmptinessConstraint**: contains the non-emptiness constraint for excluding vacious solutions


We provide default configuration files under **main_directory/config/**: configForCaseA.properties, configForCaseB.properties, and configForCaseC.properties. For example, copy and paste the content of configForCaseB.properties into solver.properties file to ask the tool to generate test data for the case staudy denoted Case B.



3.	From your terminal, go to the **main_directory**. For example, in macOS write the following in your terminal: `cd {path to main_directory}`

4.	Then, write the following command in your terminal to run the tool:  `java -jar PLEDGE.jar`

5.	If a solution is found, then the generated instance model can be found under **main_directory/generatedData/**

# Changelog

Version | Date  | Description  
--- | ---| ---------------------------------------
**1.0** | 31/01/2019 | First prototype of the tool (tested over the three case studies of the paper "Practical Model-driven Data Generation for System Testing" available at [link](https://arxiv.org/abs/1902.00397))
**1.1** | 13/03/2019 |  Improving the satisfaction of constraints including long navigations. Now, the search component of PLEDGE includes all navigable elements within a given constraint in the set of objects/links that can be dynamically tweaked (added or deleted) at run time.
**1.2** | 20/03/2019 |   Extending the search component of PLEDGE  to support  **union** and  **intersection** operations. 
