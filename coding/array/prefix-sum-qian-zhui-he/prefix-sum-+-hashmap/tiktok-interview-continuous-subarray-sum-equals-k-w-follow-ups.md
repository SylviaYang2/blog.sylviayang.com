# TikTok Interview: Continuous Subarray Sum Equals K (w/ follow-ups)

Given an array of integers `nums` and an integer `k`, return _<mark style="color:red;">**true/false**</mark> whether there is a subarray whose sum equals to_ `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

<pre><code><strong>Input: nums = [1,1,1], k = 2
</strong><strong>Output: 2
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [1,2,3], k = 3
</strong><strong>Output: 2 
</strong></code></pre>

**Constraints:**

* `1 <= nums.length <= 2 * 104`
* `-1000 <= nums[i] <= 1000`
* `-107 <= k <= 107`



### 1. Return True/False

```java
import java.util.HashSet;
import java.util.Set;

public class SubarraySum {
    public static boolean subarraySumEqualsK(int[] nums, int k) {
        int currentSum = 0;
        Set<Integer> seenSums = new HashSet<>();
        // need to initialize the set with 0!
        seenSums.add(0);

        for (int num : nums) {
            currentSum += num;

            // Check if (currentSum - k) exists in the set
            if (seenSums.contains(currentSum - k)) {
                return true;
            }

            seenSums.add(currentSum);
        }

        return false;
    }
}

```

### 2. Follow-up: return the entire subarray

<figure><img src="../../../../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

```java
import java.util.HashMap;
import java.util.Map;

public class SubarraySum {

    public static int[] findSubarraySumEqualsK(int[] nums, int k) {
        Map<Integer, Integer> sumIndices = new HashMap<>();
        int currentSum = 0;

        for (int i = 0; i < nums.length; i++) {
            currentSum += nums[i];

            if (currentSum == k) {
                return Arrays.copyOfRange(nums, 0, i + 1);
            }

            if (sumIndices.containsKey(currentSum - k)) {
                int start = sumIndices.get(currentSum - k) + 1;
                int end = i;
                return Arrays.copyOfRange(nums, start, end + 1);
            }
            sumIndices.put(currentSum, i);
        }
        return new int[]{}; // Return an empty array if no such subarray is found
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3, 4, 5};
        int k = 9;
        int[] result = findSubarraySumEqualsK(nums, k);
        System.out.println(Arrays.toString(result)); // Output: [2, 3, 4]
    }
}

```

### Complexity

* Time: O(n)
* Space: O(n)
