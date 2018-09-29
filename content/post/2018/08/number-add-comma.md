---
title: "数字每三个数添加一个逗号"
date: 2018-08-14T23:55:31+08:00
draft: false
slug: "number-add-comma"
---

```js
function addCommaNumber(num) {
  let newStr = "";
  let numStr = num.toString();
  let count = 1

  for (let i = numStr.length - 1; i >= 0; i--) {
    if (count % 3 === 0 && i !== 0) {
      newStr = ',' + numStr.charAt(i) + newStr;
    } else {
      newStr = numStr.charAt(i) + newStr;
    }

    count++;
  }

  return newStr;
}
```