# 1. PageRank Algorithm:

The PageRank Algorithm expresses the importance of a web page as a score, and for any web page, the score has to be a positive number. The main idea of the PageRank algorithm is that a good page refers to another good page, and for a good page, there’re a lot of good pages that may refer to it (recursive formula). To rank web pages, the number of incoming and outgoing links must be taken into consideration; to end up with this formula:
$$r_{j}= \sum_{i\rightarrow j} \frac{r_{i}}{d_{i}} $$

$r_{j}$: the rank of page $j$ 

$r_{i}$: the rank of page $i$

$d_{i}$: represents the number of outgoing links of $i$

$i$: every node that has an outgoing link in $j$

## 1.1 Applying on an example
<img src="/images/3.jpeg" alt="Example" width="200"/>

By applying the above equation on this figure to get the ranks of $r_{1}$,$r_{2}$,$r_{3}$, and $r_{4}$.
$$r_{1}=\frac{r_{3}}{1}+\frac{r_{4}}{2}$$
$$r_{2}=\frac{r_{1}}{3}$$
$$r_{3}=\frac{r_{1}}{3}+\frac{r_{2}}{2}+\frac{r_{4}}{2}$$
$$r_{4}=\frac{r_{1}}{3}+\frac{r_{2}}{2}$$

# 2.Matrix Formulation
The graph can be represented as an adjacency matrix that is column stochastic, which means that all columns must add up to 1.

If $j\rightarrow i \$, then
$$M_{ij}=\frac{1}{d_{j}} $$ 

and the summation of the page ranks should converge at 1
$$\sum_{i} r_{i}=1 $$
$r_{i}$: the rank of page $i$

This means that the PageRank vector can be written in the following form 
$$r=M.r$$
It can be seen from the above equation that the PageRank vector is the eigenvector of the stochastic matrix $M$, where the eigenvalue $\lambda=1$
# 3. Power Method:
The power method is an iterative method where:

start: $ r^{0}=\begin{bmatrix} \frac{1}{N} & \frac{1}{N} & \dots & \frac{1}{N} \end{bmatrix} ^{T} $

iterative step: $r^{t+1}=Mr^{t}$

stop: $\left\lvert r^{t+1}-r^{t} \right\rvert < \epsilon$, where  $\epsilon$ is the desired tolerance.

# 4.Problems:
## 4.1 Rank Sink (Spider Trap):
<img src="/images/1.png" alt="Example" width="200"/>

The rank sink case is when all the out-links are pointing to nodes from the same group; the basic example to consider is a node with out-links just pointing to itself (as shown in the above figure). To understand the problem, imagine there’s a surfer that follows the out-links that connect between the web pages. In the case of the rank sink problem, when the surfer reaches this node it won’t be able to escape out of it; this means that this node will absorb all the importance approaching 1, while the ranks of the other nodes will approach zero, so the PageRank model converges to unrealistic results

### 4.1.1 Solution (Random Jump method):
<img src="/images/RankSinkSol.png" alt="Example" width="200"/>

If the surfer gets stuck in the rank sink problem, one way to solve this problem is teleporting to a random node now and then. This can be achieved by creating virtual links that the surfer can follow  –as shown in the above figure by the green arrows – the probability of the surfer following the virtual links $(1-\beta)$ should be a lot less when comparing it to that of the actual out-links $(\beta)$; so the ranks won’t be affected.
$$A= \beta M+(1-\beta)\left[\frac{1}{N}\right]_{N \times N}$$

A: modified matrix

$\beta$: between 0.8 and 0.9

When applying the random jump method to an ideal case problem (that doesn't result in the rank sink) It doesn't affect the ranks and gives very good approximations. That means that the algorithm doesn’t have to explicitly check whether the matrix contains a rank sink problem or not, and just applies the random jump to all test cases.
## 4.2 Dangling node problem
<img src="/images/2.png" alt="Example" width="200"/>

It means that there’s a node that has no out-links (as shown in the above figure). So when the surfer reaches this node, it gets lost and has no way back to other nodes — including itself. That will result in a ranking vector of zeros, which means that the information is lost. This problem represents a mathematical problem rather than a practical one, as the matrix is not column stochastic, so our initial assumptions aren't met.
### 4.2.1 Solution (Teleportation With probapility 1.0)
<img src="/images/DanglingNodesol.png" alt="Example" width="200"/>

Unlike the rank sink problem case, applying the random jump method won’t help this time, as there is no way out of the dangling node if the surfer gets stuck. However, the solution is still to teleport. Whenever the surfer gets to the dangling node, it should teleport to another node — including itself. And since the surfer teleports with a probability of 1.0 once it reaches the dangling node. The virtual links created from such a node will have the same weight as the actual out-links  (as shown in the above figure). That can be translated into a mathematical form by changing all elements of the dangling node column in the matrix to equal $\frac{1}{N}$
$$ \sum_{j=0} ^{N-1} M_{ji}=\frac{1}{N}$$
after that, the random jump method is applied as mentioned earlier, and that's what the green arrows indicate.

# 5. Final Algorithm
<img src="/images/Flowchart.jpeg" alt="Example" width="400"/>

This flow chart is implemented using python and tested on different test cases on the provided jupyter notebook file.

# References:
“CS 246 Lecture 9: PageRank (2014).” http://stanford.io/2fDoChT

 “THE $25,000,000,000 * EIGENVECTOR THE LINEAR ALGEBRA BEHIND GOOGLE”: https://www.math.arizona.edu/~glickenstein/math443f08/bryanleise.pdf
