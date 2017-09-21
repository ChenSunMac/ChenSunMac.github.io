---
layout: post
title: LeetCode Array
date: 2017-09-22
tags: LeetCode C 
categories: C LeetCode
---

### 27. Remove Element
Given an array and a value, remove all instances of that value in place and return the new length.

*Do not allocate extra space for another array*, you must do this in place with constant memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

- Example:
Given input array $nums = [3,2,2,3]$, val = 3

Your function should return length = 2, with the first two elements of nums being 2.

#### 思路
因为不给用多余的空间，array本省插入和删除的挪动又很麻烦，所以我们利用SWAP去做，每当发现$nums[i] == val$时，我们直接将要删除的这个element和数列最后的element交换，这样，就相当于删除了这个节点的值，而且不需要移动其他element.

```c
int removeElement(int* nums, int numsSize, int val) {
    int i = 0;
    while (i < numsSize) {
        if (nums[i] == val) {
            nums[i] = nums[numsSize - 1];
            numsSize--;
        }
        else
            i++;
    }
    return numsSize;
}
```
缺点： 数列顺序会打乱
<br>
改进： 不使用swap，使用两个指针，每当检测到需要删除的值时，主数列的指针继续增加，而"新数列"的指针待命
```c
int removeElement(int* nums, int numsSize, int val) {
        int start = 0;
        for(int i = 0; i < numsSize; i++)
            if (val != nums[i]) {
                nums[start++] = nums[i];
            }
        return start;
    }
```