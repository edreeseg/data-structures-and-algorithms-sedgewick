# Quick-union [lazy approach]

### Data structure
- Integer array id[] of size N.
- Interpretation: id[i] is parent of i.
- Root of i is id[id[id[...id[i]...]]]. // Keep going until it doesn't change (algorithm ensures no cycles).

|    |0|1|2|3|4|5|6|7|8|9|
|----|-|-|-|-|-|-|-|-|-|-|
|id[]|0|1|9|4|9|6|6|7|8|9|

**Find**: Check if p and q have the same root.
**Union**: To merge components containing p and q, set the id of p's root to the id of q's root.

|    |0|1|2|3|4|5|6|7|8|9    |
|----|-|-|-|-|-|-|-|-|-|-----|
|id[]|0|1|9|4|9|6|6|7|8|**6**|
^ Only one value changes^

```js
class QuickUnion {
    constructor(N){
        this.id = [];
        for (let i = 0; i < N; i++)
            this.id[i] = i;
    }
    findRoot(x){
        if (this.id[x] === x){
            return x;
        }
        else return this.findRoot(this.id[x])
    }
    connected(p, q){
        const rootP = this.findRoot(p);
        const rootQ = this.findRoot(q);
        return rootP === rootQ;
    }
    union(p, q){
        const rootP = this.findRoot(p);
        const rootQ = this.findRoot(q);
        this.id[rootP] = rootQ;
        return this.id;
    }
}
```

```java
public class QuickUnionNF
{
    private int[] id;
    public QuickUnionUF(int N)
    { // set id of each object to itself (N array accesses)
        id = new int[N];
        for (int i = 0; i < N; i++) id[i] = i;
    }
    private int root(int i)
    { // chase parent pointers until reach root (depth of i array accesses)
        while (i != id[i]) i = id[i]; // Iterative, rather than recursive
        return i;
    }
    public boolean connected(int p, int q)
    { // check if p and q have same root (depth of p and q array accesses)
        return root(p) == root(q);
    }
    public void union(int p, int q)
    { // change root of p to point to root of q (depth of p and q array accesses)
        int i = root(p);
        int j = root(q);
        id[i] = j;
    }
}
```

## Quick-union is also too slow

### Cost model.
Number of array accesses (for read or write);

|algorithm  |initialize|union|find|
|-----------|----------|-----|----|
|quick-find |N         |N    |1   |
|quick-union|N         |N*   |N   |
\* includes cost of finding roots.

### Quick-find defect:
- Union too expensive (N array accesses).
- Trees are flat, but too expensive to keep them flat.

### Quick-union defect:
- Trees can get tall.
- Find too expensive (could be N array accesses).