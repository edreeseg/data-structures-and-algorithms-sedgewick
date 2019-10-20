# Union Find Algorithm

Two classic algorithms: quick find and quick union.

#### Steps to developing algorithm:
1) Model the problem.
2) Find an algorithm to solve it.
3) Fast enough?  Fits in memory?
4) If not, figure out why.
5) Find a way to address the problem.
6) Iterate until satisfied.

#### Dynamic connectivity problem:

Given a set of N objects:
- Union command: connect two objects.  There will be a command to connect two objects.
- Find/connected query: Is there a path connecting the two objects?

<!-- language: lang-none -->

    0      1-------2      3-------4
                          |       |
    5------6       7      8       9

    Seen above:
        union(4, 3)
        union(3, 8)
        union(6, 5)
        union(9, 4)
        union(2, 1)

    connected(0, 7) // false
    connected(8, 9) // true
    
    Adding the following:
        union(5, 0)
        union(7, 2)
        union(6, 1)
        union(1, 0)

    0------1-------2      3-------4
    |      |       |      |       |
    5------6       7      8       9

    connected(0, 7) // true

Algorithm for this lecture will NOT be able to tell exactly what the path between nodes is, but will rather simply tell us if there IS a path (boolean).

There are numerous applications for this type of algorithm, with nodes representing such things as pixels in a photo, computers in a network, friends in a social network, and transistors in a computer chip.

Convention to name nodes from 0 to N-1 - convenient because it abstracts away unnecessary details on objects, and also maps very well to the indexes of an array.

We assume "is connected to" is an equivalence relation:
- Reflexive: p is connected to p.
- Symmetric: If p is connected to q, then q is connected to p.
- Transitive: If p is connected to q and q is connected to r, then p is connected to r.

Connected components:  Maximal set of objects that are mutually connected.

<!-- language: lang-none -->
    0   1     2---3
      /       | / |
    4---5     6   7

    { 0 } { 1 4 5} { 2 3 6 7}
    ^ 3 connected components ^

Connected components are essentially subsets of components that are mutually connected.  0 is by itself, not connected to anything else, so is in and of itself a connected component.  The rest of the objects are split into the remaining two connected components, because they are all connected.

The extension of this is the knowledge that no object **outside** of a particular connected component is connected, and use of this knowledge will allow us to make our algorithms more efficient.

## Implementing the operations

### Find query:
Check if two objects are the same component. e.g., object 0 and object 2 are not in the same connected component, meaning they are not connected.

### Union command:
Replace components containing two objects with their union.  e.g. union(2, 5) would lead to us replacing the two connected components containing these objects, { 1 4 5 } and { 2 3 6 7 }, with their union, { 1 2 3 4 5 6 7 }.

### Goal:
Design efficient data structure for union-find.
- Number of objects *N* can be huge.
- Number of operations *M* can be huge.
- Find queries and union commands may be intermixed.

```java
public class UF {
    public UF(int N) // initialize union-find data structure with N objects (0 to N-1)
    void union(int p, int q); // add connection between p and q
    boolean connected(int p, int q); // are p and q in the same component?
    int find(int p); // component identifier for p (0 to N-1)
    int count(); // number of components
}
```

```java
public static void main(String[] args)
{
    int N = StdIn.readInt(); // First takes number of objects
    UF uf = new UF(N); // Creates UF data structure based on number of objects
    while (!StdIn.isEmpty()) // While there is still input
    {
        int p = StdIn.readInt(); // Read the first int of a line
        int q = StdIn.readInt(); // Read the second int
        if (!uf.connected(p, q)) // If the two objects identified p and q are not connected
        {
            uf.union(p, q); // Perform union method on these objects, to connect them
            StdOut.println(p + " " + q); // Print out the two object identifiers
        } // If two objects are already connected, nothing happens
    }
}
```