# React.js官方文档学习笔记

## 文件分离和离线转换

第一个HTML：

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="build/react.js"></script>
    <script src="build/JSXTransformer.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/jsx">
      React.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
```

为了更好的管理代码和运行代码，可将JSX代码放到单独的`src/helloworld.js`文件，使用本地工具`react-tools`转成标准的JavaScript：`jsx --watch src/ build/`。然后再在HTML中的`<script>`标签中引入`src="build/helloworld.js"`即可。

## 创建组件与渲染组件

第一个React组件：

```js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
React.render(
  <CommentBox />,
  document.getElementById('example')
);
```

使用`createClass`创建一个新component，其中必须要有一个`render()`函数去`return`相应的组件树（必须包含在一个`div`或者`span`里面）。

另外需要做的事情是用`React.render()`实例化根组件：`<CommentBox />`（注意`/`这个关闭符号，在其他的如`input`等标签中也需要关闭）,第二个是注入到原始DOM里面，也就是`document.getElementById()`（此ID为前面的第一个HTML中的`<div id="example"></div>`
）

## 组件组合与数据传递

每当创建新的子组件之后，都需要在根组件中的`render()`函数中`return`相应的组件树：

```html
<div className="commentBox">
    <h1>Comments</h1>
    <CommentList />
    <CommentForm />
</div>
```

与`React.render()`实例化跟组件一样，`<CommentList />`实例化每一个子组件，在JSX中只能使用`className`，因为`class`为保留字，其他的也类似。

```js
return (
      <div className="comment">
        <h2 className="commentAuthor">
            {this.props.author}
        </h2>
            {this.props.children}
      </div>
    );
```

在`return`的组件树中通过一个属性value的方式可以在组件之间传递数据。从父节点传递到子节点的数据称为 `props`，是属性（properties）的缩写，通过`this.props.key`就可以访问到相应的`value`，通过`children`可以访问任何内嵌元素。在 JSX 中将JavaScript表达式放在大括号中，可以作为属性或者子节点，即遇到`<`符号渲染HTML标签，遇到`{`识别为JS表达式。

## 使用第三方库

首先需要在`<head>`中加入`<script src="http://cdnjs.cloudflare.com/ajax/libs/showdown/0.3.1/showdown.min.js"></script>`，示例为添加**Showdown**处理 Markdown 文本并且转换为原始的 HTML。

```js
var converter = new Showdown.converter();
var Comment = React.createClass({
  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        {converter.makeHtml(this.props.children.toString())}
      </div>
    );
  }
});
```
只需要`new`出`converter`就可以调用它的`makeHtml`方法啦，非常的灵活。

## 数据模型

在前面我们已经提到了使用属性值的方式保存和传递数据，那如果数据来自HTML外部或者服务器呢？我们用一种模块化的方式将数据传递：

```js
var data = [
  {author: "Pete Hunt", text: "This is one comment"},
  {author: "Jordan Walke", text: "This is *another* comment"}
];
```

从的`render()`方法中传递给CommentList子组件的方法是：在实例化的时候加入key-value参数：

```js
<div className="commentBox">
    <h1>Comments</h1>
    <CommentList data={this.props.data} />
    <CommentForm />
</div>
```

在最终渲染的时候再加入data数值就可以将源代码中组件外的数据一直传递到最终的CommentList子组件了：

```js
React.render(
  <CommentBox data={data} />,
  document.getElementById('content')
);
```

在子组件中就可以渲染该数据：

```js
var commentNodes = this.props.data.map(
function (comment) {
    return (
        <Comment author={comment.author}>
          {comment.text}
        </Comment>
    );
});
```

如果数据来自于服务器就需要在渲染的时候使用获取数据的URL：

```js
React.render(
  <CommentBox url="comments.json" />,
  document.getElementById('content')
);
```

## 组件内部数据：状态

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
## 引用组件获取本地DOM元素

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

## 事件与表单

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
