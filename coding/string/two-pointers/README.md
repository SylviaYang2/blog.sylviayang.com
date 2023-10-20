# Two Pointers

### 判断是否palindrome：由两端向中间靠近

### 寻找palindrome：由中心向两端扩展

* **背向**
  * 由中心向两端扩展：**寻找**palindrome -> [5. longest palindromic substring](../../array/two-pointers/5.-longest-palindromic-substring-medium.md)

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
