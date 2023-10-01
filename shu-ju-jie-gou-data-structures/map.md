---
description: Map
---

# Map

Map可以返回key的Set，value的Collection，key-value的EntrySet；

**Demo**：

```java
Map<String, Integer> map = new HashMap<>();
map.put("sylvia", 520);
map.put("yang", 520);

Set<String> set = map.keySet();
List<Integer> values = new ArrayList<>(map.values());
for (Map.Entry<String, Integer> entry: map.entrySet()) {
    String key = entry.getKey();
    int value = entry.getValue();
}
```

<figure><img src="../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>
