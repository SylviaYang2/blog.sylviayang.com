---
description: String
---

# String

### Java

```java
String s1 = new String();
String s2 = "sylvia";
int len2 = s2.length();
s2.substring(3, 6) // return "via"

StringBuilder sb = new StringBuilder(s2);
sb.append("yang");
String s3 = sb.toString(); // return "sylviayang"

char[] s3Arr = s3.toCharArray();
char ch = s3.charAt(3); // return 'v'
int index = s3.indexOf('y') // return 1

```

* 如果希望String可变，使用toCharArray将其转换为char array；
* 如果经常连接String，可以使用StringBuilder
