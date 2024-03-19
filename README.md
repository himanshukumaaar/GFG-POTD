DATE * 19 MARCH 2024* 
//Possible Paths in a Tree//

Given a weighted tree with n nodes and (n-1) edges. You are given q queries. Each query contains a number x. For each query, find the number of paths in which the maximum edge weight is less than or equal to x.

Note: Path from A to B and B to A are considered to be the same.

Example 1:
Input: 
n = 3
edges {start, end, weight} = {{1, 2, 1}, {2, 3, 4}}
q = 1
queries[] = {3}
Output: 
1
Explanation:
Query 1: Path from 1 to 2

Input: 
n = 3
edges {start, end, weight} = {{1, 2, 1}, {2, 3, 4}}
q = 1
queries[] = {3}
Output: 
1
Explanation:
Query 1: Path from 1 to 2

Your Task:  
You don't need to read input or print anything. Complete the function maximumWeight()which takes integers n, list of edges where each edge is given by {start,end,weight}, an integer q and a list of q queries as input parameters and returns a list of integers denoting the number of possible paths for each query. 

Expected Time Complexity: O(nlogn + qlogn)
Expected Auxiliary Space: O(n)

Constraints:
2 ≤ n ≤ 104
1 ≤ q ≤ 104
1 ≤ edges[i][0], edges[i][1] ≤ n
edges[i][0] != edges[i][1]
0 ≤ edges[i][2] ≤ 105
0 ≤ queries[i] ≤ 105.


*SOLUTION*

class Solution:
    def maximumWeight(self, n, edges, q, queries):
        # code here
        store = [i for i in range(n+1)]
        sz = [1 for _ in range(n+1)]
        sz[0] = 0
        
        def find(i):
            if store[i] != i:
                store[i] = find(store[i])
            return store[i]
            
        
        edges.sort(key=lambda x: x[2])

        queries = sorted(enumerate(queries), key=lambda x: x[1])
        
        ans = [0]*len(queries)
        start = 0
        acc = 0
        for i, q in queries:
            while start < len(edges) and edges[start][2] <= q:
                x, y, _ = edges[start]
                rx, ry = find(x), find(y)
                if rx != ry:
                    acc += sz[rx]*sz[ry]
                    sz[rx] += sz[ry]
                    sz[ry] = 0
                    store[ry] = rx
                start += 1
            ans[i] = acc
        
        return ans
