# Extension: Rotate Image 90 degrees Counterclockwise

<figure><img src="../../../../.gitbook/assets/image (32).png" alt="" width="563"><figcaption></figcaption></figure>

### 思路：Mirror the matrix along the other diagonal

```java
public void rotate(int[][] matrix) {
    // 1. transpose the matrix
    for (int i = 0; i < matrix.length; i++) {
        for (int j = n - i; j < matrix[0].length; j++) {
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[n - j - 1][n - i - 1];
            matrix[n - j - 1][n - i - 1] = tmp;
        }
    }
    // 2. reverse columns
    for (int[] row: matrix) {
        reverse(row);
    }
}

private void reverse(int[] row) {
    int i = 0;
    int j = row.length - 1;
    while (i < j) {
        int tmp = row[i];
        row[i] = row[j];
        row[j] = tmp;
        i++;
        j--;
    }
}
```
