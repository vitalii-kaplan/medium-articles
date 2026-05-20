# If the human is one of the agents
What roles does a software developer play in AI-assisted projects, and can we give these roles to AI too?

## The problem
As I showed in my previous research, it can take several hundred prompts to create a relatively simple product. But it doesn't mean that these prompts should be created by a human. So, what should an agent do to substitute for the developer? Can we give it a set of instructions and limitations to simulate the developer and product owner?

The idea of AI writing prompts to AI is not new. It is a core idea of agentic systems.
AI can take different roles and help other agents through a text interface. If all agents are LLMs, we have a fully autonomous system. In AI-assisted development, a human can be treated as one of the agents with several roles. Can we formalize these roles and give them to AI? In my experiment with a game, I was one of the agents. So it was not an autonomous system.
Can we change it and how?
In this article I am going to reflect on my contribution to the development process, treating myself as one of the agents. This may help to build AI that can do this part of the job.

## Self-reflection
I found that my main roles were to check that:
- The code should be readable by a human
- Code complexity should not be too high; introduce classes where needed
- The product should use proper algorithms and data structures. For example use HashTalbe for search and ArrayList for retrieval by index, and nether vice-versa. This possibly can be solved by creating `UX.md` with a detailed description of the expected final user experience
- Functional tests should cover functionality. This can be done with the help of `UX.md`, but it also needs examples of the application state and steps from the original state to the final state.

## The roles
So my work as a "human agent" can be split into these roles:
- Code readability watcher
- Code complexity checker and refactorer
- Creator of `UX.md`
- Algorithm checker based on `UX.md`
- Application states and user behavior examples provider
- Functional tests writer

### Code readability watcher
This possibly can be done by AI, if we formalize what good code is. There are many general recommendations and code styles for different languages. Mature development studios have their own coding styles. So it can be explained to AI.

### Code complexity checker and refactorer
This role does not just check, but actively changes the code. Not by itself, but by prompting. The formal rules and heuristics of how to write good code with low complexity should be placed in `AGENTS.md` from the beginning. But maybe it is overkill to use all of them with each prompt. So, the most important rules should be in `AGENTS.md`, and some rules should be checked regularly by this role, for example before pushing to the repository.

### Creator of `UX.md`
This is where the programming happens now. It can't be done by AI, because it is about the product, not code. The product owner should create this specification. Maybe with help of AI. Maybe as a result of AI interview of the final user. But I don't see how this part can be done solely by AI if AI is not the final user.

### Algorithm checker based on `UX.md`
This certainly can be done by AI. With the current level of abstraction, it treats prompts as a mix of code instructions and business logic. This work can be done by AI. It can be either a code scan using `UX.md` or a request to the main programming AI agent to check that the solution is good for a particular UX.

### Application states and user behaviour examples provider
This is another example of the abstraction of new programming. AI can't create examples of the application state without a human. They are part of business logic and should be provided by the product owner. Again, maybe with help from AI, like: record the current state of the application, record my activity, record the final state. But I don't see how a fully autonomous system can create them by itself, except if AI is the final user.

### Functional tests writer
This certainly can be done by AI if examples are provided.

## Conclusions
As I see it, "Creator of `UX.md`" and "Application states and user behavior examples provider" can't be done by AI without a human. They are the new abstraction layer where programming occurs. They can be facilitated by AI, but I don't see how they can be fully automated. Other roles can be done by AI with no problems.
