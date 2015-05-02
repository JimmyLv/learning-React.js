# 组件内部数据：state

在前面提到的`props`是不可变的，`props`来自于父组件。每一个组件可以根据接受的`props`数据渲染自己一次，而`this.state` 是组件私有的状态数据，可以通过调用`this.setState()` 来改变它，每当状态更新之后，组件都会重新渲染自己。

初始化状态，`getInitialState()`在组建的生命周期中仅执行一次：

```js
var LikeButton = React.createClass({
  getInitialState: function() {
    return {liked: false};
  },
  handleClick: function(event) {
    this.setState({liked: !this.state.liked});
  },
  render: function() {
    var text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <p onClick={this.handleClick}>
        You {text} this. Click to toggle.
      </p>
    );
  }
});
```
在React中通过`onClick={this.handleClick}`的方式给组件绑定事件处理器（各种事件中如`onClick`、`onSubmit`用于用户的点击和提交表单事件）。事件处理的具体内容放在`handleClick()`函数中，可以通过这种方式重新`setState()`一下状态数据。
