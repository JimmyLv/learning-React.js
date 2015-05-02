# 文件分离和离线转换JSX

第一个HTML：

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="build/react.js"></script>
    <script src="build/JSXTransformer.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/jsx">
      React.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
```

为了更好的管理代码和运行代码，可将JSX代码放到单独的`src/helloworld.js`文件，使用本地工具`react-tools`转成标准的JavaScript：`jsx --watch src/ build/`。然后再在HTML中的`<script>`标签中引入`src="build/helloworld.js"`即可。
