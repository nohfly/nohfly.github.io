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

The implementation of genetic algorithm in python will be comprised of seven steps: **1) set initial population**, **2) calculate fitness score**, **3) rank initial population**, **4) select elite population**, **5) crossover**, **6) mutation** and **7) iterate through generations**.

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
Initial population is the mix of first random individuals where we can start apply genetic algorithm. Given the character sets and their occurrences(*query*), *initial_population()* generates a *popsize* population with randomly sequenced individuals.
```python
import random

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
Given the dictionary of character keys and their occurrences, we can now get initial population composed of random individual strings.
```python
query = {'A': 4, 'B': 3, 'C': 2, 'D': 1}
popsize = 50  
  
initialpop = initial_population(query, popsize)
>>> ["ABCDCABABA","AABCBCBADA", ...]
```
### 2) calculate fitness score
Next, we apply fitness function to calculate fitness score of each individuals to our environment: characters should be bagged. To enforce this environment to our individuals, two rules will be applied:
*1) It is not preferable to have different neighboring characters
2) Occurrence of each character should be the same as stated in the query*
```python
def fitness(query, individual):  
   score = 0  
  
  # Deduct score whenever the neighboring char is different  
  for i in range(len(individual)-1):  
      if individual[i] != individual[i+1]:  
         score -= 1  
  
  # Deduct score if occurrence of char is different from the query  
  for c in query.keys():  
      cnt = individual.count(c)  
      score -= abs(query[c] - cnt) * 1000  
  
  return score
```
The score deduction from the second rule, *2) Occurrence of each character should be the same as stated in the query*, has 1,000 times more weight than that of the first one, as it is more important to comply with the given query.
### 3) rank initial population
Using the defined *fitness()* function, we can now rank a population based on fitness scores of individuals.
```python
import fitness

def rank_population(query, population):  
   score_result = []  
  
   for indiv in population:  
      score = fitness(query, indiv)  
      score_result.append((indiv, score))  
  
   score_result.sort(reverse=True, key=lambda x: x[1])  
  
   rankedpop = [pop for pop, score in score_result]  
   popscore = [score for pop, score in score_result]  
  
   return rankedpop, popscore
```
We now apply *rank_population()* to our *initialpop* to get *rankedpop* and *popscore*.
```python
query = {'A': 4, 'B': 3, 'C': 2, 'D': 1}
popsize = 50  
  
initialpop = initial_population(query, popsize)
>>> ["ABCDCABABA","AABCBCBADA", ...] 

rankedpop, popscore = rank_population(query, initialpop)  
>>> ['CCBDAAABBA', 'BACBACAABD', 'ABCAADABBB', ...]
>>> [-5, -8, -2006, ...]
```
### 4) select elite population
From the ranked population, we select parents(*elitepop*) which will reproduce offsprings. Selection of parents can be designed in many ways; select top three individuals and include one random individual, select top two individuals and include parent from previous generation, and etc. Introduction of randomness will vary on how parents are being selected. Here, for the simplicity, we just select top scored individuals as parents.
```python
rankedpop, popscore = rank_population(query, initialpop)  
>>> ['CCBDAAABBA', 'BACBACAABD', 'ABCAADABBB', ...]
>>> [-5, -8, -2006, ...]

elitesize = 3  
  
elitepop = rankedpop[:elitesize]  
elitescore = popscore[:elitesize]
>>> ['CCBDAAABBA', 'BACBACAABD', 'ABCAADABBB']
>>> [-5, -8, -2006]
```
### 5) crossover
For the crossover, we use two-point crossover approach. Given the parent A and B, the char sequence of children will be:
```
child A = [head of parent B] + [middle of parent A] + [tail of parent B]
child B = [head of parent A] + [middle of parent B] + [tail of parent A]
```
This represents the reproduction of child genotypes from the parent. The locations(*copoints*) which define head, middle and tail of the sequence will be determined randomly.
```python
from itertools import combinations
import random

def crossover(parents):  
   children = []  
  
   matecomb = combinations(parents, 2)  
  
   for comb in matecomb:  
      parent1 = list(comb[0])  
      parent2 = list(comb[1])  
  
      copoints = sorted(random.choices(range(len(parent1) + 1), k=2))  
  
      child1 = parent2[:copoints[0]] + parent1[copoints[0]:copoints[1]] + parent2[copoints[1]:]  
      child2 = parent1[:copoints[0]] + parent2[copoints[0]:copoints[1]] + parent1[copoints[1]:]  
  
      children.append("".join(child1))  
      children.append("".join(child2))  
  
   return children
```
### 6) mutation
For the mutation, we go through character sequence of each individual in parent populations, and with the probability of *mutation_rate*, we change the character at certain location into the other character mentioned in *query*.
```python
import random

def mutation(query, parents, mutation_rate):  
   children = []  
  
   for individual in parents:  
      indiv = list(individual)  
  
      for i in range(len(indiv)):  
         if random.random() < mutation_rate:  
            new_char = random.choice(list(query.keys()))  
            indiv[i] = new_char  
  
      children.append("".join(indiv))  
  
   return children
```
### 7) iterate through generations
After the *crossover* and *mutation* step to populate more individuals, the total population goes through *rank_population()*. Then the whole process, except the **1) set initial population**, iterates through generations until the optimal solution is found.
The integrated method of genetic algorithm is as follows:
```python
import initial_population, rank_population, crossover, mutation

def runGA(query, initialpop_size, elitepop_size, generation_cnt, mutation_rate):  
   # Initial population setting  
   initialpop = initial_population(query, initialpop_size)  
  
   # Rank initial population  
   rankedpop, popscore = rank_population(query, initialpop)  
  
   # Select elite population  
   elitepop = rankedpop[:elitepop_size]  
   elitescore = popscore[:elitepop_size]  
  
   print(f"Initial population: {elitepop[0]} - {elitescore[0]}")  
  
   # Iterate for generation cnt  
   for i in range(generation_cnt):  
      totalpop = elitepop  
  
      # Crossover  
      co_children = crossover(totalpop)  
      totalpop += co_children  
  
      # Mutation  
      mt_children = mutation(query, totalpop, mutation_rate)  
      totalpop += mt_children  
  
      # Remove duplicates  
      totalpop = list(set(totalpop))  
  
      # Rank total population  
      rankedpop, popscore = rank_population(query, totalpop)  
  
      # Select elite population  
      elitepop = rankedpop[:elitepop_size]  
      elitescore = popscore[:elitepop_size]  
  
      if (i + 1) % 100 == 0:  
         print(f"{i + 1}th generation: {elitepop[0]} - {elitescore[0]}")  
  
   return elitepop, elitescore
```
Given the query, our *runGA()* will produce following result:
```python
import runGA

def main():  
   # Query statement  
   query = {'A': 4, 'B': 3, 'C': 2, 'D': 1}  
   print(f"Query: {query}\n")  
  
   # Set GA parameters  
   initialpop_size = 100  
   elitepop_size = 5  
   generation_cnt = 1000  
   mutation_rate = 0.1  
  
   # Run genetic algorithm  
   result, result_score = runGA(query, initialpop_size, elitepop_size, generation_cnt, mutation_rate)  
  
   # Print result  
   print(f"\nResult: {result}")  
   print(f"Result score: {result_score}")

>>> Query: {'A': 4, 'B': 3, 'C': 2, 'D': 1}
>>>
>>> Initial population: BBDCAABAAC - -6
>>> 100th generation: BBBDAAAACC - -3
>>> 200th generation: BBBDAAAACC - -3
>>> 300th generation: BBBDAAAACC - -3
>>> 400th generation: DBBBAAAACC - -3
>>> 500th generation: DBBBAAAACC - -3
>>> 600th generation: DBBBAAAACC - -3
>>> 700th generation: DBBBAAAACC - -3
>>> 800th generation: DBBBAAAACC - -3
>>> 900th generation: DBBBAAAACC - -3
>>> 1000th generation: DBBBAAAACC - -3
>>>
>>> Result: ['DBBBAAAACC', 'BBBAAAACCD', 'BBBDAAAACC', 'BBBAAAADCC', 'DBBAAAABCC']
>>> Result score: [-3, -3, -3, -3, -4]
```
The top-scored individual in the initial population was "BBDCAABAAC" with the score of *-6*, and before the iteration reaches 100th generation, the top-scored individual was "BBBDAAAACC" with the score of *-3*.
With simple heuristics, we can come up with the answer of "AAAABBBCCD" and that the best score will always be *-3* for the given query.
Using the genetic algorithm, we do not need to specify how to make answers to the pro
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcxMjYyODgzOCwxNjU2NzI0Njk0LDU4MD
QyMDAxMCw3MTIxNjY4MzksLTIxMTc4MjkyMCwtOTE0MjQ0OTg4
LC0xODI0ODc4MzczLDM4ODU1NDA5NiwxMjIxMTk0OTE3LDE1MT
QzNjcwMiwtNzI0MjY3MDcsMTQ0MzQ1OTg4NV19
-->