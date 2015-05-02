# 使用第三方库：library

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
