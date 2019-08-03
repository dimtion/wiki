# Math

## Topology

### Distances

#### Hamming distance

The hamming distance of two vectors $`a, b \in \mathrm{F}`$ is the number of
elements of $`a`$ that differ from $`b`$.

In Python it can be calculated:
```python
def hamming_distance(a, b):
    if len(a) != len(b):
        raise ValueError("a must be the same length than b")
    return sum(x != y for x, y in zip(a, b))
```



## Information theory and statistics

### Probability mass function

Supose $`X: \Omega \to A \subseteq \R`$ a discrete random variable, We call
$`P`$ the mass function of $`X`$ defined by:

```math
P(x) = P[X = x]
```

The mass function is the analog of the density function $`X`$ were continuous
random variable.


### Entropy

Entropy in information theory (also called Shanon Entropy) is a generalization
of Thermodynamics Entropy (Boltzmann Entropy).

The entropy $`H`$ of a discrete random variable $`X`$ with possible values
$`\{ x_{1}, x_{2}, \dots, x_{n} \}`$, with a probability mass function $`P`$ is defined by:

```math
H(X) = E(-\log(P(X)))
```

More explicitly:

```math
H(X) = - \sum_{i=1}^{n} P(x_{i}) \log (P(x_{i}))
```

### Cross Entropy
