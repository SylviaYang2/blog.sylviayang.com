# (Hackerrank) Product of the Maximum and Minimum in a Dataset

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Since the 1 <= x\[i] <= 10^9, (10^9)^2 = 10^18 is significantly larger than the **`Integer.MAX_VALUE (2^31)`**, we are using **`Long`** to store our result

{% code overflow="wrap" %}
```java
import java.util.PriorityQueue;
import java.util.List;
import java.util.ArrayList;

public static List<Long> maxMin(String[] operations, int[] x) {
    List<Long> results = new ArrayList<>();
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

    for (int i = 0; i < operations.length; i++) {
        if (operations[i].equals("push")) {
            minHeap.add(x[i]);
            maxHeap.add(x[i]);
        } else if (operations[i].equals("pop")) {
            minHeap.remove(x[i]);
            maxHeap.remove(x[i]);
        }

        long min = minHeap.isEmpty() ? 0 : (long) minHeap.peek();
        long max = maxHeap.isEmpty() ? 0 : (long) maxHeap.peek();
        results.add(min * max);
    }

    return results;
}

```
{% endcode %}

### Complexity Analysis

* Time complexity: **O(nlogn)** - The time complexity for each operation is O(logn) because inserting or removing an element from a priority queue takes O(logn) time. Therefore, the overall time complexity of this solution is O(nlogn)
* Space complexity: **O(n)**
