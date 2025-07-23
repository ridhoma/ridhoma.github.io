---
layout: default
title: A/B Test with Extreme Heterogenous Treatment Effect
parent: Causal Inference
nav_order: 1
---

# A/B Test with Extreme Heterogenous Treatment Effect

## The Usual A/B Test

The vanilla A/B test assumes that the treatment is equal for every sample in the same experiment group, called the **Stable Unit Treatment Value Assumption (SUTVA)**. This assumption holds for many A/B test scenarios in digital products (web or app). When you change the color of a button from red to blue, all users in the treatment group will get the same blue, no users who get purple. The treatment is equal for all users being treated.

With SUTVA assumption, the mathematical model of the A/B test becomes fairly simple. For instance, in an A/B test to measure changes in conversion rate (i.e. binary outcome where users either convert or not), we can simply take the difference of conversion rate between control and treatment groups, effectively measuring the **Average Treatment Effect (ATE)**. Let's consider $\rho$ as the conversion rate and the subset index ($0,1$) indicates random group assignment (control, treatment), the ATE is defined as:
$$
\widehat{ATE} = \rho_1 - \rho_0 
$$
In a large sample size $n$ and $s >\!\!> 0$, such as in a large scale digital product, the conversion rate $\rho$ can be estimated with $\hat{\rho} = s/n$. Then the uncertainty distribution of ATE can be approximated by,
$$
\widehat{ATE} \sim \text{Normal}(\hat{\rho}_1 - \hat{\rho}_0, \text{var}(\hat{\rho}_1) + \text{var}(\hat{\rho}_0))
$$
with the variance $\text{var}(\hat{\rho})$ is defined as ;
$$
\text{var}(\hat{\rho})=\hat{\rho}(1-\hat{\rho})/n
$$

This formulation allows us to calculate $p$-value and decide the statistical significance, for example, using [Binomial proportion test](https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval). The procedure is straightforward and easily automated, for example using this [A/B Test Calculator](https://abtestguide.com/calc/) or more fanciers tools that are integrated to company's analytics infrastructure.


## When SUTVA Breaks, comes Heterogenous Treatment Effect

I have came across many experiments where the SUTVA assumption breaks. Discussed in this article is an example from search engine product. 

Running a search engine, we always try to make improvement on the search algorithm. Any changes in the algorithm might have different effect from one search query to the others. For example, our experiment may change the search result for search query _“fried chicken”_ more than it changes the result for _“fish and chips”_ (btw, the product was a search engine for recipes). As you can see, the SUTVA assumption no longer holds because the treatment is now not equal across samples (i.e. depending on the search query). 

The unequal treatment leads to unequal effect. Some search queries weren't affected that much by the changes in the algorithm, hence we expect small differences in the outcome metrics (e.g. Click-Through Rate). On the other hands, queries that are altered significantly are expected to have bigger difference in the outcome metrics. This is a phenomena called **Heterogenous Treatment Effect (HTE)**, where the expected treatment outcome is different across samples.

In teh vicinity of HTE, we can no longer rely on $ATE$ as the sole measure of our experiment. For instance, $ATE=0$ (or small) does not necessarily mean the experiment changes nothing. It can be that there are equal samples with negative effect than samples with equally positive effect, resulting to 0 average effect. 

> 💡 _In a search algorithm experiments, identifying the treatment levels and the corresponding effects are required to get non-trivial insights from the experiments._
