Tags: [[Dimension Reduction]], [[Feature Space]], [[Data Visualization]], , [[Explainable AI]]

### What is UMAP?

Dimensional technique reduction that uses n-neighbor distance, topology simplices/directed graphs, fuzzy probability, and manifold theory to represent high dimensional data (aka high feature/category) in to lower dimensional space while preserving both local and global features.

| Step 1: Scattered points of sin(x)                                            | Step 2: Fuzzy probability b/t neighboring points                      |
| ----------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| ![[Pasted image 20240830132540.png \| 300]]                                   | ![[Pasted image 20240830133908.png\|300]]                             |
| **Step 3: Graph edges to preserve local and global features in the manifold** | **Step 4: representation of the manifold in lower dimensional space** |
| ![[Pasted image 20240830133919.png\|300]]                                     | ![[Pasted image 20240830133937.png\|300]]                             |

### Understanding UMAP Parameters

`n_neighbors`
* The most important parameter is `n_neighbors` - the number of approximate nearest neighbors used to construct the initial high-dimensional graph. It effectively controls how UMAP balances local versus global structure:

	- low values will push UMAP to focus more on local structure by constraining the number of neighboring points considered when analyzing the data in high dimensions
	
	- high values will push UMAP towards representing the big-picture structure while losing fine detail.

`min_dist`
* The second parameter min_dist, or the minimum distance between points in low-dimensional space. This parameter controls how tightly UMAP clumps points together

### Additional Examples

![[Pasted image 20240830135323.png | 400]]
The *figure* above shows UMAP's ability to capture both global and local features of data. In this case the Fashion MNIST Dataset

### Video Link

SciPy Lecture from the Researcher who Created UMAP --  [UMAP | SciPy 2018 |](https://www.youtube.com/watch?v=nq6iPZVUxZU)
