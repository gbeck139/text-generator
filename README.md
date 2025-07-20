# Markov Chain Text Generator

A command-line tool, written in Java, that generates text based on the statistical properties of an input file. This project was developed for a Data Structures & Algorithms course in collaboration with a partner and achieved a top 15% runtime out of 150+ students.

## Project Overview

This tool analyzes an input text to build a Markov chain model. This model is then used to generate new text that mimics the style and vocabulary of the original source. The generator can operate in two modes:

1.  **Deterministic ("one"):** Always chooses the single most probable word to follow the current word.
2.  **Stochastic ("all"):** Chooses the next word from all possibilities based on a weighted probability distribution, resulting in more random and varied output.

## Features

* Generates text of a user-specified length (`k`) from a given starting word (`seed`).
* Supports both deterministic and stochastic generation modes.
* Can also output the `k` most probable words that are observed to follow a given seed word in the text.

## Architecture & Data Structures

The core of the project is the probabilistic model, which is built using a **nested HashMap** data structure to achieve an average-case **O(1) time complexity for lookups**. This was chosen for its efficiency in insertion and search, which are the most frequent operations.

* **Outer HashMap:** The keys are words from the input text. The values are custom `SpecialHashMap` objects that store all the words that have been observed to follow the key.
* **Inner `SpecialHashMap`:** This custom wrapper class extends `HashMap` and maps a following word (String) to a `Word` object. It also tracks the total count of all following words and holds a reference to the single most probable `Word` for fast access.
* **`Word` Object:** A custom object that stores the word as a string, its frequency of occurrence, and its calculated probability of appearing after another word.

This process involves parsing the input file, tracking word pairs, and then iterating through the completed data structure to calculate the probability of each word following another.

## Performance Analysis

To validate the efficiency of the design, experiments were conducted to analyze the runtime as a function of both the input file size and the vocabulary size (word variety).

#### 1. Testing with Increasing File Size

In this experiment, the vocabulary was limited to 100 unique words, while the file size (N) was increased. As shown in the plot, the runtime scales linearly, O(N), with the number of words in the input file. This is expected, as the primary operation is a single pass through the file to build the model. While other operations exist, they are not nested and the overall complexity remains linear, or O(N).

![Chart showing linear increase in time with increasing file size](/docs/IncreasingFileSizeGraph.png)

#### 2. Testing with Increasing Word Variety

In this experiment, the file size and the number of unique words (N) both increased together. The runtime again demonstrated linear behavior. While the nested data structure could theoretically lead to O(N²) behavior in a worst-case scenario where every word follows every other word, this is not observed with typical text inputs and file sizes. The file-reading process, which is O(N), remains the dominant factor in the overall runtime.

![Chart showing linear increase in time with increasing word variety](/docs/IncreasingWordVarietyGraph.png)

## How to Run

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/gbeck139/text-generator.git
    cd text-generator
    ```

2.  **Compile the Java code:**
    ```bash
    javac comprehensive/*.java
    ```

3.  **Run the text generator:**
    The program takes 3 or 4 command-line arguments depending on the desired functionality.

    *To generate text:*
    ```bash
    java comprehensive.TextGenerator <filename> <seed_word> <num_words_to_generate> <"one"|"all">
    ```

    *To find the k most probable words:*
    ```bash
    java comprehensive.TextGenerator <filename> <seed_word> <k>
    ```
    **Example:**
    ```bash
    java comprehensive.TextGenerator input.txt hello 50 all
    ```