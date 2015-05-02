# 数据模型：key-value

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
