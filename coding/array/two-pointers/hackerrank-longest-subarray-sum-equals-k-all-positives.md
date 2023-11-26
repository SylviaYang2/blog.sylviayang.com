# (Hackerrank) Longest Subarray Sum Equals k (all positives)

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Approach 1: Two pointers - Sliding Window

**Intuition:** We are using a sliding window of two pointers i.e. left and right, since the the values in array are **all positives** as per the constraints.

The **left** pointer denotes the starting index of the subarray and the **right** pointer denotes the ending index. Now as we want the longest subarray, we will move the right pointer in a forward direction every time adding the element i.e. a\[right] to the sum.&#x20;

But when the sum of the subarray crosses k, we will move the left pointer in the forward direction as well to **shrink** the size of the subarray as well as to decrease the sum. Thus, we will consider the length of the subarray whenever the sum becomes **equal** to k.

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

````java
```java
public int maxSubArrayLen(int[] nums, int k) {
        int n = nums.length;
        int left = 0;
        int right = 0;
        int maxLen = 0;
        int sum = nums[0];

        while (right < n) {
            if (left <= right && sum > k) {
                sum -= nums[left];
                left++;
            }

            if (sum == k) {
                maxLen = Math.max(maxLen, right - left + 1);
            }

            right++;
            if (right < n) {
                sum += nums[right];
            }
        }
        return maxLen;
    }
```
````

### Complexity Analysis

**Time Complexity:** O(2\*N), where N = size of the given array.\
**Reason:** The outer while loop i.e. the right pointer can move up to index n-1(_the last index_). Now, the inner while loop i.e. the left pointer can move up to the right pointer at most. So, every time the inner loop does not run for n times rather it can run for n times in total. So, the time complexity will be O(2\*N) instead of O(N2).

**Space Complexity:** O(1) as we are not using any extra space.
