---
title: "二分查找真正难的地方是边界"
date: 2026-10-13
tags: ["CS"]
summary: "二分查找的思路三分钟就能讲完，但写好边界条件需要一套固定的思考方式。"
---

## 二分的直觉与陷阱

二分查找的思路一眼就能看懂：每次把搜索区间砍掉一半。但它也是面试里最容易写错的题目之一，原因几乎总是同一个——**区间到底表示什么**。

写之前先回答一个问题：你的区间是闭区间 `[lo, hi]`，还是左闭右开 `[lo, hi)`？这个问题回答清楚了，循环条件、中点更新、返回值就都跟着确定下来。

## 两种写法的对照

### 闭区间写法

区间为 `[lo, hi]`，两端都可能是答案，所以循环条件是 `lo <= hi`。

```python
def binary_search(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

### 左闭右开写法

区间为 `[lo, hi)`，`hi` 本身不在候选集合里，所以循环条件是 `lo < hi`，右边界收缩时直接赋值 `hi = mid`。

```python
def binary_search(nums, target):
    lo, hi = 0, len(nums)
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid
    return -1
```

两种写法都对，混着写一定会出错。选一种，然后在所有二分题里坚持它。

## 一个不容易注意的细节

> 计算中点时写 `lo + (hi - lo) // 2`，而不写 `(lo + hi) // 2`。

在 Python 里整数不会溢出，所以两者等价。但在 C++ 或 Java 中，`lo + hi` 可能超过 `int` 的上界，得到负数，随后数组下标越界。这个坑在标准库的实现里真实出现过，属于「知道一次就再也不会写错」的知识。

## 边界题才是主战场

真正需要二分的题目，往往不是「找一个数」，而是「找满足条件的第一个位置」。比如：

| 问题 | 单调条件 |
| --- | --- |
| 第一个大于等于 target 的位置 | `nums[i] >= target` |
| 旋转数组的最小值 | `nums[i] <= nums[n-1]` |
| 求平方根的整数部分 | `i * i <= x` |

这类问题可以统一成一个模板：**在一个布尔数组里，找第一个 `true` 的位置**。只要能把原问题转化成这个形式，代码几乎不用改。

```python
def first_true(lo, hi, pred):
    """返回 [lo, hi) 中第一个使 pred 为真的下标"""
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if pred(mid):
            hi = mid
        else:
            lo = mid + 1
    return lo
```

其中 `pred` 必须满足「前面全假、后面全真」。这是二分能用的全部前提。

## 小结

二分查找的难点不在思路，而在于把不变量写下来。我的习惯是每次动笔前先注释一行区间定义，再开始写循环——这一个动作能消掉大部分边界错误。
