---
title: "Probability spaces and the evolution of sigma-algebras"
date: 2026-11-02
category: Statistics and Data Science
layout: post
---

Usually, probability spaces are exemplified via throwing dice or flipping coins. A typical textbook example looks like this:

Consider three consecutive independent tosses of a fair coin. The sample space is defined explicitly as the set of all eight possible sequence outcomes:

$$\Omega = \lbrace H, T \rbrace^3 = \lbrace HHH, HHT, HTH, HTT, THH, THT, TTH, TTT \rbrace.$$

The $\sigma$-algebra is taken to be the full power set $\mathcal{F} = \mathcal{P}(\Omega)$, and the probability measure assigns equal weight $P(\lbrace\omega\rbrace) = 1/8$ to each elementary outcome $\omega \in \Omega$. In this setting, the random variables we define—such as $X_1$, the indicator of whether the first toss was Heads—are little more than coordinate projections $\pi_1: \lbrace H, T \rbrace^3 \to \lbrace 0, 1 \rbrace$.

However, this pedagogical framing has always seemed odd to me. Random variables (which are the objects we actually use in practice) become trivial identity or projection functions. Worse, as soon as one wants to discuss a slightly richer experiment—say, recording the ambient room temperature during the second flip, or accounting for a gust of wind—the original space $\Omega$ must be discarded and rebuilt as a larger product space.

## The "Grand Sample Space" View

An alternative that has always seemed far more intuitive to me (and one with deep roots in mathematical probability and decision theory) is to imagine a single, very large probability space $(\Omega, \mathcal{F}, P)$ where each $\omega \in \Omega$ represents a complete "possible world" or state of nature (echoing Leonard Savage's states of the world; [Savage, 1954](#savage1954)).

Ours is one specific world $\omega^\ast \in \Omega$, but we do not know which one. Grouping these possible worlds into measurable subsets according to the axioms of a $\sigma$-algebra sets up our universe of discourse, allowing us to quantify the propensity (or our epistemic certainty) of events by assigning probabilities to them. In this formulation, random variables $X: \Omega \to S$ are not identity functions; they are measurement instruments or evaluation maps that inspect an attribute of the world.

Kolmogorov's Extension Theorem ([Kolmogorov, 1933](#kolmogorov1933)) provides the mathematical backbone for this view: it guarantees that we can construct consistent projective limit spaces (such as $\mathbb{R}^\infty$ or $\mathbb{R}^{[0, \infty)}$) representing trajectories across all time without having to assemble ad-hoc product spaces on the fly.

The practical upside is immediate: in statistical practice, $(\Omega, \mathcal{F}, P)$ remains a background object. We rarely construct $\Omega$ explicitly; instead, we work with pushforward measures on the observation spaces. Yet having $\Omega$ latent in the background provides the essential foundational guarantee that all random variables live on a shared domain and can be coupled.

## The Pitfall of the Naive Conception

One might argue that the toy coin-toss construction is merely an educational stepping stone. But it introduces two notable issues:

1. By illustrating probability spaces with an idiosyncratic, single-purpose construction, the initial simplicity is offset by confusion down the line when probability spaces are relegated to the background.
2. Even mathematically literate practitioners can carry this toy intuition forward in misleading ways (for example, see [this discussion](https://www.youtube.com/watch?v=VIy_-mxBQ14)). To be clear, the video is solid in my opinion. The specific point I'm making regards the naive picture of probability spaces.

This view often leads to the idea that as time progresses or as new phenomena are observed, our "universal" sample space $\Omega$ is physically expanding, and consequently its $\sigma$-algebra must grow along with it.

## The Dual Nature of "Evolving" Information

In standard probability theory, information *does* involve an increasing sequence of $\sigma$-algebras—specifically, a filtration $\mathcal{F}_0 \subseteq \mathcal{F}_1 \subseteq \dots \subseteq \mathcal{F}$. However, this does not mean the sample space $\Omega$ expands. The apparent paradox between "information expanding" and "possibilities shrinking" is cleanly resolved by distinguishing between **measurability** and **conditioning**:

* **Filtrations (Expanding Resolving Power):** A sub-$\sigma$-algebra $\mathcal{F}_t = \sigma(X_1, \dots, X_t)$ models the collection of *questions that are answerable* at time $t$. At $t=0$, we can only evaluate trivial events ($\lbrace\emptyset, \Omega\rbrace$). As we observe random variables, the partition of $\Omega$ becomes finer. More subsets become discernible and measurable, which is why $\mathcal{F}_t$ expands as an algebraic structure.
* **Conditioning (Restricting Possibilities):** Once a specific observation $X_1 = x_1$ is realized, the set of *surviving possible worlds* shrinks to the fiber $A = \lbrace\omega \in \Omega : X_1(\omega) = x_1\rbrace$. Operationally, this corresponds to restricting $\Omega$ to $A$, passing to the **trace $\sigma$-algebra** $\mathcal{F}\vert_A = \lbrace E \cap A : E \in \mathcal{F} \rbrace$, and updating the measure via conditional probability $P(\cdot \mid A)$.

Thus, the sample space does not evolve unnaturally. Gaining information means expanding our algebraic capacity to distinguish events, while simultaneously restricting the set of plausible worlds by conditioning out counterfactuals.

## Epistemic Probability and the Past

This perspective also clarifies why it is entirely coherent to assign non-trivial probabilities (strictly between 0 and 1) to past, unknown events. 

In the "Grand Sample Space" conception, the true state of nature $\omega^\ast$ is fixed from the start. Whether an event $A$ occurred in the past or will occur in the future simply depends on whether $\omega^\ast \in A$. If probability were viewed purely as physical propensity governing unresolved futures, past events would trivially collapse to 0 or 1 ("it either happened or it didn't"). 

However, because $P$ quantifies our epistemic uncertainty regarding where $\omega^\ast$ lies, conditioning on present data leaves open the conditional probability of unobserved historical events. A practical situation where this is indispensable is **backward time-series forecasting (backcasting)** and historical smoothing (such as reconstructing historical climate temperatures before modern sensor records). If unknown past events could only take probabilities of 0 or 1, the entire machinery of prediction intervals and distributional forecasts for historical data would collapse into incoherence.

## Conclusion

Viewing probability spaces through the lens of general states of nature reconciles mathematical rigor with statistical intuition. It grounds random variables as meaningful queries on a latent world, explains how information restricts possibility spaces without mutating the underlying canvas, and provides a unified foundation for uncertainty about both the future and the past.

## References

<!--book, one author-->
<a id="kolmogorov1933">Kolmogorov, A. N. (1933).</a>  
*Grundbegriffe der Wahrscheinlichkeitsrechnung.*  
Springer. (English translation: *Foundations of the Theory of Probability*, Chelsea Publishing Company, 1950).

<!--book, one author-->
<a id="savage1954">Savage, L. J. (1954).</a>  
*The Foundations of Statistics.*  
John Wiley & Sons.
