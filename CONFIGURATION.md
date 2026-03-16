# IntegrEA Configuration Mapping Guide

This document maps every `config.toml` key to its corresponding internal Java property within the `AppConfig.java` class, alongside a detailed description of its effect on the Genetic Algorithm framework.

## Parameter Mapping Table

| TOML Section | TOML Key | AppConfig Class Field | Data Type | Effect on GA |
| :--- | :--- | :--- | :--- | :--- |
| `[general]` | `problem_type` | `general.problemType` | `String` | Determines which module `ControllerFactory` builds (`sudoku`, `tsp`, `knapsack`, etc.). |
| `[genetic_algorithm]`| `population_size` | `geneticAlgorithm.populationSize` | `int` | Dictates the size of candidate individuals evolved per generation. |
| `[genetic_algorithm]`| `max_generations` | `geneticAlgorithm.maxGenerations` | `int` | Sets the hard termination limit for the evolutionary loop. |
| `[genetic_algorithm]`| `crossover_rate` | `geneticAlgorithm.crossoverRate` | `double` | Probability factor determining if parents share genetic traits to produce offspring. |
| `[genetic_algorithm]`| `mutation_rate` | `geneticAlgorithm.mutationRate` | `double` | Probability factor controlling random gene perturbations to maintain population diversity. |
| `[genetic_algorithm]`| `elitism_count` | `geneticAlgorithm.elitismCount` | `int` | Defines how many top-performing individuals natively survive the generation culling. |
| `[genetic_algorithm]`| `reset_threshold` | `geneticAlgorithm.resetThreshold` | `int` | Number of stagnation generations before convergence-wall logic randomly resets the population. |
| `[genetic_algorithm]`| `reset_percentage` | `geneticAlgorithm.resetPercentage` | `double` | The fraction of the population to be repopulated randomly upon hitting the reset threshold. |
| `[strategy]` | `selection` | `strategy.selection` | `String` | Selects parent evaluation operator (e.g., [`tournament`](target/site/apidocs/de/fhdwhannover/integrea/ga/core/selection/TournamentSelection.html), `roulette`). |
| `[strategy]` | `tournament_size` | `strategy.tournamentSize` | `int` | Sample size subset evaluated when `selection = "tournament"` is active. |
| `[strategy]` | `crossover` | `strategy.crossover` | `String` | Defines the problem-specific operator mixing genetic structures (`sudoku_block`, `ordered`, `subtree`). |
| `[strategy]` | `mutation` | `strategy.mutation` | `String` | Defines the problem-specific operator altering genetic structures (`swap`, `bit_flip`, `point_and_subtree`). |
| `[data]` | `file_path` | `data.filePath` | `String` | Path to dataset required by the problem's loader (e.g., `src/main/resources/data/tsp.csv`). |
| `[sudoku_config]` | `difficulty` | `sudokuConfig.difficulty` | `String` | Maps to specific grids within the TOML library. |
| `[cryptanalysis]` | `language` | `cryptanalysisConfig.language` | `String` | Decides the target unigram statistical baselines (e.g., `german`, `english`). |
| `[knapsack_config]` | `capacity_limit` | `knapsackConfig.capacityLimit` | `double` | Hard constraint limit determining penalty/repair triggers. |
| `[knapsack_config]` | `constraint_strategy`| `knapsackConfig.constraintStrategy` | `String` | Sets fallback mechanism if overweight (`repair` via heuristical removal or `penalty`). |
| `[symbolic_regression]`| `parsimony_coefficient`| `symregConfig.parsimonyCoefficient`| `double` | Evaluator coefficient penalizing bloat to prevent infinitely expanding ASTs (see [`SymRegFitnessFunction`](target/site/apidocs/de/fhdwhannover/integrea/ga/problems/symreg/SymRegFitnessFunction.html)). |
| `[symbolic_regression]`| `max_depth` | `symregConfig.maxDepth` | `int` | Caps initial randomly-generated node depth. |

## Input Validation
As of the latest stable build, `AppConfig.java` enforces validation logic mathematically restricting configurations. Entering logically impossible parameters (such as `population_size = -10` or `mutation_rate = 3.5`) will preemptively throw a `ConfigurationException` before the engine boots.
