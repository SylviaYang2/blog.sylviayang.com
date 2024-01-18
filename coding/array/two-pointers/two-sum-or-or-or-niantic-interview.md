# Two Sum ||| (Niantic Interview)

What's different than 167. Two Sum || is that this time the input `num[]`has <mark style="color:red;">**duplicates**</mark>.

Return the number of pairs whose values add up to target.

```java
Example:
input: int[] nums = {2,3,3,4,4,5}, target = 7
output: 5
```



```java
public static int numPairs2(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int count = 0;
  
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum < target) {
          left++;
        } else if (sum > target) {
          right--;
        } else {
            int leftEqCount = 1;
            while (nums[left] == nums[left + 1]) {
                left++;
                leftEqCount++;
            }
            int rightEqCount = 1;
            while (nums[right] == nums[right - 1]) {
                right--;
                rightEqCount++;
            }
            count += leftEqCount * rightEqCount;
            left += 1;
            right -= 1;
        }
    }
    return count;
}
```
