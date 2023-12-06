# Parentheses Problems

有关括号问题，你只要记住以下性质，思路就很容易想出来：

**1、一个「合法」括号组合的左括号数量一定等于右括号数量，这个很好理解**。

**2、对于一个「合法」的括号字符串组合 `p`，必然对于任何 `0 <= i < len(p)` 都有：子串 `p[0..i]` 中左括号的数量都大于或等于右括号的数量**。

如果不跟你说这个性质，可能不太容易发现，但是稍微想一下，其实很容易理解，因为从左往右算的话，肯定是左括号多嘛，到最后左右括号数量相等，说明这个括号组合是合法的。

反之，比如这个括号组合 `))((`，前几个子串都是右括号多于左括号，显然不是合法的括号组合。

{% content-ref url="string/backtracking/22.-generate-parentheses-medium.md" %}
[22.-generate-parentheses-medium.md](string/backtracking/22.-generate-parentheses-medium.md)
{% endcontent-ref %}

{% content-ref url="string/stack/20.-valid-parentheses-easy.md" %}
[20.-valid-parentheses-easy.md](string/stack/20.-valid-parentheses-easy.md)
{% endcontent-ref %}

{% content-ref url="string/stack/32.-longest-valid-parentheses-hard.md" %}
[32.-longest-valid-parentheses-hard.md](string/stack/32.-longest-valid-parentheses-hard.md)
{% endcontent-ref %}

{% content-ref url="string/stack/921.-minimum-add-to-make-parentheses-valid-medium.md" %}
[921.-minimum-add-to-make-parentheses-valid-medium.md](string/stack/921.-minimum-add-to-make-parentheses-valid-medium.md)
{% endcontent-ref %}

{% content-ref url="string/stack/1541.-minimum-insertions-to-balance-a-parentheses-string-medium.md" %}
[1541.-minimum-insertions-to-balance-a-parentheses-string-medium.md](string/stack/1541.-minimum-insertions-to-balance-a-parentheses-string-medium.md)
{% endcontent-ref %}
