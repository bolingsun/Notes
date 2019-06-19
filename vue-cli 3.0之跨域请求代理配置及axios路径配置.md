# vue-cli 3.0之跨域请求代理配置及axios路径配置
个人笔记

配置：
一：在web配置文件 config\index.js文件中设置：
````
proxyTable: {
      '/api': {
        // target: 'https://www.easy-mock.com/mock/5d01cfb04b4e406118bae1f6/example/', //我实际开发的时候后台接口示例
        target: 'http://127.0.0.1:8100/', //对应自己的接口
        changeOrigin: true, // 是否开启跨域
        ws: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    },
````
配置后需要重启服务。

二、配置axios的baseUrl。
main.js：
````
axios.defaults.timeout = 5000 // 请求超时
axios.defaults.baseURL = '/api/'  // api 即上面 vue.config.js 中配置的地址
````
发送请求的例子：
````
axios.post('/postData/', {
    name: 'cedric',
}).then((res) => {
  console.log(res.data)
})
````
此时，虽然请求发送到了：http://localhost:8080/api/postData/, 但是已经代理到了地址：http：//127.0.0.1.8100/postData/. 控制台显示请求的地址为：http://localhost:8080/api/postData/。
我的例子是：
我控制台显示发送的请求是
http://localhost:9000/api/user/logout
实际上请求的地址是：
https://www.easy-mock.com/mock/5d01cfb04b4e406118bae1f6/example/user/logout

生产环境：
只需要将 main.js 中 axios 作如下修改：
````
axios.defaults.timeout = 5000 // 请求超时
axios.defaults.baseURL = 'http://api.demourl.com/'
````
实际请求地址为：http://api.demourl.com/postData/


转载自：
https://www.cnblogs.com/s313139232/p/10598071.html
https://www.cnblogs.com/cckui/p/10331432.html
