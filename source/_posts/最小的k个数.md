---
title: 最小的k个数
tags: 排序
category: 算法
---

经典top(k)问题

<!--more-->

# 题目描述

输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

**示例 1：**

```txt
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

**示例 2：**

```txt
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

 

**限制：**

- `0 <= k <= arr.length <= 10000`
- `0 <= arr[i] <= 10000`

## 堆

​       用一个大根堆来维护，首先将前k个数推进去，然后后面进来的每个数都跟堆头（最大的元素）进行比较，如果小于那个数就推进去，比那个数大就不推进去，可以保证每时每刻这个堆里的数就是遍历到的序列里最小的k个数。

**复杂度分析**

- 时间复杂度：O(nlog k)，其中 n是数组 `arr` 的长度。由于大根堆实时维护前 k 小值，所以插入删除都是  O(log k)的时间复杂度，最坏情况下数组里 n个数都会插入，所以一共需要O(nlog k) 的时间复杂度。
- 空间复杂度：O(k)*O，因为大根堆里最多 k个数。

```c++
class Solution {
    vector<int> a;
public:
    //priotity_queue也可以
    void push(int x) {
        a.push_back(x);
        int n = a.size();
        while ((n / 2) >= 1) {
            int cnt = n / 2;
            if (a[n - 1] > a[cnt - 1])swap(a[n - 1], a[cnt-1]);
            else break;
            n = cnt;
        }
    }

    void pop() {
        swap(a[0], a[a.size() - 1]);
        a.pop_back();
        int n = 1;
        while ((n * 2) <= a.size()) {
            int cnt = n * 2;
            if (cnt + 1 <= a.size() && a[cnt - 1] < a[cnt])cnt++;
            if (a[n-1] < a[cnt - 1])swap(a[n-1], a[cnt - 1]);
            else break;
            n = cnt;
        }
    }

public:
    vector<int> getLeastNumbers(vector<int> &arr, int k) {
        vector<int> ans;
        if (k == 0)return a;
        for (int i = 0; i < k; i++)push(arr[i]);
        for (int i = k; i < arr.size(); i++) {
            if (arr[i] < a[0]) {
                pop();
                push(arr[i]);
            }
        }
        return a;
    }
};
```

## 基于快速排序的思想

​       基于快排的原理，选择一个基准数，然后将这个基准数放到正确的位置，看有多少数比这个基准数小或相等，如果个数正好等与k，左边的k个数就直接输出就好，如果基准数及其左边的个数少于k个，就处理右边的序列，从右边的序列中处理处剩下的最小的数，如果个数大于k个，就处理基准数左边的序列，从中选取k个最小的数。与快排不同的是，快排需要处理完一个序列后，还要将其左边和右边的序列都进行处理。

  **复杂度分析**

- 时间复杂度：期望为 O(n)*O*(*n*) ，由于证明过程很繁琐。

  最坏情况下的时间复杂度为 O(n^2)。情况最差时，每次的划分点都是最大值或最小值，一共需要划分 n - 1次，而一次划分需要线性的时间复杂度，所以最坏情况下时间复杂度为 O(n^2)。

- 空间复杂度：期望为 O(log n)，递归调用的期望深度为 O(log n)，每层需要的空间为 O(1)，只有常数个变量。

  最坏情况下的空间复杂度为 O(n)。最坏情况下需要划分 n*次，即 `change` 函数递归调用最深 n - 1*层，而每层由于需要 O(1)*的空间，所以一共需要 O(n)*的空间复杂度。

```c++
class Solution {
public:
    int quick(vector<int> &a, int l, int r, int k) {//基于快排原理
        int n = rand() % (r - l + 1) + l;
        swap(a[n], a[l]);
        int i = l, j = r;
        while (i < j) {
            while(i<j&&a[j] >= a[l])j--;
            while(i<j&&a[i] <= a[l])i++;
            if (i < j)swap(a[i], a[j]);
        }
        swap(a[i], a[l]);
        return i;
    }

public:
    void change(vector<int> &a, int l, int r, int k) {
        if (l > r)return;
        int cnt = quick(a, l, r, k);
        int num = cnt - l + 1;
        if (num == k)return;
        else if (num < k)change(a, cnt + 1, r, k - num);
        else change(a, l, cnt - 1, k);
    }

public:
    vector<int> getLeastNumbers(vector<int> &arr, int k) {
        srand((unsigned) time(0));
        change(arr, 0, arr.size()-1, k);
        vector<int> ans;
        for (int i = 0; i < k; i++)
            ans.push_back(arr[i]);
        return ans;
    }
};

```

