# Two Pointers

1. 双指针：
   * **相向**：
     * [344.Reverse String](344.-reverse-string.md)
   * **同向**
   * **背向**
     * 由中心向两端扩展：**寻找**palindrome -> [5. longest palindromic substring](5.-longest-palindromic-substring-medium.md)

```java
// 以s[left]和s[right]为中心的最长palindrome
private String palindrome(String s, int left, int right) {
    while (left >= 0 && right < s.length && s.charAt(left) == s.charAt(right) {
        left--;
        right++;
    }
    return s.substring(left + 1, right);
}
```



2. **快慢指针**

* Remove Duplicates: 26, 27, 83, 283
* **Sliding Window滑动窗口**: 167

```
// Some code
```

3. **二分查找Binary Search**

*



