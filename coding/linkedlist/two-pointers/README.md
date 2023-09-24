# Two Pointers

1. 双指针：
   * 相向
     * 由两端向中心靠近：**判断**palindrome

```java
boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

* 同向
* 背向
  * 由中心向两端扩展：**寻找**palindrome

<pre class="language-java"><code class="lang-java"><strong>// 以s[left]和s[right]为中心的最长palindrome
</strong><strong>String palindrome(String s, int left, int right) {
</strong>    while (left >= 0 &#x26;&#x26; right &#x3C; s.length() &#x26;&#x26; s.charAt(left) == s.charAt(right)) {
        left--;
        right++;
    }
    return s.substring(left + 1, right);
}
</code></pre>

2. 快慢指针
