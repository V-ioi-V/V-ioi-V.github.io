---
title: STL算法:next_permutation求全排列 
tags: 常识
category: 算法
date: 2019-03-07 18:00 
---



求全排列用

<!--more-->

<font size=5> 



## 代码

- next_permutation:由原排列得到字典序中下一次最近排列

```c++
int main() {
    int a[] = {1, 2, 3};
    do {
        cout << a[0] << " " << a[1] << " " << a[2] << endl;
    } while (next_permutation(a, a + 3));
    return 0;
}
/*1 2 3
3 2
1 3
3 1
1 2
2 1*/
```

- prev_permutation:由原排列得到字典序中上一次最近排列

```c++
int main() {
    int a[] = {3, 2, 1};
    do {
        cout << a[0] << " " << a[1] << " " << a[2] << endl;
    } while (prev_permutation(a, a + 3));
    return 0;
}
/*3 2 1
1 2
3 1
1 3
3 2
2 3*/
```

