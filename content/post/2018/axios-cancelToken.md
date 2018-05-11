---
title: "Axios CancelToken 取消请求"
date: 2018-05-11T13:26:11+08:00
draft: false
slug: "axios-cancelToken"
---

## 背景

项目所用的前端框架为React，不过下面所讲的场景和React没啥关系，其他前端架构也会有类似问题。

假如说页面中有两项菜单可供选择，这两项菜单下的内容是同一个页面，同一个组件，两项菜单下的页面结构是一样的，只是数据不同，数据比如都是使用 axios 请求 REST API 获取到的，然后用 `setState` 把 API 传过来的值展示到页面中。

```
axios.get('/test', {
    params: {
      type: 'fruit'
    }
  })
  .then(function (res) {
    if (res.status === 200) {
      this.setState({
        data: res.data
      }
    }
  })
```

项目初次加载和点击菜单选项时都会执行类似于上面的请求 API 的方法。

正常点击切换两项菜单时问题不大，能正确展示数据，当我点击很快时，由于向 API 请求数据需要时间，就可能把某次请求pending，然后 `setState` 就可能会把比如第一个菜单项下的内容展示到第二个菜单下。

## 解决方案

按钮点击很快这件事本身可以做处理，但我感觉限流也是权宜之计，比如极端情况，某段时间 API 返回数据挺慢，在限流的时间内还没返回数据，这样 `setState` 还是有可能把数据展示错乱。关键在于请求 pending 时就切换菜单会出现数据错乱。

如果我们在发新的请求之前能把之前 pending 的请求取消掉就会解决这个问题，这就是 axios 的 CancelToken 所做的事情。

我们在点击菜单项绑定的函数中每次定义下面两个变量

```
const CancelToken = axios.CancelToken
const source = CancelToken.source()
```

注意上面两个变量初始化时每次切换菜单项时都要重新定义，不然只定义一次的话，上面两个变量的值只是初始化时的值。我们可以把上面两个变量写到我们发送请求的前面，然后把发送请求包装成一个函数，在 `componentDidMount()` 和 切换菜单项时调用这个函数。然后完整的代码如下。

```
handleRequest() {
  const CancelToken = axios.CancelToken
  const source = CancelToken.source()

  axios.get('/test', {
      params: {
        type: 'fruit'
      },
      cancelToken: source.token
    })
    .then(function (res) {
      if (res.status === 200) {
        this.setState({
          data: res.data
        }
      }
    })
    .catch(function(thrown) {
      if (axios.isCancel(thrown)) {
        console.log('Request canceled', thrown.message);
      } else {
        // handle error
      }
    })    
}

```

然后我们在每次点击时都要把之前 pending 的请求取消掉，在点击菜单项绑定的函数里加入下面这条语句：
```
// cancel the request (the message parameter is optional)
source.cancel('Operation canceled by the user.');
```

至此，我们就解决了点击切换菜单项太快数据展示错乱的问题。



