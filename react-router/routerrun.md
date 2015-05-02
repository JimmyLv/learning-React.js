# Router.run

The main API into react router. It runs your routes, matching them against a location, and then calls back with the next state for you to render.

```js
// src/index.react.js
run(appRoutes, function (Handler, state) {
  React.render(<Handler />, document.body);
});
```

我们项目的`appRoutes`路由规则放在`app-routes.react.js`中，通过`import appRoutes from './app-routes.react'`引入，我们会在之后对路由规则进行介绍。

### Signature

`Router.run(routes, [location,] callback(Root, state))`

callback的参数：

- `Root`

A ReactComponent class with the current match all wrapped up inside it, ready for you to render.

- `state`

An object containing the matched state.

### Basic Usage

```js
Router.run(routes, function (Root) {
  // whenever the url changes, this callback is called again
  React.render(<Root/>, document.body);
});
```

所以我们的url一有变化，Handler组件就会被回调函数重新渲染。
