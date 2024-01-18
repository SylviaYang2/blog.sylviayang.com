# Print All Increasing Subsequences (Niantic Interview)

Given a list or array of integer, the task is to print **all such subsequences** of this list such in which the elements are arranged in increasing order.\
A _Subsequence_ of the list is an ordered subset of that list’s element having same sequential ordering as the original list. \
**Examples:**&#x20;

> **Input:** arr = {1, 2]} \
> **Output:** \
> 2 \
> 1 \
> 1 2\
> **Input:** arr = {1, 3, 2} \
> **Output:** \
> 2 \
> 3 \
> 1 \
> 1 2 \
> 1 3&#x20;

### Approach: Backtrack

#### Note: Need to handle duplicates -> Use a Set

`For example, input: [1, 1, 2, 2], the output should be: [[1], [2], [1, 2]]`

{% code overflow="wrap" %}
```java
public class IncreasingSubsequences {
    Set<List<Integer>> result = new ArrayList<>();
    public static List<List<Integer>> findIncreasingSubsequences(int[] arr) {
        backtrack(arr, 0, new ArrayList<>());
        return result;
    }

    private static void backtrack(int[] arr, int currentIndex, List<Integer> currentSubseq) {
        if (currentSubseq.size() > 1) {
            result.add(new ArrayList<>(currentSubseq));
        }

        for (int i = currentIndex; i < arr.length; i++) {
            if (currentSubseq.isEmpty() || arr[i] > currentSubseq.get(currentSubseq.size() - 1)) {
                currentSubseq.add(arr[i]);
                backtrack(arr, i + 1, currentSubseq, result);
                currentSubseq.remove(currentSubseq.size() - 1);
            }
        }
    }
}

```
{% endcode %}

