# Niantic OA

1. You are given a matrix of characters and an array of distinct strings words , Your task is to implement a simplified string search by determining the number of times in which strings from words can be found in the matrix under the following constraints:

* A string is found when it can be formed by combining characters along some path in the matrix.
* A path may start at any cell, and initially must go from left to right or from top to bottom.
* All paths are allowed to change direction once to go in the opposite direction (i.e., from left -> right to right -> left, or from top -> bottom to bottom -> top).

#### Example 1:

```
char[][] matrix = {{'a', 'a', 'a'},
                  {'a', 'a', 'a'}};
String word = "aaaa";

count should be 4
```

#### Example 2:

```
char[][] matrix2 = {{'a', 'b', 'a', 'c'},
                    {'x', 'a', 'c', 'd'},
                    {'y', 'r', 'd', 's'}};
String[] words = {"ac", "cat", "car", "bar", "acdc", "abacaba"};

count should be 7
```

```java
private static int count(char[][] matrix, String word) {
    for (int i = 0; i < matrix.length; i++) {
      for (int j = 0; j < matrix[i].length; j++) {
        dfs(matrix, word, i, j, "right", false);
        dfs(matrix, word, i, j, "down", false);
      }
    }
    return count;
}

  private static void dfs(char[][] matrix, String word, int i, int j, String dir, boolean turned) {
    if (i < 0 || i >= matrix.length || j < 0 || j >= matrix[i].length) {
      return;
    }
    
    if (word.length() == 1 && word.charAt(0) == matrix[i][j]) {
      count++;
      return;
    }

    if (matrix[i][j] != word.charAt(0)) {
        return;
    }

    if (dir.equals("right")) {
      dfs(matrix, word.substring(1), i, j - 1, "left", true);
      dfs(matrix, word.substring(1), i, j + 1, "right", true);
    } else if (dir.equals("left")) {
      dfs(matrix, word.substring(1), i, j - 1, "left", true);
    } else if (dir.equals("down")) {
      dfs(matrix, word.substring(1), i - 1, j, "up", true);
      dfs(matrix, word.substring(1), i + 1, j, "down", true);
    } else {
      dfs(matrix, word.substring(1), i - 1, j, "up", true);
    }
  }
```

#### Complexity: 0(matrix.length \* matrix\[0].length \* words.length \* max(words\[i].length)



2.  You are given an array of integers numbers. Your task is to count the number of distinct pairs (i, j) such that `0 <= i < j < numbers.length`, and `numbers[i]` can be obtained from `numbers[j]` by swapping no more than two digits of `numbers[j]`

    Note: One may not need to swap any digits in the number at all, so if `i < j` and `numbers[i] = numbers[j]`, the pair should also be counted.

**Example:**

1. For numbers = \[1, 23, 156, 1650, 651, 165, 32], the output should be **`solution(numbers) = 3`**

numbers \[1] = 23 can be obtained from numbers\[6] = 32 by swapping it's only two digits.

numbers\[2]=156 can be obtained from numbers\[4]=651 by swapping its only two digitsby swapping 6 and 1.

numbers\[2]=156 can be obtained from numbers \[5]=165 by swapping 6 and 5.

2. For numbers = \[123, 321, 123] the output should be **`solution(numbers) = 3.`**

numbers\[1] = 321 can be obtained from numbers \[0] = 123 by swapping 1 and 3.

numbers\[2] = 123 can be obtained from numbers\[0] = 123 by not swapping any digits at all.

numbers\[2] = 123 can be obtained from numbers\[1] = 321 by swapping 3 and 1.

{% code overflow="wrap" %}
```java
In this code, we first create a hash map where each key is a sorted representation of a number and each value is a list of indices where numbers with that sorted representation appear. Then, we only compare numbers that share the same sorted representation.

public class DistinctPairs {
    private static int countSwappablePairs(int[] numbers) {
        Map<String, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < numbers.length; i++) {
            char[] chars = String.valueOf(numbers[i]).toCharArray();
            Arrays.sort(chars);
            String sorted = new String(chars);
            map.computeIfAbsent(sorted, k -> new ArrayList<>()).add(i);
        }

        int count = 0;
        for (String key : map.keySet()) {
            List<Integer> indices = map.get(key);
            for (int i = 0; i < indices.size(); i++) {
                for (int j = i + 1; j < indices.size(); j++) {
                    if (canSwap(numbers[indices.get(i)], numbers[indices.get(j)])) {
                        count++;
                    }
                }
            }
        }
        return count;
    }

    private static boolean canSwap(int a, int b) {
        char[] aChars = String.valueOf(a).toCharArray();
        char[] bChars = String.valueOf(b).toCharArray();

        int diffCount = 0;
        for (int i = 0; i < aChars.length; i++) {
            if (aChars[i] != bChars[i]) {
                diffCount++;
                if (diffCount > 2) {
                    return false;
                }
            }
        }
        return true;
    }
}
```
{% endcode %}
