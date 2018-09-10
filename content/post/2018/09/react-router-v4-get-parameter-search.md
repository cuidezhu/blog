---
title: "React Router V4 在 URL 中 传递和获取 '?sort=name' 形式的值"
date: 2018-09-10T17:02:23+08:00
draft: false
slug: "react-router-v4-get-parameter-search"
---

有时我们需要在 `Link` 传递的 URL 中加入类似 `?sort=name` 的参数， 来组成这样的 URL: `a.com?sort=name`。

问号 `?` 后加参数的形式，这样不占用我们 URL 中的变量，而又有刷新后参数还在的优点。

由于 `this.props.location.query` 在 React Router v4 中不再存在，所以需要使用 `this.props.location.search`，然后使用 [query-string](https://github.com/sindresorhus/query-string) 来自己解析参数。

我们在父级页面中这样使用 `Link` 来传来参数,

```js
<Link to={{
  pathname: '/courses',
  search: '?sort=name'
}}/>
```

然后安装 `query-string`:

```zsh
yarn add query-string
```

在子页面中解析传来的参数：

```js
import queryString from 'query-string'

class New extends Component {
  constructor(props) {
    super(props);
    let urlParams = queryString.parse(this.props.location.search)
  }
  // ...
}
```

这样我们就拿到了 URL 中的参数对象。