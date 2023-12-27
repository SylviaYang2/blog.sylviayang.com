# Minimum Knight Moves (No Boundaries)

{% code overflow="wrap" %}
```java
import java.util.LinkedList;
import java.util.Queue;

public class KnightShortestPath {

    public static int findShortestPath(int startX, int startY, int targetX, int targetY) {
        // Define possible move offsets
        int[][] moveOffsets = {{-2, -1}, {-1, -2}, {1, -2}, {2, -1}, {2, 1}, {1, 2}, {-1, 2}, {-2, 1}};

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{startX, startY, 0}); // Initial position and move count

        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int x = current[0];
            int y = current[1];
            int moves = current[2];

            if (x == targetX && y == targetY) {
                return moves;
            }

            for (int[] offset : moveOffsets) {
                int newX = x + offset[0];
                int newY = y + offset[1];
                queue.add(new int[]{newX, newY, moves + 1});
            }
        }

        // If no path is found
        return -1;
    }
}
```
{% endcode %}

### Complexity:

Time Complexity: **`O(8^n)` (8 possible directions, n is the number of moves)**
