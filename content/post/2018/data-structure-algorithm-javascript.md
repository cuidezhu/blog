---
title: "常见的数据结构和算法（JavaScript实现）"
date: 2018-06-21T20:47:22+08:00
draft: false
slug: "Data-Structure-Algorithm-JavaScript"
---

## 二分查找

二分查找是针对的是已经有序的序列，序列是列表和数组的统称， 在 JS 里通常是数组。我们下面来实现下在 JS 数组里二分查找某个元素：

```js
function binarySearch(orderArr, element) {
  let start = 0;
  let end = orderArr.length - 1;
  
  while (start <= end) {
    let mid = Math.floor((start + end) / 2);

    if (element < orderArr[mid]) {
      end = mid;
    } else if (element > orderArr[mid]) {
      start = mid + 1;
    } else {
      console.log('element index is ' + mid)
      return mid;
    }
  }
  
  console.log('not found')
  return -1;
}

let a = [0,1,2,3,4,5,6,7,8]

binarySearch(a, 2)    //output "element index is 2"
```

这里我们要注意对边界值的判断，因为取中间值 mid 的时候是使用 `floor()` 向下取整，所以当查找的值大于中间值时我们要对 `start` 这个变量加 1，因为这时要查找的值肯定不等于中间值，没必要再判断这时的中间值，并且如果不加 1 还会导致遗漏判断尾边界值。 