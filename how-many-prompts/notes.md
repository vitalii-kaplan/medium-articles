# How Many Prompts Are Needed for a Turn-Based Strategy Game in JS?

My answer is 290 for the code only.
Approximately 40 for assets and 30 for deployment.

## The Project

I had the idea for this project for several months. It is a turn-based strategy game about space exploration, asteroid mining, and IPOs on space stocks. A game for myself. I like strategies.

Turn-based is the simplest: you can apply changes to the game state at the moment the turn ends. All changes are induced by the user, with no need for loops on the server. No need for a server and DB. It is like a card game, but for one person. A version of solitaire if you would like.

Several weeks ago my 11-year-old son said something like this:

> I prompted ChatGPT, for creation of a new game... for an iPad... like Mario... and it made something. But I don't understand what to do with the output. You are a programmer... do you know what we should do with the output?

Sure I know that it doesn't work this way.
I answered that it was impossible to create a game with a single prompt.

So, how many prompts does one need to create a game?

## Scope

I started with some kind of formalization of the problem. How can we simplify the problem without losing generality? Which assumptions are good?

I reduced it to:

- A turn-based strategy game in JS.
- Estimated play time of one hour per game session, from start to end.
- Simple but understandable graphics.
- No sounds.
- HTML, CSS, JavaScript only, because I don't want other engines except the browser.
- On the server side, Nginx serving static files with no DB or other server-side activity.
- No registration.
- No save/load.

## Development Setup

- VS Code.
- GitHub Copilot.
- ChatGPT Plus integrated into VS Code as a plugin.
- Krita + AI Image Generation plugin with Flux 2 Klein for images.
- ChatGPT web version for general questions and architecture.
- Chrome browser with debug console.

I started with GitHub Copilot and switched to ChatGPT. They are different and I found that for my style and for this game ChatGPT is better. I am not going to compare them here. I don't have enough research material about it to make any statements.

## Prompt Log

To answer the question of the article I recorded prompts and replies.

I collected them in a JSON file with these fields:

| Field | Meaning |
| --- | --- |
| `index` | Incremental integer Id |
| `prompt` | My request |
| `worked_for` | Time it worked, nullable |
| `reply` | An answer |
| `files_changed` | Lines of code changed |
| `lines_added` | Lines of code added |
| `lines_removed` | Lines of code removed |
| `files` | List of changed files |

You can find it here https://pallasprospect.com/development-log.json 

It contains "interaction_count": 287. I lost about 10 first prompst. So it was bout 290+ prompts for the code only. Part of the records are from Copilot and part are from ChatGPT. They are in the same format.

I found that it is difficult to extract these history records. They are local, and stored by both AIs in DBs, but I did not find how to extract them in a convenient way, so I started to fill the log by copy-paste and formatting and later I just asked AI to fill the last prompts/replies preserving the JSON structure.

So, after 290+ prompts for code, approximately 40 for assets, and 30 for deployment I can present you the results of my work. In both meanings: as a game 
https://pallasprospect.com/
and as this article.

## Starting Conditions

I need to mention here that I had an idea and the main details of the game in my mind before the start of development, so I did not need to discuss it with AI.

I worked before as a software developer in game development studios. I have 20 years of programming experience with several languages and an MS in mathematics. I know how web services work. I know HTML, CSS, JS, and how to debug them.

So it was not just "I want a game, create it for me". And I needed more than 300 prompts and several weeks to create a game I like to play by myself. And, joyfully, my son played it too.

## Insights

It was a very interesting experience of AI-enhanced development. I found both AI engines to be good and helpful co-workers. I can summarize my experience with the following insights:

- It is ok to have the prompts as a mix of code change commands and game-related asset descriptions. So it is possible for an LLM to understand when I speak about an abstract game entity related to a sci-fi environment and at the same time about a code entity like a class or object. The level of abstraction it understands is very high, even to curiosity: we have a rocket in the scene, I intended the player to disassemble the rocket, so it would be possible to build in these cells. But Copilot itself decided to prevent building in these cells, arguing that "but there is a rocket there!".

- At one moment I found that half the time we don't discuss code, but game rules. In pre-AI development times we created special configs for game designers. We can't allow them to change the code, so we needed a special layer for GD: they changed configs. Now I see that this layer is not needed. A designer can by prompt directly change his version of the code, test the idea and later include the solution with the help of a software developer to the production code.

- If many elements should be changed, it is a good idea to prepare one, make changes for it, fix the process, and after that give a command to change others like in the example.

- We have a table on the page with several columns. The last column has buttons. When I asked to add new columns to the table, it placed the new column before the column with buttons. As I intended. I did not explain it.

- You are a game developer and your part of the work is to play the game after changes to find bugs. It makes bugs. Not more than a middle software developer on the same tasks.

- Tests are the problem in games. So TDD doesn't normally work here. It is not about AI. It is about the development of something that doesn't have a strict specification and can be changed because, for example, the player doesn't feel something we want him to feel. Games are magic.

- AI generates beautiful spaghetti. OOP and classes are still on you. It is ok for it to solve everything by adding many-many if-else. So, check the code, regularly extract functionality, create new classes.

- Understood properly the instruction: "In Market field in the row Raw Resources: at the right corner add a checkbox 'Auto sell all'". And it added and implemented "select all checkboxes" functionality.

- Understood when I ask about text only and when about text and logic, no need for clarification.

- Default visual design (html, css) in VSCode was bad. Fixed it in web version of ChatGPT. Without your knowledge of CSS and HTML and JS debug in browser, it will be difficult to tune the visual.

- Sometimes (two-three times in this project) it ignores a new prompt and answers again to the previous one.

- I asked it to record all important information in README.md for me and in AGENTS.md for itself. Sometimes it forgets to change .md files. I need to remember about it.

## Summary 
Creating a game with AI is possible, but it is still real development work, not one prompt story. It needs an idea, judgment, debugging, iteration, and many prompts. I hope my experience and results will help readers create games and other services with AI. It becomes easier and more fun today.
