# 48. Rotate Image (Medium)

<figure><img src="../../../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

### 难点：in-place rotation

思路：旋转90度 -> 把行变成列，把列变成行

1. Transpose the matrix
2. Reverse Columns

![](<../../../../.gitbook/assets/image (49).png>)![](<../../../../.gitbook/assets/image (50).png>)

![](<../../../../.gitbook/assets/image (51).png>)

```java
class Solution {
    public void rotate(int[][] matrix) {
        // 1. transpose the matrix
        for (int i = 0; i < matrix.length; i++) {
            for (int j = i; j < matrix[0].length; j++) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = tmp;
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
}
```

### 时间复杂度

**O(N^2)**

### 空间复杂度

**O(1)**&#x20;
