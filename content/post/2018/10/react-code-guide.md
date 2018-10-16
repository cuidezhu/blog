---
title: "React 代码规范"
date: 2018-10-15T23:59:32+08:00
draft: false
slug: "react-code-guide"
---

根据编码实践总结出如下编写 React 代码规范：

1. 所有的语句不应加分号，React 使用 JSX 语法，因为不管加不加分号最终 Babel 转译成 ES5 时都会加上分号，而加分号还需分别什么时候加什么时候不加，所以现在 React 项目推崇不加分号；

2. 组件名、JS 文件名使用大驼峰命名法，以英文意思为依据，如  LoginControl；

3. JS 变量名命名采用小驼峰命名法，如 currentDay；

4. JS 函数命名采用小驼峰命名法，如 fetchData()；

5. CSS class 命名采用短横线命名法，如 commits-detail；

6. 所有的 CSS 均使用 SCSS 编写，如果某个块只需要一个 CSS 属性，也可以把 CSS 直接加到 JS 文件里，如  style={{ marginBottom: 15 }} ，注意在 JSX 里写 CSS 需要把属性名的的短横线变成小驼峰命名法；

7. 代码缩进均使用两个空格缩进，务必把编辑器的 Tab 键设置为两个空格；

8. state or 类属性？与渲染 UI 直接相关的使用 state，只是组件内的变量可用类属性如 this.age = 18；

9. 后端返回对象判空方法: Object.keys(this.state.result).length !== 0 然后使用三目运算符决定是否渲染 UI 或者显示 Loading 效果；

10. 判断 undefined 或者 null 或空字符串方法：!!siteData 原理是先转化成 Boolean 再取反；

11. 遍历数组必须为每一个值添加唯一的 key 值；

12. JSX/HTML 使用双引号，JS 使用单引号；

13. 使用 let 代替 var 来声明变量；

14. 每个二元运算符（+、 =、 -、*、/ 等）左右两侧要有一个空格，如 let age = 18，代码块起始的左花括号 { 前要有一个空格，如 `if (condition) {`；

15. 每个独立的代码段后使用一个换行；