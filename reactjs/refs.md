# 引用组件：refs

当我们需要访问真正的浏览器本地DOM元素的时候，可以利用Ref属性给子组件命名，然后利用`this.refs`引用组件，再调用`getDOMNode()`方法获取DOM元素。

```js
  var App = React.createClass({
    getInitialState: function() {
      return {userInput: ''};
    },
    handleChange: function(e) {
      this.setState({userInput: e.target.value});
    },
    clearAndFocusInput: function() {
      this.setState({userInput: ''}, function() {
        this.refs.theInput.getDOMNode().focus();
      });
    },
    render: function() {
      return (
        <div>
          <div onClick={this.clearAndFocusInput}>
            Click to Focus and Reset
          </div>
          <input
            ref="theInput"
            value={this.state.userInput}
            onChange={this.handleChange}
          />
        </div>
      );
    }
  });
```

另外需要说明的是用户在表单填入的内容，属于用户跟组件的互动，所以不能用 `this.props`读取。本例中`<input>`文本输入框中的值，不能用`this.props.value`读取，而是要定义一个`onChange`事件的回调函数，通过`event.target.value`读取用户输入的值。`textarea` 元素、`select`元素、`radio`元素都属于这种情况。
