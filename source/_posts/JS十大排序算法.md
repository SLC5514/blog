---
title: JS十大排序算法
date: 2019-12-06 10:07:25
categories:
  - [JavaScript]
  - [算法]
tags:
  - JS
  - 算法
---

排序算法（**Sorting algorithm**）是计算机科学最古老、最基本的课题之一。要想成为合格的程序员，就必须理解和掌握各种排序算法。

目前，最常见的排序算法大概有七八种，其中"快速排序"（**Quicksort**）使用得最广泛，速度也较快。

<!--more-->

{% img [class names] /images/sort/js-sort.webp %}

## 冒泡排序

通过相邻元素的比较和交换，使得每一趟循环都能找到未有序数组的最大值或最小值。

最好：`O(n)`，只需要冒泡一次数组就有序了。
最坏：`O(n²)`
平均：`O(n²)`

### 单向冒泡

```js
function bubbleSort(nums) {
  for (let i = 0, len = nums.length; i < len - 1; i++) {
    // 如果一轮比较中没有需要交换的数据，则说明数组已经有序。主要是对[5,1,2,3,4]之类的数组进行优化
    let mark = true;
    for (let j = 0; j < len - i - 1; j++) {
      if (nums[j] > nums[j + 1]) {
        [nums[j], nums[j + 1]] = [nums[j + 1], nums[j]];
        mark = false;
      }
    }
    if (mark) return;
  }
}
```

### 双向冒泡

普通的冒泡排序在一趟循环中只能找出一个最大值或最小值，双向冒泡则是多一轮循环既找出最大值也找出最小值。

```js
function bubbleSort_twoWays(nums) {
  let low = 0;
  let high = nums.length - 1;
  while (low < high) {
    let mark = true;
    // 找到最大值放到右边
    for (let i = low; i < high; i++) {
      if (nums[i] > nums[i + 1]) {
        [nums[i], nums[i + 1]] = [nums[i + 1], nums[i]];
        mark = false;
      }
    }
    high--;
    // 找到最小值放到左边
    for (let j = high; j > low; j--) {
      if (nums[j] < nums[j - 1]) {
        [nums[j], nums[j - 1]] = [nums[j - 1], nums[j]];
        mark = false;
      }
    }
    low++;
    if (mark) return;
  }
}
```

## 选择排序

{% img [class names] /images/sort/select-sort.gif %}

和冒泡排序相似，区别在于选择排序是将每一个元素和它后面的元素进行比较和交换。

最好：`O(n²)`
最坏：`O(n²)`
平均：`O(n²)`

```js
function selectSort(nums) {
  for (let i = 0, len = nums.length; i < len; i++) {
    for (let j = i + 1; j < len; j++) {
      if (nums[i] > nums[j]) {
        [nums[i], nums[j]] = [nums[j], nums[i]];
      }
    }
  }
}
```

## 插入排序

{% img [class names] /images/sort/insert-sort.gif %}

以第一个元素作为有序数组，其后的元素通过在这个已有序的数组中找到合适的位置并插入。

最好：`O(n)`，原数组已经是升序的。
最坏：`O(n²)`
平均：`O(n²)`

```js
function insertSort(nums) {
  for (let i = 1, len = nums.length; i < len; i++) {
    let temp = nums[i];
    let j = i;
    while (j >= 0 && temp < nums[j - 1]) {
      nums[j] = nums[j - 1];
      j--;
    }
    nums[j] = temp;
  }
}
```

## 快速排序

{% img [class names] /images/sort/quick-sort.gif %}

选择一个元素作为基数（通常是第一个元素），把比基数小的元素放到它左边，比基数大的元素放到它右边（相当于二分），再不断递归基数左右两边的序列。

最好：`O(n * logn)`，所有数均匀分布在基数的两边，此时的递归就是不断地二分左右序列。
最坏：`O(n²)`，所有数都分布在基数的一边，此时划分左右序列就相当于是插入排序。
平均：`O(n * logn)`

参考学习链接：
[算法 3：最常用的排序——快速排序](https://wiki.jikexueyuan.com/project/easy-learn-algorithm/fast-sort.html)

```js
var quickSort = function(arr) {
  if (!Array.isArray(arr)) return;
  if (arr.length <= 1) {
    return arr;
  }

  var pivotIndex = Math.floor(arr.length / 2);
  var pivot = arr.splice(pivotIndex, 1)[0];
  var left = [];
  var right = [];

  for (var i = 0; i < arr.length; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }

  // return [...quickSort(left), numValue, ...quickSort(right)];
  return quickSort(left).concat([pivot], quickSort(right));
};
```

## 归并排序

{% img [class names] /images/sort/merge-sort.gif %}

递归将数组分为两个序列，有序合并这两个序列。

最好：`O(n _ logn)`
最坏：`O(n _ logn)`
平均：`O(n * logn)`

参考学习链接：
[图解排序算法(四)之归并排序](https://www.cnblogs.com/chengxiao/p/6194356.html)

```js
function mergeSort(nums) {
  // 有序合并两个数组
  function merge(l1, r1, l2, r2) {
    let arr = [];
    let index = 0;
    let i = l1,
      j = l2;
    while (i <= r1 && j <= r2) {
      arr[index++] = nums[i] < nums[j] ? nums[i++] : nums[j++];
    }
    while (i <= r1) arr[index++] = nums[i++];
    while (j <= r2) arr[index++] = nums[j++];
    // 将有序合并后的数组修改回原数组
    for (let t = 0; t < index; t++) {
      nums[l1 + t] = arr[t];
    }
  }
  // 递归将数组分为两个序列
  function recursive(left, right) {
    if (left >= right) return;
    // 比起(left+right)/2，更推荐下面这种写法，可以避免数溢出
    let mid = parseInt((right - left) / 2) + left;
    recursive(left, mid);
    recursive(mid + 1, right);
    merge(left, mid, mid + 1, right);
    return nums;
  }
  recursive(0, nums.length - 1);
}
```

## 桶排序

取 n 个桶，根据数组的最大值和最小值确认每个桶存放的数的区间，将数组元素插入到相应的桶里，最后再合并各个桶。

最好：`O(n)`，每个数都在分布在一个桶里，这样就不用将数插入排序到桶里了(类似于计数排序以空间换时间)。
最坏：`O(n²)`，所有的数都分布在一个桶里。
平均：`O(n + k)`，k 表示桶的个数。

参考学习链接：
[拜托，面试别再问我桶排序了！！！](https://zhuanlan.51cto.com/art/201811/586129.htm)

```js
function bucketSort(nums) {
  // 桶的个数，只要是正数即可
  let num = 5;
  let max = Math.max(...nums);
  let min = Math.min(...nums);
  // 计算每个桶存放的数值范围，至少为1，
  let range = Math.ceil((max - min) / num) || 1;
  // 创建二维数组，第一维表示第几个桶，第二维表示该桶里存放的数
  let arr = Array.from(Array(num)).map(() => Array().fill(0));
  nums.forEach(val => {
    // 计算元素应该分布在哪个桶
    let index = parseInt((val - min) / range);
    // 防止index越界，例如当[5,1,1,2,0,0]时index会出现5
    index = index >= num ? num - 1 : index;
    let temp = arr[index];
    // 插入排序，将元素有序插入到桶中
    let j = temp.length - 1;
    while (j >= 0 && val < temp[j]) {
      temp[j + 1] = temp[j];
      j--;
    }
    temp[j + 1] = val;
  });
  // 修改回原数组
  let res = [].concat.apply([], arr);
  nums.forEach((val, i) => {
    nums[i] = res[i];
  });
}
```

## 基数排序

使用十个桶 0-9，把每个数从低位到高位根据位数放到相应的桶里，以此循环最大值的位数次。
**但只能排列正整数，因为遇到负号和小数点无法进行比较**。

最好：`O(n * k)`，k 表示最大值的位数。
最坏：`O(n * k)`
平均：`O(n * k)`

参考学习链接：
[算法总结系列之五: 基数排序(Radix Sort)](https://www.cnblogs.com/sun/archive/2008/06/26/1230095.html)

```js
function radixSort(nums) {
  // 计算位数
  function getDigits(n) {
    let sum = 0;
    while (n) {
      sum++;
      n = parseInt(n / 10);
    }
    return sum;
  }
  // 第一维表示位数即0-9，第二维表示里面存放的值
  let arr = Array.from(Array(10)).map(() => Array());
  let max = Math.max(...nums);
  let maxDigits = getDigits(max);
  for (let i = 0, len = nums.length; i < len; i++) {
    // 用0把每一个数都填充成相同的位数
    nums[i] = (nums[i] + "").padStart(maxDigits, 0);
    // 先根据个位数把每一个数放到相应的桶里
    let temp = nums[i][nums[i].length - 1];
    arr[temp].push(nums[i]);
  }
  // 循环判断每个位数
  for (let i = maxDigits - 2; i >= 0; i--) {
    // 循环每一个桶
    for (let j = 0; j <= 9; j++) {
      let temp = arr[j];
      let len = temp.length;
      // 根据当前的位数i把桶里的数放到相应的桶里
      while (len--) {
        let str = temp[0];
        temp.shift();
        arr[str[i]].push(str);
      }
    }
  }
  // 修改回原数组
  let res = [].concat.apply([], arr);
  nums.forEach((val, index) => {
    nums[index] = +res[index];
  });
}
```

## 计数排序

以数组元素值为键，出现次数为值存进一个临时数组，最后再遍历这个临时数组还原回原数组。
因为 JavaScript 的数组下标是以字符串形式存储的，所以**计数排序可以用来排列负数，但不可以排列小数**。

最好：`O(n + k)`，k 是最大值和最小值的差。
最坏：`O(n + k)`
平均：`O(n + k)`

```js
function countingSort(nums) {
  let arr = [];
  let max = Math.max(...nums);
  let min = Math.min(...nums);
  // 装桶
  for (let i = 0, len = nums.length; i < len; i++) {
    let temp = nums[i];
    arr[temp] = arr[temp] + 1 || 1;
  }
  let index = 0;
  // 还原原数组
  for (let i = min; i <= max; i++) {
    while (arr[i] > 0) {
      nums[index++] = i;
      arr[i]--;
    }
  }
}
```

## 计数排序优化

把每一个数组元素都加上 `min` 的相反数，来避免特殊情况下的空间浪费，通过这种优化可以把所开的空间大小从 `max+1` 降低为 `max-min+1`，`max` 和 `min` 分别为数组中的最大值和最小值。

比如数组 `[103, 102, 101, 100]`，普通的计数排序需要开一个长度为 104 的数组，而且前面 100 个值都是 `undefined`，使用该优化方法后可以只开一个长度为 4 的数组。

```js
function countingSort(nums) {
  let arr = [];
  let max = Math.max(...nums);
  let min = Math.min(...nums);
  // 加上最小值的相反数来缩小数组范围
  let add = -min;
  for (let i = 0, len = nums.length; i < len; i++) {
    let temp = nums[i];
    temp += add;
    arr[temp] = arr[temp] + 1 || 1;
  }
  let index = 0;
  for (let i = min; i <= max; i++) {
    let temp = arr[i + add];
    while (temp > 0) {
      nums[index++] = i;
      temp--;
    }
  }
}
```

## 堆排序

根据数组建立一个堆（类似完全二叉树），每个结点的值都大于左右结点（最大堆，通常用于升序），或小于左右结点（最小堆，通常用于降序）。对于升序排序，先构建最大堆后，交换堆顶元素（表示最大值）和堆底元素，每一次交换都能得到未有序序列的最大值。重新调整最大堆，再交换堆顶元素和堆底元素，重复 n-1 次后就能得到一个升序的数组。

最好：`O(n * logn)`，logn 是调整最大堆所花的时间。
最坏：`O(n * logn)`
平均：`O(n * logn)`

参考学习链接：
[常见排序算法 - 堆排序 (Heap Sort)](http://bubkoo.com/2014/01/14/sort-algorithm/heap-sort/)
[图解排序算法(三)之堆排序](https://www.cnblogs.com/chengxiao/p/6129630.html)

```js
function heapSort(nums) {
  // 调整最大堆，使index的值大于左右节点
  function adjustHeap(nums, index, size) {
    // 交换后可能会破坏堆结构，需要循环使得每一个父节点都大于左右结点
    while (true) {
      let max = index;
      let left = index * 2 + 1; // 左节点
      let right = index * 2 + 2; // 右节点
      if (left < size && nums[max] < nums[left]) max = left;
      if (right < size && nums[max] < nums[right]) max = right;
      // 如果左右结点大于当前的结点则交换，并再循环一遍判断交换后的左右结点位置是否破坏了堆结构（比左右结点小了）
      if (index !== max) {
        [nums[index], nums[max]] = [nums[max], nums[index]];
        index = max;
      } else {
        break;
      }
    }
  }
  // 建立最大堆
  function buildHeap(nums) {
    // 注意这里的头节点是从0开始的，所以最后一个非叶子结点是 parseInt(nums.length/2)-1
    let start = parseInt(nums.length / 2) - 1;
    let size = nums.length;
    // 从最后一个非叶子结点开始调整，直至堆顶。
    for (let i = start; i >= 0; i--) {
      adjustHeap(nums, i, size);
    }
  }

  buildHeap(nums);
  // 循环n-1次，每次循环后交换堆顶元素和堆底元素并重新调整堆结构
  for (let i = nums.length - 1; i > 0; i--) {
    [nums[i], nums[0]] = [nums[0], nums[i]];
    adjustHeap(nums, 0, i);
  }
}
```

## 希尔排序

通过某个增量 gap，将整个序列分给若干组，从后往前进行组内成员的比较和交换，随后逐步缩小增量至 1。希尔排序类似于插入排序，只是一开始向前移动的步数从 1 变成了 gap。

最好：`O(n * logn)`，步长不断二分。
最坏：`O(n * logn)`
平均：`O(n * logn)`

参考学习链接：
[图解排序算法(二)之希尔排序](https://www.cnblogs.com/chengxiao/p/6104371.html)

```js
function shellSort(nums) {
  let len = nums.length;
  // 初始步数
  let gap = parseInt(len / 2);
  // 逐渐缩小步数
  while (gap) {
    // 从第gap个元素开始遍历
    for (let i = gap; i < len; i++) {
      // 逐步其和前面其他的组成员进行比较和交换
      for (let j = i - gap; j >= 0; j -= gap) {
        if (nums[j] > nums[j + gap]) {
          [nums[j], nums[j + gap]] = [nums[j + gap], nums[j]];
        } else {
          break;
        }
      }
    }
    gap = parseInt(gap / 2);
  }
}
```

{% blockquote
___n
https://www.jianshu.com/p/a6ef6e0d1227
JS十大排序算法
%}{% endblockquote %}

{% blockquote
码农1213
https://www.cnblogs.com/bear-blogs/p/10808399.html
js 常用排序
%}{% endblockquote %}
