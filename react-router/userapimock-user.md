# userApi与mock-user数据

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

而在mock-user中设置了一些pattern去匹配React组件所想要访问的后台的url，匹配到了就返回相应的数据。在调试的时候只需要运行`gulp dev --dev`就可以使用mock-user中的数据了，注意`--dev`参数。
