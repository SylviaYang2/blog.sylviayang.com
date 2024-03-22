# Navan OA

1. Given a number line from 0 to n and a string denoting a sequence of moves, determine the number of subsequences of those moves that lead from a given point x to end at another point y. Moves will be given as a sequence of l and r instructions. An instruction l = left movement, so from position j the new position is j - 1, an instruction r = right movement, so from position j the new position would be j + 1. For example, given a number line from 0 to 6, and a sequence of moves rrlrlr, the number of subsequences that lead from 1 to 4 on the number line is 3.

<figure><img src="../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

Sample Case:\
INPUT:\
rrlrlr\
6\
1\
2\
OUTPUT:\
7

**Explanation:**

s1 = "r", the move sequence is 1 -> 2\
s2 = "r", the move sequence is 1 -> 2 -> 3 -> 2\
s3 = "r", the move sequence is 1 -> 2 -> 1 -> 2\
s4 = "r", the move sequence is 1 -> 0 -> 1 -> 2\
s5 = "r", the move sequence is 1 -> 2 -> 3 -> 2 -> 3 -> 2\
s6 = "r", the move sequence is 1 -> 2 -> 1 -> 2 -> 1 -> 2\
s7 = "r", the move sequence is 1 -> 2 -> 3 -> 2 -> 1 -> 2

Hence, output is 7.

Complete the function distinctMoves which must return an integer that represents the number of distinct subsequences. As this number may be large, return the value modulo 10^9 + 7.

distinctMoves has the following parameter(s):\
s : a string that represents a sequence of moves\
n : an integer that represents the upper bound of the number line\
x : an integer that represents the starting point\
y : an integer that represents the ending point

1 <= |s| <= 10^3\
0<= x, y, n <= 2500

```java
import java.util.*;

public class DistinctMovesCalculator {

    private static final int MOD = 1000000007;

    public static void main(String[] args) {
        System.out.println(calculateDistinctMoves("rrlrlr", 6, 1, 3));
    }
    
    public static int calculateDistinctMoves(String moveSequence, int upperBound, int start, int end) {
        // Convert the move sequence string to a character array for easier access
        char[] moves = moveSequence.toCharArray();
        
        // prevSame records the index of the previous occurrence of the same move ('l' or 'r')
        int[] prevSame = new int[moves.length];
        int lastLeftIndex = -1, lastRightIndex = -1;
        
        // Populate prevSame with the last indices of 'l' and 'r' moves
        for (int i = 0; i < moves.length; i++) {
            if (moves[i] == 'l') {
                prevSame[i] = lastLeftIndex;
                lastLeftIndex = i;
            } else {
                prevSame[i] = lastRightIndex;
                lastRightIndex = i;
            }
        }
        
        // dp[i][j] represents the number of distinct subsequences of length i to end up at position j
        long[][] dp = new long[moves.length + 1][upperBound + 1];
        dp[0][start] = 1; // Start position
        
        // Calculate the number of distinct moves for each subsequence length and position
        for (int i = 1; i <= moves.length; i++) {
            for (int j = 0; j <= upperBound; j++) {
                dp[i][j] = dp[i - 1][j]; // Carry over the previous count
                if (moves[i - 1] == 'l' && j + 1 <= upperBound) {
                    dp[i][j] += dp[i - 1][j + 1]; // Add ways to come from the right
                    // Adjust for duplicates if a previous 'l' exists
                    if (prevSame[i - 1] >= 0) dp[i][j] -= dp[prevSame[i - 1]][j + 1];
                } else if (j - 1 >= 0) {
                    dp[i][j] += dp[i - 1][j - 1]; // Add ways to come from the left
                    // Adjust for duplicates if a previous 'r' exists
                    if (prevSame[i - 1] >= 0) dp[i][j] -= dp[prevSame[i - 1]][j - 1];
                }
                dp[i][j] = (dp[i][j] + MOD) % MOD; // Ensure non-negative and within MOD
            }
        }
        
        // Return the total number of distinct subsequences that end at the 'end' position
        return (int) dp[moves.length][end];
    }
}

```



2.  Optimally divide a network of data centers into localized regions. Given a network of g\_nodes data centers and g\_edges bidirectional connections between them, the ith connection connects data centers g\_from\[i] and g\_to\[i] with a latency of g\_ weight\[i]. The max-latency of a network is the maximum latency of any connection. Divide this network into at most k networks by removing some of the connections such that the maximum of the max-latency of all regions formed is minimized. Given g\_nodes, g\_from, and g\_to, find the minimum possible value of the maximum max-latency of the networks formed.

    Example: Suppose g\_nodes = 3, m = 3, k = 2, g\_from = \[1, 2, 3], g\_to = \[2, 3, 1], and g\_ weight = \[4, 5, 3].

    The optimal strategy is to remove connections between 1 and 2, and 2 and 3. The minimum internal latency after this splitting is 3.

<figure><img src="../.gitbook/assets/image (220).png" alt="" width="259"><figcaption></figcaption></figure>

```java
import java.util.*;

public class NetworkDivision {
    static class Edge implements Comparable<Edge> {
        int from, to, weight;

        Edge(int from, int to, int weight) {
            this.from = from;
            this.to = to;
            this.weight = weight;
        }

        public int compareTo(Edge compareEdge) {
            return this.weight - compareEdge.weight;
        }
    }

    static class DisjointSetUnion {
        int[] parent, rank;

        DisjointSetUnion(int n) {
            parent = new int[n];
            rank = new int[n];
            for (int i = 0; i < n; ++i) {
                parent[i] = i;
            }
        }

        int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }

        boolean union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX != rootY) {
                if (rank[rootX] > rank[rootY]) {
                    parent[rootY] = rootX;
                } else if (rank[rootX] < rank[rootY]) {
                    parent[rootX] = rootY;
                } else {
                    parent[rootY] = rootX;
                    rank[rootX] = rank[rootX] + 1;
                }
                return true;
            }
            return false;
        }
    }

    public static int minimumMaxLatency(int gNodes, int[] gFrom, int[] gTo, int[] gWeight, int k) {
        int n = gFrom.length;
        Edge[] edges = new Edge[n];
        for (int i = 0; i < n; i++) {
            edges[i] = new Edge(gFrom[i] - 1, gTo[i] - 1, gWeight[i]); // Adjust for 0-based indexing
        }
        Arrays.sort(edges);

        DisjointSetUnion dsu = new DisjointSetUnion(gNodes);
        int lastWeight = -1;
        int networksFormed = gNodes;

        for (Edge edge : edges) {
            if (dsu.union(edge.from, edge.to)) {
                networksFormed--;
                lastWeight = edge.weight;
                if (networksFormed == k) {
                    break;
                }
            }
        }

        return lastWeight;
    }

    public static void main(String[] args) {
        int gNodes = 3;
        int[] gFrom = {1, 2, 3};
        int[] gTo = {2, 3, 1};
        int[] gWeight = {4, 5, 3};
        int k = 2;

        System.out.println("The minimum possible value of the maximum max-latency is: " +
                minimumMaxLatency(gNodes, gFrom, gTo, gWeight, k));
    }
}

```
