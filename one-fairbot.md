# My "Payorian FairBot" was just the original FairBot

[The modal agent prisoner's dilemma tournament](https://arxiv.org/abs/1401.5577) defined "modal agents" encoded as formulas of Peano arithmetic (PA) with one free variable. 
$X(Y)$ means the agent $X$ cooperates in a match against the agent $Y$, and is constructed by plugging in the Gödel number of the formula defining $Y$ as the free variable in the formula defining $X$.

The simplest interesting agent in the tournament was called FairBot.
Using $F(Y)$ to mean FairBot cooperates with another agent $Y$, it satisfies:
$$\mathrm{PA} \vdash F(Y) \leftrightarrow \Box Y(F)$$
I will call this condition Löbian fairness, and refer to this agent as the Löbian FairBot.

That's because in [a previous post](https://www.lesswrong.com/posts/LaCP6WyNzX8kiZn3w/payorian-cooperation-is-easy-with-kripke-frames), I defined an alternative "Payorian FairBot", which satisfies a condition I'll call Payorian fairness:
$$\mathrm{PA} \vdash F(Y) \leftrightarrow \Box(\Box F(Y) \to Y(F))$$

I wondered, though, is this really a distinct agent?
The answer is no: these two fairness conditions are equivalent.
The Payorian FairBot is Löbian-fair, and the original Löbian FairBot is Payorian-fair.

## Elementary and sophisticated proofs

One way to prove the equivalence of these two fairness conditions is to simply grind through provability logic in both directions.
In this post I'll give this elementary proof.

After working out the elementary proof, I was reviewing the original modal combat paper, and realized that to anyone that fully understood it, it may be obvious that the Payorian and Löbian FairBots are equivalent.
Note that both FairBots cooperate ($F(X)$) if and only if some sentence is provable, but only for the Payorian FairBot does that sentence include $F(X)$.
Theorem 4.6 of the paper shows that you can eliminate this kind of self-reference from the definition.
When you apply this procedure to Payorian fairness, what you get is Löbian fairness.

The sophisticated proof along the lines of theorem 4.6 is quite short, but trying to give the background felt like writing another post, so I'm leaving it out.

I'll note one insight that you only get from the elementary proof: proving that Payorian fairness implies Löbian fairness doesn't actually requires Löb's theorem.
So while the conditions are equivalent in PA, Payorian fairness is in another sense stricter.

## The elementary proof

These fairness conditions must hold in PA, but we will show their equivalence using [the modal logic GL](https://plato.stanford.edu/entries/logic-provability/#AxioRule), using the [arithmetical adequacy of GL](https://www.lesswrong.com/w/provability-logic#Arithmetical_adequacy).

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
But I'll note when it only needs a weaker theory, which will be [K4](https://plato.stanford.edu/entries/logic-modal/#ModAxiConFra) or [K](https://plato.stanford.edu/entries/logic-modal/#ModLog).

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

## Further questions

### Software

Working out the elementary proof was fun, and I do think there's some value to putting it in public, but surely it's not really necessary to do these kinds of proofs in the computer age?
But I don't personally know how to treat provability logic as a routine calculation, even though it's decidable.
If you know how to decide it on your actual computer, may I suggest the question in this post as the subject of a tutorial for whatever software you use?

But perhaps the sophisticated proof leads to a more interesting computer program.
Does anyone know how to write a program that simplifies bots in the way given in Theorem 4.6?
I'm imagining something where you input $F(X) \leftrightarrow \Box (\Box F(X) \rightarrow X(F))$ and the output is $F(X) \leftrightarrow \Box X(F)$.
But I can't think of any other interesting examples to try it on.

### Is there only one FairBot?

I was too lazy to write up the sophisticated proof along the lines of theorem 4.6.
But also, it felt like a waste to explain high-powered concepts like uniqueness of arithmetic fixed points just to answer an elementary question.
If someone else wants to write about it, may I suggest that you do a little more with these very general theorems than just specialize them to this case?
How about a more general condition for equivalence to FairBot?
Maybe a strengthening of theorem 4.10?
