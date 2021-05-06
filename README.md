# Local Search for Constrained Graph Clustering in Biological Network

> **Contributors:** Duy Hoang Tran<sup>1</sup>, Behrouz Babaki<sup>2</sup>, Dries Van Daele<sup>3</sup>, Pieter Leyman<sup>1</sup> and Patrick De Causmaecker<sup>1</sup>    
> <sup>1</sup> *KU Leuven Kulak, Department of Computer Science, CODeS, Belgium*  
> <sup>2</sup> *Polytechnique Montreal, Department of Computer and Software Engineering, Canada*  
> <sup>3</sup> *KU Leuven, Department of Computer Science, Belgium*  

## 1. Problem description

This work addresses a constrained graph clustering problem with applications on Protein-Protein Interaction (PPI) networks in cancer research. The method takes a subgraph of the PPI network as input and facilitates the identification of pathways in PPI networks, an important aid in identifying cancer driver genes from gene prioritization lists.  

Interaction graph G<sub>i</sub> = (V, E<sub>i</sub>) is an unweighted graph, where V represents a set of genes and E<sub>i</sub> describes the interaction between each pair of genes. Two other weighted graphs express which pair of genes must or cannot be assigned to the same pathway: cannot-link graph G<sub>c</sub> = (V, E<sub>c</sub>) and must-link graph G<sub>m</sub> = (V, E<sub>m</sub>). The edges in E<sub>c</sub> describe the penalty score for each pair of genes that should not be present in the same pathway, while the penalty for the pair which must belong to the same pathway is represented by E<sub>m</sub>. The goal is to partition V into k groups such that each subset is a connected component in the interaction graph and the penalty of cannot-link and must-link constraint violation is minimized.

## 2. Problem instances

There are three sets of instances: smaller-instance set (n &le; 300), small-instance set (500 &le; n &le; 2000) and large-instance set (10000 &le; n &le; 40000). The set of smaller instances includes 54 real problem instances extracted from the HINT+HI2012 PPI network that belong to six different graph sizes: 50, 75, 100, 150, 200, and 300 nodes. The small-instance set contains 60 synthetic problem instances having n &isin; {250, 500, 750, 1000, 1500, 2000}. An n-size interaction network is formed from the physical protein-protein interaction network for human \[1\] by taking n random connected nodes and transferring all incident edges. The set of large instances includes 102 synthetic problem instances using original graphs from a collection of species-specific protein-protein association networks \[2\] whose sizes are from 10,000 to 40,000 nodes. The instances can be downloaded from [here](http://dx.doi.org/10.17632/572nyx5rbs.2).  

\[1\] R. Rossi, N. Ahmed, The Network Data Repository with Interactive Graph Analytics and Visualization, in Proceedings of the Twenty-Ninth AAAI Conference on Artificial Intelligence, 2015.  
\[2\] D. Szklarczyk, et al., The STRING database in 2017: quality-controlled protein-protein association networks, made broadly accessible, Nucleic Acids Research (2016).  

## 3. Local search algorithms (ls)

### Usage

```
Usage: java -jar cgcp.jar <nCluster> <gamma> <graph> <cannotLink> <mustLink> <output> [options]
    <nClusters>   : Number of clusters k.
    <gamma>      : Trade-off between cannot-link penalty and must-link penalty.
    <graph>      : Path of the interaction graph file.
    <cannotLink> : Path of the cannot-link graph file.
    <mustLink>   : Path of the must-link graph file.
    <output>     : Path of the output clusters file.

Options:
    -seed <seed>         : random seed (default: system clock).
    -time <timeLimit>    : time limit in seconds (default: 600).
    -maxIters <maxIters> : maximum number of consecutive rejections (default: 50000).
    ILS parameters:
        -wP <wP>         : articulation point move probability = wp / |V| (default: 0.2293).
        -itersP <itersP> : non-improvement iterations before perturbation (default: 310).
        -p0 <p0>         : initial perturbation size = p0 * |V| (default: 0.1055).
        -pMax <pMax>     : maximum perturbation level (default: 12).
        -apMove <apMove> : 1 accept AP move, 0 not accept AP move (default: 1).

Examples:
    java -jar cgcp.jar 2 0.5 graph.txt cannot_links.txt must_links.txt output.txt -time 180
    java -jar cgcp.jar 2 0.5 graph.txt cannot_links.txt must_links.txt output.txt -time 180 -wP 0.2 -itersP 100 -p0 0.2 -pMax 6

```

### Requirements

Java 1.8 are required.

## 4. Multilevel algorithm (ml)

### Usage

```
Usage: java -jar cgcp.jar <nCluster> <gamma> <graph> <cannotLink> <mustLink> <output> [options]
    <nCluster>   : Number of clusters k.
    <gamma>      : Trade-off between can-not-link penalty and must-link penalty.
    <graph>      : Path of the interaction graph file.
    <cannotLink> : Path of the cannot-link graph file.
    <mustLink>   : Path of the must-link graph file.
    <output>     : Path of the output clusters file.

Options:
    -crs <coarsener> : Heavy-edge matching (hem), Algebraic multigrid (amg) (default: amg).
    -seed <seed>     : random seed (default: system clock).

Examples:
    java -jar cgcp.jar 32 0.5 graph.txt cannot_links.txt must_links.txt output.txt
    java -jar cgcp.jar 32 0.5 graph.txt cannot_links.txt must_links.txt output.txt -crs hem


```

### Requirements

Java 1.8 are required.