SalesmansDilema
######

This problem is SRM270 from Topcoder, you can view it by launching the Arena and (assuming there isn't an active contest) clicking on practice -> SRM -> 270.


TODO- need the problem statement

I solution I came up with was 2 parts. First check if the graph has loops which can cause endless profit or if the destination is unreachable. If neither of those was true, then calculate the maximum profit possible while reaching the destination.

For the first part a modified form of graph traversal was used. I adapted BFS (breadth first search) for this part, but I think DFS, or pre/in/post-order could have been chosen instead and that have worked also. For this you need to use some extra data structures in addition to the normal BFS structures (a queue of which towns to visit next). The extra information you need to store is: the towns which have been visited (a boolean array "seen"), and the profit at each town which have been visited (an int array "profitAtTown"). The modification to BFS follows next. 

When a town is visited (eg: townX), if it's the first time to visit that town, you update "seen" to true. At this point you also store the profit made as the town is visited, you set profitAtTown[townX] = currentProfit
/// code 
seen[townX] = true;
profitAtTown[townX] = currentProfit;
/// code
If instead a town is visited which has been seen before (ie: seen[townX] is true on arrival to the town), then that means there is a cycle in the graph. If the profitAtTown is less than the current profit it means that a profitable cycle has been found! At this point, the result is known so the graph traversal could stop and the result be printed out.

If once graph traversal has ended and seen[destination] is false then that means the final destination is not reachable so the program should output "IMPOSSIBLE" and exit. If neither of the above conditions is true, then that means there is no profitable cycle and the destination is reachable. To find the most profitable solution I just used Dijkstra's algorithm for this.

That's it, a short blog post on how I solved SRM270 - SalesmansDilemma. My source code for this is available at https://github.com/willb611/topcoder/blob/master/srm270/SalesmansDilemma.java.
  