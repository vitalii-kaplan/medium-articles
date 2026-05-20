# Can LLMs Write Code That Writes Code?

## 1. Title

Working title: **Can LLMs Write Code That Writes Code?**

Questions to answer:

- Is this title specific enough?
  - Answer: Yes. The source notes are specifically about using an LLM to help build `knime2py`, a tool that converts KNIME workflows into generated Python scripts and notebooks. The title captures the central issue: the LLM is not only writing final application code, but helping create code that generates other code.
- Does it promise one clear idea?
  - Answer: Yes. The clear idea is whether LLMs can reliably help build a code generator, not merely whether they can write isolated code snippets.
- Should the title emphasize the practical risk, the engineering question, or the surprising possibility?
  - Answer: The title should emphasize the engineering question. The notes focus on a real engineering project, verification, tests, workflow translation, node types, hidden parameters, and the limits of reproducing KNIME behavior in Python.

Possible alternatives:

- Can LLMs Safely Generate Code Generators?
- Why Code That Writes Code Is Harder for LLMs
- The Hidden Risk of Asking LLMs to Generate Generators
- When LLMs Write Code That Writes More Code

## 2. Opening Hook

Purpose: Give the reader a reason to care immediately.

Questions to answer:

- What practical frustration or surprising problem opens the article?
  - Answer: The article can open with the problem of converting a KNIME workflow into readable, verifiable Python code. KNIME hides important logic inside a proprietary visual workflow, while Python code can be inspected, tested, and reviewed by final users.
- What happens when generated code is not the final artifact, but a tool that creates more code?
  - Answer: The task becomes harder because the LLM is helping create an intermediate layer: a code generator or exporter. The developer describes the expected final Python code and the KNIME configuration, but the LLM must help create the generator that will produce that final code.
- Why is this different from asking an LLM to write a normal function, script, or test?
  - Answer: A normal function or script can be inspected directly as the final artifact. In this project, the generated implementation must parse workflows, reconstruct graphs, traverse nodes, translate settings, and emit Python scripts or notebooks. The correctness of the tool must be judged through the code it produces.
- What serious reader already suspects this might break?
  - Answer: A serious reader would suspect failure around hidden KNIME parameters, ML algorithm differences between KNIME and Python libraries, randomness, unsupported serialization formats, UI nodes, and flow-control structures such as loops.

Draft notes:

- Start with the difference between generating code once and generating a program that will generate many future files.
- Avoid a generic opening about how AI has changed programming.
- Make the risk concrete: a small mistake in a generator can multiply across an entire codebase.

## 3. Context / Problem Statement

Purpose: Define exactly what problem the article discusses.

Questions to answer:

- What counts as "code that writes code" in this article?
  - Answer: In this article, "code that writes code" means `knime2py`: a KNIME-to-Python exporter that parses a KNIME workflow, reconstructs nodes and connections, and emits runnable Python workbooks as `.py` scripts or `.ipynb` notebooks.
- Are we talking about scaffolding tools, code generators, transpilers, schema-to-code tools, template engines, build scripts, migrations, or agentic coding loops?
  - Answer: The sources describe a code generator/exporter. It is not a general scaffolding tool or migration system. It translates supported KNIME workflow nodes into Python code using pandas and scikit-learn, and also emits graph JSON and Graphviz DOT files.
- What is outside the scope?
  - Answer: Implementing all cases supported by KNIME is outside the scope. KNIME is a complex product with years of development, and the project does not aim to reflect all of its complexity in one open-source solution. Unsupported cross-serialization between KNIME Java objects and Python objects is also outside the supported behavior.
- Why is generated-generator code harder to evaluate than ordinary generated code?
  - Answer: It must be evaluated at two levels: the generator itself and the Python code it emits. The project therefore uses functional tests comparing KNIME workflow results with results from generated Python code on the same data. The emitted outputs also include graph JSON, DOT, scripts, and notebooks, so correctness includes structure, execution order, data passing, and node behavior.
- What common assumption is being challenged?
  - Answer: The article challenges the assumption that if an LLM can write code, it can straightforwardly write a reliable code generator. In this project, the LLM must help encode ML algorithms, data transformations, graph traversal, hidden configuration behavior, and unsupported cases.

Draft notes:

- Clarify that this is not just about whether LLMs can output syntactically valid code.
- The real question is whether they can produce reliable generator logic that preserves intent, constraints, naming, formatting, and edge cases across repeated use.
- Scope should likely include practical engineering tools, not theoretical compiler construction.

## 4. Project Setup, Structure, Tests, and Interfaces

Purpose: Give the reader enough concrete project context to understand what the LLM was helping build.

Questions to answer:

- What was the development environment?
  - Answer: The project used a ChatGPT Personal Plus plan, specialized browser chats, and Codex in VS Code. Browser chats were split by activity: Development, Documentation, Deploy, UI, Tests, and RAG. Each chat had its own history and context. Browser chats were used for discussion and architecture, while Codex was used for direct work with code, implementation of decisions, and tests.
- How was project memory and context managed?
  - Answer: The project used `README.md` and `AGENTS.md`. Codex used `AGENTS.md` as long-term memory. The project also had scripts with examples of runs and parameters, YAML files for project structure and GitHub builders, and a `Makefile` for routine operations.
- What is the internal structure of `knime2py`?
  - Answer: `knime2py` parses a KNIME workflow, reconstructs nodes and connections, splits isolated connected graphs, traverses each graph, and emits a linear sequence of Python code for each node. It also uses common context variables to pass data and parameters from node to node.
- What files does `knime2py` emit?
  - Answer: It emits runnable Python workbooks as `.py` scripts and `.ipynb` notebooks. It also emits graph JSON files and Graphviz DOT files. The example output contains `input__g01_workbook.py`, `input__g01_workbook.ipynb`, `input__g01.json`, and `input__g01.dot`.
- How is the generated Python organized?
  - Answer: The generated script has a consolidated import block, node sections, and a shared `context` dictionary for passing intermediate data. The output example shows node sections such as `CSV Reader #90`, `X_Partitioner #60`, `Equal Size Sampling #33`, and `Logistic Regression Learner #29`.
- How are tests organized?
  - Answer: The project has about 85% test coverage. It has unit tests for main functionality and nodes, plus functional tests comparing KNIME workflow outputs with outputs from generated Python code on the same data. The KNIME workflow, original data, and result data are stored as test data. Python code for each node is generated and run at least once in tests.
- How are different node types tested?
  - Answer: Data-processing nodes and write-read nodes are tested by strict equality between KNIME results and generated Python results. ML nodes and nodes with randomness are tested by distributions and statistical measures, such as confusion matrix, because KNIME Java implementations and Python libraries can produce different exact results even with the same seed.
- What user interfaces exist for the project?
  - Answer: `knime2py` can be run from source because it is open source. It also has a Docker image, which was downloaded more than 700 times. A custom KNIME node UI was created for users who are not comfortable running from source or Docker; it requires a `.pex` or `.exe` package. A web interface was also created at <https://k2pweb.org>, where more than 1,500 nodes were requested for export to Python during the first two months.
- Where is the project distributed?
  - Answer: The project is on GitHub at <https://github.com/vitalii-kaplan/knime2py>. It has releases as Docker images, PEX files, and EXE files.

## 5. Core Claim

Purpose: State the main argument early.

Questions to answer:

- What is the central claim of the article?
  - Answer: LLMs can help build code generators, but the hard part is not producing code text. The hard part is making the generator preserve workflow semantics, handle node-specific behavior, expose unsupported parts clearly, and produce output that can be tested against the original system.
- Is the answer "yes, but only under narrow conditions"?
  - Answer: Yes. The notes support a "yes, but" answer. LLMs were useful in the `knime2py` project, but the project needed scoped goals, separate development contexts, `AGENTS.md` memory, examples, tests, functional comparisons, and acceptance of unsupported KNIME complexity.
- What must be true for LLM-generated code generators to be useful?
  - Answer: The source configuration must be explicit enough to parse, the target output must be described, the generated code must be human-readable, unsupported segments must be easy to find and fix, and there must be tests that compare generated Python behavior with KNIME workflow behavior.
- What should the reader remember in one sentence?
  - Answer: LLMs can help write code generators when humans define the target behavior and verification process, but they do not remove the engineering work of understanding, testing, and limiting the generator.

Candidate thesis:

LLMs can write useful code generators when the target pattern is narrow, explicit, and easy to verify, but they become risky when the generator must encode hidden domain rules, long-term architectural constraints, or many edge cases.

Questions to refine the thesis:

- Is "useful" the right standard, or should the article focus on "safe," "maintainable," or "production-ready"?
  - Answer: "Useful" is supported by the notes, but "verifiable" is the stronger standard for this article. The notes repeatedly emphasize that Python output should be readable, inspectable, manually fixable, and tested against KNIME results.
- Should the thesis emphasize verification cost?
  - Answer: Yes. The project has unit tests and functional tests, about 85% test coverage, and tests that run generated Python code and compare it with KNIME workflow outputs. Verification is central to whether the generator is credible.
- Should the thesis compare generated code with generated generators?
  - Answer: Yes. The notes explicitly say the principal difference is that the LLM does not create the final Python script directly. It helps create an intermediate exporter that generates the final Python code.

## 6. Explanation

Purpose: Explain why the core claim is true.

Use 3-5 supporting sections. Each section should follow:

**Claim -> Explanation -> Example -> Implication**

### 6.1 Generated Generators Multiply Mistakes

Questions to answer:

- Why is a bug in a generator more dangerous than a bug in one generated file?
  - Answer: A generator applies its logic repeatedly across many nodes and workflows. If its translation rule is wrong, every generated script section for that node type or behavior can be wrong.
- How does one wrong assumption spread across many outputs?
  - Answer: In `knime2py`, the exporter traverses the workflow graph and emits a linear sequence of Python code for each node. A wrong assumption in a node exporter, graph traversal, context variable, or parameter interpretation can appear in every generated workbook that uses that pattern.
- What kinds of mistakes multiply: naming, imports, validation rules, null handling, formatting, security assumptions, performance patterns?
  - Answer: The provided sources mention parameter names and values from `settings.xml`, data transformation behavior, ML algorithm parameters, randomness, serialization behavior, UI parameters, loop parameters, graph wiring, execution order, and context variables. They do not provide information about security assumptions.
- What example would show this clearly?
  - Answer: A concrete example is the generated `input__g01_workbook.py`, where the CSV Reader output is stored in `context['90:1']` and reused by several `X_Partitioner` nodes. If the CSV Reader translation or context wiring were wrong, the error would affect multiple downstream ML pipelines.

Paragraph prompts:

- Point: A generator turns one mistake into a repeatable mistake.
  - Answer: In `knime2py`, a node exporter is not a one-off code block. It becomes a reusable translation rule for KNIME nodes of that type.
- Why it matters: The output may look consistent while being consistently wrong.
  - Answer: Consistency alone is not proof of correctness. The generated Python can have a clean structure, imports, node sections, and a shared context dictionary while still differing from KNIME behavior because of hidden algorithm parameters or library differences.
- Example: A generator creates API clients but mishandles optional fields in every endpoint.
  - Answer: No information provided.
- Consequence: Review must include both the generator and representative outputs.
  - Answer: The project follows this principle by testing both main functionality and generated Python behavior. Functional tests compare KNIME workflow results with generated Python results on the same data.

### 6.2 LLMs Are Good at Surface Patterns, Weaker at Durable Rules

Questions to answer:

- What parts of generator writing are LLMs naturally good at?
  - Answer: The notes show that LLMs were useful for implementing node exporters from examples, `settings.xml` files, algorithm names, and references to already implemented nodes of the same class, such as "Implement GBT Predictor the same way we implemented DT Predictor."
- What parts require understanding durable project rules?
  - Answer: Durable rules include how KNIME nodes connect, how data and parameters move through the shared context, how graph traversal creates execution order, how Learner and Predictor nodes relate, how loops are represented, how unsupported nodes are surfaced, and how generated code remains readable and inspectable.
- How do hidden conventions, architecture, and business rules enter generator logic?
  - Answer: Hidden behavior enters through KNIME node settings, algorithm defaults not fully exposed in `settings.xml`, Java-based implementations, serialized object formats, workflow graph structure, node states, port wiring, and loop variables.
- Where does pattern imitation stop being enough?
  - Answer: It stops being enough for ML nodes, randomness, serialization, UI nodes, and flow-control nodes. These cases require more than copying the shape of a previous exporter because their behavior depends on hidden parameters, runtime semantics, or platform-specific internals.

Paragraph prompts:

- Point: LLMs often recognize the shape of a generator faster than they understand its responsibility.
  - Answer: The project used prompts based on existing implemented algorithms and node settings, which helps with shape and pattern. But the notes show that hidden KNIME parameters and library differences still caused behavioral differences.
- Why it matters: Generator code must encode rules, not merely resemble existing examples.
  - Answer: The output must match a workflow's behavior closely enough to be useful. For data-processing nodes, strict equality with KNIME output is expected. For ML and randomness, equality may be impossible, so the rule changes to checking distributions and statistical measures.
- Example: A model can imitate a template for CRUD files but miss authorization, naming, or migration constraints.
  - Answer: No information provided.
- Consequence: The more implicit the rules are, the less trustworthy the generated generator is.
  - Answer: The KNIME examples support this. Data-processing nodes with explicit `settings.xml` behavior were the most straightforward. ML nodes with hidden KNIME parameters and serialization nodes with undocumented Java internals were much harder or unsupported.

### 6.3 Verification Cost Is the Real Constraint

Questions to answer:

- How do you verify code that writes code?
  - Answer: In this project, verification includes unit tests for main functionality and node behavior, plus functional tests that compare KNIME workflow output with the output of generated Python code run on the same data.
- Is it enough to inspect the generator source?
  - Answer: No. The notes say Python code for each node is generated and run at least once in tests. That means inspection of the generator source is not enough; representative generated outputs must execute.
- How many generated outputs must be checked?
  - Answer: No exact number is provided. The notes say Python code for each node is generated and run at least once in tests, and functional tests compare KNIME workflow results with generated Python results.
- What tests would make this acceptable?
  - Answer: The sources describe unit tests, functional tests against KNIME workflow outputs, strict equality tests for data-processing and write-read nodes, and distribution/statistical checks for ML nodes and nodes with randomness.
- When does reviewing the generator cost more than writing it manually?
  - Answer: No information provided.

Paragraph prompts:

- Point: The productivity question is not "did the LLM produce code quickly?" but "can we prove the generator behaves correctly?"
  - Answer: The notes support this. The project relies on tests, functional comparisons, generated output inspection, and clear unsupported segments rather than treating LLM-produced code as correct by default.
- Why it matters: A fast generator that requires slow review may not save time.
  - Answer: No information provided.
- Example: Snapshot tests, golden files, schema fixtures, and edge-case inputs can make outputs inspectable.
  - Answer: The notes mention stored KNIME workflow data, original data, and result data as test data. They do not mention snapshot tests, golden files, or schema fixtures by those names.
- Consequence: LLM-generated generators need stronger tests than ordinary one-off scripts.
  - Answer: The project uses stronger validation than only checking generated syntax: it runs generated Python and compares results with KNIME outputs. For random and ML nodes, tests check distributions and statistical measures rather than strict equality.

### 6.4 The Best Use Cases Are Narrow and Mechanical

Questions to answer:

- Which generator tasks are suitable for LLMs?
  - Answer: Based on the notes, data-processing nodes are the most suitable. They take tables as input, return tables as output, have parameters in `settings.xml`, and can be tested by strict equality against KNIME workflow results.
- What makes a task narrow enough?
  - Answer: A task is narrow enough when the XML configuration gives clear parameter names and values, the transformation can be described directly, and tests can compare the generated Python output with KNIME output on the same data.
- When is the source of truth explicit?
  - Answer: The source of truth is explicit when it is present in `workflow.knime`, per-node `settings.xml` files, graph connections, port wiring, and stored test data. The README says the tool reads a workflow directory with `workflow.knime` and per-node `settings.xml` files.
- What examples are safe or at least manageable?
  - Answer: The notes identify data-processing nodes and text reading/writing nodes as easiest to implement and test. The output example also shows generated code for a CSV Reader and Equal Size Sampling node.

Possible examples:

- Creating boilerplate files from a small, explicit schema.
  - Answer: No information provided.
- Generating repetitive test cases.
  - Answer: No information provided.
- Producing adapter code from a documented API shape.
  - Answer: No information provided.
- Creating Markdown documentation stubs from structured metadata.
  - Answer: No information provided.

Paragraph prompts:

- Point: LLMs are most useful when the generator maps explicit input to predictable output.
  - Answer: Data-processing nodes fit this pattern because their `settings.xml` files expose parameter names and values, and their outputs can be compared directly with KNIME outputs.
- Why it matters: Explicit constraints reduce hidden reasoning.
  - Answer: The notes contrast explicit data-processing nodes with ML nodes, where hidden KNIME parameters and different library implementations make exact reproduction difficult.
- Example: A schema-to-TypeScript-type generator with snapshot tests is easier to trust than a generator that infers architecture.
  - Answer: No information provided.
- Consequence: The right question is whether the generator's rules can be written down and tested.
  - Answer: The project reflects this: supported nodes are translated into Python, unsupported nodes get clear TODO stubs with settings parameters, and generated code is tested against KNIME output where possible.

### 6.5 The Dangerous Use Cases Require Engineering Judgment

Questions to answer:

- Which generator tasks should not be delegated casually?
  - Answer: ML nodes, nodes with randomness, serialization nodes, UI nodes, and flow-control nodes should not be delegated casually. The notes identify each of these as having special problems.
- What makes a generator architecture-sensitive?
  - Answer: It becomes architecture-sensitive when it must reconstruct workflow graphs, split isolated connected components, preserve port wiring, order execution, pass data through shared context variables, represent loops, and emit scripts or notebooks that remain readable and debuggable.
- When does the tool become a source of technical debt?
  - Answer: No information provided.
- How can generated generator code create maintenance traps?
  - Answer: The notes imply traps around unsupported KNIME complexity, hidden ML parameters, incompatible serialization, UI semantics that do not map directly to Python, and loops that require non-local code generation. They do not provide a specific maintenance incident.

Possible examples:

- Generating service layers with business logic.
  - Answer: No information provided.
- Generating migrations without deep schema knowledge.
  - Answer: No information provided.
- Generating framework abstractions that future developers must live with.
  - Answer: No information provided.
- Generating code-mod tools that rewrite existing project files.
  - Answer: No information provided.

Paragraph prompts:

- Point: The risk rises when the generator decides architecture instead of applying a known rule.
  - Answer: The `knime2py` generator must do architectural work when it creates graphs, traverses them, handles loops, and chooses how KNIME structures become Python structures. These are higher-risk than local table transformations.
- Why it matters: Architecture choices are long-lived, while LLM outputs are often optimized for local plausibility.
  - Answer: No information provided.
- Example: A generator creates repository classes that conflict with an existing transaction model.
  - Answer: No information provided.
- Consequence: Human engineers must own the design, even if the LLM helps draft the implementation.
  - Answer: The development setup supports this: browser chats were used for architecture discussion, while Codex was used for implementation of decisions and tests. The notes describe LLM assistance inside a managed development process, not autonomous ownership.

## 7. Example / Case / Mini-Demonstration

Purpose: Show the argument concretely.

Questions to answer:

- What small example can demonstrate the difference between a useful generator and a risky one?
  - Answer: A useful example is a data-processing node such as CSV Reader or Equal Size Sampling, where `settings.xml` provides parameters and tests can check strict equality. A risky example is an ML workflow with Logistic Regression, Decision Trees, Random Forest, SVM, Gradient Boosted Trees, or Neural Network nodes, where KNIME and Python libraries may produce different results.
- Can the example fit in a Medium article without becoming too long?
  - Answer: A reduced example can fit. The full `input__g01_workbook.py` is too large for a compact article section, but a short excerpt showing `CSV Reader #90`, `X_Partitioner #60`, `Equal Size Sampling #33`, and `Logistic Regression Learner/Predictor #29/#31` would show the main idea.
- Should the example use code, pseudo-code, or a described failure case?
  - Answer: The sources support using a small real code excerpt from `output_example/input__g01_workbook.py`, plus a described failure case for ML/randomness where strict equality with KNIME is not expected.
- What expected output and actual failure will make the point obvious?
  - Answer: For data-processing nodes, the expected output is strict equality between KNIME results and generated Python results. For ML and randomness, the "failure" is that exact equality may not hold even with the same seed, because KNIME Java implementations and Python libraries generate different behavior. Tests must compare distributions or statistical measures instead.

Possible mini-demo:

- Ask an LLM to write a generator that creates API client functions from an endpoint list.
  - Answer: No information provided.
- The happy path works.
  - Answer: No information provided.
- The failure appears when an endpoint has optional parameters, authentication differences, pagination, or non-JSON responses.
  - Answer: No information provided.
- Show how the generator multiplies that hidden assumption across multiple generated functions.
  - Answer: No information provided.

Questions for the demo:

- What is the input?
  - Answer: The input is a KNIME workflow directory containing `workflow.knime` and per-node `settings.xml` files. The example output uses `/work/input/workflow.knime` and a CSV file at `/work/data/HW_Churn_test/One2ManyEncoded.csv`.
- What generator does the LLM write?
  - Answer: The LLM helped develop `knime2py`, a KNIME-to-Python code generator/exporter. The sources do not identify one single generated source file as "the generator" written by the LLM.
- What output does it produce?
  - Answer: It produces runnable Python workbooks as `.py` and `.ipynb`, graph JSON, and Graphviz DOT files. In the example, it produced `input__g01_workbook.py`, `input__g01_workbook.ipynb`, `input__g01.json`, and `input__g01.dot`.
- What edge case exposes the weakness?
  - Answer: Hidden KNIME ML parameters, differences between KNIME Java implementations and Python libraries, randomness even with the same seed, undocumented Java serialization blobs, UI nodes, and while-loop flow control expose weaknesses.
- What test would have caught it?
  - Answer: Functional tests comparing generated Python output to KNIME workflow output would catch data-processing differences. For ML and random nodes, distribution and statistical checks such as confusion matrix comparisons are used instead of strict equality.

## 8. Counterpoint / Limitation

Purpose: Admit where the argument weakens.

Questions to answer:

- In what cases are LLM-generated generators genuinely useful?
  - Answer: They are genuinely useful for supported, explicit, testable translations. The notes identify data-processing nodes and text read/write nodes as easy to implement and test. The project also successfully exported an example workflow with 45 nodes, 55 edges, and zero unimplemented nodes.
- When is the risk low?
  - Answer: Risk is lower when parameters are visible in `settings.xml`, the transformation is deterministic, the generated Python can be checked by strict equality against KNIME, and unsupported code segments are clearly emitted for manual inspection.
- What would a smart critic say?
  - Answer: A smart critic would point out that KNIME is complex, some algorithm behavior is hidden, Java and Python implementations differ, random results may differ even with the same seed, and some serialization formats cannot be supported without undocumented Java class details.
- Are there cases where generator code is easier to review than hand-written repeated code?
  - Answer: No information provided.
- Does this argument change when there are strong tests, typed schemas, linters, or golden files?
  - Answer: Strong tests matter. The notes describe unit tests, functional tests, strict equality checks, and statistical checks. The sources do not mention typed schemas, linters, or golden files.

Draft notes:

- Do not imply that LLMs should never write generators.
- The stronger position is that generators need explicit contracts and verification.
- Acknowledge that repetitive generator tasks can be a good fit.

## 9. Practical Takeaway

Purpose: Give the reader something usable.

Questions to answer:

- What should engineers do before asking an LLM to write a generator?
  - Answer: They should define the final generated code shape, provide examples of source configuration such as `settings.xml`, describe the expected transformation, identify unsupported cases, and decide how the generated output will be tested against the original system.
- What checklist should they apply?
  - Answer: They should check whether the source configuration is explicit, whether the target code should be `.py`, `.ipynb`, graph JSON, or DOT, whether each node type has examples, whether generated code is readable, whether unsupported segments are visible, and whether functional tests can compare outputs.
- How should they evaluate whether the result is safe?
  - Answer: They should run generated Python on the same data as the KNIME workflow and compare results. For deterministic data-processing nodes, strict equality is expected. For ML and randomness, distributions and statistical measures should be checked.
- What should they measure besides generation speed?
  - Answer: They should measure test coverage, functional equivalence to KNIME, number of unsupported nodes, readability of generated code, ease of manual repair, reproducibility within Python, and whether graph structure and execution order are preserved.

Possible checklist:

- Is the source of truth explicit?
  - Answer: In `knime2py`, the source of truth is explicit when the workflow directory contains `workflow.knime` and per-node `settings.xml` files, and when original data and expected KNIME result data are available for tests.
- Are the transformation rules written down?
  - Answer: They are written down through prompts, examples, node exporter implementations, `README.md`, `AGENTS.md`, test data, and project documentation. The notes do not provide the full rules themselves.
- Are edge cases known?
  - Answer: Some are known: ML hidden parameters, randomness, serialization, UI nodes, and flow-control nodes. The notes do not claim all edge cases are known.
- Can representative outputs be snapshot-tested?
  - Answer: The sources do not mention snapshot tests. They do mention functional tests that run generated Python and compare it with stored KNIME workflow results.
- Can the generated output be reviewed cheaply?
  - Answer: The project aims for generated Python code to be human-readable, easy to understand, and easy to inspect. The sources do not quantify review cost.
- Is the generator applying architecture or inventing it?
  - Answer: In this project, the generator applies a known architecture: parse a workflow, reconstruct nodes and connections, split isolated graphs, traverse the graph, emit node sections, use shared context variables, and write workbooks plus graph files.
- Who owns maintenance when the generator breaks?
  - Answer: No information provided.

Candidate takeaway:

Use LLMs to draft generators only when you can describe the input, output, rules, and tests clearly. If those are vague, the problem is not ready for automation.

## 10. Ending

Purpose: Leave the reader with the broader insight.

Questions to answer:

- What larger lesson does this reveal about LLM code generation?
  - Answer: The larger lesson is that LLM code generation becomes more serious when the output is not the final code but a system that creates final code. At that point, correctness depends on explicit rules, tests, and human ownership of the architecture.
- How does code that writes code expose the limits of pattern imitation?
  - Answer: It exposes those limits because similar-looking generated code can still fail to match KNIME behavior. Hidden ML parameters, different library implementations, randomness, serialization formats, and workflow control flow cannot be solved by surface pattern imitation alone.
- What should the reader keep thinking about after the article ends?
  - Answer: The reader should keep thinking about verification. The question is not only whether an LLM can generate a code generator, but whether the generated outputs are readable, testable, close enough to the original system, and honest about unsupported behavior.

Draft notes:

- End by distinguishing code production from engineering responsibility.
- The final insight should be that generated generators do not remove the need for understanding; they increase the need to make understanding explicit.

Possible ending direction:

The question is not only whether an LLM can produce a generator. It often can. The better question is whether the rules behind that generator are explicit enough that a team can verify, maintain, and trust what it produces.

## 11. Possible Future Article Material

- The exact differences between KNIME ML algorithm results and Python library results are not fully described here.
- The notes say this can become a separate article with detailed examples.
- The same is true for randomness: even with the same seed, KNIME Java libraries and Python libraries may produce different results.
