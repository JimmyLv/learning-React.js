# 路由规则：routes

```js
// src/app.react.js
const appRoutes = (
  <Route name='root' path="/" handler={App}>
    <DefaultRoute name='home' handler={Home}/>
    <Route name='user' handler={User}/>
    <Route name='assets' handler={Assets}/>
    <NotFoundRoute handler={NotFound}/>
  </Route>
)
```

这样就定义了包含四条路由规则的路由，分别指向不同的路径。（严格来说还有一个指向`/`的路由，访问`/`会默认会渲染Home组件，注意这个时候其他项都没有被激活。

### DefaultRoute

A DefaultRoute will be the matched child route when the parent's path matches exactly.

You'd want to use this to ensures a child RouteHandler is always rendered when there is no child match. Think of it like index.html in a directory of a static html server.

### NotFoundRoute

A NotFoundRoute is active when the beginning of its parent's path matches the URL but none of its siblings do. It can be found at any level of your hierarchy, allowing you to have a context-aware "not found" screens.

You'd want to use this to handle bad links and users typing invalid urls into the address bar.
