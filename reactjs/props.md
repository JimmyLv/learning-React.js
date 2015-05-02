# 组件组合与数据传递：props

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
