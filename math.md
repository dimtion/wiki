
# Topology

## Distances

### Hamming distance

The hamming distance of two vectors $a, b \in \mathrm{F}$ is the number of
elements of $a$ that differ from $b$.

In Python it can be calculated:
```python
def hamming_distance(a, b):
    if len(a) != len(b):
        raise ValueError("a must be the same length than b")
    return sum(x != y for x, y in zip(a, b))
```

