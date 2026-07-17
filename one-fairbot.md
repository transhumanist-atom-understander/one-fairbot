# My "Payorian FairBot" was just the original FairBot

[MIRI's proof-based prisoner's dilemma tournament](https://arxiv.org/abs/1401.5577) defined "agents" encoded as formulas of Peano arithmetic (PA) with one free variable.
$A(X)$ means the agent $A$ cooperates in a match against the agent $X$, and is constructed by plugging in the Gödel number of the formula defining $X$ as the free variable in the formula defining $A$.

The simplest interesting agent in the tournament was called FairBot.
Using $F(X)$ to mean FairBot cooperates with another agent $X$, it satisfies:
$$\mathrm{PA} \vdash F(X) \leftrightarrow \Box X(F)$$
I will call this condition Löbian fairness, and refer to this agent as the Löbian FairBot.

That's because in [a previous post](https://www.lesswrong.com/posts/LaCP6WyNzX8kiZn3w/payorian-cooperation-is-easy-with-kripke-frames), I defined an alternative "Payorian FairBot", which satisfies a condition I'll call Payorian fairness:
$$\mathrm{PA} \vdash F(X) \leftrightarrow \Box(\Box F(X) \rightarrow X(F))$$

I wondered, though, is this really a distinct agent?
The answer is no: these two fairness conditions are equivalent.
The Payorian FairBot is Löbian-fair, and the original Löbian FairBot is Payorian-fair.

## Elementary and sophisticated proofs

One way to prove the equivalence of these two fairness conditions is to simply grind through provability logic in both directions.
I'll call this the "elementary proof", since it doesn't use the deep theorems about provability logic like the fixed point theorem.
Instead, once you set it up, you're just mechanically applying the rules of inference of provability logic.
In fact, since the logic is decidable, I should have been able to just plug it into a computer program.
But I don't know how to do that, so I did the proof on paper.

After working out the elementary proof, I was reviewing [the MIRI paper I linked earlier](https://arxiv.org/abs/1401.5577), and realized that to anyone who fully understood it, it may be obvious that the Payorian and Löbian FairBots are equivalent.
Note that both FairBots cooperate ($F(X)$) if and only if some sentence is provable, but only for the Payorian FairBot does that sentence include $F(X)$.
Theorem 4.6 of the paper shows that when an agent's cooperation condition references its own cooperation in this way, this reference can be eliminated.
That is, there's another equivalent condition without it, which I'll call the "simplification".

Theorem 4.6 proves a generality, whereas we're concerned with a particular case.
But if you follow the proof of the theorem, you can adapt it to a short demonstration that the Löbian and Payorian FairBots are equivalent.
You just have to show that the simplification of Payorian fairness, which the theorem asserts must exist, is Löbian fairness.
I'll call this the sophisticated, as opposed to elementary, proof.

I'll note that there is one insight that you only get from the elementary proof.
When proving that Payorian fairness implies Löbian fairness, we'll find that we don't need to use Löb's theorem.
So while in PA the conditions are equivalent, I suppose that in a weaker system, Payorian fairness is a stricter condition.

The rest of this post will be a detailed explanation of the elementary proof.

## The elementary proof

We will show the equivalence of the two fairness conditions using [the modal logic GL](https://plato.stanford.edu/entries/logic-provability/#AxioRule).

Our GL proof will be in terms of propositional variables $P$ and $Q$.
The intended interpretation of $P$ is that the FairBot cooperates, and $Q$ that its opponent cooperates.

Motivated by that interpretation, the GL sentence that I'll call "Löbian fairness" is:

$$P \leftrightarrow \Box Q$$

and the GL sentence that I'll call "Payorian fairness" is:

$$P \leftrightarrow \Box(\Box P \rightarrow Q)$$

We'll prove that one is provable if and only if the other is:

$$\Box(P \leftrightarrow \Box Q) \leftrightarrow \Box(P \leftrightarrow \Box(\Box P \rightarrow Q))$$

This is a theorem of GL for propositional variables $P$ and $Q$.
We can just forget, while proving it, that we intend to substitute in the weird circularly referential PA sentences $F(X)$ and $X(F)$.

### Arguments in GL and weaker modal logics

In this post, I'm going to notate valid arguments in GL like this:

$$\frac{\begin{gathered}\text{Premise 1}\\ \text{Premise 2}\end{gathered}}{\text{Conclusion}}$$

If you like, you can interpret that as saying that $(\text{Premise 1} \land \text{Premise 2}) \rightarrow \text{Conclusion}$ is a theorem of GL.

I'll note when a derivation only needs a weaker theory: either [K4](https://plato.stanford.edu/entries/logic-modal/#ModAxiConFra) or [K](https://plato.stanford.edu/entries/logic-modal/#ModLog).
Adding one axiom to K yields K4, and adding another axiom to K4 yields GL.
(Or you can define GL by adding one axiom to K, but that axiom implies K4's extra axiom.)
So when I say some derivation can be done in K or K4, it can also be done in GL.
I consider this interesting because GL's extra axiom is Löb's theorem, so if a derivation can be done in K4, that means we didn't need Löb's theorem.

### Outline of the elementary proof

Repeating the bi-implication we're going to prove, with Löbian fairness on the left and Payorian fairness on the right:

$$\Box(P \leftrightarrow \Box Q) \leftrightarrow \Box(P \leftrightarrow \Box(\Box P \rightarrow Q))$$

That is, we need to prove that the provability of each fairness condition implies the provability of the other.

For example, from left to right in the bi-implication, from Löbian fairness to Payorian fairness, would be this argument:

$$\frac{\Box(P \leftrightarrow \Box Q)}{\Box(P \leftrightarrow \Box(\Box P \rightarrow Q))}$$

But instead of making that argument directly, I'm going to use the fact that in K4 (and therefore GL), $\Box A \rightarrow \Box B$ (the form of the above argument) is a theorem if $(A \land \Box A) \rightarrow B$ is.
So the argument I'll actually make is this one:

$$\frac{\begin{gathered}P \leftrightarrow \Box Q\\ \Box(P \leftrightarrow \Box Q)\end{gathered}}{P \leftrightarrow \Box(\Box P \rightarrow Q)}$$

That feels more natural to me, because I can imagine that each line corresponds to a line in a valid PA argument.

We'll have to make two arguments like that, one for each direction between Löbian and Payorian fairness.
And the conclusion of each argument is itself a bi-implication, so that's four implications we need to prove.

Each of the four implications will get its own subsection of this post.
And for each implication, I'm going to divide its proof into what seem to me like the significant chunks.
I'll trust that a patient reader can fill in the proofs for the chunks, as well as use them to assemble the full proof required.

### A Löbian FairBot is Payorian-fair

We are assuming Löbian fairness, so we have available as premises

$$\begin{gather}
  P \leftrightarrow \Box Q\\
  \Box(P \leftrightarrow \Box Q)
\end{gather}$$

From these premises we will prove Payorian fairness:

$$P \leftrightarrow \Box(\Box P \rightarrow Q)$$

The following two subsections prove each of the two directions of this bi-implication.

In the chunks in this section, I won't directly use our two premises above.
Instead, for example, when arguing forward from $P$, I'll use $\Box Q$ as a premise, and you'll have to remember that one of our premises connects $P$ to $\Box Q$.
That's what I mean by trusting the reader to chain the chunks into a full proof.

#### Forward: from $P$ to $\Box (\Box P \rightarrow Q)$

This is the easy direction.
In K:

$$\frac{\begin{gathered}\Box Q\end{gathered}}{\Box(\Box P \rightarrow Q)}$$

#### Reverse: from $\Box (\Box P \rightarrow Q)$ to $P$

One intermediate statement we'll need can be proved in K4:

$$\frac{\begin{gathered}\Box(\Box Q \rightarrow P)\end{gathered}}{\Box(\Box Q \rightarrow \Box P)}$$

Note that the premise here is one direction of the bi-implication defining Löbian fairness.
It's boxed, but that's fine because we're assuming the boxed as well as the unboxed form of Löbian fairness.

Having proved that, we can use it to get from $\Box(\Box P \rightarrow Q)$ to $\Box Q$ in GL:

$$\frac{\begin{gathered}\Box(\Box Q \rightarrow \Box P)\\ \Box(\Box P \rightarrow Q)\end{gathered}}{\Box Q}$$

It works because once you chain the two implications you can apply Löb's theorem.

Then, remember, we're assuming the equivalence of $\Box Q$ and $P$, so we're done.

### A Payorian FairBot is Löbian-fair

This time, we are assuming Payorian fairness, so as premises we have

$$\begin{gather}
  P \leftrightarrow \Box(\Box P \rightarrow Q)\\
  \Box (P \leftrightarrow \Box(\Box P \rightarrow Q))
\end{gather}$$

We will separately prove the two directions in the statement of Löbian fairness:

$$P \leftrightarrow \Box Q$$

Starting with the reverse direction this time, since it's trivial.

#### Reverse: from $\Box Q$ to $P$

Just this K derivation again:

$$\frac{\begin{gathered}\Box Q\end{gathered}}{\Box(\Box P \rightarrow Q)}$$

#### Forward: from $P$ to $\Box Q$

For this direction, we will need not just $P$ but $\Box P$.
But $P$ implies $\Box P$, because $P$ is an assertion of provability: our premises tell us it is equivalent (and provably equivalent) to some boxed sentence.
In K4, such assertions imply their own provability.
Spelling that out as a chunk:

$$\frac{\begin{gathered}
P \leftrightarrow \Box(\Box P \rightarrow Q)\\
\Box(P \leftrightarrow \Box(\Box P \rightarrow Q))\\
P
\end{gathered}}{\Box P}$$

I just want to emphasize that this chunk is a lot simpler than it looks, because it doesn't actually matter what's behind the box.

From there, we can do this derivation in K4:

$$\frac{\begin{gathered}\Box P\\ \Box(\Box P \rightarrow Q)\end{gathered}}{\Box Q}$$

Notice that we only needed K4 here, not GL.
That's what I meant earlier when I said that proving a Payorian FairBot is Löbian-fair doesn't require Löb's theorem.
