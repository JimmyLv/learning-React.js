# 创建组件与渲染组件：component

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
