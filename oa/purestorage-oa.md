# PureStorage OA

### Find Doubles

Given a list of N integers, not necessarily unique, find all elements of the list for which there exists exactly one element of the list which is twice that number. The integers range from 0 to 1000. The list has no more than 100,000 elements. Your code should find the appropriate values and print them to STDOUT in sorted order.&#x20;

Examples:&#x20;

12345678908→0123 (8 is 4\*2, but 8 is present twice; 0 is its own double, so it's part of the result) ;

7 17 11 1 23 -> (nothing is exactly twice another element) ;

112 -> 11 (1 and 1 both have their double 2 present, and 2 is present in the list only once);

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class FindDoubles {

    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 8};
        int[] arr2 = {7, 17, 11, 1, 23};
        int[] arr3 = {1, 1, 2};

        findAndPrintDoubles(arr1);
        findAndPrintDoubles(arr2);
        findAndPrintDoubles(arr3);
    }

    public static void findAndPrintDoubles(int[] arr) {
        Map<Integer, Integer> countMap = new HashMap<>();

        // Count occurrences of each element
        for (int num : arr) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }

        // Find and print elements with exactly one double
        for (int num : arr) {
            int doubleValue = num * 2;

            if (countMap.containsKey(doubleValue) && countMap.get(doubleValue) == 1) {
                System.out.print(num + " ");
            }
        }

        System.out.println(); // Move to the next line after printing the result
    }
}

```



### Find Repetitions

You receive a short string short\_s and a long string long\_s, such that short\_s can be found repeated a number of times in long\_s. The goal is to compute the maximum number of consecutive repetitions of short\_s within long\_s, and return that number. If any of short\_s or long\_s are empty, then the answer is 0.&#x20;

The following constraints apply: 0 <= len(short\_s) < 10 0 < len(long\_s) < 1,000,000&#x20;

Examples:&#x20;

short\_s="AB" long\_s="ABBAC" "AB" is only found once in "ABBAC" so the answer is 1.&#x20;

short\_s="AB" long\_s="АВСАВСАВАВ" "AB" is found in long\_s at index 0, 3, 6 and 8. Because it repeats twice consecutively at the end, the answer is 2.

```java
public class FindRepetitions {

    public static void main(String[] args) {
        String short_s1 = "AB";
        String long_s1 = "ABBAC";
        System.out.println(findMaxConsecutiveRepetitions(short_s1, long_s1));

        String short_s2 = "AB";
        String long_s2 = "АВСАВСАВАВ";
        System.out.println(findMaxConsecutiveRepetitions(short_s2, long_s2));
    }

    public static int findMaxConsecutiveRepetitions(String short_s, String long_s) {
        if (short_s.isEmpty() || long_s.isEmpty()) {
            return 0;
        }

        int maxRepetitions = 0;
        int currentIndex = 0;

        while (currentIndex < long_s.length()) {
            int foundIndex = long_s.indexOf(short_s, currentIndex);

            if (foundIndex == -1) {
                break;
            }

            int consecutiveRepetitions = 1;
            currentIndex = foundIndex + short_s.length();

            // Count consecutive repetitions
            while (currentIndex < long_s.length() && long_s.startsWith(short_s, currentIndex)) {
                consecutiveRepetitions++;
                currentIndex += short_s.length();
            }

            maxRepetitions = Math.max(maxRepetitions, consecutiveRepetitions);
        }

        return maxRepetitions;
    }
}

```



### Bakery Quality Control

Your program controls boxes of pastries coming out of a bakery. For each produced box, you are required to compare its contents to a list of expected items (its "template") and determine whether the box is correct or not. The contents of a box is described by a string such as "pcm" for 'p'ie, 'c'ookie, and 'm'uffin, or "ddp" for 'd'onut, 'd'onut, and 'p'ie.&#x20;

The template is described in the same manner. So given a list of (box, template) pairs, your program should indicate how many times it found a mismatch between a box and its template and return that total. A box contains no more than 10 items, and there are no more than 1000 boxes to check at a time. Items in a box can be repeated, so "cc" (cookie cookie) is not the same as "c" (cookie). Items are not ordered, so the box "cm" (cookie, muffin) matches the template "mc" (muffin, cookie).&#x20;

Example:&#x20;

Your program is provided with the following list: \[ \["cm", "mc"], \["ccm", "mc"], \["pm", "mc"], \["c", "mc"] ] The pair \["cm", "mc"] is valid; the order of items in the box doesn't matter. The pair \["ccm", "mc"] is invalid; there are too many cookies in the box. The pair \["pm", "mc"] is invalid; there is a pie in the box, and no cookie. The pair \["c", "mc"] is invalid; there is a muffin missing in the box. Given that list, your program should return 3, as it found 3 invalid boxes out of 4.

```java
import java.util.ArrayList;
import java.util.List;

public class BakeryQualityControl {

    public static void main(String[] args) {
        List<String[]> pairs = new ArrayList<>();
        pairs.add(new String[]{"cm", "mc"});
        pairs.add(new String[]{"ccm", "mc"});
        pairs.add(new String[]{"pm", "mc"});
        pairs.add(new String[]{"c", "mc"});

        int mismatches = findMismatches(pairs);
        System.out.println("Total mismatches: " + mismatches);
    }

    public static int findMismatches(List<String[]> pairs) {
        int mismatches = 0;

        for (String[] pair : pairs) {
            String box = pair[0];
            String template = pair[1];

            if (!isValidBox(box, template)) {
                mismatches++;
            }
        }

        return mismatches;
    }

    public static boolean isValidBox(String box, String template) {
        // Count occurrences of each item in the box
        int[] boxCount = new int[26]; // Assuming items are lowercase letters

        for (char item : box.toCharArray()) {
            boxCount[item - 'a']++;
        }

        // Check if the box matches the template
        for (char item : template.toCharArray()) {
            if (boxCount[item - 'a'] <= 0) {
                return false; // Item is missing in the box
            }
            boxCount[item - 'a']--;
        }

        return true;
    }
}

```



### Racing Results

Unfortunately, the database containing all this year's racing results has crashed. The only thing left is a backup of database records and results for each race. We urgently need to know who is the winner!&#x20;

• Your program will receive input as a list of elements in the form of \[race, racer\_name, position], where all elements are integers:&#x20;

• race is a database record number between 2001 and 2000+N where N is the total number of races in the championship.&#x20;

• racer\_name is a database record number between 1001 and 1000+R where R is the total number of racers participating.&#x20;

• position is a value between 1 (won the race) and R (arrived last).&#x20;

Points are given according to the racers' position in a race: the 1st position is worth 10 points, 2nd is worth 6 points, 3rd is 4 points, 4th is 3 points, 5th is 2 points and 6th is worth 1 point. Positions further down earn no points. In case of an equal number of points at the end of the championship, the winner is the racer will the lowest record number. There are at most 100 racers, and at most 100 races in the championship. Your program is expected to output the record number of the winner, followed by how many points he or she got.

Here's an example, with two races and three racers. The raw results are:&#x20;

\[2001, 1001, 3]\
\[2001, 1002, 2]&#x20;

\[2002, 1003, 1]&#x20;

\[2002, 1001, 2]&#x20;

\[2002, 1002, 3]

\[2001, 1003, 1]

The two races are coded 2001 and 2002. The racers with records 1001, 1002 and 1003 competed. Based on the raw results, they have the following points: Racer 1001: 4+6=10 (3rd and 2nd positions) Racer 1002: 6+4=10 (2nd and 3rd positions) Racer 1003: 10+10=20 (1st in both races).

Your program is expected to output the following line in that case: 1003 20

```java
import java.util.HashMap;
import java.util.Map;

public class RacingResults {

    public static void main(String[] args) {
        int[][] results = {
                {2001, 1001, 3},
                {2001, 1002, 2},
                {2002, 1003, 1},
                {2002, 1001, 2},
                {2002, 1002, 3},
                {2001, 1003, 1}
        };

        int[] winnerInfo = findWinner(results);
        System.out.println(winnerInfo[0] + " " + winnerInfo[1]);
    }

    public static int[] findWinner(int[][] results) {
        Map<Integer, Integer> racerPoints = new HashMap<>();
        Map<Integer, Integer> racerRaces = new HashMap<>();

        for (int[] result : results) {
            int race = result[0];
            int racer = result[1];
            int position = result[2];

            int points = calculatePoints(position);
            racerPoints.put(racer, racerPoints.getOrDefault(racer, 0) + points);
            racerRaces.put(racer, racerRaces.getOrDefault(racer, 0) + 1);
        }

        int maxPoints = 0;
        int winner = -1;

        for (Map.Entry<Integer, Integer> entry : racerPoints.entrySet()) {
            int racer = entry.getKey();
            int points = entry.getValue();

            if (points > maxPoints || (points == maxPoints && racer < winner)) {
                maxPoints = points;
                winner = racer;
            }
        }

        return new int[]{winner, maxPoints};
    }

    public static int calculatePoints(int position) {
        switch (position) {
            case 1:
                return 10;
            case 2:
                return 6;
            case 3:
                return 4;
            case 4:
                return 3;
            case 5:
                return 2;
            case 6:
                return 1;
            default:
                return 0;
        }
    }
}

```
