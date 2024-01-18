# Snowflake OA

### Unequal Elements&#x20;

The problem requires us to find the maximum length of a subsequence from the `skills` array such that there are at most `k` instances where consecutive elements in the subsequence are different. A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

The goal is to find the maximum length of a subsequence of skills such that there are no more than k unequal adjacent elements in the subsequence. Formally, find a subsequence of skills, call it x, of length m such that there are at most k indices where x\[i] != x\[i+1] for all 0 <= i < m.&#x20;

**Example**:

skills = \[1, 1, 2, 3, 2, 1], k = 2.&#x20;

The longest possible subsequence is x = \[1, 1, 2, 2, 1]. There are only two indices where x\[1] != x\[2] and x\[3]!= x\[4]. Return its length, 5.&#x20;

**Function Description**: Complete the function findMaxLength in the editor below.&#x20;

findMaxLength has the following parameter(s): int skills\[n]: the different skill types;

int k: the maximum count of unequal adjacent elements.

**Returns** int: the maximum value of m



### **Dynamic Programming Approach**

We use a dynamic programming (DP) approach where `dp[i][j]` represents **the length** of the longest valid subsequence ending **at index `i` with `j` changes** (where a change is defined as an instance where two consecutive elements in the subsequence are different).

**Steps**

1. **Initialization:**
   * We start by initializing a 2D array `dp` with dimensions `n` (length of `skills`) and `k + 1` (since the number of changes can range from 0 to `k`).
   * The first element of each subsequence can form a valid subsequence of length 1 with any number of changes from 0 to `k`, so we initialize `dp[0][j]` to 1 for all `j`.
2. **Building the DP Table:**
   * We iterate over the `skills` array starting from index 1, as we've already initialized index 0.
   * For each element at index `i`, we consider every possible number of changes `j` from 0 to `k`.
3. **Filling DP Entries:**
   * For each `dp[i][j]`, we have three cases:
     * **Case 1:** If the current skill is the same as the previous skill (`skills[i] == skills[i - 1]`), then the current skill can be added to the subsequence without increasing the number of changes. So, `dp[i][j] = dp[i - 1][j] + 1`.
     * **Case 2:** If the current skill is different, we can include it by using one of our changes (if `j > 0`). This means `dp[i][j] = dp[i - 1][j - 1] + 1`.
     * **Case 3:** We also consider the possibility of excluding the current skill to see if a longer valid subsequence can be formed. This is especially relevant when the current skill cannot be included without exceeding the change limit. In this case, we compare with `dp[i - 1][j + 1]` if `j < k`.
4. **Finding the Maximum Length:**
   * After filling the entire `dp` table, the maximum length of the valid subsequence is the maximum value in the last row of the `dp` table, as it represents the longest subsequence that can be formed ending at each index with a certain number of changes.
5. **Return Result:**
   * We iterate through the last row of the `dp` table to find the maximum value, which is the length of the longest subsequence satisfying the given condition.

{% code overflow="wrap" %}
```java
public class MaxLengthSubsequence {
    
    public static int findMaxLength(int[] skills, int k) {
        int n = skills.length;
        // dp[i][j] will store the length of the longest subsequence ending at index i with j changes
        int[][] dp = new int[n][k + 1];

        // Initialize the first element's values
        for (int j = 0; j <= k; j++) {
            dp[0][j] = 1;
        }

        for (int i = 1; i < n; i++) {
            for (int j = 0; j <= k; j++) {
                // Case 1: Keep the current skill (no change)
                if (skills[i] == skills[i - 1]) {
                    dp[i][j] = dp[i - 1][j] + 1;
                } else {
                    // Case 2: Change the current skill
                    if (j > 0) {
                        dp[i][j] = dp[i - 1][j - 1] + 1;
                    }
                }

                // Case 3: Exclude the current skill
                if (j < k) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j + 1]);
                }
            }
        }

        // The answer is the maximum value in the last column of dp table
        int maxLength = 0;
        for (int j = 0; j <= k; j++) {
            maxLength = Math.max(maxLength, dp[n - 1][j]);
        }
        return maxLength;
    }

    public static void main(String[] args) {
        int[] skills = {1, 1, 2, 3, 2, 1};
        int k = 2;
        System.out.println("Maximum Length of Subsequence: " + findMaxLength(skills, k));
    }
}
```
{% endcode %}

### Time Complexity

This dynamic programming solution has a time complexity of O(n \* k), which is quite efficient for moderate values of `n` and `k`. The space complexity is also O(n \* k) due to the 2D DP table used.





### Sequential String&#x20;

A special string s of length n consists of characters 0-9 only. Its characters can only be accessed sequentially i.e the first '1' chosen is the leftmost '1' in s. There is an array arr of m strings, also consisting of characters 0-9. Calculate the minimum number of characters needed from s to construct a permutation of each of the strings in arr.&#x20;

Return an array of integers where the ith element denotes the minimum length of a substring that contains a permutation of the ith string in arr. If a string cannot be constructed, return -1 at that index.

**Example**:&#x20;

Consider n= 12, s= "064819848398", m = 3, arr = \["088", "364", "07"]&#x20;

• To construct "088", Alice needs to access the first 7 characters ("0648198") of the special string and use only '0', '8, and '8'. Since the characters can be rearranged, the results for '088', '808', and '880' are all 7.&#x20;

• To construct "364", access the first 10 characters ("0648198483") of the special string and use only '6', '4', and '3'. Rearrange to match "364".&#x20;

• String "07" cannot be constructed from the special string. No '7' is available.&#x20;

The return array is \[7, 10, -1]. Note that only bolded characters are used to construct the strings.

{% code overflow="wrap" %}
```java
import java.util.HashMap;
import java.util.Map;

public class SequentialString {
    public static void main(String[] args) {
        String s = "064819848398";
        String[] arr = {"088", "364", "07"};
        int[] result = minLengthForPermutationSequentialFromStart(s, arr);

        // Print the result
        for (int length : result) {
            System.out.println(length);
        }
    }

    private static int[] minLengthForPermutationSequentialFromStart(String s, String[] arr) {
        int[] result = new int[arr.length];

        for (int i = 0; i < arr.length; i++) {
            result[i] = getMinLength(s, arr[i]);
        }

        return result;
    }

    private static int getMinLength(String s, String target) {
        Map<Character, Integer> targetCount = new HashMap<>();
        Map<Character, Integer> currentCount = new HashMap<>();
        for (char c : target.toCharArray()) {
            targetCount.put(c, targetCount.getOrDefault(c, 0) + 1);
            currentCount.put(c, 0);
        }

        int charNeeded = target.length();
        int right = 0;

        while (right < s.length() && charNeeded > 0) {
            char c = s.charAt(right);
            if (currentCount.containsKey(c)) {
                if (currentCount.get(c) < targetCount.get(c)) {
                    charNeeded--;
                }
                currentCount.put(c, currentCount.get(c) + 1);
            }
            right++;
        }

        return charNeeded == 0 ? right : -1;
    }
}
```
{% endcode %}



### String Patterns&#x20;

Given the length of a word (wordLen) and the maximum number of consecutive vowels that it can contain (max Vowels), determine how many unique words can be generated. Words will consist of English alphabetic letters a through z only. Vowels are v. (a, e, i, o, u); consonants are c: the remaining 21 letters. In the explanations, v and c represent vowels and consonants.&#x20;

Example 1: wordLen = 1, maxVowels = 1, Patterns: (v, c)&#x20;

That means there are 26 possibilities (v has 5 possibilities and c has 21 posibilities.), one for each letter in the alphabet.&#x20;

Example 2: wordLen = 2, maxVowels = 1, Patterns: {vc, cv, cc}. There is a vowel in the first position, the second position or no position. The total number of unique words is (5 \* 21) + (21 \* 5) + (21 \* 21) = 651 and 651 modulo 1000000007 = 651.

Example 3: wordLen = 4, maxVowels = 1, Patterns: {cccc, vccc, cvcc, ccvc, cccv, vcvc, cvcv, vccv}.

There are 412,776 possibilities



{% code overflow="wrap" %}
```java
public class UniqueWordsCalculator {

    private static final int TOTAL_VOWELS = 5;
    private static final int TOTAL_CONSONANTS = 21;
    private static final int MODULUS = 1000000007;

    public static int countWords(int wordLen, int maxVowels) {
        int[][] dp = new int[wordLen + 1][maxVowels + 1];

        // Base case: 0 length word
        dp[0][0] = 1;

        // Build the table
        for (int i = 1; i <= wordLen; i++) {
            for (int j = 0; j <= maxVowels; j++) {
                // Add consonant
                dp[i][0] = (int)((dp[i][0] + (long)dp[i - 1][j] * TOTAL_CONSONANTS) % MODULUS);

                // Add vowel if j < maxVowels
                if (j < maxVowels) {
                    dp[i][j + 1] = (int)((dp[i][j + 1] + (long)dp[i - 1][j] * TOTAL_VOWELS) % MODULUS);
                }
            }
        }

        // Sum all possibilities for the given length
        long totalPossibilities = 0;
        for (int j = 0; j <= maxVowels; j++) {
            totalPossibilities = (totalPossibilities + dp[wordLen][j]) % MODULUS;
        }

        return (int)totalPossibilities;
    }

    public static void main(String[] args) {
        System.out.println("Unique words (wordLen = 4, maxVowels = 1): " + countWords(4, 1));
        System.out.println("Unique words (wordLen = 1, maxVowels = 1): " + countWords(1, 1));
        System.out.println("Unique words (wordLen = 2, maxVowels = 1): " + countWords(2, 1));
    }
}
```
{% endcode %}



### Smallest Set Covering Intervals&#x20;

Given a series of integer intervals, determine the size of the smallest set that contains at least 2 integers within each interval.&#x20;

Example : first = \[0, 1, 2], last = \[2, 3, 3].

The intervals start at first\[i] and end at last\[i].&#x20;

Both intervals 0 and 1 contain 1 and 2. These are the only two integers that interval 0 shares, so both integers must be included in the answer set. Both intervals 1 and 2 contain 2 and 3. These are the only two values that interval 2 shares. The smallest set that contains at least 2 integers from each interval is {1, 2, 3}.&#x20;

Function Description: Complete the function interval. interval has the following parameter: int first\[n]: each element represents the start of interval\[i], int last\[n]: each element represents the end of interval\[i].

Returns: int: the size of the smallest interval possible

```java
import java.util.*;
 
class GFG {
 
    // Sort with respect to end point
    public static int compare(List<Integer> a,
                              List<Integer> b)
    {
        if (a.get(1).equals(b.get(1))) {
            return a.get(0).compareTo(b.get(0));
        }
        else {
            return a.get(1).compareTo(b.get(1));
        }
    }
 
    public static int
    intersectionSizeTwo(List<List<Integer> > intervals)
    {
        int n = intervals.size();
 
        // Sort the array
        Collections.sort(intervals, GFG::compare);
        List<Integer> res = new ArrayList<>();
 
        // Known two rightmost point
        // in the set/res
        res.add(intervals.get(0).get(1) - 1);
        res.add(intervals.get(0).get(1));
 
        for (int i = 1; i < n; i++) {
 
            int start = intervals.get(i).get(0);
            int end = intervals.get(i).get(1);
 
            // Means there is no common between
            // curr interval and intervals
            // before this
            if (start > res.get(res.size() - 1)) {
                res.add(end - 1);
                res.add(end);
            }
 
            // Atleast 1 value from current
            // interval matches with previous
            // sets just add 1 max value
            else if (start > res.get(res.size() - 2)) {
                res.add(end);
            }
        }
        return res.size();
    }
 
    // Driver Code
    public static void main(String[] args)
    {
        // ranges
        List<List<Integer> > range = Arrays.asList(
            Arrays.asList(3, 6), Arrays.asList(2, 4),
            Arrays.asList(0, 2), Arrays.asList(4, 7));
 
        // Function Call
        System.out.println(intersectionSizeTwo(range));
    }
}
```

Here is the modified Java code:

```java
import java.util.*;

class GFG {

    // Sort with respect to end point
    public static int compare(List<Integer> a, List<Integer> b) {
        if (a.get(1).equals(b.get(1))) {
            return a.get(0).compareTo(b.get(0));
        } else {
            return a.get(1).compareTo(b.get(1));
        }
    }

    public static int interval(int[] first, int[] last) {
        int n = first.length;
        List<List<Integer>> intervals = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            intervals.add(Arrays.asList(first[i], last[i]));
        }

        // Sort the array
        Collections.sort(intervals, GFG::compare);
        Set<Integer> res = new HashSet<>();

        for (int i = 0; i < n; i++) {
            int start = intervals.get(i).get(0);
            int end = intervals.get(i).get(1);

            int count = 0;
            for (int num : res) {
                if (num >= start && num <= end) {
                    count++;
                }
            }

            while (count < 2) {
                res.add(end);
                end--;
                count++;
            }
        }
        return res.size();
    }

    // Test the function with example
    public static void main(String[] args) {
        int[] first = {0, 1, 2};
        int[] last = {2, 3, 3};
        System.out.println("Size of the smallest interval possible: " + interval(first, last));
    }
}
```

This code first creates a list of intervals from your `first` and `last` arrays. It then sorts these intervals and iterates over them, adding the necessary integers to a set to ensure that each interval contains at least two of these integers. Finally, it returns the size of this set.



### Task Scheduling&#x20;

We have two servers, a paid one and a free one. The free server can be used only if the paid server is occupied. The ith task is expected to take time\[i] units of time to complete and the cost of processing the task on the paid server is cost\[i]. The task can be run on the free server only if some task is already running on the paid server. The cost of the free server is 0 and it can process any task in 1 unit of time.&#x20;

Find the minimum cost to complete all the tasks if tasks are scheduled optimally.&#x20;

Example 1: Suppose n = 4, cost = \[1, 1, 3, 4] and time = \[3, 1, 2, 3]. Output: 1. Explanation: The first task must be scheduled on the paid server for a cost of 1 and it takes 3 units of time to complete. In the meantime, the other three tasks are executed on the free server for no cost as the free server takes only 1 unit to complete any task. Return the total cost, which is 1.&#x20;

Example 2: n = 4, cost = \[1, 2, 3, 2], time = \[1, 2, 3, 2]. Output: 3. Explanation: The first and the second tasks can be scheduled on the paid server and in parallel. The last two can be scheduled on the free server. The total cost is 1 + 2 = 3.

Example 3: n = 4, cost = \[2, 3, 4, 2], time = \[1, 1, 1, 1]. Output: 4

Example 4: n = 4, cost = \[1,1,4,1,2,5], time = \[1,1,1,1,1,1]. Output: 4

Function Description: Complete the function getMinCost in the editor below. The function getMinCost has the following parameter: int cost\[n]: the costs of scheduling the tasks on a remote server int time\[n]: the times taken to run the tasks on a remote server Return int: the minimum cost to complete all the tasks

```java
class Solution {
    public int paintWalls(int[] cost, int[] time) {
        int n = cost.length;
        int[][] dp = new int[n + 1][n + 1];
        
        for (int i = 1; i <= n; i++) {
            dp[n][i] = (int) 1e9;
        }
        
        for (int i = n - 1; i >= 0; i--) {
            for (int remain = 1; remain <= n; remain++) {
                int paint = cost[i] + dp[i + 1][Math.max(0, remain - 1 - time[i])];
                int dontPaint = dp[i + 1][remain];
                dp[i][remain] = Math.min(paint, dontPaint);
            }
        }
        
        return dp[0][n];
    }
}
```

### Palindromic Sequence &#x20;

A palindrome, a subsequence and a score are defined as follows:&#x20;

• A palindrome is a sequence of characters that reads the same forward and backward. For example, madam and dad are palindromes, but eva and sam are not.&#x20;

• A subsequence is a group of characters chosen from a list while maintaining their order. For instance, the subsequences of abc are \[a, b, c, ab, ac, bc, abc]&#x20;

• The score of strings is the maximum product of the lengths of two non-overlapping palindromic subsequences of s that will be refered to as a and b. In other words, score(s) = max(length(a)) × max(length(b)).&#x20;

Given a string of characters s, calculate its score using the formula above.&#x20;

Example: s = "attract"&#x20;

• The Palindromic subsequences are \[a, t, r, c, aa, tt, ata, ara, ttt, trt, tat, tct, atta].&#x20;

• The two non-overlapping palindromic subsequences with the maximum score are "atta", |atta| =4 and |c| or |t| = 1, 4 x 1 = 4.&#x20;

• Note that the subsequence "atta" overlaps the subsequence r, so only one of them be chosen.

```java
public static int longestPalindromicSubsequenceProduct(String x) {
      int n = x.length();
      int[][] dp = new int[n][n];

      for (int i = 0; i < n; i++) {
          dp[i][i] = 1;
      }

      for (int k = 1; k < n; k++) {
          for (int i = 0; i < n - k; i++) {
              int j = i + k;
              if (x.charAt(i) == x.charAt(j)) {
                  dp[i][j] = 2 + dp[i + 1][j - 1];
              } else {
                  dp[i][j] = Math.max(dp[i][j - 1], dp[i + 1][j]);
              }
          }
      }

      int maxProd = 0;
      for (int i = 0; i < n; i++) {
          for (int j = 0; j < n - 1; j++) {
              maxProd = Math.max(maxProd, dp[i][j] * dp[j + 1][n - 1]);
          }
      }

      return maxProd;
  }
```

