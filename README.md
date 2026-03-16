# IntegrEA - Genetic Algorithm Library Manual

IntegrEA is a flexible, highly modular Genetic Algorithm (GA) framework written in Java 21. It abstracts the standard evolutionary operations (Selection, Crossover, Mutation) away from the specific "Problem" implementations, allowing the core GA engine to seamlessly solve radically different types of computational problems without altering the underlying architecture.

## Quick Start

You can run the Genetic Algorithm engine out of the box using the centralized TOML configuration.

1. **Configure:** Edit `src/main/resources/config.toml` to select your problem and parameters:
   ```toml
   [general]
   problem_type = "tsp"

   [data]
   file_path = "src/main/resources/data/tsp_10_cities.csv"
   ```
2. **Build and Run:** Use Maven to compile and run the application:
   ```bash
   mvn clean compile exec:java -Dexec.mainClass="de.fhdwhannover.integrea.Main"
   ```

## Supported Problem Domains
The architecture implements a universal `GAProblem<T>` interface. Built-in modules include:
- **Sudoku:** Uses `Integer` genes to evolve constraint-satisfying grids based on pre-populated cell locks.
- **Cryptanalysis:** Uses `Character` genes to evolve decryption keys against an enciphered string, scoring fitness via n-gram frequency analysis.
- **Symbolic Regression:** Uses `SymRegNode` trees to dynamically evolve mathematical formulas (with protected division) to minimize RMSE.
- **Traveling Salesperson Problem (TSP):** Finds the optimal shortest path between fully connected cities.
- **Knapsack Problem:** Optimizes item selection based on weight constraints and value maximization using heuristic repair strategies.

## Saving and Resuming Progress

IntegrEA allows you to save the exact state of an evolutionary run (including the full population and generation counter) and resume it later. This is particularly useful for complex problems like Symbolic Regression that may take a long time to converge.

To use this feature, configure the `[persistence]` section in your `config.toml`:

```toml
[persistence]
# Set this path to save the state automatically when the application finishes or is interrupted
export_file_path = "state.json"

# Set this path to resume a run from a previously saved state file
# import_file_path = "state.json"
```

## Implementing Your Own Problem

IntegrEA is designed to be extensible. To integrate a custom problem domain, follow this 5-step checklist:

1. **Define the Gene Type:** Determine what type of data your chromosome represents (e.g., `Integer`, `Double`, `CustomObject`).
2. **Implement `FitnessFunction<T>`:** Create a class that calculates and returns an unscaled integer representing the fitness of a single `Individual<T>`.
3. **Implement `GAProblem<T>`:** Create your main problem class to handle data parsing (`loadData`), define constraints, and initialize the starting `Population<T>`.
4. **Create a `GAConfig` mapping:** Map any problem-specific attributes from `config.toml` into a specialized config class (e.g., `MyProblemGAConfig`).
5. **Register in `ControllerFactory`:** Add a new `case` in `ControllerFactory.java` to route your specific `problem_type` string to your initialization logic.

## Documentation

Explore the following resources for detailed configuration, testing, and API references:

| Resource | Description | Link                                  |
| :--- | :--- |:--------------------------------------|
| **Configuration Guide** | A complete mapping of `config.toml` variables. | [CONFIGURATION.md](CONFIGURATION.md)  |
| **Testing Guide** | How to run smoke tests and coverage reports. | [TESTING.md](TESTING.md)              |
| **JavaDoc API** | The full, generated HTML documentation of the library. | [JavaDoc Index](./apidocs/index.html) |

**Generating Docs:** If the `target` folder is missing, run `mvn javadoc:javadoc` to build the API HTML files.

*Note: If viewing in an IDE like IntelliJ, right-click the file at `./target/site/apidocs/index.html` and select 'Open in Browser' to view the full API reference.*
