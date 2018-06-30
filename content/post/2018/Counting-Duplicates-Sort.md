---
title: "统计字符串中的字符重复出现的次数并按次数排序"
date: 2018-06-29T20:29:58+08:00
draft: false
slug: "Counting Duplicates Sort"
---

我们有某个字符串 `text`  假设 `text` 是由字母和数字组成的，现在我们要字符串中的字符重复出现的字数，然后按字数进行排序，输出。

```js
function countDuplicates(text) {
  let textArr = text.split('');
  let countObj = {};
  let resultArr = [];
  
  textArr.forEach(v => {
    if (countObj[v]) {
      countObj[v] += 1;
    } else {
      countObj[v] = 1;
    }
  })
  
  
  for (let char in countObj) {
    resultArr.push([char, countObj[char]]);
  }
  
  resultArr.sort((a, b) => a[1] - b[1]);
  console.log(resultArr);
}

let text = 'abba222ccxxxd';
countDuplicates(text);
```

`output: [["d", 1], ["a", 2], ["b", 2], ["c", 2], ["2", 3], ["x", 3]]`