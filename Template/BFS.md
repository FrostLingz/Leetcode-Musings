# BFS
### The essence of BFS: It is essentially a "diagram" that allows you to go from a starting point to an end point, asking for the shortest path.

```Java
// Calculate the shortest path from start to target
int BFS(Node start, Node target) {
    Queue<Node> q; // datastructurer
    Set<Node> visited; // avoid repeating
    
    q.offer(start); // Add the starting point to the queue
    visited.add(start);
    int step = 0; // Record the number of steps spread

    while (q not empty) {
        int sz = q.size();
        
        /* Spread all nodes in the current queue in all directions */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            
            /* determine whether the target is reached */
            if (cur is target)
                return step;
                
            /*  Add neighboring nodes of cur to the queue */
            for (Node x : cur.adj()) {
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
            }
        }
        /* Update the number of steps */
        step++;
    }
}
```
