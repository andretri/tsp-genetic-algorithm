# Solving the Travelling Salesman Problem (TSP) Using Genetic Algorithms


## Contents
* [An Introduction to the Problem](#an-introduction-to-the-problem)

* [The Genetic Algorithm](#the-genetic-algorithm)
  * [Population](#population)
  * [Selection](#selection)
  * [Crossover](#crossover)
  * [Mutation](#mutation)
  * [Elitism](#elitism)
  * [Algorithm Convergence](#algorithm-convergence)

* [Code Implementation](#code-implementation)
  * [Graph Class](#graph-class)
  * [Genetic Algorithm Class](#genetic-algorithm-class)

*  [Code Execution](#code-execution)

## An Introduction to the Problem 

The Travelling Salesman Problem (TSP) is **finding the minimal path that traverses though all cities** so that _a salesman can travel with the minimal cost._ The solution to this - classic in algorithms - problem can be achieved with many different approaches (_Greedy_ and _Brute Force_ to name a few) but all these have one common drawback: The Search Space of the Problem itself!

For example, given a weighted graph G with |V| vertices and |E| edges, _finding the minimal route that traverses though all vertices of the graph_ is equivalent to _finding_ all _possible permutations_ of the graph's vertices and taking the path with the minimum cost, that is factorial(|V|) = |V|!.

In other words these types of algorithms can't be used to solve the TSP because they find the solution in exponential time, unfeasible in real-life tasks.

That's where the Genetic Algorithms are used, to provide _approximately_ optimal solutions in feasible time.

## The Genetic Algorithm 

The **Genetic Algorithm** is a **Possibilistic Algorithm** inspired by the _Darwinean Theory of Evolution_. It searches through the space of possible solutions so as to find acceptable - according to some criteria - solutions.
 
To use a Genetic Algorithm to a Problem, one must define:

* ## Population
In this problem the population is a list of strings _(genomes)_ that each letter _(allele)_ is a vertex of the given graph.

__Example:__ For a Graph with vertices V = {a,b,c,d,e}, an allele is any of the elements in V, a genome is any permutation of the elements in V and a population is a list of _N_ genomes.

* ## Fitness Function
It's the measure used to compare genomes to each other. For this problem a fitness function (and the one we use in this implementation) is the sum of the edge's weights through the graph for a certain path - encoded in the genome.

* ## Selection
Each generation must have _N_ genomes at a time. To achieve evolution we select some at the genomes to _Crossover_ them and produce _Offsprings_ that will be part of the new population. To select them there are many techniques, two of which are:
  1. Roulette Selection: 
```
> sum_of_fitness = 0

> for each genome in population:
    calculate its fitness with a Fitness Function
    sum_of_fitness += genome_fitness

> for each genome in population:
    probability = genome_fitness/sum_of_fitness

> choose a random genome with the calculated probabilities
```
  2. Tournament Selection:
```
> choose K genomes from the population

> for each genome chosen:
    calculate its fitness with a Fitness function

> choose the fittest genome 
```

* ## Crossover
Now that we have our parent genomes from the Selection procedure, now we crossover them to produce - potentially - fitter offsprings.

There are many options and techniques that the Crossover may be done, but the one that we use in our implementation is the following:

```
> Select a subset from the first parent.

> Add that subset to the offspring.

> Any missing values are then added to the offspring from  the second parent in order that they are found.

Example: 
Parent  1: a,[b, c],d, e
Parent  2: b, e, a, c, d
Offspring: e, b, c, a, d
```

* ## Mutation
After making the offsprings of the next generation, to finalize the creation of the new population we must mutate the genomes.

To mutate a genome we just select two random alleles and swap them in a genome

_Note: the mutation does not occur always. It occurs with a user-defined probability._

* ## Elitism
In Genetic Algorithms sometimes the fittest genomes from the current generation are passed directly to the next, contesting with the offsprings and crossover to - potentially - fitter solutions.

This step occurs during the calculation of the Fitness function value for each genome. When all values are computed the genome with the minimum value is appended directly to the next generation's population.

## Algorithm Convergence

The Algorithm stops if:
  * All genomes in the population are the same __or__
  * Reached the max. Generation limit

These cases indicate that the algorithm has converged to a minima _(global or local)_.

_Note: To make the algorithm more consistent it is recommended to have a sufficient large population and mutation rate._

_**Warning**: Setting the mutation rate too high _increases_ the probability of searching more areas in search space, however, prevents population to converge to any optimum solution. [\[Source\]](https://www.researchgate.net/post/Why_is_the_mutation_rate_in_genetic_algorithms_very_small) Likewise setting the elititsm rate too high decreases the probability of searching more areas in search space, leading the population to converge faster to a solution but - potentially - preventing it from reaching any optimum._


## Code Implementation
The Code makes use of two Classes:

* ## Graph Class
    This Class is responsible for:
    * Creating a graph
    * Setting Vertices and Edges
    * Getting Vertices and Edges
    * Computing the Cost from a Vertex to another given a path

    The Functions that make the above possible are:
    * **\_\_init__**(self, graph={}) 
        > graph: The Graph Dictionary {'vertex':{'adjacent': edge_weight}}
    * **setVertex**(self, vertex)
    * **setAdjacent**(self, vertex, adj, weight=0)
    * **getVertices**(self)
    * **getAdjacent**(self, vertex)
    * **getPathCost**(self, path)

* ## Genetic Algorithm Class
    This Class is responsible for:
    
    * Initiating a Genetic Algorithm with Certain Parameters
    * Running the Evolution Cycle for _self.generations_ Generations and returning the best path along with its travel cost.

    The functions that make this possible are:
     * **\_\_init__**(self, generations=100, population_size=10, tournamentSize=4, mutationRate=0.1, elitismRate=0.1)
    * **optimize**(self, graph)
      > graph: The Graph Dictionary {'vertex':{'adjacent': edge_weight}}

## Code Execution

>Note: the referenced scripts were tested with Python 3.5.4 _via_ Anaconda3.

To execute the genetic algorithm for TSP write in Terminal:
```python
python GeneticAlgorithmTSP.py
```

>Note: to initiate the TSP for another graph, please change the code @\_\_main__ function inside the GeneticAlgorithmTSP.py script.