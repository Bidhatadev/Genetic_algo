# Genetic_algo
# Genetic Algorithm for Solving Quadratic Equations

This project implements a genetic algorithm to find the solution for a quadratic equation of the form `3x^2 - 11x + 4` using binary encoding, uniform crossover, and mutation.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Algorithm Details](#algorithm-details)
  - [Fitness Function](#fitness-function)
  - [Binary Encoding](#binary-encoding)
  - [Uniform Crossover](#uniform-crossover)
  - [Mutation](#mutation)
- [Results](#results)

## Introduction

This project demonstrates how to use genetic algorithms to solve a quadratic equation. The genetic algorithm evolves a population of binary-encoded individuals over several generations to find the best solution.

## Features

- Binary encoding of individuals
- Uniform crossover to produce offspring
- Mutation to introduce variability
- Roulette wheel selection for choosing parents
- Fitness function based on the quadratic equation `3x^2 - 11x + 4`

## Installation (Jupyter)

To run this project locally, follow these steps:

### 1. Clone the repository:

   ```bash
   git clone https://github.com/PrijeshBikramShahi/Genetic_Algorithm.git
   cd Genetic_Algorithm
   ```

### 2. Create a virtual environment (optional but recommended):

   It's good practice to use virtual environments to isolate project dependencies.

   ```bash
   python3 -m venv venv
   source venv/bin/activate   # On Windows use `venv\Scripts\activate`
   ```

### 3. Install Jupyter Notebook and dependencies:

   Install Jupyter Notebook and other dependencies using `pip`.

   ```bash
   pip install jupyter
   pip install -r requirements.txt
   ```

   Replace `requirements.txt` with the actual file containing your project dependencies, if applicable.

### 4. Start Jupyter Notebook:

   Launch Jupyter Notebook to run the genetic algorithm notebook.

   ```bash
   jupyter notebook
   ```

### 5. Open the notebook:

   Navigate to the `genetic_algorithm.ipynb` file in your Jupyter Notebook interface and open it to execute the genetic algorithm code.

### 6. Run the genetic algorithm:

   Follow the instructions within the notebook to run the genetic algorithm and view results.

### 7. Shutdown Jupyter Notebook:

   Once finished, you can shutdown Jupyter Notebook by pressing `Ctrl + C` in the terminal where it's running and confirming the shutdown.

### 8. Deactivate virtual environment (if used):

   ```bash
   deactivate   # Only if you used a virtual environment
   ```
## Algorithm Details
import random
import numpy as np

# Parameters
POP_SIZE = 20
GENS = 100
MUT_RATE = 0.01
CROSS_RATE = 0.7

# Binary encoding length
SIGN_BIT = 1
INT_BITS = 6
FRAC_BITS = 6
TOTAL_BITS = SIGN_BIT + INT_BITS + FRAC_BITS

def binary_to_float(binary):
    sign = -1 if binary[0] == '1' else 1
    integer = int(binary[1:1+INT_BITS], 2)
    fraction = int(binary[1+INT_BITS:], 2) / (2**FRAC_BITS)
    return sign * (integer + fraction)

def float_to_binary(value):
    sign = '1' if value < 0 else '0'
    integer = bin(int(abs(value)))[2:].zfill(INT_BITS)
    fraction = bin(int((abs(value) - int(abs(value))) * (2**FRAC_BITS)))[2:].zfill(FRAC_BITS)
    return sign + integer + fraction

def fitness(value):
    return abs(2 * value**2 + 5 * value - 3)

def initial_population(size):
    return [''.join(random.choice('01') for _ in range(TOTAL_BITS)) for _ in range(size)]

def select(population):
    selected = sorted(population, key=lambda x: fitness(binary_to_float(x)))
    return selected[:POP_SIZE//2]

def crossover(parent1, parent2):
    if random.random() < CROSS_RATE:
        point1 = random.randint(1, TOTAL_BITS - 1)
        point2 = random.randint(1, TOTAL_BITS - 1)
        if point1 > point2:
            point1, point2 = point2, point1
        child1 = parent1[:point1] + parent2[point1:point2] + parent1[point2:]
        child2 = parent2[:point1] + parent1[point1:point2] + parent2[point2:]
        return child1, child2
    else:
        return parent1, parent2

def mutate(individual):
    individual = list(individual)
    for i in range(TOTAL_BITS):
        if random.random() < MUT_RATE:
            individual[i] = '1' if individual[i] == '0' else '0'
    return ''.join(individual)

# Genetic Algorithm
population = initial_population(POP_SIZE)
for generation in range(GENS):
    selected = select(population)
    next_population = []
    while len(next_population) < POP_SIZE:
        parent1, parent2 = random.choice(selected), random.choice(selected)
        child1, child2 = crossover(parent1, parent2)
        next_population.append(mutate(child1))
        next_population.append(mutate(child2))
    population = next_population

# Find the best solution
best_individual = min(population, key=lambda x: fitness(binary_to_float(x)))
best_value = binary_to_float(best_individual)
best_fitness = fitness(best_value)

print(f'Best value: {best_value}, Fitness: {best_fitness}')

```
### Results
  After running the genetic algorithm, the best solution found will be printed along with its fitness value:
  
```text
Best individual found: 0010011101
Best value: -2.984375, Fitness: 0.10888671875
```


