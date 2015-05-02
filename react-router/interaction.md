# 子组件的组合：interaction

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
