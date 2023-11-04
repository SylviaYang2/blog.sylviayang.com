---
description: BlackRock
---

# BlackRock OA & Hirevue

<figure><img src="../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>

20: [https://leetcode.com/problems/valid-parentheses/](https://leetcode.com/problems/valid-parentheses/)

````java
```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (char c: s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') { // c is one of the open brackets
                stack.push(c);
            } else if (!stack.isEmpty() && stack.peek() == pairs(c)) { // c is one of the close brackets
                stack.pop();
            } else {
                return false;
            }
        }
        return stack.isEmpty(); // check if all of the parentheses are paired
    }

    private char pairs(char c) {
        if (c == ')') return '(';
        else if (c == ']') return '[';
        else return '{';
    }
}
```
````



399: [https://leetcode.cn/problems/evaluate-division/solutions/548634/399-chu-fa-qiu-zhi-nan-du-zhong-deng-286-w45d/](https://leetcode.cn/problems/evaluate-division/solutions/548634/399-chu-fa-qiu-zhi-nan-du-zhong-deng-286-w45d/)

{% content-ref url="../coding/graph/union-find/399.-evaluate-division-medium.md" %}
[399.-evaluate-division-medium.md](../coding/graph/union-find/399.-evaluate-division-medium.md)
{% endcontent-ref %}

<figure><img src="../.gitbook/assets/image (161).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

### Approach: DFS with Memorization

To solve this problem, you can represent the **exchange rates** as a graph.&#x20;

Each currency is a **node**, and each exchange rate is a directed edge with a **weight** corresponding to the exchange rate.&#x20;

To maximize the target currency for 1 unit of the original currency, you'll **search** for paths in this graph to get the **maximum** multiplication factor. However, be careful not to get into infinite loops (i.e., continuously exchanging between two currencies to artificially increase the amount).

{% code overflow="wrap" %}
````java
```java
import java.util.*;

public class CurrencyExchange {

    private static class Edge {
        String to;
        double rate;

        Edge(String to, double rate) {
            this.to = to;
            this.rate = rate;
        }
    }

    private static double dfs(String curr, String target, double amount, Set<String> visited, Map<String, List<Edge>> graph) {
        // if found the target
        if (curr.equals(target)) return amount;
        // if got into a loop
        if (visited.contains(curr)) return -1.0;
        visited.add(curr);
        double maxAmount = -1.0;
        for (Edge edge : graph.getOrDefault(curr, new ArrayList<>())) {
            double newAmount = dfs(edge.to, target, amount * edge.rate, new HashSet<>(visited), graph);
            maxAmount = Math.max(maxAmount, newAmount);
        }
        return maxAmount;
    }

    public static double getMaxCurrency(String original, String target, Map<String, List<Edge>> graph) {
        return dfs(original, target, 1.0, new HashSet<>(), graph);
    }

    public static void main(String[] args) {
        // Parse the input
        Scanner sc = new Scanner(System.in);
        String[] rates = sc.nextLine().split(";");
        Map<String, List<Edge>> graph = new HashMap<>();
        for (String rate : rates) {
            String[] parts = rate.split(",");
            String from = parts[0];
            String to = parts[1];
            double exchangeRate = Double.parseDouble(parts[2]);
            graph.putIfAbsent(from, new ArrayList<>());
            graph.get(from).add(new Edge(to, exchangeRate));
        }
        String originalCurrency = sc.nextLine();
        String targetCurrency = sc.nextLine();
        sc.close();

        // Calculate the result
        double result = getMaxCurrency(originalCurrency, targetCurrency, graph);
        if (result > 0) {
            System.out.printf("%.2f\n", result);
        } else {
            System.out.println("-1.0");
        }
    }
}
```

For the given examples:

Example 1:
Input:
```
USD,CAD,1.3;USD,GBP,0.71;USD,JPY,109;GBP,JPY,155
USD
JPY
```
Output:
```
110.05
```

Example 2:
Input:
```
USD,GBP,0.7;USD,JPY,109;GBP,JPY,155;CAD,CNY,5.27;CAD,KRW,921
USD
CNY
```
Output:
```
-1.0
```
````
{% endcode %}

{% code overflow="wrap" %}
````java
Note:
In our approach, we are using a new `visited` set for each recursive call to ensure that the state from one path doesn't affect the state of another path. But, in many DFS algorithms, especially when you're searching for paths or cycles, you'd mark a node as visited when you enter it and then unmark it when you leave.

If you want to use a single `visited` set across all recursive calls and revert the state after each call, you should indeed remove `curr` from the `visited` set after the for loop. This would make it a bit more efficient in terms of space.

Here's how you can modify the code:

```java
private static double dfs(String curr, String target, double amount, Set<String> visited, Map<String, List<Edge>> graph) {
    if (curr.equals(target)) return amount;
    if (visited.contains(curr)) return -1.0;
    visited.add(curr);
    double maxAmount = -1.0;
    for (Edge edge : graph.getOrDefault(curr, new ArrayList<>())) {
        double newAmount = dfs(edge.to, target, amount * edge.rate, visited, graph);
        maxAmount = Math.max(maxAmount, newAmount);
    }
    visited.remove(curr);
    return maxAmount;
}
```

With this approach, you'll call the `dfs` method like this:
```java
double result = dfs(original, target, 1.0, new HashSet<>(), graph);
```

And you don't need to create a new HashSet for each recursive call, making it slightly more efficient.
````
{% endcode %}



### BQ:

BQ: Why Blackrock? especially this positon? 思路就是:

* 自己了解并认同公司的mission,并且自己的career goal和公司的misson很符合 \*  因为从公司的角度来说, th‍‍‍‌‍‍‍‌‌‍‍‌‌‌‍‍‍‌‌ey are trying to find the people who are aligned with the company’s vision
* 回答时表现的真诚、积极
