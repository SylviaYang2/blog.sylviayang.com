# QuickSort & QuickSelect

## QuickSort:

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

Steps:

1. Pick the last one as pivot (OR: use random pivot -> reduce the worst case)
2. move all smaller elements to the left
3. move pivot to its final place

这个分区容易理解点，有点双指针技巧含义，选最后一个元素作为分区点，指针 p 表示比分区值小的元素应该放的位置，指针 i 只用来遍历。当 i 遍历到比分区值小的元素时，放到指针 p 的位置（通过交换实现）。当 p 遍历完时，\[lo, p - 1] 都是比分区值小的元素，\[p, hi - 1] 都是比分区值大的元素，最后交换一下分区值和 p 所指向的元素便实现了 pivot 左边都是比它小的元素，右边都是比它大的元素。

```java
public void sort(int[] nums) { 
    sort(nums, 0, nums.length - 1); 
} 
 
private void sort(int[] nums, int lo, int hi) { 
    // when there's only one element, or invalid boundaries, return
    if (lo >= hi) { 
        return;
    }
    int pivot = partition(nums, lo, hi); 
    sort(nums, lo, pivot - 1); 
    sort(nums, pivot + 1, hi); 
}

private static int partition(int[] arr, int lo, int hi) {
    int pivot = nums[hi];
    int pIndex = lo;

    for (int i = lo; i < hi; i++) {
        if (nums[i] <= pivot) {
            swap(nums, i, pIndex);
            pIndex += 1;
        }
    }
    swap(nums, pIndex, hi);
    return pIndex;
}
     
private void swap(int[] nums, int i, int j) { 
    int temp = nums[i]; 
    nums[i] = nums[j]; 
    nums[j] = temp; 
}
```

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

### Optimization:

1. Shuffle the elements in nums

```java
void shuffle(int[] nums) { 
    int n = nums.length; 
    Random rand = new Random(); 
    for (int i = 0 ; i < n; i++) { 
        // 从 i 到最后随机选一个元素 
        int r = i + rand.nextInt(n - i); 
        swap(nums, i, r); 
    } 
}
```



<figure><img src="../../../.gitbook/assets/image (31).png" alt="" width="517"><figcaption></figcaption></figure>



## QuickSelect:

Quickselect: (find the kth smallest / largest element)

* The partition part is the same as that of quick sort

<figure><img src="../../../.gitbook/assets/image (35).png" alt="" width="563"><figcaption></figcaption></figure>

```java
int pivot = partition(arr, start, end)

if (pivot > k - 1) {
    return quickselect(start, pivot - 1);
} else if (pivot < k - 1) {
    return quickselect(pivot + 1, end);
} else {
    return nums[k-1];
}
```
