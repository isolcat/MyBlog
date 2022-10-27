---
title: Acwing算法基础
author: Sat Naing
datetime: 2022-07-31T16:55:12.000+00:00
slug: how-do-i-develop-my-portfolio-and-blog
featured: false
draft: false
tags:
  - 算法
  - Blog
description:
  Acwing算法基础
---

# Acwing算法基础

## 课下工作

- 需要背住`代码模板`
- 做题目 快速的使用模板做出相应的题目
- 重复**3-5次**已经ac的算法题目，删了再重新写一遍，加强记忆




## 快速排序

快排的基本思想是**分治**

分治算法都有三步：
1. 分成子问题
2. 递归处理子问题
3. 子问题合并



基本步骤：

1. 确定分界点：q[l],q[(1+r)/2],q[r],随机
2. 调整区间：使得第一个区间的数全部小于等于x，第二个区间全部大于等于x
3. 递归处理左右两端

<img src="https://pic.rmb.bdstatic.com/bjh/1817ac096945c6f1345c04b6bf0678dd.png" alt="image-20220621145730371.png" style="zoom:50%;" />

第二步是最复杂的，因此我们在第二步的时候可以采用`两个指针`来进行运算!![20220702213636.png](https://pic.rmb.bdstatic.com/bjh/d43bc3f5ca6e18a4a58cf34f9616bf1d.png)

当左指针遇见的数小于等于x的时候，指针不变，当指针遇见的数大于x的时候，与右指针的数进行交换；右指针同理的思路；当两个指针相遇或者穿过的时候，调整范围完成

### 快速排序模板

```c++
void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;//当只有一个数的时候，不进行排序，直接返回

    int i = l - 1, j = r + 1, x = q[l + r >> 1];//l+r>>1相当于(l+r)/2,由于加号比移位符优先级高，所以不需要加()
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);//swap为交换函数
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}
```

```javascript
function quickSort(ary,l,r){
    if(l>=r) return;//当只有一个数的时候，不进行排序，直接返回
    
    let std = ary[l],i = l -1,j = r +1
    while(i<j){
        while(std>ary[++i]);
        while(std<ary[--j]);
        if(i<j)
        {
            [ary[i],ary[j]] = [ary[j],ary[i]]//交换两个数
        }
    }
    //递归处理左右两段
    quickSort(ary,l,j)
    quickSort(ary,j+1,r)

}
```

## 归并排序

快排是**先分完再递归两边**，归并是**先递归两边再排**(快排不稳定，归并排稳定)

![20220702213655.png](https://pic.rmb.bdstatic.com/bjh/d88c88e5efb2ea6b956f33832b7c6596.png)

1. 确定分界点：`mid=(l+r)/2`
2. 递归排序left right
3. 归并————合二为一(将两个有序的数组合并成一个有序的数组)

![](https://raw.githubusercontent.com/isolcat/images/master/20220702213706.png)

<img src="C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220622222325099.png" alt="image-20220622222325099" style="zoom:33%;" />

两个顺序排列的数组(从小到大排列)，现需要合并两个数组，将两个数组也进行顺序排列，这时就可以用归并排序。先取两个数组的第一个数为该数组的指针，将两个数组的指针进行大小的比较，这时候较小的那个指针就是两个数组中最小的那位，我们将其写在新的数组res上的第一位，然后该指针向后移动一位，然后再与下方的指针进行比较，再进行同样的操作，直到有一个数组的指针指向了末尾

### 归并排序模板

```cpp
void merge_sort(int q[], int l, int r)//q是我们需要排序的数组，l为需要排序的左数组，r为需要排序的右数组
{
    if (l >= r) return;//当只有一个数的时候不进行排序，直接返回

    int mid = l + r >> 1;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);

    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];//tmp是cpp的模板，使用模板来定义函数和类
        else tmp[k ++ ] = q[j ++ ];

    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];

    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```

```javascript
function merge_sort(nums, l, r) {
    if(l >= r) return;
    const mid = l + r >> 1;
    merge_sort(nums, l, mid);
    merge_sort(nums, mid + 1, r);
    
    let i = l, j = mid + 1, k = 0;
    while(i <= mid && j <= r) {
        if(nums[i] <= nums[j]) 
            tmp[k++] = nums[i++];
        else 
            tmp[k++] = nums[j++];
    }   
    while(i <= mid) tmp[k++] = nums[i++];
    while(j <= r) tmp[k++] = nums[j++];
    for(i = l, j = 0; j < k; ++i, ++j) nums[i] = tmp[j];
}
```

求逆序对数量的题基本上就用归并排序，不然很容易超时

求逆序对数量的思想：<img src="https://pic.rmb.bdstatic.com/bjh/4a3a95d48a219584bda44ce27239e4c1.png" alt="20220702213717.png" style="zoom:50%;" />

### 总结

大范围快排，小范围归并



## 整数二分

**有单调性一定可以二分**，可以二分的题目不一定有单调性(二分的本质不是单调性)

二分的本质是**边界**，只要找到某种性质，使得整个区间一分为二，那么就可以用二分把边界点二分出来

<img src="https://pic.rmb.bdstatic.com/bjh/bc618eff296d7a87feecef0a97db82ac.png" alt="20220702213725.png" style="zoom:50%;" />

**二分的中心思想**：时时刻刻保证答案在我们划分的区间内部，当区间为1的时候，则为我们需要的答案

整数二分模板：

```cpp
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

```js
let arr = []
//greater or equal to n;  GE
let checkGE = n => i => arr[i] >= n;
//less or equal to n; LE
let checkLE = n => i => arr[i] <= n;

//查找check区间的左边界
let bsearchL = (l, r, checkMid) => {
    while (l < r) {
        let mid = parseInt((l + r) / 2)
        //满足条件向左缩小
        if (checkMid(mid)) r = mid;
        //不满足向右
        else l = mid + 1;
    }
    return l;
}

//查找check区间的右边界
let bsearchR = (l, r, checkMid) => {
    while (l < r) {
        let mid = parseInt((l + r + 1) / 2)
        //满足条件向右缩小
        if (checkMid(mid)) l = mid;
        //不满足向左
        else r = mid - 1;
    }
    return l;
}
```



## 浮点数二分

```cpp
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```



## 前缀合运算

前缀和的数组下标一定要**从1开始**，为了让我们可以定义出来S0（方便我们处理边界问题，比如当我们求[1,10]的时候就是S10-S0）

前缀和算不上一个模板，更多的是一种思想，我们需要掌握这个思想

<img src="https://pic.rmb.bdstatic.com/bjh/926aee5f0a9c22a7c556d9371b3a9f0a.png" alt="20220707172813.png" style="zoom:50%;" />

前缀和的作用就是求出**数组中的任意一段数组的和**：

<img src="C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220707174404116.png" alt="image-20220707174404116" style="zoom:50%;" />

### 一维前缀和模板

```cpp
S[i] = a[1] + a[2] + ... a[i]
a[l] + ... + a[r] = S[r] - S[l - 1]
```

一维前缀和的思想还可以运用在**求面积上**，例如，求出图中的`小方块`的面积：

<img src="C:\Users\HSDN\AppData\Roaming\Typora\typora-user-images\image-20220707213821937.png" alt="image-20220707213821937" style="zoom:50%;" />

### 二维前缀和模板

```cpp
S[i, j] = 第i行j列格子左上部分所有元素的和
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] + S[x1 - 1, y1 - 1]
```



## 差分

`前缀和`和`差分`互为逆运算

<img src="https://pic.rmb.bdstatic.com/bjh/7fd2c90ac64bb0c703d3588768d3a19b.png" alt="20220708203729.png" style="zoom: 80%;" />

例如:

> 输入一个长度为 nn 的整数序列。
>
> 接下来输入 mm 个操作，每个操作包含三个整数 l,r,cl,r,c，表示将序列中 [l,r][l,r] 之间的每个数加上 cc。
>
> 请你输出进行完所有操作后的序列。

### 一维差分

```cpp
给区间[l, r]中的每个数加上c：B[l] += c, B[r + 1] -= c
```



### 二维差分

```cpp
给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：
S[x1, y1] += c, S[x2 + 1, y1] -= c, S[x1, y2 + 1] -= c, S[x2 + 1, y2 + 1] += c
```



## 双指针算法

`快排`也是双指针算法

双指针算法的每道题的基本模板是差不多的，基本上都是这个，遇见了双指针算法的题先将模板套用上去：

`第一层循环j不一定等于0，可以等于其他的数`，`check(i,j)意思为当满足某种性质的时候，j++`

![image.png](https://pic.rmb.bdstatic.com/bjh/aba62229aa415f8109271bb6622fc1d1.png)

双指针算法的作用：**优化**，将一些朴素的暴力算法(例如复杂度为O(n^2)给优化为O(n))：![image.png](https://pic.rmb.bdstatic.com/bjh/9f1c858f1cf7edfd9d0e729889f1f473.png)

```cpp
for (int i = 0, j = 0; i < n; i ++ )
{
    while (j < i && check(i, j)) j ++ ;

    // 具体问题的逻辑
}
常见问题分类：
    (1) 对于一个序列，用两个指针维护一段区间
    (2) 对于两个序列，维护某种次序，比如归并排序中合并两个有序序列的操作
```



## n的二进制表示中 第k位是？

<img src="https://pic.rmb.bdstatic.com/bjh/094d62d51fa0079901f507f0a8dd66fd.png" alt="image.png" style="zoom:50%;" />

### 例如：

给定一个长度为n的数列，请你求出数列中每个数的二进制表示中1的个数

<img src="https://pic.rmb.bdstatic.com/bjh/5041aeb1caec7fa675eea024ab0709b3.png" alt="image.png" style="zoom: 80%;" />



## 离散化

<img src="https://pic.rmb.bdstatic.com/bjh/27449be38eb25f5c6524c24d30bb3ddf.png" alt="image.png" style="zoom:80%;" />

离散化会导致的问题：

- a[]中可能有重复的元素 `去重`

- 如何算出x离散化后的值  `二分`

  

  具体代码的实现：

  ```cpp
  vector<int> alls; // 存储所有待离散化的值
  sort(alls.begin(), alls.end()); // 将所有值排序
  alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素
  
  // 二分求出x对应的离散化的值
  int find(int x) // 找到第一个大于等于x的位置
  {
      int l = 0, r = alls.size() - 1;
      while (l < r)
      {
          int mid = l + r >> 1;
          if (alls[mid] >= x) r = mid;
          else l = mid + 1;
      }
      return r + 1; // 映射到1, 2, ...n
  }
  ```

  

  ### 例题

  <img src="https://pic.rmb.bdstatic.com/bjh/dcde3198aab730d09305b5e8143b0fcc.png" alt="image.png" style="zoom: 67%;" />



## 区间合并

<img src="https://pic.rmb.bdstatic.com/bjh/60afd49f0fb93485e35cd644b301b7cd.png" alt="image.png" style="zoom:50%;" />

```cpp
// 将所有存在交集的区间合并
void merge(vector<PII> &segs)
{
    vector<PII> res;

    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);

    if (st != -2e9) res.push_back({st, ed});

    segs = res;
}
```

### 例题：

> 给定 nn 个区间 [li,ri][li,ri]，要求合并所有有交集的区间。
>
> 注意如果在端点处相交，也算有交集。
>
> 输出合并完成后的区间个数。
>
> 例如：[1,3][1,3] 和 [2,6][2,6] 可以合并为一个区间 [1,6][1,6]。

合并完后可以得到三个区间

<img src="https://pic.rmb.bdstatic.com/bjh/18df60169dc06588e58b48c76da03cb2.png" alt="image.png" style="zoom:50%;" />

#### 步骤：

1. 按照区间左端点进行排序
2. 左端点简称`st`,右端点简称`ed`
3. <img src="https://pic.rmb.bdstatic.com/bjh/f126eee5d276bde3b0d4a5e3b9c67f8b.png" alt="image.png" style="zoom:50%;" />

只会出现上述的这三种情况(绿色)，不会出现在st前面的情况(因为我们事先从小到大进行了排序的)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

void merge(vector<PII> &segs)
{
    vector<PII> res;

    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);

    if (st != -2e9) res.push_back({st, ed});

    segs = res;
}

int main()
{
    int n;
    scanf("%d", &n);

    vector<PII> segs;
    for (int i = 0; i < n; i ++ )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        segs.push_back({l, r});
    }

    merge(segs);

    cout << segs.size() << endl;

    return 0;
}
```

