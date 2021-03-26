---
title: "Intuition Behind the Poisson Distribution"
date: 2021-03-26
slug: "poisson-binomial"
description: "Intuition Behind the Poisson Distribution."
keywords: ["binomial", "bernoulli", "poisson"]
draft: false
tags: ["poisson process"]
math: true
toc: true
---

## Motivation

When we want to model a complicated stochastic process such as the daily revenue of a company or daily returns of investments, it is generally useful to divide the problem into simpler parts until we are faced with some indivisible question. Such a question may be broadly phrased as: Did some random event occur or not? For example, such a random event may be receiving an order for your e-commerce site, a customer arriving at a store or an investor placing an order on a stock exchange. We can start by modelling this simple random occurrence and build our complicated model by forming combinations of events, and assigning different attributes to these events such as the price of an order. Thus, let's consider modelling the occurrence of a random event.

## Event Occurrences

Let's start modelling the random occurrence of an event by making a simplifying assumption that events are independent from each other. For example, we are assuming that receiving an order from customer A, does not affect the probability of receiving an order from customer B. While this is generally not the case for many problems, it can provide a good starting point for an approximate solution. However, we should always keep in mind that we are assuming events are independent. 

Without reference to multiple events of the same type occurring in an interval, the independence assumption naturally leads to a model of a single event as a Bernoulli random variable $X$ with probability of occurrence $p$:

\begin{equation}
    p(X=i) = p^{i}(1-p)^{1-i}, \quad i \in \{ 0,1 \}
\end{equation}

Now that we have some way of modelling the occurrence of a single event, we can start expanding our model to include multiple events of the same type. Consider a sequence $ \lbrace X_{k} \rbrace_{k=1}^{K} $ made up of independent Bernoulli random variables $X_k$. Each random variable in the sequence has an associated interval of time $(t_{k-1}, t_k]$ such that if an event occurs in this interval, then $X_k = 1$ and if it does not $X_k = 0$. Then, we can interpret this sequence as a model of occurrence of events over an interval $(0,T)$ divided into arbitrarily spaced points. 

An important point to remember is that while the length of the sub-intervals $(t_{k-1}, t_k]$ may be different, we are assuming that the probability $p$ of an event occurring in each of these intervals is equal. This may not be the case in general but we will refer to this problem later.

## Number of Events

Using our model, we can start asking important questions such as: Given a sequence of $K$ random variables, what is the probability of $n$ events occuring? Let's denote the total number of events as a random variable $N$, then the probability distribution of $N$ for a sequence of $K$ independent intervals is defined by the binomial distribution $\mathcal{B}(K,p)$ shown as:


\begin{aligned}
    p(N=n) &= \mathcal{B}(n; K,p), \quad n \in \mathbb{N} \cr
    &= {K \choose n} p^{n} (1-p)^{K-n}
\end{aligned}

where the expected number of events is given by $\mu = pK $. 

Notice that, this formulation of random events has a clear drawback which is that it is only able to consider a single event within an interval. Since we are arbitrarily choosing the intervals, it is possible that multiple events occur in such an interval. A quick solution to this problem is to increase the number of random variables in the sequence thereby also increasing the number of intervals. This way we can have a more granular model of the process. Hence, a more general solution may be to assume that there are an infinite number of random variables in the sequence $ \lbrace X_{k} \rbrace_{k=1}^{\infty}$. Then, by finding the limiting distribution of the binomial distribution as $K$ goes to infinity, we can have a better model for random events happening in the interval $(0,T)$.

For the sake of similarity, let's keep the expected number of events $\mu$ constant and find the limiting distribution of number of events when we divide the interval $(0,T)$ into infinite sub-intervals.

\begin{aligned}
    p(N=n) &= \lim_{K \to +\infty} \mathcal{B}(n; K, \frac{\mu}{K}) \cr
    &= \lim_{K \to +\infty} {K \choose n} \bigg(\frac{\mu}{n} \bigg)^{n} \bigg(1-\frac{\mu}{K} \bigg)^{K-n} \cr 
    &= \lim_{K \to +\infty} \frac{K!}{(K-n)! n!} \bigg( \frac{\mu}{K} \bigg)^{n} \bigg( 1- \frac{\mu}{K} \bigg)^{-n} \bigg( 1- \frac{\mu}{K} \bigg)^{K} \cr
    &= \frac{\mu^{n}}{n!} \bigg[ \lim_{K \to +\infty} \frac{1}{K^n} \prod_{i=0}^{n-1} (K-i) \bigg] \bigg[ \lim_{K \to +\infty} \bigg( 1- \frac{\mu}{K} \bigg)^{-n} \bigg] \bigg[ \lim_{K \to +\infty} \bigg( 1- \frac{\mu}{K} \bigg)^{K} \bigg] \cr
    &= \frac{\mu^n e^{-\mu}}{n!} \cr
    &= \mathcal{P}(\mu)
\end{aligned}

where $\mathcal{P}(\mu)$ is the Poisson distribution with parameter $\mu$ [1]. Therefore, we have shown that for an interval $(0,T)$ where the expected number of events equals $\mu$, the number of events has a Poisson distribution. Notice that keeping $\mu$ constant in our derivation from the binomial distribution corresponds to the probability $p$ of an event at time $t$ going to zero as $K$ goes to infinity. 

## References

[1] Kingman, J. F. C. (1993), Poisson processes , Vol. 3 , The Clarendon Press Oxford University Press , New York .