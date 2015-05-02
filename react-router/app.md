# 主组件和主页面App

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
