---
layout: default
title: "Efficient Discrete Inference with Knowledge Compilation"
date: "2026-03-18"
tags: ["probabilistic programming", "discrete inference", "knowledge compilation"]
---

# Discrete Probabilistic Programs

Many existing probabilistic programming languages such as Stan, Pyro, and Gen allow us to write expressive programs with a wide range of discrete and continuous distributions, containing complex logic that combines latent variables and allowing for models with stochastic support. This expressivity does come at a price: in most cases we can only hope to perform *approximate* inference, using something like MCMC or variational inference. Exactly computing (and representing) the posterior distribution becomes quickly intractable once you allow non-conjugate priors or arbitrary interactions between latent variables.

Although there are many restricted classes of models and PPLs where exact inference is possible (affine combinations of Gaussians, sum-product networks, to name two), one natural family of models are those involving only *discrete* parameters. In fact, we can simplify even further and consider only parameters taking boolean values, since more complex categorical distributions can instead be constructed using multiple boolean parameters (e.g. by considering the joint distribution on `(b1, b2)` which has 4 different possible values).

We can write a simple model that flips three coins, combines the results, and observes that the resulting statement is true:

$$
\begin{aligned}
b_1 &\sim \text{Bernoulli}(0.5) \\
b_2 &\sim \text{Bernoulli}(0.2) \\
b_3 &\sim \text{Bernoulli}(0.1) \\
\text{result} &= (b_1 \land b_2) \lor (b_1 \land \neg(b_2)) \\
\\
P(b_1 \mid \text{result} = \text{TRUE}) &= ?
\end{aligned}
$$

Before we go further, let's define a simple functional language that allows us to express probabilistic models with discrete parameters. It has the following terms:
- `Let(expr, name, body)`: Let us assign the expression `expr` to the variable name `name` in the `body`, like a `let x = 5 in _` statement
- `Var(name)`: Allows us to reference a variable assigned using a `Let` expression
- `Flip(p)`: Represents a boolean variable which is `True` with probability `p` and `False` with probability `1-p`
- `True, False`: Represents the constant values `True` and `False`
- `IfThenElse(cond_expr, true_expr, false_expr)`: Allows us to branch based on a (possibly random) expression `cond_expr`
- `Observe(expr)` observe that `expr` evaluates to `True`

Now let's write some programs! First we can start with a simple 