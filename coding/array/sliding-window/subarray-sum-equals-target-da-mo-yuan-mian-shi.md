# Subarray Sum Equals Target (达摩院面试)

Given a non-empty array `nums` contains positive integers and a positive integer `target`.\
Find the first subarray in `nums` that sums up to `target` and return the **begin and end index** of this subarray. If there is no such subarray, return `[-1, -1]`.

**Example 1:**

{% code overflow="wrap" %}
```
Input: nums = [4, 3, 5, 7, 8], target = 12
Output: [0, 2]
Explanation: 4 + 3 + 5 = 12. Although 5 + 7 = 12, [4, 3, 5] is the first subarray that sums up to 12.
```
{% endcode %}

**Example 2:**

```
Input: nums = [1, 2, 3, 4], target = 15
Output: [-1, -1]
Explanation: Thers is no such subarray that sums up to 15.
```

### Approach: Sliding Window

```java
private static int[] find(int[] a, int target) {
    if (a.length < 2 || target < 0) {
      return new int[]{-1,-1};
    }
    int start = 0, end = 1, n = a.length;
    int runningSum = a[start] + a[end];
    while (end < n) {
      if (runningSum == target) {
        return new int[]{start,end};
      } else if (runningSum > target) {
        runningSum -= a[start];
        start++;
      } else if (runningSum < target) {
        end++;
        if (end < n) {
          runningSum += a[end];
        }			
      }
    }
    return new int[]{-1,-1};
}
```

