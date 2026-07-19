# My "Payorian FairBot" was just the original FairBot

[MIRI's proof-based prisoner's dilemma tournament](https://arxiv.org/abs/1401.5577) defined agents encoded as formulas of Peano arithmetic (PA) with one free variable.
$A(X)$ means the agent $A$ cooperates in a match against the agent $X$, and is constructed by plugging the Gödel number of the formula defining $X$ into the formula defining $A$.

The simplest interesting agent in the tournament was called FairBot.
Using $F(X)$ to mean FairBot cooperates with another agent $X$, for all agents $X$ we have:
$$\mathrm{PA} \vdash F(X) \leftrightarrow \Box X(F)$$
I will call this condition Löbian fairness, and refer to this agent as the Löbian FairBot.

In [a previous post](https://www.lesswrong.com/posts/LaCP6WyNzX8kiZn3w/payorian-cooperation-is-easy-with-kripke-frames) I defined an alternative "Payorian FairBot", which satisfies a condition I'll call Payorian fairness:
$$\mathrm{PA} \vdash F(X) \leftrightarrow \Box(\Box F(X) \rightarrow X(F))$$

I wondered, though, is this really a distinct agent?
The answer is no: these two fairness conditions are equivalent.
The Payorian FairBot is Löbian-fair, and the original Löbian FairBot is Payorian-fair.

## Elementary and sophisticated proofs

One way to prove the equivalence of these two fairness conditions is to simply grind through provability logic in both directions.
I'll call this the "elementary proof", since it doesn't use any of the theorems about provability logic beyond the one that says it applies to PA.
The bulk of the proof is just mechanically applying the rules of inference of provability logic.
In fact, since the logic is decidable, I should have been able to just plug the question into a computer program.
But I don't know how to do that, so I did the proof on paper.

After working out the elementary proof, I was reviewing [the MIRI paper I linked earlier](https://arxiv.org/abs/1401.5577), and realized that to anyone who fully understood it, it might be obvious that the Payorian and Löbian FairBots are equivalent.
Note that both FairBots cooperate ($F(X)$) if and only if some sentence is provable, but only for the Payorian FairBot does that sentence include $F(X)$.
Theorem 4.6 of the paper shows that when an agent's cooperation condition references its own cooperation in this way, this reference can be eliminated.
That is, there's another equivalent condition without it, which I'll call the "simplification".

If you follow the proof of theorem 4.6, you can adapt it to a proof that the Löbian and Payorian FairBots are equivalent.
The theorem implies that there exists a simplification of Payorian fairness, but instead, you want to show that Löbian fairness is the simplification.
Once the background is in place, this more sophisticated proof requires much less work in provability logic than the elementary proof.

But there is one insight that you only get from the elementary proof.
When proving that Payorian fairness implies Löbian fairness, you find that you don't need to use Löb's theorem.
So while in PA the conditions are equivalent, I suppose that in a weaker system, Payorian fairness is a stricter condition.

The rest of this post will be a detailed explanation of the elementary proof.
Not really because of that additional insight, and not just because I had already written most of it by the time I understood the sophisticated proof.
But also because this kind of elementary reasoning is my main tool for thinking through these cooperation problems, and I suspect others do a lot of it behind the scenes, even though they don't put it in the final writeup—understandably, as you'll see.

## The elementary proof

We will show the equivalence of the two fairness conditions using [the modal logic GL](https://plato.stanford.edu/entries/logic-provability/#AxioRule).

In this post, I'm going to notate valid arguments in GL by listing the premises and the conclusion separated by a horizontal bar:

$$\frac{\begin{gathered}
\text{Premise 1}\\
\text{Premise 2}\end{gathered}}
{\text{Conclusion}}$$

This means that $(\text{Premise 1} \land \text{Premise 2}) \rightarrow \text{Conclusion}$ is a theorem of GL.

I'll note when a derivation only needs a weaker theory: either [K4](https://plato.stanford.edu/entries/logic-modal/#ModAxiConFra) or [K](https://plato.stanford.edu/entries/logic-modal/#ModLog).
Adding one axiom to K yields K4, and adding another axiom to K4 yields GL.
So when I say some derivation can be done in K or K4, it can also be done in GL.
I consider this interesting because GL's extra axiom is Löb's theorem, so if a derivation can be done in K4, that means we didn't need Löb's theorem.

### The fairness conditions in GL

Our GL proof will be in terms of propositional variables $P$ and $Q$.
The intended interpretation of $P$ is that the FairBot cooperates, and $Q$ that its opponent cooperates.

Motivated by that interpretation, the GL sentence that I'll call "Löbian fairness" is:

$$P \leftrightarrow \Box Q$$

and the GL sentence that I'll call "Payorian fairness" is:

$$P \leftrightarrow \Box(\Box P \rightarrow Q)$$

But despite the intended interpretation, $P$ and $Q$ are just propositional variables, and we can forget, while we're doing the proof, that we ultimately want to talk about the weird circularly-referential PA sentences $F(X)$ and $X(F)$.

From here, you may expect that I will make GL arguments with one of these fairness conditions as a premise and the other as a conclusion.
Actually, our GL arguments will have two premises: one raw, one boxed.
For example, when arguing that a Löbian FairBot is Payorian-fair, the GL argument will have as premises both $P \leftrightarrow \Box Q$ and $\Box(P \leftrightarrow \Box Q)$.
I'll explain why we can get away with that extra boxed premise in the next section.

### How the GL proofs we'll do translate to PA

The real fairness conditions at the top of my post are defined in terms of theorems of PA, about $F(X)$ and $X(F)$.
How will our arguments in GL, about "fairness conditions" that are GL sentences about propositional variables $P$ and $Q$, help us prove what we want about PA?

We'll need one basic theorem about the relationship between GL and PA.
It says that when you take a theorem of GL and substitute in sentences of PA for the propositional variables, you get a theorem of PA.
This is theorem 4.1 in the MIRI paper, arithmetical soundness.

Here's how we'll use GL to prove that the Löbian fairness condition implies the Payorian fairness condition, as I originally defined them in terms of PA.
(The other direction, from Payorian to Löbian, will work the same way.)

Suppose that we have a Löbian FairBot $F$.
That means we have a certain theorem of PA for each opponent.
Consider that theorem for some particular opponent $X$:
$$\mathrm{PA} \vdash F(X) \leftrightarrow \Box X(F)$$

Since this is a theorem, by definition it has a proof.
Therefore there's another theorem of PA which asserts that it has a proof:
$$\mathrm{PA} \vdash \Box (F(X) \leftrightarrow \Box X(F))$$
So we have as theorems of PA not just the unboxed form but consequently also the boxed form of this sentence.

This is the argument we'll make in GL:

$$\frac{\begin{gathered}
P \leftrightarrow \Box Q\\
\Box(P \leftrightarrow \Box Q)\end{gathered}}
{P \leftrightarrow \Box(\Box P \rightarrow Q)}$$

Remember, this means we'll prove a theorem of GL: a material implication from premises to conclusion.

Take that GL implication and substitute in $F(X)$ for $P$ and $X(F)$ for $Q$, and you get an implication in PA:
$$\mathrm{PA} \vdash (F(X) \leftrightarrow \Box X(F)) \land \Box (F(X) \leftrightarrow \Box X(F)) \rightarrow (F(X) \leftrightarrow \Box(\Box F(X) \rightarrow X(F)))$$
Then, by modus ponens, we have:
$$\mathrm{PA} \vdash F(X) \leftrightarrow \Box(\Box F(X) \rightarrow X(F))$$

This all works for arbitrary $X$, so we have that $F$ is Payorian-fair.

And that's the last reasoning _about_ GL we're going to have to do.
The rest of this post will be reasoning _in_ GL.

### Outline of the elementary proof

We'll also, of course, have to prove the other direction, from Payorian to Löbian fairness:

$$\frac{\begin{gathered}
P \leftrightarrow \Box(\Box P \rightarrow Q)\\
\Box (P \leftrightarrow \Box(\Box P \rightarrow Q))\end{gathered}}
{P \leftrightarrow \Box Q}$$

And since the conclusion of each of these two arguments is itself a bi-implication, that's four implications we need to prove.
Each of the four implications will get its own subsection of this post.

For each implication, I'm going to divide its proof into what seem to me like the significant chunks.
I'll try to make it clear how these chunks can be assembled into a full proof.
And I'm sure that a patient reader can fill in their own proofs for each chunk.

### A Löbian FairBot is Payorian-fair

We are assuming Löbian fairness, so we have available as premises

$$\begin{gather}
  P \leftrightarrow \Box Q\\
  \Box(P \leftrightarrow \Box Q)
\end{gather}$$

In the chunks in this section, I won't directly use our two premises above.
Instead, for example, when arguing forward from $P$, I'll use $\Box Q$ as a premise, and you'll have to remember that one of our premises connects $P$ to $\Box Q$.

From these premises we will prove Payorian fairness:

$$P \leftrightarrow \Box(\Box P \rightarrow Q)$$

The following two subsections prove each of the two directions of this bi-implication.


#### Forward: from $P$ to $\Box (\Box P \rightarrow Q)$

This is the easy direction.

From $P$ we have $\Box Q$, and in K:

$$\frac{\begin{gathered}
\Box Q\end{gathered}}
{\Box(\Box P \rightarrow Q)}$$

In this subsection, we assumed a Löbian FairBot, and proved that if it cooperates ($P$), then the Payorian cooperation condition holds.

#### Reverse: from $\Box (\Box P \rightarrow Q)$ to $P$

One intermediate statement we'll need can be proved in K4:

$$\frac{\begin{gathered}
\Box(\Box Q \rightarrow P)\end{gathered}}
{\Box(\Box Q \rightarrow \Box P)}$$

Note that the premise here is one direction of the bi-implication defining Löbian fairness.
It's boxed, but that's fine because we're assuming the boxed as well as the unboxed form of Löbian fairness.

Having proved that, we can use it to get from $\Box(\Box P \rightarrow Q)$ to $\Box Q$ in GL:

$$\frac{\begin{gathered}
\Box(\Box Q \rightarrow \Box P)\\
\Box(\Box P \rightarrow Q)\end{gathered}}
{\Box Q}$$

It works because once you chain the two implications you can apply Löb's theorem.

Then, remember, we're assuming the equivalence of $\Box Q$ and $P$, so we're done.

In this subsection, we've proved that the Payorian cooperation condition is sufficient for a Löbian FairBot to cooperate.

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

All that's different is that this time it's the conclusion that's equivalent to $P$, rather than the premise.

In this subsection, we've proved that a Payorian FairBot cooperates if there's a proof that its opponent cooperates.

#### Forward: from $P$ to $\Box Q$

For this direction, we will need not just $P$ but $\Box P$.
But $P$ implies $\Box P$, because $P$ is an assertion of provability: our premises tell us it is equivalent, and provably equivalent, to some boxed sentence.
In K4, such assertions imply their own provability.
Spelling that out as a chunk:

$$\frac{\begin{gathered}
P \leftrightarrow \Box(\Box P \rightarrow Q)\\
\Box(P \leftrightarrow \Box(\Box P \rightarrow Q))\\
P
\end{gathered}}{\Box P}$$

I just want to emphasize that this chunk is a lot simpler than it looks, because it doesn't actually matter what's behind the box.

From there, we can do this derivation in K4, using two premises that both follow from $P$:

$$\frac{\begin{gathered}
\Box P\\
\Box(\Box P \rightarrow Q)\end{gathered}}
{\Box Q}$$

In this subsection we've proved that the Payorian FairBot cooperating implies a proof that its opponent cooperates.

Notice that we only needed K4 here, not GL.
That's what I meant earlier when I said that proving a Payorian FairBot is Löbian-fair doesn't require Löb's theorem.

