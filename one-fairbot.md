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
In this post I'll give this elementary proof.

After working out the elementary proof, I was reviewing the original modal combat paper, and realized that to anyone who fully understood it, it may be obvious that the Payorian and Löbian FairBots are equivalent.
Note that both FairBots cooperate ($F(X)$) if and only if some sentence is provable, but only for the Payorian FairBot does that sentence include $F(X)$.
Theorem 4.6 of the paper shows that you can eliminate this kind of self-reference from the condition.
When you apply this procedure to Payorian fairness, what you get is Löbian fairness.
You can prove equivalence of the Payorian and Löbian FairBots this way, in what I'll call a sophisticated (as opposed to elementary) proof.
The sophisticated proof is quite short, but trying to give the background felt like writing another post, so I'm leaving it out.

There's a distinction here something like the distinction between arithmetic and number theory.
One might learn to do long multiplication in elementary school, and not learn until college how to prove that all numbers have a unique prime factorization.
The elementary proof I'll give feels like something I could have learned to do in another universe's elementary school.
The analogy is strengthened because GL is decidable, so doing it on paper was like doing long multiplication instead of using a calculator.
Proving theorem 4.6, on the other hand, is college-level.

I'll note one insight that you only get from the elementary proof: proving that Payorian fairness implies Löbian fairness doesn't actually require Löb's theorem.
So while in PA the conditions are equivalent, Payorian fairness is in another sense stricter.

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

### Outline of the elementary proof

In this post, I'm going to notate valid arguments in GL like this:

$$\frac{\begin{gathered}\text{Premise 1}\\ \text{Premise 2}\end{gathered}}{\text{Conclusion}}$$

If you like, you can interpret that as saying that $(\text{Premise 1} \land \text{Premise 2}) \rightarrow \text{Conclusion}$ is a theorem of GL.

I'll note when a derivation only needs a weaker theory: either [K4](https://plato.stanford.edu/entries/logic-modal/#ModAxiConFra) or [K](https://plato.stanford.edu/entries/logic-modal/#ModLog).

We need to prove that the provability of each fairness condition implies the provability of the other.
So, for example, from left to right in the bi-implication I wrote earlier, from Löbian fairness to Payorian fairness, would be this argument:

$$\frac{\Box(P \leftrightarrow \Box Q)}{\Box(P \leftrightarrow \Box(\Box P \rightarrow Q))}$$

But instead of making that argument directly, I'm going to use the fact that in K4, $\Box A \rightarrow \Box B$ (the form of the above argument) is entailed by $(A \land \Box A) \rightarrow B$.
So the argument I'll actually make is this one:

$$\frac{\begin{gathered}P \leftrightarrow \Box Q\\ \Box(P \leftrightarrow \Box Q)\end{gathered}}{P \leftrightarrow \Box(\Box P \rightarrow Q)}$$

That feels more natural to me, because I can imagine each line corresponds to a line in a valid PA argument.

We'll have to make two arguments like that, one for each direction between Löbian and Payorian fairness.
And the conclusion of each argument is itself a bi-implication, so that's four implications we need to prove.

Each of the four implications will get its own subsection of this post.
And for each implication, I'm going to divide its proof into what seem to me like the significant chunks.
I'll trust that a patient reader can fill in the proofs for the chunks, as well as use them to assemble the full proof required.

### A Löbian FairBot is Payorian-fair

We are assuming Löbian fairness, so we have available as premises

$$\begin{gather*}
  P \leftrightarrow \Box Q\\
  \Box(P \leftrightarrow \Box Q)
\end{gather*}$$

From these premises we will prove Payorian fairness:

$$P \leftrightarrow \Box(\Box P \rightarrow Q)$$

The following two subsections prove each of the two directions of this bi-implication.

I'll never use our two premises above directly as a premise in a chunk.
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

With that, we can prove $\Box Q$ in GL:

$$\frac{\begin{gathered}\Box(\Box Q \rightarrow \Box P)\\ \Box(\Box P \rightarrow Q)\end{gathered}}{\Box Q}$$

It works because once you chain the two implications you can apply Löb's theorem.

### A Payorian FairBot is Löbian-fair

This time, we are assuming Payorian fairness, so as premises we have

$$P \leftrightarrow \Box(\Box P \rightarrow Q)$$
$$\Box (P \leftrightarrow \Box(\Box P \rightarrow Q))$$

We will separately prove the two directions in the statement of Löbian fairness:

$$P \leftrightarrow \Box Q$$

#### Forward: from $P$ to $\Box Q$

In K4, assertions of provability imply their own provability.
That is, if $A \leftrightarrow \Box B$, then $A \rightarrow \Box A$.

$P$ is of this form, asserting that $\Box P \rightarrow Q$ is provable.
So, let's start by noting that from $P$, we have $\Box P$.

From there, we can do this derivation in K4:

$$\frac{\begin{gathered}\Box P \\ \Box(\Box P \rightarrow Q)\end{gathered}}{\Box Q}$$

#### Reverse: from $\Box Q$ to $P$

Just this K derivation again:

$$\frac{\begin{gathered}\Box Q\end{gathered}}{\Box(\Box P \rightarrow Q)}$$
