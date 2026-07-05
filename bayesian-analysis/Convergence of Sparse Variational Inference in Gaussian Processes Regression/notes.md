# Very Short one

This gives some insight into how many inducing points are needed to achieve certain error bounds in HSGPs, which are nice to have ahead of time.

specifically, for a gaussian kernel, its:

$$\Omega((\log N)^D)$$

for matern, its:

$$
N^{\Omega(
    \frac{(2\nu D)}
    {(2\nu + 5D)(2\nu + D)} - \epsilon
)}
$$

Gaussian is much less expensive, asymptotically, but if you think that you have interesting interpolation between terms, you're going to have a shitty day.