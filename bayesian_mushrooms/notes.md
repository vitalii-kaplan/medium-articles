# Bayes' theorem with mushrooms and a geometrical proof

## Mushrooms
You are gathering mushrooms. You know that 10% of all mushrooms in the forest are poisonous. You pick up the next mushroom, and your friend tells you: "Throw it away. It has a red cap. Half of all poisonous mushrooms have red caps." Should you throw away this mushroom or keep it?

This practical situation does not have a solution in its current form. It is missing important knowledge: how many mushrooms have red caps? In other words, how many edible mushrooms have red caps? Without this, you cannot answer the question.

## Motivation
I decided to write this article because I have read many explanations and proofs of Bayes' theorem, but all of them were relatively complex. I decided to write my own explanation with geometrical examples and a proof. I think such a powerful theorem can be illustrated as clearly as the pigeonhole principle. I hope I have found this illustration.

## Three points inside the segment
But let's go back to mushrooms. If, for example, no edible mushrooms have red caps, the solution is obvious: you should throw away all red-capped mushrooms. If, on the other hand, all edible mushrooms have red caps, you should throw away everything except the red-capped mushrooms.

So, to answer the practical question, you need three statements:
- how many mushrooms in the forest are poisonous
- how many poisonous mushrooms are red-capped
- how many mushrooms overall are red-capped (or how many edible mushrooms are red-capped)

## Geometrical illustrations
This gives us a geometrical illustration.
For simplicity, let's assume, without loss of generality, that there are only red caps and brown caps (not red), and no other colors.
Let's place all our mushrooms from left to right. First, place all poisonous mushrooms: brown poisonous mushrooms, and then red-capped poisonous mushrooms. Next, place red-capped edible mushrooms and, finally, brown edible mushrooms.
(bayesian_mushrooms/on_one_line.png) 

As I mentioned before, we need three statements. So we have three points inside the segment: one dividing brown poisonous mushrooms from red-capped poisonous mushrooms, one dividing red-capped poisonous mushrooms from red-capped edible mushrooms, and one dividing red-capped edible mushrooms from brown edible mushrooms.
(bayesian_mushrooms/segment.png)

## Probabilities
Now let's add some formalization. If `Poisonous`, `Edible`, `Red`, `BrownPoisonous`, `RedPoisonous`, `RedEdible`, and `BrownEdible` are counts of mushrooms of certain types, then by definition of probability:
Probability of a poisonous mushroom P(Poisonous) = `Poisonous / (Poisonous + Edible)`
Probability of a red-capped mushroom P(Red) = `Red / (Poisonous + Edible)`
Probability that it has a red cap, given that it is poisonous, P(Red|Poisonous) = `RedPoisonous / Poisonous` -- this is the knowledge your friend told you, remember? Half of all poisonous mushrooms have red caps.
P(Red|Poisonous) is short for P(Red|Poisonous = true).
And we need to find P(Poisonous|Red) = `RedPoisonous / Red` -- if I see a red cap, what is the probability that it is poisonous?

These formulas are just ratios: poisonous mushrooms to all mushrooms, red-capped mushrooms to all mushrooms, red-capped poisonous mushrooms to all poisonous mushrooms, and red-capped poisonous mushrooms to all red-capped mushrooms. I hope you can clearly see them in the image, like pigeons in holes.

## Proof
Now:

P(Red|Poisonous)/P(Poisonous|Red) = (`RedPoisonous / Poisonous`) / (`RedPoisonous / Red`) = `(1 / Poisonous) / (1 / Red)` = `Red / Poisonous` = P(Red)/P(Poisonous)

Or in the classical view:
P(Poisonous|Red) = P(Red|Poisonous) * P(Poisonous) / P(Red)

And it gives us the classical Bayesian situation:
- Prior knowledge P(Poisonous): how many mushrooms in the forest are poisonous
- Likelihood P(Red|Poisonous): how many poisonous mushrooms are red-capped
- Evidence P(Red): how many mushrooms overall are red-capped
- Posterior knowledge P(Poisonous|Red): whether it is better to throw away a mushroom with a red cap.

## The outcome 
In this form, with a geometrical proof, Bayes' theorem sounds like a tautology. We showed that 1 = 1, actually. And this is a very interesting outcome. The power of the theorem is not in a complex relation between probabilities, but in interpretation: what meanings the subsegments have.
In the next article, I am going to show how the same illustration can be used to interpret ML model results. The same theorem, the same geometrical relation, but different meanings.
