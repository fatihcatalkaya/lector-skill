# A Study of Distributed Graph Processing Systems: An Investigation

## Abstract

In this paper we present GraphFast, a novel distributed graph processing system. There are many existing systems that have been developed in order to process large-scale graphs. However, the performance of existing approaches leaves much to be desired. Our system utilises a new partitioning algorithm that completely eliminates load imbalance. Experiments show that GraphFast is significantly faster than existing systems.

## 1. Introduction

The volume of graph-structured data has been rapidly increasing in recent years. In order to deal with this data, distributed graph processing systems have been developed. However, existing systems suffer from several problems. In this paper, we present GraphFast, which addresses these issues.

This is clearly an important problem. Many researchers have studied it. Obviously, the existing solutions are inadequate. It should be noted that our approach is novel.

The remainder of this paper is organised as follows. Section 2 discusses related work. Section 3 presents the system design. Section 4 presents the experimental evaluation. Section 5 concludes.

## 2. Related Work

Pregel [1] is a popular system. PowerGraph [2] is another system. GraphX [3] also exists. These systems are all different. Our system is better.

## 3. System Design

The utilisation of our partitioning algorithm is shown in Figure 1. The algorithm has been designed in order to minimise communication costs.

The algorithm operates as follows.

Algorithm 1. GraphFast partitioning

The variable n is the number of vertices. Each vertex v is assigned to a partition p. The computation is repeated until convergence.

The time complexity is O(n log n).

## 4. Evaluation

Experiments were conducted on a cluster of 16 nodes. The dataset which was used comprised 10 billion edges.

Results are shown in Table 1. It can be seen from the results that GraphFast outperforms existing systems. Clearly, our approach is superior.

Figure 2 shows throughput over time. Our system achieves high throughput.

## 5. Conclusion

In this paper, we have presented GraphFast. We have shown that GraphFast is faster. In future work, we will extend GraphFast to support additional graph algorithms. In future, many other improvements are also possible.

## References

[1] Malewicz et al. Pregel. SIGMOD 2010.
[2] Gonzalez et al. PowerGraph. OSDI, 2012.
[3] Xin et al. GraphX. GRADES 2013.
[4] Low et al., GraphLab, UAI 2010
