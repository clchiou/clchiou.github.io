---
layout: post
title: Quick Notes on Poker Solver
---

[Science](http://www.sciencemag.org/content/347/6218/145)
published a review of poker solver algorithm.

Researchers focus on **two-player limit Texas Hold'em.**
Their grand scheme is this:

1. Transform a poker game to an abstract game that has a smaller state space.
2. Apply an equilibrium solver algorithm on the abstract game.
3. Map the equilibrium back to the original game.

There are lossy and lossless transformations.
A finer-grained lossy transformation is usually **but not always**
better than a coarse-grained lossy transformation.

There are two popular families of equilibrium solver:

* General purpose linear programming solver.
* Counterfactual regret (CFR) minimization.

I am not sure what kind of equilibrium they are looking for.
Subgame-perfect equilibrium?
Sequential equilibrium?
Or just simple Nash equilibrium?
