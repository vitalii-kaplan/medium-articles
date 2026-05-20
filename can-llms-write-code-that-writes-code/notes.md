# Notes: Can LLMs Write Code That Writes Code?

## Original Task

Given a KNIME workbook, can we create Python code from it?

## Why This Matters

KNIME is a proprietary platform with encapsulated ML algorithms. This is not bad for commercial use, but it is problematic for research, where peer review is needed, and for engineering fields where the cost of error is high.

In these applications, a Python script is better because it can be verified by final users.

## Why Use an LLM?

Developing applications with the help of LLMs is modern and growing in popularity. Coding with AI can improve developer productivity when it is carefully managed. LLMs are already used for the development of many commercial applications.

The principal difference in this development project, in terms of AI-assisted coding, is that the LLM does not create the final code directly. The final code would be a Python script generated from a KNIME workbook. Instead, the LLM creates intermediate code: a code generator or exporter.

This means that the prompts from the developer and other parts of the context describe not only the code generator, but also the final Python code. The AI/LLM should create this intermediate layer by itself on the basis of the prompts, workbook configs, and the description of the expected final Python code.

In addition, the LLM should not only understand how to write code. It also needs to know ML algorithms, data structures, and data transformation algorithms.

## Project Aim

The original aim of the project is not to implement all cases supported by KNIME. KNIME is a very complex product with years of development behind it. It is impossible to reflect all of its complexity in one open-source solution.

But `knime2py` should create Python code that is as close as possible to the original internal KNIME workflow. The Python code should also be human-readable, easy to understand, and easy to inspect, so users can find non-implemented code segments and fix them manually.

## Development Setup

- ChatGPT Personal Plus plan
- Browser chats:
  - Development
  - Documentation
  - Deploy
  - UI
  - Tests
  - RAG
- Codex in VS Code

I used specialized browser chats for discussion of different aspects of development. Each chat had its own history and context.

- Browser chats were used for particular development activities, such as development, deployment, and architecture discussion.
- Codex was used for direct work with code, implementation of decisions, and tests.

The project has `README.md` and `AGENTS.md` files. Codex uses `AGENTS.md` as long-term memory.

The project also has:

- Scripts with examples of runs and parameters
- YAML files for description of the project structure and GitHub builders
- A `Makefile` for routine operations

The project is on GitHub:

<https://github.com/vitalii-kaplan/knime2py>

It has releases as:

- Docker image
- PEX file
- EXE file

## Tests

The project has test coverage of about 85%:

<https://codecov.io/github/vitalii-kaplan/knime2py>

Unit tests cover the main functionality and individual nodes.

But the project also has functional tests that compare:

- Results of data processing made by the KNIME workflow
- Results of applying the generated Python code to the same data

The KNIME workflow, original data, and result data are stored as test data. Python code for each node is generated and run at least once in tests.

## Code Generation Process

The process of code generation consists of two phases:

1. Creation of graphs of connected nodes. There is one graph per isolated set of connected nodes.
2. Traversal through the graph and creation of a linear sequence of code for each node in the graph. There are also common context variables for all nodes, used to pass data and parameters from node to node.

A detailed description of the `knime2py` work process can be found in `knime2py_README.md`.

The description of the internal structure of the resulting Python scripts (`.py` and `.ipynb`) and graph description files can be created on the basis of `./output_example`.

## Types of Nodes and Code Generation Problems

### 1. Data Processing Nodes

These nodes take tables with data and return tables with data.

They were the most straightforward to implement. As with all nodes, the AI received the `settings.xml` file of the node from the KNIME project. This gave it an understanding of parameter names and possible values.

The prompt for such nodes was formed as a description of the transformation that should be made by Python code, based on values from the XML configuration file.

Paired with tests based on other examples of `settings.xml` for the same node, the generator was tuned for different node configurations and states.

Tests for these kinds of nodes check that the result of the Python script on the same data is exactly the same as the result from the KNIME workflow.

### 2. ML Nodes and Nodes With Randomness

These are nodes for ML algorithms, such as Linear Regression, Neural Network, and Decision Trees.

In KNIME, each algorithm is presented as two nodes:

- Learner
- Predictor

Here, the initial request to the AI contained `settings.xml`, the name of the algorithm, and references to already implemented algorithms of the same class. For example: "Implement GBT Predictor the same way we implemented DT Predictor."

The main problem with these types of nodes was not how to explain to the AI what to do or what the result should be. The main problem was hidden KNIME parameters for each algorithm.

The KNIME UI allows users to change the main meta-parameters, and these are reflected in the node's `settings.xml`. But it does not give full control of the process like a Python script does. This is also one reason why a script plus libraries is more suitable for scientific applications than KNIME.

So it was easy to implement algorithms with the same names, but they differ in internal implementation. KNIME's understanding of a neural network can be different from the Python + pandas + scikit-learn understanding of the same algorithm.

As a result, applying the KNIME workflow and the Python script to the same data produced different results. How different they are is the topic of future articles. I will describe it in detail with examples.

It should also be mentioned that we have the same problem with nodes that use randomness. Even if we use the same seed, the libraries in KNIME (Java) and Python generate different random results.

So tests with these kinds of nodes do not check strict equality. Instead, they check distributions and statistical measures of results, such as the confusion matrix.

### 3. Reading, Writing, and Serialization Nodes

Nodes that read and write text formats were easy to implement and test.

But some KNIME nodes serialize blobs with undocumented internal structure, possibly by Java serialization. Without a detailed description of the Java classes of serialized objects, it is impossible to support cross-serialization from KNIME to Python and from Python to KNIME.

So this is not supported. Python code for serialization nodes uses Python serialization. It is impossible to read in Python an object that was serialized in KNIME, and vice versa. But it is possible to serialize an object in Python and read it in Python.

Tests check strict equality of the write-read sequence. Python code should give the same data as KNIME.

### 4. UI Nodes

These are not yet fully supported.

KNIME UI nodes introduce interactivity to the workflow. In KNIME cloud UI mode, users can change workflow parameters, but not the workflow itself.

These types of nodes cannot be implemented in Python in exactly the same way. But it is possible to use the default selection and introduce a special section at the beginning of the script for convenient tuning of these parameters for the rest of the script.

If parameters are set, these nodes are similar to data processing nodes.

### 5. Flow Control Nodes

It is possible to have while loops in KNIME. There are nodes for the loop beginning, condition check, and loop end. Nodes between them are internal to the loop and use loop parameters.

This introduces a new problem for code generation. Instead of local work with each node, we should add an additional block of Python code and use loop parameters as external parameters in each node inside the loop.

Tests for loops with data processing nodes check equality between the KNIME workflow and the Python script on the same data.

## UI for Users

1. `knime2py` is open source, so everyone can clone the sources and run them.
2. There is a Docker image, and it was downloaded more than 700 times.
3. Not all KNIME users are comfortable running from sources or from an image, so I created a UI in the form of a custom KNIME node. It needs a `.pex` or `.exe` package of `knime2py` to work.
4. Ultimately, I created a web interface for `knime2py`: <https://k2pweb.org>. More than 1,500 nodes were requested for export to Python during the first two months.
