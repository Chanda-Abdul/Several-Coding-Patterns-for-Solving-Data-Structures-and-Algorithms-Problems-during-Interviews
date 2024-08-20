https://leetcode.com/discuss/study-guide/3903992/graph-its-implementation-and-some-popular-bfsdfs-problems

**BFS**

```
import java.util.*;

class Solution {
    public List<Integer> bfsOfGraph(int n, List<List<Integer>> adj) {
        // Create a visited array to keep track of visited nodes
        boolean[] vis = new boolean[n];

        // 1. Pick a starting node, push it into the queue, and mark it as visited.
        Queue<Integer> q = new LinkedList<>();
        vis[0] = true;
        q.add(0);
        List<Integer> ans = new ArrayList<>();

        // 4. Repeat steps 2 and 3.
        while (!q.isEmpty()) {

            // 2. In every iteration, pop out the 'x' node and put it in the solution list.
            int node = q.poll();
            ans.add(node);

            // 3. All the unvisited adjacent nodes from 'x' are pushed into the queue.
            for (int a : adj.get(node)) {
                if (!vis[a]) {
                    vis[a] = true;
                    q.add(a);
                }
            }
        }
        return ans;
    }


```

**DFS**

```
class Solution {
  public:
  
    void dfs(int node, vector<int>adj[], vector<bool>&vis, vector<int>& ans) {
        vis[node] = 1;
        ans.push_back(node);
        
        for(auto a:adj[node]) {
            if(!vis[a]) {
                dfs(a,adj,vis,ans);
            }
        }
    }
  
    // Function to return a list containing the DFS traversal of the graph.
    vector<int> dfsOfGraph(int n, vector<int> adj[]) {
        vector<bool>vis(n,0);
        int s = 0;
        vector<int>ans;
        dfs(s, adj, vis, ans);
        return ans;
    }
};
```
