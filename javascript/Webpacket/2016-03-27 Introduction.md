# 使用

I also recommend you add node_modules/.bin to your PATH variable to avoid having to type node_modules/.bin/webpack every time. 

## 创建项目并使用webpack

初始化项目

```bash
$ npm init
$ npm install jquery --save
$ npm install webpack --save-dev
```

webpack.config.js 配置

```javascript
module.exports = {
    entry:  './src',
    output: {
        path:     'builds',
        filename: 'bundle.js',
    },
};
```

页面引入

```html
<!DOCTYPE html>
<html>
<body>
    <h1>My title</h1>
    <a>Click me</a>

    <script src="builds/bundle.js"></script>
</body>
</html>
```

by default Webpack hides modules that are not yours. To see all the modules compiled by Webpack, we can pass the --display-modules flag:

```bash
$ webpack --display-modules
bundle.js  257 kB       0  [emitted]  main
   [0] ./src/index.js 53 bytes {0} [built]
   [1] ./~/jquery/dist/jquery.js 248 kB {0} [built]
```

## 设置你的第一个loader

我们的期望是这样的：当我们引入一个按钮组件的时候，我们希望能够自动引入相关的图片，样式资源。

### 安装loader

To install a loader in Webpack you do two things: npm install {whatever}-loader, and add it to the module.loaders part of your Webpack configuration

```bash
$ npm install babel-loader --save-dev
```

我们希望babel处理所有以js作为后缀的文件，但是我不希望babel去处理第三方的类库，如jQuery，所以我们可以设置一些过滤规则。（Loaders can have both an include or an exclude rule. It can be a string, a regex, a callback, whatever you want.）

```javascript
module.exports = {
    entry:  './src',
    output: {
        path:     'builds',
        filename: 'bundle.js',
    },
    module: {
        loaders: [
            {
                test:   /\.js/,
                loader: 'babel',
                include: __dirname + '/src',
            }
        ],
    }
};
```

接下来可以使用ES6的语法编写一个Button组件了。我们会使用到Mustache，我们也需要Sass and HTML files的loader：

```
$ npm install mustache --save
$ npm install css-loader style-loader html-loader sass-loader node-sass --save-dev
```

Now in order to tell Webpack to “pipe” things from one loader to another we simply pass a series of loaders, from right to left, separated by a !. Alternatively you can use an array by using the loaders attribute instead of loader:

```javascript
{
    test:    /\.js/,
    loader:  'babel',
    include: __dirname + '/src',
},
{
    test:   /\.scss/,
    loader: 'style!css!sass',
    // Or
    loaders: ['style', 'css', 'sass'],
},
{
    test:   /\.html/,
    loader: 'html',
}
```

You’ve now learned how to setup loaders and how to define the dependencies of each part of your app. This may look like it doesn’t matter much now, but let’s push our example further.

## Code splitting

```javascript
import $ from 'jquery';

// This is a split point
require.ensure([], () => {
  // All the code in here, and everything that is imported
  // will be in a separate file
  const library = require('some-big-library');
  $('foo').click(() => library.doSomething());
});
```

Everything in the require.ensure callback would be split into a chunk – a separate bundle that Webpack will load only when needed, through an AJAX request.

```javascript
if (document.querySelectorAll('a').length) {
    require.ensure([], () => {
        const Button = require('./Components/Button');
        const button = new Button('google.com');

        button.render('a');
    });
}
```

会生成 builds/1.bundle.js 文件

add configuration:

```javascript
path:       'builds',
filename:   'bundle.js',
publicPath: 'builds/',
```

The output.publicPath option tells Webpack where to find built assets from the point of view of the page (so in our case in /builds/). 

重命名bundle：

```javascript
require.ensure([], () => {
    const Button = require('./Components/Button');
    const button = new Button('google.com');

    button.render('a');
}, 'button');
```

添加第二个组件之后重复引入jQuery的问题。这个时候需要使用到组件：

Webpack comes will a handful of plugins to perform all various kinds of optimizations. The one that interests us in this case is the CommonChunksPlugin: it analyzes your chunks for recurring dependencies, and extracts them somewhere else. It can be a completely separate file (like vendor.js) or it can be your main file.

```javascript
var webpack = require('webpack');

module.exports = {
    entry:   './src',
    output:  {
      // ...
    },
    plugins: [
        new webpack.optimize.CommonsChunkPlugin({
            name:      'main', // Move dependencies to our main file
            children:  true, // Look for common dependencies in all children,
            minChunks: 2, // How many times a dependency must come up before being extracted
        }),
    ],
    module:  {
      // ...
    }
};
```

If we specified per example name: 'vendor':

```javascript
new webpack.optimize.CommonsChunkPlugin({
    name:      'vendor',
    children:  true,
    minChunks: 2,
}),
```

Since that chunk doesn’t exist yet, Webpack would have created a builds/vendor.js


## To production and beyond

Ok first of all, we’re going to add several plugins to our configuration, but we only want to load them when NODE_ENV equals production so let’s add some logic for that in our configuration. Since it’s just a JS file, that’s easy to do:

```javascript
if (production) {
    plugins = plugins.concat([
       // Production plugins go here
    ]);
}
```


# References

[webpack your bags](http://blog.madewithlove.be/post/webpack-your-bags/)




