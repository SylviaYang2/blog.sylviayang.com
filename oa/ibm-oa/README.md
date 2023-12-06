---
description: OA
---

# IBM OA

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```java
int[] results = new int[queries.length];
ArrayList<Integer> goodArray = new ArrayList<>();
for (int i = 0; N > 0; i++) {
    if (N & 1 == 1) { // i.e. the least significant digit is 0 -> even number
       goodArray.add(1 << i);
    } 
    N >>= 1;
}

for (int i = 0; i < queries.length; i++) {
   // convert to zero-based
   int L = queries[i][0] - 1;
   int R = queries[i][1] - 1;
   int m = queries[i][2];
   long product = 1;
   
   for (int j = L; j <= R; j++) {
      product = product * goodArray.get(j) % m;
   }
   results[i] = (int) product;
}

return results;
```





<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
```java
import java.io.*;
import java.util.*;

public class SourceCodeFileSorter {

    public static void sortSourceFiles(String baseFilename) throws IOException {
        // Readers for input file
        BufferedReader reader = new BufferedReader(new FileReader(baseFilename));
        
        // Writers for output files
        PrintWriter cWriter = new PrintWriter(baseFilename.replaceAll(".txt", "_c.txt"), "UTF-8");
        PrintWriter cppWriter = new PrintWriter(baseFilename.replaceAll(".txt", "_cpp.txt"), "UTF-8");
        PrintWriter csWriter = new PrintWriter(baseFilename.replaceAll(".txt", "_cs.txt"), "UTF-8");

        String line;
        while ((line = reader.readLine()) != null) {
            if (line.endsWith(".c")) {
                cWriter.println(line);
            } else if (line.endsWith(".cpp")) {
                cppWriter.println(line);
            } else if (line.endsWith(".cs")) {
                csWriter.println(line);
            }
        }

        // Close all resources
        reader.close();
        cWriter.close();
        cppWriter.close();
        csWriter.close();
    }

    public static void main(String[] args) {
        // The baseFilename is provided as an argument to main
        if (args.length > 0) {
            String baseFilename = args[0];
            try {
                sortSourceFiles(baseFilename);
            } catch (IOException e) {
                e.printStackTrace();
            }
        } else {
            System.out.println("No base file name provided!");
        }
    }
}
```

Make sure to replace the `baseFilename` with the actual file name you want to process. The program reads from the input file, checks the extension of each line, and writes the line to the corresponding output file based on the extension. It assumes the input file is a text file with one file name per line.
````
{% endcode %}



<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

````java
```java
public class Solution {
    // Function to apply updates and get the final array
    public static int[] getFinalData(int[] data, int[][] updates) {
        // Applying each update
        for (int[] update : updates) {
            // Note: Assuming 1-based indexing as per the problem statement
            int l = update[0] - 1; // Convert to 0-based index
            int r = update[1] - 1; // Convert to 0-based index
            
            // Negate the values from index l to r
            for (int i = l; i <= r; i++) {
                data[i] = -data[i];
            }
        }
        // Return the updated array
        return data;
    }

    public static void main(String[] args) {
        // Example
        int[] data = {1, 2, 3, 4};
        int[][] updates = {{1, 2}, {3, 4}};

        // Getting the final data after updates
        int[] finalData = getFinalData(data, updates);

        // Printing the final data
        for (int i : finalData) {
            System.out.print(i + " ");
        }
    }
}
```
````







<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
To solve this problem, you need to create a function that calculates the sum of a sequence starting from `i`, incrementing by 1 each time, until it reaches `j`, and then decrementing by 1 each time until it reaches `k`.

```java
public class SequenceSum {

    public static long getSequenceSum(int i, int j, int k) {
        long sum = 0;
        
        // Increment from i to j
        for (int x = i; x <= j; x++) {
            sum += x;
        }
        
        // Decrement from j to k
        for (int x = j - 1; x >= k; x--) {
            sum += x;
        }
        
        return sum;
    }

    public static void main(String[] args) {
        int i = 5;
        int j = 9;
        int k = 6;
        
        // Calculate the sequence sum
        long result = getSequenceSum(i, j, k);
        System.out.println("The sequence sum is: " + result);
    }
}
```

This program defines a `getSequenceSum` function that calculates the sequence sum as described. Then it provides an example usage of the function with the given `i`, `j`, and `k` values, and prints out the result.
````
{% endcode %}





<figure><img src="../../.gitbook/assets/image (8) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (9) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
To solve this problem in Java, you need to read the log file, parse each log entry to find the number of bytes in the response, and then write the required information to an output file. The following Java code accomplishes this:

```java
import java.io.*;
import java.util.regex.*;

public class LogProcessor {

    public static void processLogFile(String filename) throws IOException {
        File inputFile = new File(filename);
        File outputFile = new File("bytes_" + filename);
        
        BufferedReader reader = new BufferedReader(new FileReader(inputFile));
        BufferedWriter writer = new BufferedWriter(new FileWriter(outputFile));

        String line;
        int countLargeResponses = 0;
        long totalBytesLargeResponses = 0;
        while ((line = reader.readLine()) != null) {
            // Assuming the bytes are the last number in each log entry
            Pattern pattern = Pattern.compile("\\s(\\d+)$");
            Matcher matcher = pattern.matcher(line);
            if (matcher.find()) {
                int bytes = Integer.parseInt(matcher.group(1));
                if (bytes > 5000) {
                    countLargeResponses++;
                    totalBytesLargeResponses += bytes;
                }
            }
        }
        
        writer.write(String.valueOf(countLargeResponses));
        writer.newLine();
        writer.write(String.valueOf(totalBytesLargeResponses));
        
        reader.close();
        writer.close();
    }

    public static void main(String[] args) {
        // Assuming the filename is passed as the first argument to the program
        if (args.length > 0) {
            try {
                processLogFile(args[0]);
            } catch (IOException e) {
                e.printStackTrace();
            }
        } else {
            System.out.println("No filename provided!");
        }
    }
}
```

This Java program will process the log file given as an argument, count the number of requests with more than 5000 bytes, sum the bytes for such requests, and write these two pieces of information to an output file prefixed with "bytes_". Make sure to replace `filename` with the actual log file name when running the program.
````
{% endcode %}







<figure><img src="../../.gitbook/assets/image (10) (1).png" alt="" width="375"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class FactorFinder {

    public static long pthFactor(long n, long p) {
        List<Long> factors = new ArrayList<>();

        // Find factors
        for (long i = 1; i <= Math.sqrt(n); i++) {
            if (n % i == 0) {
                factors.add(i);
                if (i != n / i) {
                    factors.add(n / i);
                }
            }
        }
        
        // Sort the list of factors
        Collections.sort(factors);

        // Return the pth factor if it exists
        if (p <= factors.size()) {
            return factors.get((int)p - 1);
        }

        return 0; // Return 0 if the pth factor does not exist
    }

    public static void main(String[] args) {
        long n = 20;
        long p = 3;
        System.out.println("The " + p + "th factor of " + n + " is: " + pthFactor(n, p));
    }
}
```
The code snippet is an efficient way to find all the factors of a given number `n` by leveraging the mathematical property that factors of a number come in pairs which multiply to the number itself.

Here's a breakdown of why it's implemented this way:

1. **Iterating only up to the square root of `n`**:
   - Any factor greater than the square root of `n` will have a corresponding factor that is less than the square root of `n`. For example, if `n` is 36, the factor pairs are (1, 36), (2, 18), (3, 12), (4, 9), and (6, 6). Notice that after 6, the factors repeat but in reverse order. Therefore, there's no need to check for factors greater than the square root because you would be finding the same pairs again.

2. **Checking if `n` is divisible by `i`**:
   - The condition `n % i == 0` checks if `i` is a factor of `n`. If `n` divided by `i` leaves no remainder, then `i` is a factor.

3. **Adding both factors to the list**:
   - When `i` is a factor, both `i` and `n / i` are factors of `n`. For instance, if `n` is 36 and `i` is 4, then 4 is a factor, and so is `36 / 4`, which is 9. Both numbers are added to the list of factors.

4. **Avoiding duplicates**:
   - The condition `if (i != n / i)` ensures that both factors are not the same, which can happen when `n` is a perfect square. In the case of `n` being 36 again, the pair (6, 6) should only add the number 6 to the list once to avoid duplicates.

This approach ensures that the algorithm is efficient, running in O(√n) time complexity, which is much faster than checking every number up to `n`. This efficiency is crucial for large values of `n`.
````
{% endcode %}

<figure><img src="../../.gitbook/assets/image (32).png" alt="" width="375"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
```java
public class ArrayCostMinimizer {
    public static long getMinimumCost(int[] arr) {
        long minCost = Long.MAX_VALUE;
        for (int i = 0; i <= arr.length; i++) {
            // Try to insert a value between arr[i - 1] and arr[i]
            int left = (i > 0) ? arr[i - 1] : Integer.MIN_VALUE;
            int right = (i < arr.length) ? arr[i] : Integer.MAX_VALUE;

            // If we're at the ends of the array, we consider only one neighbor
            if (i == 0) {
                minCost = Math.min(minCost, calculateCost(arr, right));
            } else if (i == arr.length) {
                minCost = Math.min(minCost, calculateCost(arr, left));
            } else {
                // Try inserting a value that minimizes the local difference
                int insertValue = left + (right - left) / 2;
                minCost = Math.min(minCost, calculateCostWithInsert(arr, insertValue, i));
            }
        }
        return minCost;
    }

    private static long calculateCost(int[] arr, int newValue) {
        long cost = 0;
        for (int j = 0; j < arr.length - 1; j++) {
            cost += (long) Math.pow(arr[j] - arr[j + 1], 2);
        }
        cost += (long) Math.pow(arr[arr.length - 1] - newValue, 2);
        return cost;
    }

    private static long calculateCostWithInsert(int[] arr, int newValue, int position) {
        long cost = 0;
        for (int j = 0; j < arr.length; j++) {
            if (j == position - 1) {
                cost += (long) Math.pow(arr[j] - newValue, 2);
            } else if (j == position) {
                cost += (long) Math.pow(newValue - arr[j], 2);
            } else {
                cost += (long) Math.pow(arr[j] - arr[j - 1], 2);
            }
        }
        return cost;
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 2, 10};
        System.out.println("The minimum possible cost is: " + getMinimumCost(arr));
    }
}
```

This method iterates through all possible insertion points, including before the first element and after the last element, to find the position where the new element minimizes the cost. The `calculateCost` and `calculateCostWithInsert` helper methods are used to compute the cost of the array with the inserted element. The `main` method provides an example usage with the array `{1, 3, 5, 2, 10}`.
````
{% endcode %}





<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
```java
public class FlipStringToTarget {

    public static int minimumFlips(String target) {
        int flips = 0;
        char lastChar = '0'; // Initialize to the opposite of the first character of target

        for (int i = 0; i < target.length(); i++) {
            if (target.charAt(i) != lastChar) {
                flips++; // Increment flips when a different digit is encountered
                lastChar = target.charAt(i); // Update lastChar to the current character
            }
        }

        return flips;
    }

    public static void main(String[] args) {
        String target = "0011";
        System.out.println("Minimum flips required: " + minimumFlips(target));
    }
}
```

This program defines a `minimumFlips` method that computes the number of flips required to convert a string of zeroes to the target binary string. The `main` method tests this with the target string "0011".

Approach:
When analyzing the problem statement, a key observation is that a flip affects all subsequent digits in the string. Therefore, flipping towards the target sequence should be done in a left-to-right sequence, and only when necessary, to avoid undoing any previous work.

Here's the thought process for the solution:

1. **Start with an initial string of zeroes**:
   - The initial state is known, and the target state is given. The task is to reach the target with a minimum number of flips.

2. **Understand the flip operation**:
   - A flip operation inverts the current digit and all digits to the right. Thus, a flip is only needed when the current state does not match the target at the current position.

3. **Initialize variables**:
   - Set `flips` to 0 because no flips have been made initially.
   - Set `lastChar` to '0' to represent the initial state of the string.

4. **Iterate through the target string**:
   - Since the initial string consists of all zeroes, you only need to flip when the target digit is different from the current digit in the initial string.
   - If the current digit in the target string is different from the `lastChar`, a flip is required.

5. **Flip and update state**:
   - When a flip is made, the current state of the string from that position onward changes. So, update the `lastChar` to the current target character, indicating the new state after the flip.

6. **Increment flip count**:
   - Each time a flip occurs (when the target character is different from `lastChar`), increment the flip counter.

7. **Continue until the end of the string**:
   - Move through each character in the target string. Because you're always comparing to the last character and flipping when they don't match, you ensure that you only flip the minimum number of times needed.

By the end of the target string, you will have the minimum number of flips needed to match the target, because the algorithm only flips when necessary to match the current target character, and the flip affects all remaining characters, aligning them
````
{% endcode %}



<figure><img src="../../.gitbook/assets/image (34).png" alt="" width="375"><figcaption></figcaption></figure>

<pre class="language-java" data-overflow="wrap"><code class="lang-java"><strong>```java
</strong>import java.util.*;

public class MaxMinOperations {

    public static long[] maxMin(String[] operations, int[] x) {
        TreeSet&#x3C;Integer> elements = new TreeSet&#x3C;>();
        long[] results = new long[operations.length];

        for (int i = 0; i &#x3C; operations.length; i++) {
            if (operations[i].equals("push")) {
                elements.add(x[i]);
            } else if (operations[i].equals("pop")) {
                elements.remove(x[i]);
            }

            if (elements.isEmpty()) {
                results[i] = 0;
            } else {
                // As TreeSet keeps elements ordered, first() gives min and last() gives max
                results[i] = (long) elements.first() * (long) elements.last();
            }
        }

        return results;
    }

    public static void main(String[] args) {
        // Example usage
        String[] operations = {"push", "push", "pop", "push"};
        int[] x = {1, 2, 1, 2};

        long[] results = maxMin(operations, x);
        System.out.println("Products of max and min after each operation:");
        for (long result : results) {
            System.out.println(result);
        }
    }
}
```

Here's the thought process and reasoning for using a `TreeSet`:
1. **Maintaining Order**:
   - A `TreeSet` in Java automatically keeps the elements sorted. This property is extremely useful for our problem because we need to frequently access the smallest and largest elements to calculate the product.
   
2. **Efficient Access to Extremes**:
   - To find the product of the minimum and maximum values after each operation, we need efficient access to these values. `TreeSet` provides `first()` and `last()` methods, which give us the smallest and largest elements in constant time, O(log n), where n is the number of elements in the set.

3. **Handling Duplicates**:
   - The problem does not specify how to handle duplicates. A `TreeSet` does not allow duplicate values, which aligns with the behavior of a mathematical set. If the problem required keeping duplicates, a different data structure would be necessary.

4. **Ease of `push` and `pop` Operations**:
   - The `add` method of `TreeSet` is used for the `push` operation, which inserts the element in the correct sorted position.
   - The `remove` method of `TreeSet` is used for the `pop` operation, which removes the specified element.

5. **Calculating Results**:
   - After each operation, we check if the set is empty. If it is, the product of the minimum and maximum is 0 by definition since there are no elements.
   - If the set is not empty, we calculate the product of the minimum (`first()`) and maximum (`last()`) elements.

6. **Storing Results**:
   - We store the result after each operation in an array to return it as the final output of the function.

Using `TreeSet` allows us to handle the `push` and `pop` operations and to retrieve the minimum and maximum values both efficiently and elegantly, with minimal code and time complexity, hence it's the suitable choice for this problem.


Time Complexity Analysis:
1. **Insertion (add)**: O(log n)
2. **Deletion (remove)**: O(log n)
3. **Search**: O(log n)
4. **Retrieval of Min (first) or Max (last)**: O(1) 

Here, `n` is the number of elements currently in the `TreeSet`. 

If there are `m` operations to process, the total runtime of the `maxMin` function will be O(m log n), where `n` is the size of the set at the time of each operation. This is assuming the size of the set is roughly the same throughout the operations; if the size of the set varies significantly, then `n` would represent the largest size of the set at any point during the processing of operations.

For the worst-case scenario where each operation is a `push`, `n` could be as large as `m` (if we only push and never pop), resulting in a worst-case runtime of O(m log m).
</code></pre>



<figure><img src="../../.gitbook/assets/image (35).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (36).png" alt="" width="375"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
To solve this in Java, you would need to make HTTP GET requests to the given API endpoint and parse the JSON responses to find the food outlet with the highest user rating and a number of votes greater than or equal to the given minimum.

Here's a high-level overview of the steps you'd take in your Java code:

1. Use a library like `HttpClient` to make HTTP GET requests.
2. Paginate through the results by incrementing the `page` parameter until `totalPages` is reached.
3. Parse the JSON response using a library like `Gson` or `Jackson`.
4. Keep track of the outlet with the highest rating that meets the minimum vote requirement.
5. In case of a tie in rating, prefer the outlet with more votes.
6. Return the name of the winning restaurant.

Below is a simplified version of what the Java code could look like, without actual HTTP call implementation:

```java
import com.google.gson.*;
import java.net.http.*;
import java.net.URI;

public class FoodOutletFinder {

    private static final HttpClient client = HttpClient.newHttpClient();
    private static final Gson gson = new Gson();

    public static String finestFoodOutlet(String city, int votes) {
        String baseUrl = "https://jsonmock.hackerrank.com/api/food_outlets?city=";
        int currentPage = 1;
        int totalPages = Integer.MAX_VALUE;
        Outlet bestOutlet = null;

        while (currentPage <= totalPages) {
            String url = baseUrl + city + "&page=" + currentPage;
            HttpRequest request = HttpRequest.newBuilder().uri(URI.create(url)).build();
            HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
            JsonObject jsonResponse = JsonParser.parseString(response.body()).getAsJsonObject();

            totalPages = jsonResponse.get("total_pages").getAsInt();
            JsonArray data = jsonResponse.getAsJsonArray("data");

            for (JsonElement element : data) {
                Outlet outlet = gson.fromJson(element, Outlet.class);
                if (outlet.userRating.votes >= votes) {
                    if (bestOutlet == null || outlet.userRating.averageRating > bestOutlet.userRating.averageRating ||
                        (outlet.userRating.averageRating == bestOutlet.userRating.averageRating && outlet.userRating.votes > bestOutlet.userRating.votes)) {
                        bestOutlet = outlet;
                    }
                }
            }

            currentPage++;
        }

        return bestOutlet == null ? "" : bestOutlet.name;
    }

    // Assuming a class Outlet that matches the JSON schema provided by the API
    private static class Outlet {
        int id;
        String name;
        String city;
        double estimatedCost;
        UserRating userRating;
    }

    private static class UserRating {
        double averageRating;
        int votes;
    }

    // main method for testing
    public static void main(String[] args) throws Exception {
        String city = "cityName";
        int minimumVotes = 100;
        String bestRestaurant = finestFoodOutlet(city, minimumVotes);
        System.out.println("The best restaurant is: " + bestRestaurant);
    }
}
```

This code provides a structure but lacks the actual implementation of handling HTTP requests and parsing JSON. You'll need to handle exceptions and possibly deal with asynchronous code depending on your HTTP client's capabilities.
````
{% endcode %}





<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
To convert integers to their Roman numeral equivalents in Java, you can follow this approach:

1. Create a mapping of Roman numeral symbols to their values.
2. Write a function that processes each number by subtracting the largest Roman numeral value that is less than or equal to it until the number is reduced to zero.

Here's a Java method that implements this logic:

```java
public class RomanNumeralsConverter {

    private static final int[] VALUES = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    private static final String[] SYMBOLS = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    public static String[] romanizer(int[] numbers) {
        String[] result = new String[numbers.length];
        for (int i = 0; i < numbers.length; i++) {
            result[i] = toRoman(numbers[i]);
        }
        return result;
    }

    private static String toRoman(int number) {
        StringBuilder roman = new StringBuilder();
        for (int i = 0; i < VALUES.length; i++) {
            while (number >= VALUES[i]) {
                number -= VALUES[i];
                roman.append(SYMBOLS[i]);
            }
        }
        return roman.toString();
    }

    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};
        String[] romanNumerals = romanizer(numbers);
        for (String numeral : romanNumerals) {
            System.out.println(numeral);
        }
    }
}
```

In this code:
- `VALUES` and `SYMBOLS` arrays are used to map integer values to their respective Roman numeral symbols.
- The `romanizer` function iterates over the array of numbers and converts each number to a Roman numeral.
- The `toRoman` function constructs a Roman numeral string for an individual number.
- The `main` method demonstrates how to use the `romanizer` function.
````
{% endcode %}





<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
````java
To solve this in Java, we need to sort the array of scores in descending order, then determine each player's rank, and finally count how many players have a rank less than or equal to `k` and have a score greater than 0.

Here's a Java function that implements this logic:

```java
import java.util.Arrays;

public class VideoGameRanking {

    public static int numPlayers(int k, int[] scores) {
        // Sort scores in descending order
        Arrays.sort(scores);
        int n = scores.length;
        int count = 0;
        int rank = 1;

        // Iterate from the highest score to the lowest
        for (int i = n - 1; i >= 0; i--) {
            // If it's not the first score and it's different from the previous one, increment rank
            if (i < n - 1 && scores[i] != scores[i + 1]) {
                rank = n - i;
            }
            // Check if the player's rank is within the cutoff and the score is greater than 0
            if (rank <= k && scores[i] > 0) {
                count++;
            }
        }
        return count;
    }

    public static void main(String[] args) {
        int[] scores = {100, 50, 50, 25};
        int k = 3;
        System.out.println("Number of players who can level up: " + numPlayers(k, scores));
    }
}
```

This code will output the number of players who can level up based on their scores and the cutoff rank `k`. In the `main` method, we provide an example usage of the `numPlayers` function.
````
{% endcode %}





<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>
