---
title: Sort Algorithm
# cover: '../image/default.png'
published: 2024-05-20
description: "Commonly Used Sorting Algorithms."
tags: ["Sort", "C++"]
category: Algorithm
draft: false
---



## 一、冒泡排序

**算法思想**：依次比较两个元素，根据大小关系交换位置，把大元素或者小元素

**复杂度分析**：

- 空间复杂度：$O(1)$
- 时间复杂度：**$O(n^2)$**

**稳定性**：稳定，相同的元素相对位置不发生改变

**实例代码**：

```c
void bubbleSort(vector<int>& arr) { // 升序方式
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        for (int j = 0; j < n - i - 1; ++j) {
            // 如果当前元素大于下一个元素，则交换它们
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
            }
        }
    }
}
```

## 二、插入排序

**算法思想**：打扑克时整理手中的牌，讲未排序部分，依次根据大小插入排序的部分

**复杂度分析**：

- 空间复杂度：$O(1)$
- 时间复杂度：$O(n^2)$

**稳定性**：稳定

**实例代码**：

```c
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        
        // 将当前元素与已排序部分中的元素依次比较并向右移动
        // 直到找到当前元素的正确位置
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            --j;
        }
        arr[j + 1] = key;
    }
}
```

## 三、选择排序

**算法思想**：每次选出最大值（最小值）与起始位置交换。

- 举例：一组数[1,8,9,1,3]
- **2** 8 9 **1** 3 第一轮选出min = 1,与2交换
- 1 **8** 9 **2** 3 第二轮选出min = 2,与8交换

**复杂度分析**：

- 空间复杂度：$O(1)$
- 时间复杂度：$O(n^2)$

**稳定性**：不稳定

**实例代码**：

```c
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        int minIndex = i;
        // 在未排序部分中找到最小元素的索引
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        // 将最小元素与未排序部分的第一个元素交换位置
        swap(arr[i], arr[minIndex]);
    }
}
```

## 四、快速排序

**算法思想**：选择一个Pivot,比Pivot大的在一侧，比Pivot小的在另一侧，分治的思想

**复杂度分析**：

- 空间复杂度：$O(1)$
- 时间复杂度：$O(nlogn)$，最坏为$O(n^2)$，每次都取到最大数的情况

**稳定性**：不稳定

**实例代码**：

```c
// 分区操作函数
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high]; // 选择最后一个元素作为基准值
    int i = low - 1; // 设置一个指针用于标记小于基准值的区域
    
    for (int j = low; j < high; ++j) {
        // 如果当前元素小于基准值，则将其交换到小于基准值的区域
        if (arr[j] < pivot) {
            ++i;
            swap(arr[i], arr[j]);
        }
    }
    
    // 将基准值放置到正确的位置上
    swap(arr[i + 1], arr[high]);
    
    return i + 1; // 返回基准值的位置
}

// 快速排序函数
void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        // 分区操作，将数组分成两部分，并返回基准值的位置
        int pivotIndex = partition(arr, low, high);
        
        // 递归排序基准值左边的部分
        quickSort(arr, low, pivotIndex - 1);
        // 递归排序基准值右边的部分
        quickSort(arr, pivotIndex + 1, high);
    }
}
```

## 五、归并排序

**算法思想**：采用分治的思想，分成小块，然后数组合并

**复杂度分析**：

- 空间复杂度：$O(n)$
- 时间复杂度：$O(nlogn)$

**稳定性**：稳定

**实例代码**：

```c
// 合并两个有序子序列
void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // 创建临时数组来存储两个子序列
    vector<int> L(n1), R(n2);

    // 将数据复制到临时数组中
    for (int i = 0; i < n1; ++i)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; ++j)
        R[j] = arr[mid + 1 + j];

    // 合并两个有序子序列到原数组
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            ++i;
        } else {
            arr[k] = R[j];
            ++j;
        }
        ++k;
    }

    // 处理剩余元素
    while (i < n1) {
        arr[k] = L[i];
        ++i;
        ++k;
    }
    while (j < n2) {
        arr[k] = R[j];
        ++j;
        ++k;
    }
}

// 归并排序
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;

        // 分解
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        // 合并
        merge(arr, left, mid, right);
    }
}
```
