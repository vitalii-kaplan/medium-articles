# When AI does virtual chores for you
In my case, it freed 90GB of disk space.

# The problem
I found that my two-year-old MacBook had almost no free disk space left. I knew I had not downloaded or stored many additional files. A year ago, the same computer gave me the same practical value, but used less disk space. Now it did not.

I am sure you know this situation, regardless of whether you use a Mac or a PC. After several years, a computer works worse than it did after purchase. Not necessarily because it is old. Let's call it dirty. There are applications that can help improve the situation, but the reasons for "worse" depend on how the computer was used, and I don't know a good universal solution.

## My solution
The three most popular operating systems in the world, Windows, macOS, and Linux, all have terminals. They can execute commands and scripts. Scripts are text, and LLMs are very good with text. So my idea was to use an LLM to write scripts that check the health of my computer, especially disk usage.

Even if you have never run scripts in the terminal, this activity does not require too much from you. You formulate a request, run scripts, collect logs, and show them to your AI. It can write scripts, read outputs, and explain what happened. It can also correct itself and improve the scripts. You just need to formulate what to do.

With this idea, I started this experiment. I am going to share the approach, the process, and the results.

## My setup
- macOS 14.4.1
- VS Code as the development environment (basically a text editor in this experiment)
- Codex, OpenAI's coding agent plugin for VS Code. It integrates ChatGPT into VS Code, so Codex can see files and run some commands on your computer.
- ChatGPT Plus. This makes Codex more powerful. You can try other LLMs if they are powerful enough and can be integrated into a code editor.

That's it. To start, you need an IDE, which is a development environment for programmers, with an AI plugin. VS Code is free. The basic version of AI is free too, but it is better to use the improved version. Technically, you can do this without an IDE. There is a version of Codex for the command line. But for me, it is more convenient in an IDE.

## The process
As in all my experiments with AI-assisted development, I started with a new empty project. I explained the purpose of the project to Codex and asked it to create `README.md` and `AGENTS.md` files. The README is for me, and AGENTS is for Codex itself, to record important information about the project. Both files are text, and it is generally a good idea to read them after creation to see how AI understands you. This is the starting point.

Next, I asked for a script that performs a health check of my operating system. You can run this script yourself or ask AI to run it. The second option is easier, but some commands are forbidden from the VS Code terminal. So to run a serious check or fix, you may need to open the terminal yourself. It is not difficult. AI can explain how to do it, step by step, like a professional system administrator.

For every action that does not just scan but also changes something, it is important to ask for a script that imitates the action and writes logs without actually changing anything. AI writes the script, you run it in read-only mode, you show the logs to AI, it confirms that everything looks OK, and only then do you run the full script.

After the health scan, we found several issues and cleaned about 90GB of disk space from unusable old applications, old application versions, and cache. The whole maintenance process took about three hours. I am very glad about the result.

## We found and fixed

Here are examples of what we found and how we fixed it:

- **Old Whisky installation and large Windows game data.** Whisky was deprecated in Homebrew, and one of its bottles contained a Steam directory using more than 5GB. We uninstalled the Whisky cask and removed its bottle data. After that, the Homebrew warning disappeared and the large bottle directory was gone.

- **Docker storage hidden in build cache.** Docker itself looked small at first: only two images, about 2GB total. But `docker system df` showed that the real problem was the build cache, which used almost 12GB. This explained why Docker Desktop used much more disk space than the visible images suggested.

- **Anaconda taking space and controlling Python commands.** Anaconda used about 15GB and was first in the shell path for `python`, `pip`, and Jupyter commands. We created a guarded cleanup script that removed Anaconda, cleaned its shell startup lines, and switched active Python and `pip3` back to Homebrew Python.

- **Broken Homebrew dependencies.** `brew missing` showed that `gtksourceview4`, `pspp`, and `spread-sheet-widget` were missing the correct ICU dependency. We upgraded `icu4c@78`, reinstalled the affected formulae, and verified that `brew missing` became clean.

- **Broken Command Line Tools.** Homebrew also reported problems with Apple's Command Line Tools. Checks showed that `xcode-select` and `xcrun` could not find the active developer tools. We reset the Command Line Tools installation, ran the macOS installer, and verified that `xcode-select -p` and `xcrun --find clang` worked again.




