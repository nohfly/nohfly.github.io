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
Initial population is the mix of first random individuals where we can start apply genetic algorithm. Given the character sets and their occurrences(*query*), *initial_population()* generates a *popsize* population with randomly sequenced individuals.
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
Given the dictionary of character keys and their occurrences, we can now get initial population composed of random individual strings.
```python
import initial_population

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
Using the defined *fitness()* function, we can now rank the initial population based on fitness scores of individuals.
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
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAzNzU4NDM4Miw1ODA0MjAwMTAsNzEyMT
Y2ODM5LC0yMTE3ODI5MjAsLTkxNDI0NDk4OCwtMTgyNDg3ODM3
MywzODg1NTQwOTYsMTIyMTE5NDkxNywxNTE0MzY3MDIsLTcyND
I2NzA3LDE0NDM0NTk4ODVdfQ==
-->