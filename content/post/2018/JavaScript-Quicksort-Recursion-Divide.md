---
title: "JavaScript 实现快速排序 -- 挖坑法和递归法"
date: 2018-06-30T20:06:11+08:00
draft: false
slug: "JavaScript-Quicksort-Recursion-Divide"
---

## 递归法

```js
function quickSort(arr) {
  let length = arr.length;
  
  let pivotIndex = parseInt((length - 1) / 2);
  let pivot = arr.splice(pivotIndex, 1)[0];
  
  let leftArr = [];
  let rightArr = [];
  
  if (length <= 1) {
    return arr;
  }
  
  arr.forEach(v => {
    if (v < pivot) {
      leftArr.push(v);
    } else {
      rightArr.push(v);
    }
  });

  return quickSort(leftArr).concat([pivot], quickSort(rightArr));
}

let testArr = [3, 5, 1, 7, 9, 10, 30, 28, 19, 6];
console.log(quickSort(testArr))
```

关于 `let pivot = arr.splice(pivotIndex, 1)[0];` 是因为我们要用 `splice()` 把 `pivot` 从数组中剔除。