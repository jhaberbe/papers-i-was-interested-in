# Prior distributions for variance parameters in hierarchical models

- A lot of people seem to prefer to use half-normal priors to inverse gamma priors. This paper is why, would be good to understand.

### Use of improper priors
- Can, but do not necessarily lead to proper posterior distributions.
- Interesting limiting cases of distributions (Ex. Jeffreys prior, Beta(1/2, 1/2))
    - While not proper densities, evidence tends make them proper (in my experience, may be wrong.)

Argument: 

$\text{Inv.Gamma}(\epsilon, \epsilon)$ doesn't have a proper limiting density, so posteriors can be sensitive to $\epsilon$. You have to pay attention(!)

Arguments
- Given an improper prior, it is impossible for a true parameter value to ever be drawn from that distribution (evident, it can't be normalized).
    - I guess my only question for that would be, we don't ever truly sample the prior, in the case where an uninformative and improper prior is chosen. We mostly use it to make (initially qualitative) statements about the degree of belief that we have in a particular parameter. That argument kinda doesn't work for things like $\text{Beta}(1/2, 1/2)$, where 0/1 are infinite. How do you compare something to infinity?

- True and inferential prior distributions for some $\sigma$ are $\text{Uniform}(0, A)$, then miscalibration is trivially zero. Extend A to $\infty$ for the possible posterior distribution, then the value will always increase, meaning that we get higher average values, which will always increase the miscalibration.
    - Basically, if the true prior is limited to A, then any increase past A for the posterior will lead to larger miscalibration.
        - Do we just throw up our hands and cry woe and despair?
-  Bickel and Blackwell (1967) and Meng and Zaslavsky (2002) Talk about the lack of unbiased estimators of $\sigma$ or $\sigma^2$


### Inverse Gamma offers conditional conjugacy

- If the prior is inv gamma, the conditional posterior is too.
    - My reasoning: If the the prior density times the conditional likelihood can be reformulated as the same prior density, by just doing an algebraic substitution of the given parameters, then its conjugate conditional to the prior distribution.
    - GPT reasoning: If the prior density multiplied by the conditional likelihood can be algebraically rewritten as the kernel of the same distribution family as the prior (with updated hyperparameters), then the prior is conditionally conjugate.

$\text{Inverse }\chi^2$ distribution is a special case of the Inv. Gamma


### Folded T.

There is an overparameterized version of which is practically useful, which goes: 

Data is distributed as:
$$y_{ij} \sim N(\mu + \xi\eta_j, \sigma^2_y)$$

The errors are distributed as:
$$\eta_j \sim N(0, \sigma^2_\eta)$$

Some stuff I don't 100% understand at the moment, and just glazed over to be honest.

### Improper Uniform Issues

- His issue with improper uniform priors, as i read it.
We view any noninformative or weakly-informative prior distribution as inherently provisional. After the model has been fit, one should look at the posterior distribution and see if it makes sense. If the posterior distribution does not make sense, this implies that additional prior knowledge is available that has not been included in the model, and that contradicts the assumptions of the prior distribution that has been used. It is then appropriate to go back and alter the prior distribution to be more consistent with this external knowledge.

we have an improper prior on a uniform distribution, after integrating over the parameters, is the argument something like: I have three individuals in a groups, suppose we don't know the group loc or scale. in a heirarchical model, we assume both group scale and individual scale. while the data may not very well support it, even if a human were looking at three normal distributions, all centered at different values, and all with slightly different means, and all looked really like three normals that are dispersed slightly differently from the group, the model structure (and really nobody) could rule out the possibility that all the individuals are actually centered at the exact same loc, with different scales possibly, and it was just a happy accident that they all occured at the same place. thus, in a purely bayesian sense, when we integrate over this improper prior, since there is an infinite amount of density as we approach zero, the model will (purely due to an improperly specified system of beliefs) state that the evidence supports a group scale of exp(-infty) = 0, or around 0.

really cool thought experiment, I have no idea if you run into that as a practical concern. I now worry a moderate amount about it, cause I could see that likely happening (or in maybe 1/4 runs or so, I have no justification for that fraction, just feel like it).

### Proper Uniform Issues

- Text states that when there are 1/2 groups, the shrinkage concludes with an improper posterior density, essentially stating that $\theta_\alpha = \infty$
    - see Gelman et al., 2003, Exercise 5.8
    - essentially, the likelihood on the variance is $\pi(\sigma) = \frac{1}{\sigma}$, so for $\sigma > 0$. 

### Inverse-gamma 

This feels like the main attack of the paper.

- The case was, at this point, setting a prior for alpha and beta both very close to zero (1e-3, say) seemed cool, and was a noninformative prior. In it's limit, it is an improper density, meaning epsilon needs to be set to a reasonable value. If the dataset has a low sigma, then inference ends up being very sensitive to the model, meaning that the prior distribution that was meant to be uninformative ends up being very informative.
    - It is now actually really nice to know this, because for gamma poinsson constructions of the negative binomial model, it actually seems to justify my own choice of inv gamma (as long as I take some care). If we choose a negative binomial, it means that we expect non-negligible variance (otherwise, it would just converge to a poisson distribution). It probably requires some extra steps, in terms of determining whether or not the data supports the use of that type of distribution, but it offers up some conditions on whether one should feel comfortable using this type of model or not. Primarily, if variance is expected to be low, then an inv gamma distribution ends up being informative and potentially pathological (you end up with posterior miscalibrationj).
        - Question, has anyone done experiments on this case.
        - Answer, they did.

In the case of low epsilon, posterior estimates shrink to zero. 

### Their recommendations

For a sufficiently high number of groups, noninformative uniform priors should play well, if you get a number of groups below 5 or so, then it will overestimate. 

Never use $\text{InvGamma}(\epsilon, \epsilon)$, ever.

If you want more prior information in your model, then decide on using a Half-Normal or Half-T distribution.

I got lucky using log-normal it looks like.