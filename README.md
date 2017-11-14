# GeometricConvolutionsBench in TensorFlow

In particular, our goal was to compare the efficiency of so-called Geometric Convolutions, 
which can be defined on an *arbitrary mesh structure*, to standard convolutions, which assume a that the nodes are organised along an 
*Euclidean grid*.

These scripts propose several implementations of geometric convolution methods that can be used for Convolutional Neural Network. 
They are inspired from original paper and codes of [^fn1] and [^fn2].

[^fn1] : Thomas N. Kipf, Max Welling, Semi-Supervised Classification with Graph Convolutional Networks (ICLR 2017)
[^fn2] : F. Monti*, D. Boscaini*, J. Masci, E. Rodolà, J. Svoboda, M. M. Bronstein, Geometric deep learning on graphs and manifolds using mixture model CNNs, CVPR 2017 (MoNet framework)

In each scenario, we assume that we are doing convolutions over a graph which is organised as a grid where each node has 8 neighbours. 
Convolutions are done in a CNN setting with same number of input and output features.

To run the code, use:
python BenchmarkStandardConvolution 1 50 50
where the paramters correspond to Batch Size, grid size (one side of the square), number of features.

nb: You may have to use 
export LD_LIBRARY_PATH=/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH
to be able to call the timeline library which is used for TensorFlow code profiling.

We use Nvidia Pascal Titan X GPU for all GPU computations.

The 4 scripts that we provide correspond to the following setting:
*BenchmarkStandardConvolution:* Standard convolutional Neural net
Average Time 0.00098s. It runs on the GPU.

*BenchmarkGraphLaplacianConvolutionSparse* of [^fn1]:
Average Time 0.00889. TensorFlow takes advantage of the *pre-computed* Sparse Laplacian Matrix to perform very efficient GPU operations.

*BenchmarkGeometricConvolutionSparse* of [^fn2]:
Average Time 1.67996. TensorFlow cannot use the GPU to perform some sparse matrix operations which related to the construction of the sparse geometric weight matrix used
in Geometric Convolutions.

*BenchmarkGeometricConvolutionDense* of [^fn2]:
Average Time 0.27200. It corresponds to the dense version implemented in the public code of [^fn2]. 
TensorFlow uses GPU computation with dense adjacency matrices. The main drawback is the memory requirements which limit the graph size and the number of features.

