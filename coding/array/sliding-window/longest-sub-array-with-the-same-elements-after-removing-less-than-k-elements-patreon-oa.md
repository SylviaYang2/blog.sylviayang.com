# Longest sub-array with the same elements after removing <= k elements (Patreon OA)

Find the length of the longest sub-array that has the same elements contiguously after removing at most k elements.

* Test 1: speed=\[1,2,1,2,1], k=1, output should be 2. Explanation: it's optimal to delete the second element to get \[1,1,2,1], so that the maximum subarray length should be 2 (\[1,1]) or alternatively we can delete the third element (yields us \[2,2]) or the fourth element (yields us \[1,1]) to archive the same max subarray length.
* Test 2: Speed=\[1,3,2,2,1,1,2], k=3, output should be 3 (\[2,2,2]). Explanation: It's optimal to remove the last second and the last third racers to get \[1,3,2,2,2], even removing 2 racers reaches the optimal longest subarray that has the same elements.
* Test 3: speed=\[1,4,4,2,2,4], k=2, the output should be 3. explanation: it's optimal to remove the two elements with value 2 to get \[1,4,4,4] so that we get a maximum subarray of length 3 (\[4,4,4])

{% code overflow="wrap" %}
```java
import java.util.HashSet;
import java.util.Set;

public class MaxRacers {

    public static void main(String[] args) {
        int[] speed1 = {1, 2, 1, 2, 1};
        int k1 = 1;
        System.out.println(getMaxRacers(speed1, k1)); // Output: 2

        int[] speed2 = {1, 3, 2, 2, 1, 1, 2};
        int k2 = 3;
        System.out.println(getMaxRacers(speed2, k2)); // Output: 3

        int[] speed3 = {1, 4, 4, 2, 2, 4};
        int k3 = 2;
        System.out.println(getMaxRacers(speed3, k3)); // Output: 3
    }

    static int getMaxRacers(int[] speed, int k) {
        // Initialize variables to track the max length of a sub-array
        int maxLength = 0;

        // Iterate over each type of speed as a target speed to achieve in the sub-array
        Set<Integer> uniqueSpeeds = new HashSet<>();
        for (int s : speed) {
            uniqueSpeeds.add(s);
        }

        for (int targetSpeed : uniqueSpeeds) {
            int left = 0;  // Left pointer for the sliding window
            int right = 0;  // Right pointer for the sliding window
            int countDiff = 0;  // Count of different speed elements in the current window
            int maxCount = 0;  // Count of the target speed elements in the current window

            // Iterate over the speed array to expand the window
            while (right < speed.length) {
                // Include the current element in the window
                if (speed[right] == targetSpeed) {
                    maxCount++;
                } else {
                    countDiff++;
                }

                // If there are more different elements than k, move the left pointer
                while (countDiff > k) {
                    if (speed[left] == targetSpeed) {
                        maxCount--;
                    } else {
                        countDiff--;
                    }
                    left++;
                }

                // Update the maxLength of the window
                maxLength = Math.max(maxLength, maxCount);
                right++;
            }
        }

        return maxLength;
    }
}

```
{% endcode %}

### Complexity Analysis:

*   Time complexity: $$O(N^2)$$

    1. **Outer Loop Over Unique Speeds:**
       * The outer loop runs once for each unique speed in the `speed` array. If `S` is the number of unique speeds, this loop runs `S` times. In the worst case, where all speeds are unique, `S` would be `n`, where `n` is the length of the `speed` array.
    2. **Inner While Loop:**
       * The inner while loop moves the `right` pointer across the array once for each unique speed, so it runs `n` times for each unique speed. However, because the `left` pointer also moves to the right and never resets for each unique speed, each element is visited at most twice by the `left` and `right` pointers (once when expanding the window and once when shrinking it). Hence, the while loop contributes at most `2n` operations per unique speed.
    3. **Shrinking the Window:**
       * The innermost while loop, which shrinks the window, can move the `left` pointer across the array at most once for each unique speed. This contributes at most `n` operations per unique speed.

    Combining these considerations, the worst-case total operations are about `S * (2n + n) = S * 3n`. In the worst case where all speeds are unique, `S = n`, leading to a time complexity of `O(n^2)`.

    *   Space complexity: $$O(N)$$

        The space complexity is `O(S)` due to the storage of unique speeds in a set. In the worst case, this is `O(n)` where `n` is the number of elements in the `speed` array if all speeds are unique. There is also a constant extra space used for variables like `left`, `right`, `count_diff`, etc., but this does not depend on the size of the input and thus does not scale with `n`.
