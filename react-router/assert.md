# Assert组件

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

其实我对这几个状态函数也不太清楚，这里有[组件的详细说明和生命周期（Component Specs and Lifecycle）](http://reactjs.cn/react/docs/component-specs.html)

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
