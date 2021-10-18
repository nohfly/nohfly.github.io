---
title: "1. Basic implementation of Genetic Algorithm"
categories:
 - project1
tags:
 - genetic algorithm
published: true
---

Genetic algorithm is one of search algorithms derived from Charles Darwin's theory of natural evolution. To survive in specific environments, individuals go through process of natural selection where the most fittest ones survive and produce offspring.

During the reproduction, the genetic transcript of the parent is copied to the children but not the exact copy is produced. The transcript of the parents gets mixed with each other producing alternations in children(***crossover***). In the transcript of children, genetic mutation occurs and alters random code of the transcript(***mutation***). Such alternations do not necessarily result in better fitness to the environment. Many offsprings may fall behind to their parents, or the alternation may not result in difference in phenotypes. But some few will show better fitness to the environment, surviving better than their parents and produce offsprings. As generations continue, such random alternations will produce most fitting individuals, hence the natural selection.

This random alteration through generations comes in handy when solving the optimization problem. As individuals approach local minimum, randomness introduced by crossover and mutation enables new individuals outside the boundary of local minimum, thereby enabling the algorithm to find global minimum.

The implementation of genetic algorithm in python will be comprised of five modules: **geneticAlgorithm**, **initialPopulation**, **crossover**, **mutation** and **fitnessFunction**.

## Sample problem
Let's say we generate length 10 string which is composed of four 'A's, three 'B's, two 'C's and one 'D'.
Such as:
```
"ADCBCAABAC"
```
What we want to do is to find a string where most of the characters are bagged together:
```
"AAAABBBCCD", "BBBDAAAACC", "DCCAAAABBB" or etc.
```
Of course we can just use combinations of "AAAA", "BBB", "CCC" and "D" to generate all correct stings. But let's assume that we cannot, which is often true in real situations where the rules are complex.

We can use genetic algorithm to solve this problem starting with setting up the initial population.

### 1) set initial population
Initial population is the mix of first random individuals where we can start apply genetic algorithm. Given the character sets and their occurrences, *initial_population()* generates 
```python
def initial_population(query, popsize):  
   population = []  
  
   charlist = list(query.keys())  
   indiv_len = sum(query.values())  
  
   for i in range(popsize):  
      sampled_char = random.choices(charlist, k=indiv_len)  
      individual = "".join(sampled_char)  
      population.append(individual)  
  
   return population
```
Given the list of characters and size of the population, we can now get initial population composed of random individual strings.
```python
import initialPopulation

query = {'A': 4, 'B': 3, 'C': 2, 'D': 1}
popsize = 50  
  
initialpop = initial_population(query, popsize)
>>> ["ABCDCABABA","AABCBCBADA", ...]
```
Next, we apply fitness function to calculate fitness score of each individuals to our environment, characters should be bagged, and rank them to select parents for the next generation.

### 2) calculate fitness score
xxx
```python
def rankPopulation(charlist, popsize):
	population = []
	for i in range(popsize):
		asdasdasd
		population.append(individual)
	return population
```
xxx

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzQwNjIxMDY0LDU4MDQyMDAxMCw3MTIxNj
Y4MzksLTIxMTc4MjkyMCwtOTE0MjQ0OTg4LC0xODI0ODc4Mzcz
LDM4ODU1NDA5NiwxMjIxMTk0OTE3LDE1MTQzNjcwMiwtNzI0Mj
Y3MDcsMTQ0MzQ1OTg4NV19
-->