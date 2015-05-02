# 事件与表单：event

当用户提交表单的时候，我们应该获取用户的名字和评论内容然后清空表单，并且提交一个请求到服务器：

```js
var CommentForm = React.createClass({
  handleSubmit: function(e) {
    e.preventDefault();
    var author = this.refs.author.getDOMNode().value.trim();
    var text = this.refs.text.getDOMNode().value.trim();
    if (!text || !author) {
      return;
    }
    // TODO: send request to the server
    this.refs.author.getDOMNode().value = '';
    this.refs.text.getDOMNode().value = '';
    return;
  },
  render: function() {
    return (
      <form className="commentForm" onSubmit={this.handleSubmit}>
        <input type="text" placeholder="Your name" ref="author" />
        <input type="text" placeholder="Say something..." ref="text" />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

我们给表单绑定一个`onSubmit`处理器，用于当表单提交了合法的输入后清空表单字段：使用`this.refs`加上名称就可以找到相应组件，然后得到合法输入就发送给server，最后使`value = ''`清空表单。而且在事件回调中调用`preventDefault()`来避免浏览器默认地提交表单。

然后让我在提交给服务器之前就刷新我们的评论列表，从而使应用感觉更快：

```js
handleCommentSubmit: function(comment) {
    var comments = this.state.data;
    var newComments = comments.concat([comment]);
    this.setState({data: newComments});
    // ...
}
```
