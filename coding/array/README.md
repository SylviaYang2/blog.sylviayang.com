# Array

1. **Interview Tip**: Whenever you're trying to solve an array problem **in-place**, always consider the possibility of iterating **backwards** instead of forwards through the array. It can completely change the problem, and make it a lot easier.
2. A lot of the problems with **subarrays equal to / greater than / less than a target** generally use one of 1 strategies for the optimal solution -&#x20;

* **sliding window** with 2 pointers&#x20;
  * given list is **all** positive or negative
    * Actually 53. can be solved using sliding window
  *   想用滑动窗口算法，先问自己几个问题：

      1、什么时候应该扩大窗口？

      2、什么时候应该缩小窗口？

      3、什么时候更新答案？

{% content-ref url="dynamic-programming/53.-maximum-subarray-medium.md" %}
[53.-maximum-subarray-medium.md](dynamic-programming/53.-maximum-subarray-medium.md)
{% endcontent-ref %}

* no restriction on the number of target elements (e.g. 525)

{% content-ref url="prefix-sum-qian-zhui-he/prefix-sum-+-hashmap/525.-contiguous-array-medium.md" %}
[525.-contiguous-array-medium.md](prefix-sum-qian-zhui-he/prefix-sum-+-hashmap/525.-contiguous-array-medium.md)
{% endcontent-ref %}

* **hashmap** storing cumulative sum (**prefix sum**)
  * if don't know the sign of the numbers, better to use a hashmap since you cannot anticipate what expanding/contracting a window might do

3. 通常我们遍历子串或者子序列有三种遍历方式:

* 以某个节点为开头的所有子序列: 如 \[a]，\[a, b]，\[ a, b, c] ... 再从以 b 为开头的子序列开始遍历 \[b] \[b, c]。&#x20;
* 根据子序列的长度为标杆，如先遍历出子序列长度为 1 的子序列，在遍历出长度为 2 的 等等。
* 以子序列的**结束节点**为基准，先遍历出以某个节点为结束的所有子序列，因为每个节点都可能会是子序列的结束节点，因此要遍历下整个序列，如: 以 b 为结束点的所有子序列: \[a , b] \[b]。以 c 为结束点的所有子序列: \[a, b, c] \[b, c] \[ c ]。
*   第一种遍历方式通常用于暴力解法, 第二种遍历方式 leetcode (5. 最长回文子串 ) 中的解法就用到了。第三种遍历方式 因为可以产生递推关系, 采用**DP**时, 经常通过此种遍历方式, 如：背包问题, 最大公共子串...





## Find all Subsequence：

{% code overflow="wrap" fullWidth="true" %}
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Subsequences {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};
        List<List<Integer>> subsequences = findSubsequences(arr);
    }

    public static List<List<Integer>> findSubsequences(int[] arr) {
        List<List<Integer>> result = new ArrayList<>();
        generateSubsequences(arr, 0, new ArrayList<>(), result);
        return result;
    }

    public static void generateSubsequences(int[] arr, int index, List<Integer> current, List<List<Integer>> result) {
        result.add(new ArrayList<>(current)); // Include the current subsequence

        for (int i = index; i < arr.length; i++) {
            current.add(arr[i]); // Include the current element in the subsequence
            generateSubsequences(arr, i + 1, current, result); // Recursively generate subsequences
            current.remove(current.size() - 1); // Backtrack by removing the last element
        }
    }
}
```
{% endcode %}

#### Complexity Analysis:

Let's analyze the time and space complexity of this algorithm:

1. **Time Complexity:**
   * There are (2^n) possible subsequences for an array of length (n). For an array of length n, there are 2^n possible subsequences. This is because each element in the array can either be included or excluded in a subsequence, leading to 2^n combinations.
2. **Space Complexity:**
   * In the worst case, there are (2^n) subsequences, each of length (O(n)).
   * Therefore, the space complexity is (O(2^n \* n)).



## Array Permutation:

1. **Choosing Elements:**
   * At each level of recursion, you choose an element to be placed at a specific position in the permutation.
   * The number of choices you have at each level is the number of remaining elements in the array.
2. **Levels of Recursion:**
   * The levels of recursion correspond to the positions in the permutation.
   * The first level chooses the element for the first position, the second level for the second position, and so on.
3. **Total Permutations:**
   * The total number of permutations is the product of the number of choices at each level.
   * The number of choices at each level decreases because you've already chosen elements for previous positions.
4. **Factorial Relationship:**
   * The total number of permutations is $$N×(N−1)×(N−2)×…×1$$, which is $$n!$$.

{% code overflow="wrap" %}
```java
import java.util.Arrays;

public class Permutations {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};
        generatePermutations(arr, 0);
    }

    public static void generatePermutations(int[] arr, int index) {
        if (index == arr.length - 1) {
            return;
        }

        for (int i = index; i < arr.length; i++) {
            swap(arr, index, i); 
            generatePermutations(arr, index + 1);
            swap(arr, index, i); // Backtrack
        }
    }

    public static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}

```
{% endcode %}

1. **Choose an Element for the Current Position:**
   * The algorithm iterates over the remaining elements in the array, choosing one element at a time to be placed at the current position.
2. **Swap the Chosen Element with the Current Position:**
   * The chosen element is swapped with the element at the current position to temporarily place it in the desired position.
3. **Recursively Generate Permutations for the Next Position:**
   * Recursively call the `generatePermutations` function to generate permutations for the next position (increment the `index`).
4. **Backtrack:**
   * After generating permutations for the next position, backtrack by swapping the elements back to their original positions.
   * This ensures that when the algorithm returns from the recursive call, the array is in the same state as before the recursion.
