---
title: "Ant Design 同一个页面内多个表单校验冲突解决"
date: 2019-02-15T19:50:32+08:00
draft: false
slug: "ant-design-two-form"
---

遇到这样一种场景，某个页面比较复杂，页面本身是一个表单，然后 Modal 里是另外一个表单，如果我们写在同一个组件里。在表单校验时就会发生两个表单会同时校验的问题。

这时我们需要把一个表单抽离出来成一个独立的组件。然后主页面和弹窗之间的数据通信可以通过 `props` 来解决。

例如：

```js
class Modal extends Component {

  handleModalSubmit = e => {
    e.preventDefault()
    this.props.form.validateFields((err, values) => {
      values.id = this.props.id
      if (!err) {
        request(``).then(res => {

        })
      }
    })
  }
  render() {
    return (
      <Form onSubmit={this.handleModalSubmit}>
      </Form>
  }
}

const WrappedModal = Form.create()(Modal)

class MainPage extends Component {
  render() {
    return (
      <Form onSubmit={this.handleSubmit}>
        <Modal
          title='弹窗表单'
          visible={this.state.visible}
          onCancel={this.handleCancel}>
          <WrappedModal id={this.state.id} />
        </Modal>
      </Form>
    )
  }
}
```
