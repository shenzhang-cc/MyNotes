# Data Structure


## 知识网络

<img src="Data%20Structure.assets/%E7%9F%A5%E8%AF%86%E7%BD%91%E7%BB%9C.jpg" style="zoom:80%;" />



## 基本概念

<img src="Data%20Structure.assets/%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5.jpg" style="zoom:80%;" />



### 复杂度渐进表示法

<img src="Data%20Structure.assets/image-20200114210100803.png" alt="image-20200114210100803" style="zoom:80%;" />

*T(n) = n! 的算法不可用*

### 复杂度分析技巧

<img src="Data%20Structure.assets/image-20200114210442483.png" alt="image-20200114210442483" style="zoom:80%;" />


### 应用实例：最大子列和问题

给定n个整数的序列<img src="https://latex.codecogs.com/gif.latex?${A_1,&space;A_2,&space;...,A_n}$" title="${A_1, A_2, ...,A_n}$" />, 求函数<img src="https://latex.codecogs.com/gif.latex?$f(i,&space;j)&space;=&space;max(0,\sum_{k=i}^jA_k)$" title="$f(i, j) = max(0,\sum_{k=i}^jA_k)$" />的最大值。

**法一**

遍历法，找出所有子列的和，求最大值

```c++
int MaxSubSeqSum_1(vector<int> A) { // 遍历法，找出所有子列的和，求最大值
    int maxSum = 0, tempSum;
    for (int i = 0; i < A.size(); i++)
    {
        for (int j = i; j < A.size(); j++)
        {
            tempSum = 0;
            for (int k = i; k < j; k++)
            {
                tempSum += A[k];
            }
            if (tempSum > maxSum) maxSum = tempSum;
        }
    }
    return maxSum;
}
```
**法二**

优化法一，去掉*k*的循环

```c++
int MaxSubSeqSum_2(vector<int> A) { //遍历法改进，去掉k的循环
    int maxSum = 0, tempSum;
    for (int i = 0; i < A.size(); i++)
        {
            tempSum = 0;
            for (int j = i; j < A.size(); j++)
            {
                tempSum += A[j];
                if (tempSum > maxSum) maxSum = tempSum;
            }
        }
    return maxSum;
}
```

**法三**

分而治之（递归式）

```c++
int MaxSubSeqSum_3(vector<int> A) {  //分而治之，递归式
    return divideAndConquer(A, 0, A.size() - 1);
}

int divideAndConquer(vector<int>& A, int left, int right) {
    int maxLeftSum = 0, maxRightSum = 0, 
        maxLeftBorderSum = 0, maxRightBorderSum = 0, maxBorderSum = 0;
    int tempSumL = 0, tempSumR = 0; 
    if (left == right)  // 递归出口
    {
        if (A[left] > 0) return A[left];
        else return 0;
    }
    int center = (left + right) / 2;
    maxLeftSum = divideAndConquer(A, left, center); // 左区域子列和最大值
    maxRightSum = divideAndConquer(A, center + 1, right);  // 右区域子列和最大值
    for (int i = center; i >= left; i--)  // 由分界线向左扫描找最大值
    {
        tempSumL += A[i];
        if (tempSumL > maxLeftBorderSum) maxLeftBorderSum = tempSumL;
    }
    for (int i = center + 1; i <= right; i++)  // 由分界向右扫描找最大值
    {
        tempSumR += A[i];
        if (tempSumR > maxRightBorderSum) maxRightBorderSum = tempSumR;
    }
    maxBorderSum = maxLeftBorderSum + maxRightBorderSum;
    return maxLeftSum > maxRightSum ? (maxLeftSum > maxBorderSum ? maxLeftSum : maxBorderSum) 
        : (maxRightSum > maxBorderSum ? maxRightSum : maxBorderSum);
}
```

**法四**
在线处理

```c++
int MaxSubSeqSum_4(vector<int> A) {  // 在线处理
    int tempSum = 0, maxSum = 0;
    for (int i = 0; i < A.size(); i++)
    {
        tempSum += A[i];
        if (tempSum > maxSum) maxSum = tempSum;
        else if (tempSum < 0) tempSum = 0;			
    }
    return maxSum;
}
```