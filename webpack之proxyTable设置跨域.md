# webpack之proxyTable设置跨域
个人笔记
为什么要使用proxyTable

很简单，两个字，跨域。
在平时项目的开发环境中，经常会遇到跨域的问题，尤其是使用vue-cli这种脚手架工具开发时，由于项目本身启动本地服务是需要占用一个端口的，所以必然会产生
跨域的问题。当然跨域有多种解决方式，这里就不一一例举，下次弄篇文章单独讲，在使用webpack做构建工具的项目中使用proxyTable代理实现跨域是一种比较方
便的选择。

如何使用proxyTable

还是拿之前使用过的vue-cli举例。我们首先要在项目目录中找到根目录下config文件夹下的index.js文件。由于我们是在开发环境下使用，自然而然是要配置在dev里面：
````javaScript
dev: {
  env: require('./dev.env'),
  port: 8080,
  autoOpenBrowser: true,
  assetsSubDirectory: 'static',
  assetsPublicPath: '/',
  proxyTable: {
    '/api': {
      target: 'http://www.abc.com',  //目标接口域名
      changeOrigin: true,  //是否跨域
      pathRewrite: {
        '^/api': '/api'   //重写接口
      }
    },
  cssSourceMap: false
}
````
上面这段代码的效果就是将本地8080端口的一个请求代理到了http://www.abc.com这一域名下：

````
'http://localhost:8080/api' ===> 'http://www.abc.com/api'
````
没有统一项目名下的使用

上面那种情况是有一个统一的项目名api的，所以说是比较好匹配的，就相当于说直接将以api开头的接口名代理一下换成目标域名访问就好了，可是如果说后台返
给我们前端的接口没有了统一的项目名呢？之前，我是一个个单独去做了转换，接口少还没什么关系，但多了肯定是不现实的，其实还有这样一种取巧的方案：
````javaScript
//先人为给接口地址前面加上一个自定义的项目名
let someApi = 'api' + '/xx/xx';

dev: {
  env: require('./dev.env'),
  port: 8080,
  autoOpenBrowser: true,
  assetsSubDirectory: 'static',
  assetsPublicPath: '/',
  proxyTable: {
    '/api': {
      target: 'http://www.abc.com',  //目标接口域名
      changeOrigin: true,  //是否跨域
      pathRewrite: {
        '^/api': '/'   //重写接口
      }
    },
  cssSourceMap: false
}
````
这里的项目名api是我们人为加上去的，经过代理之后就没了，这样我们在配置代理这里还是只需要配置一份就够了，
只是在写接口地址时要注意区分开发环境和线上环境就可以了。

公司配置的参考一下：
````javaScript
const httpUrl = 'http://192.168.97.87:8080';// 郭浩森
const projectConfig = {
  proxyTable: {
    '/api/bfs': {
      target: httpUrl,
      changeOrigin: true
    },
    '/api/lims': {
      target: httpUrl,
      changeOrigin: true
    }
  },
  /**
     * 需要编译的项目列表,只有配置了的才会发布编译
     */
  project: {
    bfs: './node_modules/zeei-bfs',
    lims: './src/view/lims',
    system: './node_modules/zeei-system' // 包含登录及桌面页
  },
  devPort: 8088, // 开发环境运行端口
  isMock: false, // 是否模拟数据
  baseURL: '', // ajax请求的基础路径

  /** npm run build发布目录配置 */

  assetsSubDirectory: 'web/', // 发布后路径 dist/version/web/
  assetsPublicPath: '/', // 这个是通过http服务器运行的url路径。在大多数情况下，这个是根目录( / )
  replaceImage: {// 图片替换

  }
};
module.exports = projectConfig;
````
关于proxyTable的原理
我在网上查了一下，这个代理实际上是利用http-proxy-middleware这个插件完成的，具体到这个插件的运行机制，由于是英文再加上能力有限就没深究了。但我想探究的是这种代理方式实际上是如何做到的，我看网上有人说实际上就是我们的本地服务器将请求转发给了目标服务器。之所以出现跨域是因为浏览器有同源策略的限制，但服务器是没有的，所以这种代理方式能够实现的机制大体就是：
本地服务器 --》 代理 --》目标服务器 --》拿到数据后通过代理伪装成本地服务请求的返回值 ---》然后浏览器就顺利收到了我们想要的数据


