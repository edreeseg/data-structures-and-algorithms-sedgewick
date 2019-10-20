# Quick-find [eager approach]

### Data structure
- Integer array id[] of size N.
- Interpretation: p and q are connected if and only if they have the same id.

<!--language: lang-none-->
    obj  0 1 2 3 4 5 6 7 8 9
    id[] 0 1 1 8 8 0 0 1 8 8

    0   1---2   3---4
    |       |   |   |
    5---6   7   8   9

**Find**: Check if p and q have the same id.

**Union**: To merge components containing p and q, change all entries whose id equals id[p] to id[q] (arbitrary).

```js
class QuickFind {
    constructor(n){
        this.objects = [];
        for (let i = 0; i < n; i++){
            this.objects[i] = i;
        }
    }
    connected(p, q){
        return this.objects[p] === this.objects[q];
    }
    union(p, q){
        const pid = this.objects[p];
        const qid = this.objects[q];
        for (let i = 0; i < this.objects.length; i++){
            if (this.objects[i] === pid)
                this.objects[i] = qid;
        }
    }
}
```

```java
public class QuickFindUF
{
    private int[] id;
    public QuickFindUF(int N)
    { // set id of each object to itself (N array accesses)
        id = new int[N];
        for (int i = 0; i < N; i++)
            if[i] = i;
    }
    public boolean connected(int p, int q)
    { // check whether p and q are in the same component (2 array accesses, constant time)
        return id[p] === id[q];
    }
    public void union(int p, int q)
    { // change all entries with id[p] to id[q] (at most 2N + 2 array accesses)
        int pid = id[p];
        int qid = id[q];
        for (int i = 0; i < id.length; i++)
            if (id[i] == pid) id[i] = qid; // MUST capture the value of pid first - as iteration continues, id[p] will change at some 
    }                                      // point to qid - bug that may not be immediately obvious.
}
```

## Quick-find is too slow

### Cost model.
Number of array accesses (for read or write).

| algorithm | initialize | union | find |
|-----------|------------|-------|------|
| quick-find| N          | N     | 1 (C)|

### Quick-find defect:
Union operation is too expensive.

e.g. Takes N^2 array accesses to process sequence of N union commands on N objects.

## Quadratic algorithms do not scale

### Rough standard (for now)
- 10^9 operations per second.
- 10^9 words of main memory.
- Touch all words in approximately 1 second (a truism [roughly] since 1950)

### Ex. Huge problem for quick-find
- 10^9 union commands on 10^9 objects.
- Quick-find takes more than 10^18 operations.
- 30+ years of computer time!

### Quadratic algorithms don't scale with technology
- New computer may be 10x as fast.
- But, has 10x as much memory => want to solve a problem that is 10x as big.
- With quadratic algorithm, takes 10x as long!