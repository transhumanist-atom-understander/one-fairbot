The modal agent prisoner's dilemma tournament defined "modal agents" encoded as formulas of Peano arithmetic (PA) with one free variable. 
$X(Y)$ means the agent $X$ cooperates in a match against the agent $Y$, and is constructed by plugging in the Gödel number of the formula defining $Y$ as the free variable in the formula defining $X$.

The simplest interesting agent in the tournament was called FairBot.
Using $F(Y)$ to mean FairBot cooperates with another agent $Y$, it satisfies:
$$\mathrm{PA} \vdash F(Y) \leftrightarrow \Box Y(F)$$
I will call this condition Löbian fairness, and refer to this agent as the Löbian FairBot.

That's because in a previous post, I defined an alternative "Payorian FairBot", which satisfies Payorian fairness:
$$\mathrm{PA} \vdash F(Y) \leftrightarrow \Box(\Box F(Y) \to Y(F))$$

I wondered, though, is this really a distinct agent?
The answer is no: these two fairness conditions are equivalent.
The Payorian FairBot is Löbian-fair, and the original Löbian FairBot is Payorian-fair.

## The elementary and deep proofs

One way to prove the equivalence of these two fairness conditions is to simply grind through provability logic in both directions.
That's how I originally convinced myself of the equivalence.
In the next section I'll give this proof, which I call the elementary proof.

Working out the elementary proof was fun but surely we don't need to post proofs like that in the computer age?
As it happens, though, I don't personally know how to treat GL as a routine calculation, even though it's decidable.
If you know how to decide GL on your actual computer, may I suggest the question in this post as the subject of a tutorial for whatever software you use?

Following the elementary proof, I'll try and give a proof using the fixed point theorem.

Although using the fixed point theorem works, it feels weird to use it on a question that can be settled by elementary means.
It might be more appropriate to prove some general condition for equivalence to FairBot, perhaps a strengthening of Theorem 4.10.
But I'll leave that to the sophisticated reader.

## Tedious elementary proof

These fairness conditions must hold in PA, but we will show their equivalence using the modal logic GL, using the [arithmetical adequacy of GL](https://www.lesswrong.com/w/provability-logic#Arithmetical_adequacy).

The proposition $P$ will stand for $F(Y)$, that the FairBot cooperates.
The proposition $Q$ will stand for $Y(F)$, that its opponent cooperates.

We will forget for the moment that $P$ and $Q$ stand for these weird self-referential PA formulas, and instead just treat them like any other proposition.
Everything we need to do, we can do with GL derivations that work for arbitrary $P$ and $Q$.

What we will prove in GL is the equivalence of Löbian and Payorian fairness.
We need to prove they imply each other.
And when proving one implies the other, we're trying to derive a bi-implication, so we need to prove each direction.
That's four implications we need to prove.

Each of the four implications will get its own subsection of this post.
And for each implication, I'm going to divide its proof into what seem to me like the significant chunks.
For each chunk, I'll just write something like: in theory T,

$$\frac{\begin{gathered}\text{Premise 1}\\ \text{Premise 2}\end{gathered}}{\text{Conclusion}}$$

The theory may be GL, since ultimately all these implications must go through in GL.
But I'll note when it only needs a weaker theory, which will be K4 or K.

I'll trust that a patient reader can fill in the proofs for the chunks, and use them to assemble the full proof required.

### A Löbian FairBot is Payorian-fair

We are assuming Löbian fairness, so we have available as premises

$$\begin{gather*}
  P \leftrightarrow \Box Q\\
  \Box(P \leftrightarrow \Box Q)
\end{gather*}$$

I'll actually never use these bi-implications directly in the chunks I give.
But, for example, when we're reasoning from $P$ I may start a chunk with $\Box Q$ as a premise.
The bi-implication is required to chain those chunks into a full proof.

From these premises we will prove Payorian fairness:

$$P \leftrightarrow \Box(\Box P \to Q)$$

We're proving a bi-implication, so we'll do the forward and reverse directions separately.

#### Forward: from $P$ to $\Box (\Box P \rightarrow Q)$

This is the easy direction.
In K:

$$\frac{\begin{gathered}\Box Q\end{gathered}}{\Box(\Box P \to Q)}$$

#### Reverse: from $\Box (\Box P \rightarrow Q)$ to $P$

One intermediate statement we'll need can be proved in K4:

$$\frac{\begin{gathered}\Box(\Box Q \to P)\end{gathered}}{\Box(\Box Q \to \Box P)}$$

Note that the premise here is one direction of the bi-implication defining Löbian fairness.

With that, we can prove $\Box Q$ in GL:

$$\frac{\begin{gathered}\Box(\Box Q \to \Box P)\\ \Box(\Box P \to Q)\end{gathered}}{\Box Q}$$

It works because once you chain the two implications you can apply Löb's theorem.

### A Payorian FairBot is Löbian-fair

This time, we are assuming Payorian fairness, so as premises we have

$$P \leftrightarrow \Box(\Box P \to Q)$$
$$\Box (P \leftrightarrow \Box(\Box P \to Q))$$

We will separately prove the two directions in the statement of Löbian fairness:

$$P \leftrightarrow \Box Q$$

#### Forward: from $P$ to $\Box Q$

In K4, assertions of provability imply their own provability.
That is, if $A \leftrightarrow \Box B$, then $A \to \Box A$.

$P$ is an assertion that a certain sentence is provable.
So, let's start by noting that from $P$, we have $\Box P$.

From there, we can do this derivation in K4:

$$\frac{\begin{gathered}\Box P \\ \Box(\Box P \to Q)\end{gathered}}{\Box Q}$$

#### Reverse: from $\Box Q$ to $P$

Just this K derivation again:
$$\frac{\begin{gathered}\Box Q\end{gathered}}{\Box(\Box P \to Q)}$$

## Proof using uniqueness of fixed points

I'll start with a suggestive observation.
Start with the Payorian fairness condition, but with the cooperation of the agents replaced with the symbols $P$ and $Q$:

$$P \leftrightarrow \Box(\Box P \to Q)$$

Think of this as an equation.

Now, looking at the Löbian fairness condition:

$$P \leftrightarrow \Box Q$$

Think of this as a proposed solution to the equation.

Now, plug it in.
That is, replace $P$ in the Payorian fairness condition with $\Box Q$.
You get:

$$\Box Q \leftrightarrow \Box(\Box \Box Q \to Q)$$

This is in fact a theorem of GL.
So a Löbian FairBot "solves" the Payorian fairness "equation".

I guess that could replace at least half of my elementary proof.
But can't we avoid the whole thing?

It seems like there should be a one-line proof of the equivalence of Löbian and Payorian FairBots using uniqueness of modal fixed points.
But the fixed point theorem as I know it requires a modalized formula, which is not what I have with the $P$'s and $Q$'s.
So I guess we need to actually use the structure of $F(Y)$ and $Y(F)$.
