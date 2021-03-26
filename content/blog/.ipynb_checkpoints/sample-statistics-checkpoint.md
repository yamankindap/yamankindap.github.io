---
title: "Reasoning about Data: Sample Statistics"
date: 2020-04-08T09:19:29-04:00
slug: "sample-statistics"
description: "This is an introductory review on the analysis of samples."
keywords: ["sample", "statistics"]
draft: false
tags: ["introductory", "statistics"]
math: true
toc: true
---

If we want to understand how to reason about any observed data set, the most basic starting point is understanding the process of _statistical inference_. I remember taking my first course in probability and statistics during my undergraduate education and somehow it felt much harder than any mathematics course I've taken so far. In hindsight, this was probably because it was the first time _uncertainty_ was a part of the subject. I believe that the way probability and statistics courses are structured emphasizes statistical inference as a tool rather than actually giving an intuition about dealing with _uncertain information_. In this post, I will try to describe one of the most fundamental concepts of statistical inference: _sample statistics_.

Before we understand how to reason about a particular data set, we need to ask how the data set was collected. The answer will have major consequences on the later stages of inference. In almost all questions of interest, we will not be able to observe all instances of a particular entity. For example, let's examine the global ratio of people infected during a global pandemic. It is very unlikely that we will be able to test billions of people to identify the infected ones. Moreover, in this particular example the whole world population would have to be tested every day since the pandemic evolves in time. Similar to it's usage in this example, all instances of a particular entity (e.g. humans) is called a _population_ in statistics. The group of people we are actually able to test and gather information about is called a _sample_. Therefore, a sample is actually a subset of the population and it is our data set.

After we understand the concept of a sample, an important question arise: Is our sample representative of the whole population? The first problem we encounter is that there might be various biases in our data set. In our pandemic example, it might be the case that only the most severely affected people get tested since more mild cases will likely do not exhibit symptoms and do not feel the need to get tested voluntarily. If we are unaware of such biases in our data set, the analysis and results will likely also be biased. Another problem might be that our sample is simply too small to accurately represent the population. The latter problem is more clearly defined in the literature of statistical inference and it will be one of the central elements of our discussion in inference. However, before we get into inference we need to define one last concept: a _probability distribution_.

Before we carry out any calculations, we need to define the question we are asking quantitatively and decide how to represent our data. For example, one of the most important quantities to understand during a pandemic is the reproduction number, denoted as $R_0$, which is defined as the expected number of new cases that will be caused by one infected person, assuming that everyone in the population is susceptible to infection. Presumably, each country or city will report varying $R_0$ values since they are observing different samples of the same population. Let's assume that our data set consists of these reported $R_0$ values, the question becomes: How do we estimate the actual reproduction number of the pandemic? 

A probability distribution provides a representation of our information gathered through data as well as our uncertainty about our knowledge. Specifically, a probability distribution attributes a probability to observing each possible value as our answer. The most common choice of a probability distribution is the _normal_ (or _Gaussian_) _distribution_ as a result of the _central limit theorem_ which we might discuss in another post. A mathematical function quantifying the relative likelihood of observing a specific value is called a _probability density function_ in the case where the possible answers takes continuous values and _probability mass function_ in the case where the possible answers are discrete values.

Let's return to our example and define the reproduction number $R_0$ as a random variable $X$ with a probability density function (PDF) $f(x)$ for generality. While there may be a definite value of $R_0$, since we don't have access to enough information, our answer will always have some uncertainty associated with it. If $X$ is a random variable defined in the _sample space_ (the set of possible values taken by the random variable) of $\mathbb{R}$, the expected value of $X$ under $f(x)$, denoted as $E_{f(x)}[X]$, is defined as the integral:

$$
    E_{f(x)}(X) = \int_{-\infty}^{+\infty} x f(x) dx = \mu
$$

The greek letter $\mu$ is frequently used to denote the expectation as a scalar value. Notice that we can only calculate this integral when the complete functional form of $f(x)$ is known and integrable. We can interpret the mathematical operator expected value as the sum of the possible values weighted by their likelihood of occurence. 

Unfortunately, in most data analysis tasks, data is collected from a source whose PDF is unknown. Thus, if we don't have access to a PDF, we have to approximate, or formally estimate, the value of $E_{f(x)}[X]$. At this point we must make further assumptions about the structure of the data set in order to simplify our analysis. One such assumption is to assume that individual data points in the set are _independent and identically distributed_ (_i.i.d._). This is similar to saying that observing any one of the data points in the set is equally probable, they come from the same distribution and they don't have any influence over each other. Now, we can use the _law of large numbers_ which in a simplified manner states that the arithmetic mean of the data set almost surely converges to the expected value as the size of the data set approaches infinity. As a result, we can use the arithmetic mean (or sample mean) of the data as an estimator of the expected value of $X$.


$$
    \hat{E}_ {f(x)}[X] = \frac{1}{N} \sum_{i=0}^{N} x_i = \hat{\mu}
$$

where $N$ is the number of data points in our data set. Since $\hat{\mu}$ is an estimate of the actual value we want to evaluate, $E_{f(x)}[X]$, we need to define a measure for it's quality of estimation. One abstract concept we need to understand is that we can also treat the sample mean of the data set as a random variable since the data points $x_i$ might as well be a different value if we were to collect the data again. Therefore, the sample mean is a function of the random variable $X$. Then the sample mean must have a mathematically defined expected value, $E[\hat{\mu}]$. The _bias of an estimator_ is defined as:


\begin{equation}
    \text{Bias}(\hat{\mu}) = \mathbb{E}[\hat{\mu}] - \mu
\end{equation} 


Let's try to calculate the value of the $\mathbb{E}[\hat{\mu}]$. Since expectation is a linear operation we can write:


\begin{equation}
    \mathbb{E}[\hat{\mu}] = \frac{1}{N} \sum_{i=0}^{N} \mathbb{E}[x_i]
\end{equation} 


Notice that even though we don't know the actual value of $\mu$, we know that each $x_i$ is coming from a distribution with expected value $\mu$. Then, we can symbolically use this information in the equation:


\begin{equation}
    \mathbb{E}[\hat{\mu}] = \frac{1}{N} \sum_{i=0}^{N} \mu
\end{equation} 


\begin{equation}
    = \frac{1}{N} N \mu 
\end{equation} 


\begin{equation}
    = \mu
\end{equation} 


Then, we can simply see that the bias of using the sample mean as an estimator for the expected value is zero. However, recall that we've mentioned one problem with using a sample might be that our data set is too small to accurately represent the population. I think we can intuitively see that this estimation will be better for large values of $N$, or as $N\to\infty$. Thus, we also need to calculate the variance of $\hat{\mu}$ as a function of $N$ to have a complete understanding of the convergence of $\hat{\mu}$ to $\mu$.

If the random variable $X$ has a probability density function $f(x)$, the variance of $X$ under $f(x)$, denoted as $\text{Var}_{f(x)}(X)$, is defined as the integral:


$$
    \text{Var} _ {f(x)}(X) = \int_{- \infty}^{+ \infty} (x-\mu)^2 f(x) dx
$$


\begin{equation}
    = \sigma^2
\end{equation}


Similar to the expected value, we can only calculate this integral when the complete functional form of $f(x)$ and the expected value, $\mu$, is known, and integrable. Notice that we can write the variance function as an expectation:


\begin{equation}
    \mathbb{Var}(X) = \mathbb{E}_{f(x)}[(X - \mathbb{E}[X])^2]
\end{equation}


\begin{equation}
    = \mathbb{E}[X^2] - \mu^2
\end{equation}


An important point to remember is that at the moment we are not trying to calculate or estimate the variance of $X$, instead we are interested in the variance of using the sample mean as an estimator for the expectation of $X$. Thus, let's derive the variance of our estimator as a function of $N$.


\begin{equation}
    \mathbb{Var}(\frac{1}{N} \sum_{i=1}^{N} x_i) = \mathbb{E}[(\frac{1}{N} \sum_{i=1}^{N} x_i- \mu)^2]
\end{equation}


\begin{equation}
    = \mathbb{E}[\frac{1}{N^2} (\sum_{i=1}^N x_i - N\mu)^2] 
\end{equation}


\begin{equation}
    = \frac{1}{N^2} \mathbb{E}[(\sum_{i=1}^N x_i - N\mu)^2]
\end{equation}


\begin{equation}
    = \frac{1}{N^2} \mathbb{Var}(\sum_{i=1}^N x_i - N\mu)
\end{equation}


\begin{equation}
    = \frac{1}{N^2} \sum_{i=1}^N \mathbb{Var}(x_i)
\end{equation}


\begin{equation}
    = \frac{\sigma^2}{N} 
\end{equation}


Notice that the variance of the estimator depends on the true variance of the process, $\sigma^2$. We can think of the variance as a theoretical bound on the variance of our estimator and we see that the variance of using the sample mean as an estimator for the expected value decreases as the number of data points, $N$, increase. However, if we don't know the actual variance of the random variable $X$, which is usually the case, we will also have to estimate the variance of $X$ in order to obtain an actual number for the population variance of our estimator.
