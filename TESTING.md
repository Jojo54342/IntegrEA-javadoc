# IntegrEA Testing Guide

This project relies on the Maven Surefire plugin for unit testing and JaCoCo for code coverage reporting. The current test suite extensively covers the "Darwinian Engine" (selection, crossover, mutation models) and problem-specific fitness verifications.

**Current Test Coverage (Core & Problems):** `64%`

## Running Tests
To compile the codebase and run all unit tests, execute the following command from the project root:
```bash
mvn clean test
```

## Generating Coverage Reports
To generate a comprehensive JaCoCo code coverage report in HTML format, run:
```bash
mvn clean test jacoco:report
```

Once the build finishes, you can find the HTML report generated in the following directory:
```
target/site/jacoco/index.html
```

You can view this report in any modern web browser to analyze branch, line, and method coverage statistics across the generic core and domain-specific `GAProblem` implementations.

## Verifying Your Own Custom Problem
When implementing a new problem (as outlined in `README.md`), you can hook into our universal smoke testing patterns to verify your implementation safely:

1. **Add to Smoke Test Suite:** Open `src/test/java/de/fhdwhannover/integrea/GAIntegrationTest.java`.
2. **Mock the Configuration:** Create a new test case (e.g., `testMyProblemSmoke()`) that utilizes the `createBaseConfig("my_problem_type", "src/main/resources/data/my_data.csv")` utility to instantiate an `AppConfig`.
3. **Execute:** Pass your config to `ControllerFactory` and invoke the `assertSmokeTest(controller)` verification wrapper.
4. **Validation:** This integration assertion will automatically verify that your custom problem's `evaluate()` loop runs without crashing and that your implemented `getPreview()` formatter generates valid data structure representations without throwing Exceptions.
