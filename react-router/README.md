# React-Router学习笔记

## `Router.run`

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

## `routes`

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

## 主组件和主页面

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

通过`import App from './app.react'`引入，并且作为主组件被渲染，当然由于DefaultRoute设置了`handler={Home}`，所以还会默认渲染Home组件。当我们访问`/`根路径的时候，React-Router会帮我们渲染那些周边的东西，即主组件App，包括AppLeftNav和ContentWithNav（之后肯定还会加入Header和Footer组件）。然后还会默认渲染一下Home组件，所以显示给我们的就是Home主页面啦。

### BasePage组件

这就是我们之前导入到`src/app.react.js`文件中的`App`组件：

```js
// src/app.react.js
var BasePage = React.createClass({
  mixins: [Navigation, State],
  getDefaultProps() {
    return {
      title: 'Title'
    }
  },
  render() {
    return (
      <AppCanvas predefinedLayout={1}>
        <AppBar
          className="mui-dark-theme"
          onMenuIconButtonTouchTap={this._toggleLeftNav}
          showMenuIconButton={true}
          title={this.props.title}
          zDepth={0}>
        </AppBar>
        <AppLeftNav ref="leftNav" />
        <ContentWithNav>
          <RouteHandler />
        </ContentWithNav>
      </AppCanvas>
    );
  },
  _toggleLeftNav() {
    this.refs.leftNav.toggle()
  }
});
```

从该文件的前面可以看到我们从`material-ui`引入了一些HTML组件，，所以可以直接调用从而轻易达到一些很酷炫的效果，我们可以暂时不用管这个标签。

```js
import {
  AppBar,
  AppCanvas,
  LeftNav,
  IconButton,
  Icons
} from 'material-ui'
```

可以注意到的是`ContentWithNav`和`BasePage`都使用了从`'react-router'`中导入的`<RouteHandler />`，但是我还不知道它是做什么用的?

然后我们可以看到的是使用`getDefaultProps()`设置了默认的title属性值，然后通过`title={this.props.title}`将AppBar显示为Title。并且定义了`onMenuIconButtonTouchTap`这个点击动作，效果就是将整个LeftNav切换显示，而且是通过`refs`的方式得到相应的LeftNav组件。

## 子组件的组合与交互

### LeftNav组件

```js
// src/LeftNav/index.react.js
  render() {
    const menuTitle = <div className="menu__title" onClick={this._onHeaderClick}>Menu Title</div>;

    return (
        <LeftNav
          ref="leftNav"
          docked={false}
          isInitiallyOpen={true}
          header={menuTitle}
          menuItems={menuItems}
          selectedIndex={this._getSelectedIndex()}
          onChange={this._onLeftNavChange}/>
    );
  },
```

重点关注一些render()函数返回的LeftNav组件，其中定义了左导航栏中的menuTitle和menuItems即menu中所要的显示的选项。对于`selectedIndex`我们定义了一个`_getSelectedIndex()`方法：选中某个菜单就会激活相应的route，从而由React-Router去选择渲染相应的组件。

```js
// src/LeftNav/index.react.js
  _getSelectedIndex: function() {
    if (!this.context.router) {
      return 0
    }
    return menuItems.findIndex(menuItem => {
      menuItem.route && this.context.router.isActive(menuItem.route)
    })
  },
```

### Home组件

```js
// src/pages/Home.react.js
  render() {
    return (
      <Paper zDepth={1}>
        <h2>{this.state.title}</h2>
        <RaisedButton secondary={true} onClick={this._login}>
          <FontIcon className="muidocs-icon-custom-github example-button-icon"/>
          <span className="mui-raised-button-label example-icon-button-label">Login</span>
        </RaisedButton>
      </Paper>
    );
  },

  _login() {
    userApi.login()
      .then(this.onLogin, this.onLoginFail)
  },
```

在Home页面中默认显示的就是`Hello World!`，这是由`getInitialState`初始化了该组件的初始状态数据，而在`RaisedButton`中定义了一个`onClick`事件，该事件回调函数`_login()`定义了`then()`方法返回的一个Promise对象。它有两个参数，分别为Promise在`success`和`failure` 情况下的回调函数。从而通过`setState()`方法改变该组件的状态数据，所以我们看到的变化就是`<h2>`标签的title不同了，正确显示的内容就是`userApi`使用`login()`与服务器交互之后获得了不同的msg或者err message。

## userApi与mock-user数据

```js
// src/services/user.js
const userApis = {
  login: {
    method: 'post',
    url: '/user/login'
  },
  logout: {
    method: 'post',
    url: '/user/logout'
  },
  assets: {
    method: 'post',
    url: '/user/assets'
  }
}
```

这里应该就是定义不同的api去与后台服务器交互，设置了不同request method和url。

```js
// src/services/mock-user.js
pattern: 'http://localhost:8080/user/(assets)',
    // callback that returns the data
    fixtures: function () {
      return {
        data: [
          {
            name: 'Mac Book',
            number: '1',
            date: '2015-4-25',
            type: 'laptop'
          },{
            name: 'Mac Book',
            number: '1',
            date: '2015-4-25',
            type: 'laptop'
          },{
            name: 'Mac Book',
            number: '1',
            date: '2015-4-25',
            type: 'laptop'
          }
        ]
      }
    },
```

而在mock-user中设置了一些pattern去匹配React组件所想要访问的后台的url，匹配到了就返回相应的数据。

## Assert组件

```js
// src/pages/Assets.react.js
  getInitialState() {
    return {
      title: 'User Assets',
      assets: []
    }
  },
  componentDidMount() {
    this._getAssets()
  },
  render() {
    return (
      <Paper zDepth={1}>
        <ul>
          {this._renderAssets()}
        </ul>
        <RaisedButton secondary={true} onClick={this._getAssets}>
          <FontIcon className="muidocs-icon-custom-github example-button-icon"/>
          <span className="mui-raised-button-label example-icon-button-label">Get Assets</span>
        </RaisedButton>
      </Paper>
    );
  },
```

首先我们使用`getInitialState()`定义了我们的组件初始状态数据，即我们的user asserts最开始为空，然后`componentDidMount()`的作用是在初始化渲染执行之后立刻调用一次，在生命周期中的这个时间点，组件拥有一个 DOM 展现，你可以通过`this.getDOMNode()`来获取相应 DOM 节点。

然后我们有一个`RaisedButton`可以引发`onClick`事件去得到Assets：

```js
_getAssets(userData) {
    userApi.assets(userData)
      .then(this.onAssetsLoad, this.onAssetsLoadFailed)
  },
  onAssetsLoad(assets) {
    this.setState({
      assets: assets.data
    })
  },
  onAssetsLoadFailed(err) {
    // handle err
  },
```

当然组件每一次更新都会在`<ul>`中使用`map()`方法渲染我们的每一个asset：

```js
  _renderAssets() {
    return this.state.assets.map(function(asset) {
      return (
        <li className="asset__item"><ul>
          <li className="asset__attribute">name: {asset.name}</li>
          <li className="asset__attribute">date: {asset.date}</li>
          <li className="asset__attribute">number: {asset.number}</li>
          <li className="asset__attribute">type: {asset.type}</li>
        </ul></li>
      )
    })
  }
```

至此，我们的项目结构已经基本了解，Let's Move On!
