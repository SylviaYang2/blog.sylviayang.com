# Trie

**Trie node structure**

Trie is a rooted tree. Its nodes have the following fields:

* Maximum of $$R$$ links to its children, where each link corresponds to one of $$R$$ character values from dataset alphabet.\
  In this article we assume that $$R$$ is 26, the number of **lowercase** latin letters.
* **Note**: if not guaranteed all **lowercase**, then we use a **HashMap**\<Character, TrieNode> structure
* Boolean field **`isEnd`** which specifies whether the node corresponds to the end of the key, or is just a key prefix.

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (9) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

### Application:

1. Autocomplete
2. Spell Checker
3. IP routing (Longest prefix matching)

### Note:

1. 关于 `Map` 和 `Set`，是两个抽象数据结构（接口），`Map` 存储一个键值对集合，其中键不重复，`Set` 存储一个不重复的元素集合。本质上 `Set` 可以视为一种特殊的 `Map`，`Set` 其实就是 `Map` 中的键。
2. 常见的 `Map` 和 `Set` 的底层实现原理有哈希表和二叉搜索树两种，比如 Java 的 HashMap/HashSet 和 C++ 的 unorderd\_map/unordered\_set 底层就是用哈希表实现，而 Java 的 TreeMap/TreeSet 和 C++ 的 map/set 底层使用红黑树这种自平衡 BST 实现的。
3. 比如 `HashMap<K, V>` 的优势是能够在 O(1) 时间通过键查找对应的值，但要求键的类型 `K` 必须是「可哈希」的；而 `TreeMap<K, V>` 的特点是方便根据键的大小进行操作，但要求键的类型 `K` 必须是「可比较」的。
4. TrieSet/TrieMap 底层则用 Trie 树这种结构来实现。`TrieMap` 也是类似的，由于 Trie 树原理，我们要求 `TrieMap<V>` 的键必须是字符串类型，值的类型 `V` 可以随意。

