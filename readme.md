# PageRank Algorithm  
![Mi proyecto](assets/PageRank.jpeg)
## Description  

This script calculates the PageRank of nodes in a directed graph, which is a ranking algorithm originally developed for web page ranking. The script processes input graph data, applies iterative calculations, and outputs the ranks of each node based on their relative importance.  

## Features  

- Efficient PageRank computation using sparse matrix operations.  
- Configurable damping factor (`β`) to adjust the balance between link-based ranking and random jumps.  
- Handles graphs with dangling nodes (nodes with no outgoing links).  
- Outputs ranked results with convergence details.  

## Prerequisites  

1. **Python**: Requires Python 3.x.  
2. **Dependencies**: Install required libraries:  
   ```bash  
   pip install numpy scipy  
   ```  

## Input Format  

The input file must define nodes and edges of the graph:  

- **Nodes**:  
   ```
   n <node_number> <node_URL>
   ```  

- **Edges**:  
   ```
   e <source_node> <destination_node>
   ```  

### Example Input File  
```
n 0 http://www.example.com  
n 1 http://www.sample.com  
e 0 1  
e 1 0  
```  

Save this data in a file (e.g., `graph.txt`) before running the script.  

## How to Run  

Run the script from the terminal:  

```bash  
python processPageRank_assignment.py <input_file>  
```  

### Example  
```bash  
python processPageRank_assignment.py gr0.California.txt  
```  

### Optional Parameters  

- **Custom Damping Factor (β)**:  
   Adjust the damping factor using the `--beta` argument (default is `0.8`).  
   ```bash  
   python processPageRank_assignment.py gr0.California.txt --beta 0.85  
   ```  

## Parameter \( \beta \)  

The β (beta) parameter is a damping factor that influences how rankings are distributed among pages:  

- **High β values**:  
  - Greater importance is given to outgoing links.  
  - Pages with more incoming links receive higher rankings.  
  - Reduces the probability of random jumps.  

- **Low β values**:  
  - Increases the likelihood of random jumps.  
  - Benefits pages with fewer incoming links, giving them better chances to be ranked.  

### Optimization Solution  

The graph matrix is normalized by dividing each link rank by the number of outgoing links. If a node has no outgoing links, the total number of nodes is used:  
```python  
matrix = graph / numpy.where(out_degree == 0, graph.shape[0], out_degree)
```  

New rankings are calculated using the dot product of the transposed matrix and the current rankings:  
```python  
new_ranks = beta * matrix.transpose().dot(ranks)
```  

## Conclusions  

- **High β values**: Pages with more incoming links are more valued, with fewer opportunities for random jumps. This can result in slower convergence.  
- **Low β values**: Increases random jumps, benefiting less-connected pages and speeding up convergence.  

## Output  

The script displays:  
1. **Convergence Time**: The time taken for the algorithm to stabilize.  
2. **Ranked Pages**: Nodes sorted by their PageRank scores.  

Example Output:  
```
It took 3.24 seconds to converge  
[0] http://www.example.com: 0.423000e+00  
[1] http://www.sample.com: 0.577000e+00  
```  

## How It Works  

1. **Input Parsing**: Reads nodes and edges from the input file.  
2. **Matrix Construction**: Creates a sparse adjacency matrix for the graph.  
3. **PageRank Algorithm**: Iteratively computes ranks until changes are below a threshold (`epsilon`).  
4. **Output**: Displays sorted PageRank results.  

## License  

This project is open-source and available under the MIT License.