---
description: Rippling OA
---

# Rippling OA

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

{% content-ref url="../coding/string/string-parsing/468.-validate-ip-address-medium.md" %}
[468.-validate-ip-address-medium.md](../coding/string/string-parsing/468.-validate-ip-address-medium.md)
{% endcontent-ref %}



<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```java
import org.json.JSONArray;
import org.json.JSONObject;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;

public class TVSeriesFetcher {

    public static ArrayList<String> showsInProduction(int startYear, int endYear) {
        ArrayList<String> allShows = new ArrayList<>();
        int page = 1;

        try {
            while (true) {
                URL url = new URL("https://jsonmock.hackerrank.com/api/tvseries?page=" + page);
                HttpURLConnection conn = (HttpURLConnection) url.openConnection();
                conn.setRequestMethod("GET");
                conn.connect();

                int responseCode = conn.getResponseCode();

                if (responseCode != 200) {
                    throw new RuntimeException("HttpResponseCode: " + responseCode);
                } else {
                    StringBuilder inline = new StringBuilder();
                    Scanner scanner = new Scanner(url.openStream());

                    while (scanner.hasNext()) {
                        inline.append(scanner.nextLine());
                    }
                    scanner.close();

                    JSONObject jsonResponse = new JSONObject(inline.toString());
                    JSONArray dataArray = jsonResponse.getJSONArray("data");

                    for (int i = 0; i < dataArray.length(); i++) {
                        JSONObject series = dataArray.getJSONObject(i);
                        if (meetsCriteria(series.getString("runtime_of_series"), startYear, endYear)) {
                            allShows.add(series.getString("name"));
                        }
                    }

                    if (page >= jsonResponse.getInt("total_pages")) {
                        break;
                    }
                    page++;
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

        Collections.sort(allShows);
        return allShows;
    }

    private static boolean meetsCriteria(String runtime, int startYear, int endYear){
        String[] years = runtime.replaceAll("[()]", "").split("-");
        int start, end;
    
        try {
            start = Integer.parseInt(years[0]);
    
            if (years.length > 1 && !years[1].isEmpty()) {
                end = Integer.parseInt(years[1]);
            } else {
                if (endYear == -1) return start >= startYear; // For ongoing series
                end = Integer.MAX_VALUE; // If end year is not specified, assume it's ongoing
            }
    
            // Check if the series falls within the specified range
            return start >= startYear && (endYear == -1 || end <= endYear);
    
        } catch (NumberFormatException e) {
            // Handle parsing errors, such as invalid format in the runtime string
            System.err.println("Error parsing runtime: " + runtime);
            return false;
    }

    public static void main(String[] args) {
        int startYear = 2010;
        int endYear = -1;  // Assuming -1 indicates ongoing series
        ArrayList<String> showNames = showsInProduction(startYear, endYear);
        showNames.forEach(System.out::println);
    }
}
```
{% endcode %}





<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```java
public class BeaconSignal {

    public static long beaconSignal(int x1, int y1, int x2, int y2, int xl, int yl, int R) {
        long count = 0;

        // Iterate over each node in the grid
        for (int x = x1; x <= x2; x++) {
            for (int y = y1; y <= y2; y++) {
                // Calculate the distance from the beacon
                double distance = Math.sqrt(Math.pow(xl - x, 2) + Math.pow(yl - y, 2));

                // Check if the distance is within the radius
                if (distance <= R) {
                    count++;
                }
            }
        }

        return count;
    }

    public static void main(String[] args) {
        int x1 = 0, y1 = 0, x2 = 1, y2 = 1, xl = 0, yl = -7, R = 8;
        long illuminatedNodes = beaconSignal(x1, y1, x2, y2, xl, yl, R);
        System.out.println("Number of illuminated nodes: " + illuminatedNodes);
    }
}
```
{% endcode %}
