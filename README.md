# Awesome Property Testing
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated collection of property tests for various data structures, algorithms, and properties.
This repository aims to provide a comprehensive set of property-based tests across different programming languages, frameworks, and domains.
Property testing is a powerful technique to help you ensure your code behaves correctly under a wide range of conditions by automatically generating input values and testing the properties of your code.

## What is Property Testing?

Property testing is a technique where you define general properties (invariants) about your program, and then the testing framework generates random inputs to check whether the properties hold true. 
Unlike traditional unit tests, which test specific scenarios, property tests explore a broader set of possible inputs, including edge cases, and often find bugs that you might not have considered.

For example, when testing a queue, a property might be that the size of the queue never goes negative after enqueue and dequeue operations, or that the first item dequeued is always the first one enqueued (FIFO).

## Table of Contents

- [Why Property Testing?](#why-property-testing)
- [Contributing](#contributing)
- [Property Tests](#property-tests)
  - [Queues](#queues)
  - [Lists](#lists)
  - [Strings](#strings)
  - [Numbers](#numbers)
  - [More Tests](#more-tests)
- [Popular Property Testing Libraries](#popular-property-testing-libraries)
- [License](#license)

## Why Property Testing?

- **Automates Edge Case Discovery**: Property tests generate a wide range of input values, which often uncovers edge cases that you wouldn't think of manually.
- **Helps Prove Correctness**: If a property test passes for all generated inputs, it helps give you confidence that your code behaves as expected across many scenarios.
- **Improves Code Quality**: Property testing encourages writing more general and robust code that handles a variety of situations, rather than specific predefined cases.

## Contributing

We welcome contributions to this repository! If you have a property test to add, follow these steps:

1. **Fork the repository** and clone it to your local machine.
2. **Add a new property test**: Create a new `.md` file under the `property-tests` directory. Add tests relevant to the data structure or algorithm you're targeting.
3. **Follow best practices**: Ensure that your property tests are written in a clear, understandable format. See the existing tests for examples.
4. **Submit a pull request**: Once you've added your test(s), open a pull request with a brief description of your changes.
5. **Test your code**: If applicable, provide links to the relevant property testing framework or language-specific tools used to verify your tests.

### Best Practices

- **Keep tests simple**: Focus on one property per test.
- **Provide clear explanations**: Describe the property you're testing and the reasoning behind it.
- **Document the tool or framework**: If your property test requires a specific framework, be sure to mention it.

### Reporting Issues

If you encounter any broken tests or have suggestions for additional properties to test, feel free to [open an issue](https://github.com/Forward-Lang/awesome-property-testing/issues). We're always looking for ways to improve the repository.

## Property Tests

### Queues

- **Test if a queue is empty after a pop from an empty queue**.
- **Test if enqueue followed by dequeue returns the correct element**.
- **Test if the queue maintains the correct size after multiple enqueue and dequeue operations**.
- **Test for commutative property for queue reversals**.
- **Test if the queue's size is always non-negative**.

[See the full list of queue property tests](property-tests/queues.md)

### Lists

- **Test if list reversal followed by another reversal results in the original list**.
- **Test if list concatenation is associative**.
- **Test if the length of a list remains the same after reversal**.
- **Test if adding elements to the front or back of the list gives the expected results**.

[See the full list of list property tests](property-tests/lists.md)

### Strings

- **Test if string reversal followed by another reversal results in the original string**.
- **Test if string concatenation is associative**.
- **Test if concatenating an empty string with another string results in the original string**.

[See the full list of string property tests](property-tests/strings.md)

### Numbers

- **Test if addition is commutative**.
- **Test if multiplication is associative**.
- **Test if dividing by a non-zero number gives the correct result**.

[See the full list of number property tests](property-tests/numbers.md)

### More Tests

- We will continue to add tests for additional data structures and algorithms over time. Stay tuned for more!

[See more tests here](property-tests/more_tests.md)

## Popular Property Testing Libraries

Here are some popular property testing libraries that can help you get started in various programming languages:

- **[QuickCheck](https://hackage.haskell.org/package/QuickCheck)** - Haskell
- **[Hypothesis](https://hypothesis.readthedocs.io/en/latest/)** - Python
- **[FsCheck](https://fscheck.github.io/FsCheck/)** - F#
- **[ScalaCheck](https://github.com/typelevel/scalacheck)** - Scala
- **[fast-check](https://github.com/dubzzz/fast-check)** - JavaScript
- **[Test.check](https://github.com/clojure/test.check)** - Clojure

Feel free to contribute any other libraries you use for property testing!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
