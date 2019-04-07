# VUE学习笔记

### Vue指令之v-text和v-html

- 默认v-text没有闪烁问题。而v-cloak有。
- v-text会覆盖元素中原本的内容，但是插值表达式只会替换自己的这个站位符

### v-bind是vue中用于绑定属性的指令

````javascript
v-bind:可以被简写为 ：要绑定的属性
````

### v-on用来绑定事件

需要在vue实例中的methods里去定义v-on后面的方法,缩写为@

````javascript
<div id="app">
	<p>{{ msg }}</p>
	<input type="button" value="按钮" v-on:click="show">
</div>
<script>
            var vm = new Vue({
                el: '#app',
                data: {
                    msg: '欢迎学习VUE'
                },
                methods:{
                    show:function(){
                        alert('hello vue')
                    }
                }
            })
</script>
````

### 事件修饰符

- .stop阻止冒泡

- .prevent阻止默认事件,阻止默认行为

- .capture添加事件侦听器时使用事件捕获模式

- .self只有当事件在该元素本身（比如不是子元素）上发生时才触发

- .once事件只触发一次

  ````javascript
  <div @click.stop="divHandler"
  </div>
  ````

  注：.stop阻止了所有的冒泡行为，而.self只阻止自己有关的

###  v-model实现双向数据绑定

- v-bind只能实现数据的单向绑定，从M自动绑定到V，无法实现数据的双向绑定

````javascript
<div id="app">
	<p>{{ msg }}</p>
	<input type="text" style="width: 100%;" v-model="msg">
</div>
	<script>
        var vm = new Vue({
            el: '#app',
            data: {
                msg: '欢迎学习VUE'
            },
            methods:{
            }
        })
	</script>
````

- 使用v-model指令，可以实现表单元素和Model中数据的双向数据绑定；
- input（radio，text,address,email......) select checkbox textarea等表单元素才可以实现双向数据绑定

### 在Vue中使用样式

#### 使用class样式

1. 数组

   ```javascript
   <h1 :class="['red', 'thin']"这是一个H1</h1>
   ```

2. 数组中使用三元表达式

   ```javascript
   <h1 :class="['red', 'thin', flag?'active':'']"这是一个H1</h1>
   ```

3. 数组中嵌套对象

   ```javascript
   <h1 :class="['red', 'thin',{'active': flag}]"这是一个H1</h1>
   ```

4. 直接使用对象

   ```javascript
   <h1 :class="{red:true, italic:true, active:true, thin:true}"这是一个H1</h1>
   ```

#### 使用内联样式

1. 直接在元素上通过:style的形式，书写样式对象

   ````shell
   <h1 :style="{color: 'red','font-size':'40px'}">这是文字</h1>
   ````

2. 将样式对象定义到data中，并直接引用到:style中

   - 在data上定义样式

   ````shell
   data:{
       h1StyleObj:{color:'red','font-size':'40px,''font-weight':'200'}
   }
   ````

   - 在元素上，通过属性绑定的形式，将样式引用到元素上

     ````shell
     <h1 :style="h1StyleObj">这是文字</h1>
     ````

3. 在：style中通过数组，引用多个data上的样式对象

   - 在data上定义样式

     ````shell
     data:{
         h1StyelObj:{color:'red','font-size':'40px,''font-weight':'200'},h1StyleObj2{fontSytle:'italic'}
     }
     ````

   - 在元素中通过数据绑定的形式，将样式用到元素上

     ````shell
     <h1 :style=[h1StyleObj,h1StyleObj2]>这是文字</h1>
     ````

### Vue指令之v-for和key属性

- 迭代数组

  ````javascript
  <ul>
  	<li v-for="(item,i) li list">索引：{{i}}---姓名：{{item.name}}---年龄：{{item.age}}</li>
  </ul>
  ````

- 迭代对象中的属性

  ````javascript
  <div v-for="(val,key,i) in userInfo">{{val}} ---{{key}}---{{i}}</div>
  ````

  遍历对象的时候，除了有val  key 外，还可以有索引值 i

- 迭代数字

  ````javascript
  <p v-for="i in 10">这是第{{i}}个p标签</p>
  ````

  in后面我们可以放普通数组，对象数组，对象，还可以放数字; v-for迭代数组的话，i从1开始

在2.2.0+版本里，当组件中使用v-for时候，key现在是必须的。

v-for循环的时候，key属性只能使用number或string

key在使用的时候，必须使用v-bind属性绑定的形式来指定key的值

在组件中，使用v-for循环的时候，或在一些特殊情况中，如果v-for有问题，必须在使用v-for的同时，指定唯一的数字/字符串类型的:key值

````javascript
<p v-for="item in list" :key="item.id">
    <input type="checkbox">{{item.id}} --- {{item.name}}
</p>
````

### Vue指令之v-if和v-show

v-if的特点是每次都会<u>**重新删除或是创建**</u>元素

v-show的特点：每次不会重新进行dom的删除和创建，只是<u>**切换了元素的display属性**</u>。

一般来说，v-if有更高的切换消耗而v-show有更高的初始渲染消耗，因此，如果需要频繁切换v-show比较好，如果在运行时条件不太可能改变的话，则v-if较好。

如果元素涉及到频繁的切换，最好不要使用v-if，而是推荐使用v-show;

如果元素可能永远也不会被显示出来被用户看到，则推荐使用v-if。

### 过滤器

过滤器可以用在两个地方：mustachc插值和v-bind表达式。

过滤器调用时候的格式: {{ data | 过滤器的名称 }}

定义过滤器的语法：Vue.filter('过滤器的名称'，function(data){})。

过滤器中的function，第一个参数永远都是过滤器管道符前面传递过来的数据。

````javascript
<div id='app'> <p> {{msg | msgFormat}} </p> </div>
    
<script>
    Vue.filter('msgFormat', function(msg){
    	return msg.replace(/单纯/g,'邪恶')
	})
    var vm = new Vue({
        el:'#app',
        data:{
            msg： '曾经，我也是一个单纯的少年，单纯的我，傻傻的问，谁是最单纯的男人'
        },
        methods:{}
    })
</script>
````

定义一个私有过滤器:

````javascript
var vm2 = new Vue({
    el:'#app2',
    data: {
    },
    methods: {},
    filters: {
        过滤器名称： function(){}
    }
})
````

### 键盘修饰符和自定义键盘修饰符

全部的按键别名：

.enter .tab .delete .esc .space .up .down .left .right

使用默认的键盘修饰符

````javascript
<input type='text' v-model='name' @keyup.enter='add'>
````

自定义全局按键修饰符:

Vue.config.keyCodes.别名=键盘码(按键值).

例如Vue.config.keyCodes.f2=113;

如何使用自定义的按键修饰符:

````javascript
<input type='text' v-model='name' @keyup.f2='add'>
````

### 自定义指令

1：使用Vue.directive(指令名称，对象，)自定义全局指令。

注：参数1指令名称不需要加v-前缀，在调用的时候系统自动加上；参数2对象上有一些钩子函数

#### 常用的钩子函数:

**bind, inserted, update,** componetUpdated, unbind

````javascript
Vue.directive('focus', {
    bind:function(el,binding){//每当指令绑定到元素上的时候，执行一次
    }，
    inseted:function(){//当元素插入到DOM中的时候执行，值触发一次
	}，
    updated:funtion(){//当Vnode更新的时候，执行updated，可能会触发多次
    }
})
````

钩子函数的参数解释:

el:表示被绑定了指令的按个元素，是一个原生的js对象;

bingding:一个对象，常用bingdingname bingding.value bingding.expression

2：自定义私有指令

````javascript
var vm2 = new Vue({
    el:'#app2',
    data: {
    },
    methods: {},
    filters: {},
    directives:{//自定义私有指令
    'fontweight':{
        bind:funtion(el,bingding){
            el.styele.fontWegiht = binding.value
            }
        }
	}
})
````

#### 函数简写

大多数情况下，我们可能只需要在bind和unpdate钩子函数做重复动作，不关心其他的钩子函数，可以这样简写：

````javascript
Vue.directive('color-swatch',function(el,bingding){
    el.style.backgroundColor = bingding.value
})
//在bind和unpdate上都写了一份这个代码
````

### 生命周期函数

#### 创建阶段的生命周期函数:beforeCreate,created,beforeMount,mounted

````javascript
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {},
    beforeCreate: {
        //这是第一个生命周期函数，表示实例完全被创建出来之前，会执行它
    },
    created:{
        //这是第二个生命周期函数，
    },
    beforeMount:{
        //这是第三个生命周期函数，表示模板已经在内存中编辑完成，但是还没有挂载到页面中
    },
    mounted:{
        //这是第四个生命周期函数，表示内存中的模板已经真实挂载到页面中，用户可以看到渲染好的页面了
    }，
    //接下来运行中的两个事件
    beforeUpdate:{
    	//数据被更新了，但是我们的界面还没有被更新
	},
    updated:{
		//页面和data数据已经保持同步了
    }
   
})
````

在beforeCreate生命周期函数执行的时候，data和methods中的数据都还没有初始化;

在created中，data和methods都已经被初始化好了。如果要调用methods中的方法或是操作data中的数据，最早也只能在crated这个阶段操作；

在beforeMount执行的时候，页面中的元素还没有被真正替换过来，知识之前写的一些模板字符串；

mounted是实例创建期间的最后一个生命周期函数，当执行mounted就表示实列被完全创建好了。

注意：这四个中比较重要的是**created**和**mounted**。如果要通过某些插件操作页面中的DOM节点，最早需要在mounted中进行。只要执行完mounted，就表示整个Vue示例已经完成初始化，组件脱离了创建阶段，进入到运行阶段。

#### 运行阶段的生命周期函数:beforeUpdate,updated

beforeUnpdate页面中显示的数据还是旧的，但是data数据是最新的，页面尚未和最新的数据保持同步;

unpdated执行的时候，页面和data数据保持同步了。

#### 销毁阶段的生命周期函数:beforeDestroy,destroyed

当执行beforeDestroy函数的时候，示例从运行阶段进入到销毁阶段，当执行beforeDestroy的时候，实例上所有的data和methods以及过滤器指令等都处于可用状态，这个时候还没有真正执行销毁过程；

当执行destroyed的时候，组件已经完成销毁，这个时候ata和methods以及过滤器指令都被销毁了。

### vue-resource实现get,post,jsonp请求

除了vue-resource外，还可以使用axios的第三方包实现数据的请求

测试URL请求资源地址：

- get请求地：http://vue.studyit.io/api/getlunbo
- post请求地址：http://vue.studyit.io/api/post
- jsonp请求地址：http://vue.studyit.io/api/jsonp

注意vue-resouse依赖vue，引入的时候，放到vue.js后面。

````javascript
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {
        getInfo(){//发起get请求
            this.$http.get('URL地址').then(funtion(result){
                  //通过result.body拿到服务器返回的成功的数据
                 console.log(result.body)
             })
        },
        postInfo(){//发起post请求,
            //手动发起的POST请求，默认没有表单格式，服务器处理不来。
            //通过post方法的第三个参数，来设置提交的内容类型为普通表单数据格式
            this.$http.post('url地址'，{},{emulateJSON: true}).then(result=>{
    			console.log(result.body)
			})
        }，
        jonpInfo(){
            //发起jsop请求
            this.$http.jsonp('url地址').then(result => {
                console.log(result.body)
            })
        }
    }
})
````

### Vue中的动画

第一种方式：自己来定义

1.使用transition元素把需要被动画控制的元素包裹起来，这个transition元素是Vue官方提供的。

<u>注意：如果需要过度的元素是通过v-for循环渲染出来的，就不能使用transition包裹，需要使用transitionGroup来包裹元素。如果要为v-for循环创建的元素设置动画，必须为每一个元素设置:key属性</u>

2，自定义两组样式来控制transition内部的元素实现动画

````javascript
<head>	
	<style>
		/*v-enter 【这是一个时间节点】进入之前，元素的初始状态*/
		/*v-leave-to 【这是一个时间节点】动画离开之后，离开的终止状态，此时动画已结束*/
		.v-enter, .v-leave-to {
			opacity: 0;
			transform: translateX(500px);
		}
		/*v-enter-active入场的时间段。.v-leave-active离场动画的时间段*/
		.v-enter-active, .v-leave-active {
			transition: all 3s ease;
		}
	</style>
</head>
<body>
	<div id="app">
		<input type="button" value="toggle" @click="flag=!flag">
		<transition>
			<h3 v-if='flag'>这是一个H3</h3>
		</transition>
	</div>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				flag: false
			},
			methods:{
			}
		})
	</script>
</body>
````

第二种方式：使用第三方类库实现动画效果：如animate.css

````javascript
<head>	
    //需要link进入animate.css文件
	<style>
	</style>
</head>
<body>
	<div id="app">
		<input type="button" value="toggle" @click="flag=!flag">
		<transition enter-active-class='bounceIn' leave-active-class='bounceOut'>
			<h3 v-if='flag' class='animated'>这是一个H3</h3>
		</transition>
	</div>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				flag: false
			},
			methods:{
			}
		})
	</script>
</body>
````

第三种方式：使用js钩子函数来实现半场动画

````javascript
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
	...
</transition>
<script>
		var vm = new Vue({
			el: '#app',
			data: {
				flag: false
			},
			methods:{
                beforeEnter(el){},
                enter(el,done){//这里的done起始就是afterEnter这个函数，done是这个函数的引用},
                afterEnter(el){}
			}
		})
	</script>
````

注意:动画钩子函数的第一个参数el表示要执行的按个dom元素，是原生的js dom对象;第二个参数是一个回调函数。当只用 JavaScript 过渡的时候，**在 enter 和 leave 中必须使用 done 进行回调**。否则，它们将被同步调用，过渡会立即完成。

beforeEnter()表示动画入场之前，动画未开始，可以在这里设置元素开始动画前的起始样式;

enter()表示动画开始之后的样式，这里可以设置元素完成动画后的结束状态;

afterEnter()动画完成后，会调用这个钩子函数，动画结束后的状态。



关于列表动画的问题

1.如果需要过度的元素是通过v-for循环渲染出来的，就不能使用transition包裹，需要使用transitionGroup来包裹元素。

2.如果要为v-for循环创建的元素设置动画，必须为每一个元素设置:key属性

3.给transitionGroup添加appear属性，实现页面刚展示出来时候，入场时候的效果

4.通过为transitionGroup设置tag属性，指定transitionGroup渲染为指定的元素，如果不指定tag属性，默认渲染为span标签

````:closed_lock_with_key:
<tansition-group appear tag="ul">
<li v-for="(item ,i) in list" :key="item.id" @click="del(i)">
{{item.id}} --- {{item.name}}
</li>
</transition-group>
````





### Vue组件

#### 定义全局组件的三种方式

1.组件化和模块化的区别，方便代码的分层开发，保证每个功能模块的职能单一;

模块化是从代码逻辑的角度进行划分的；组件化是从UI界面的角度进行划分的，前端的组件化，方面UI组件的重复利用.

**创建组件的方式一：**

使用Vue.extend来创建全局的Vue组件

````javascript
<div id="app">
    //第三步 如果要使用组件，直接把组件的名称以HTML标签的形式，引入到页面中即可
    <my-com1></my-com1>
</div>
<script>
    //第一步 使用Vue.extend来创建全局Vu组件
var com1 = Vue.extend({
    template: '<h3>这是使用vue.extend创建的组件</h3>'//通过template属性，指定了组件要展示的html结构
})
//第二步 使用Vue.component('组件的名称',创建出来的组件模板对象）
Vue.component('myCom1', com1)
</script>
````

或是简写成:

````javascript
<div id="app">
    <mycom1></my-com1>
</div>
<script>
Vue.component('mycom1', Vue.extend({
    template: '<h3>这是使用vue.extend创建的组件</h3>'
}))
</script>
````

Vue.component第一个参数：组件的名称；第二个参数Vue.extend创建的组件，其中，template就是组件需要展示的HTML结构

注意：如果使用Vue.component定义全局组件的时候，组件名称使用了驼峰命名，在引用组件的时候，需要把大写改小写，并且两个单词需要用 - 来连接；如果不适用驼峰，就直接拿名称来使用即可

**创建组件的方式二：**

````javascript
<div id="app">
    <mycom1></my-com1>
</div>
<script>
Vue.component('mycom1',{
    template: '<h3>这是使用vue.extend创建的组件</h3>'
})
</script>
````

Vue.component(组件名称，对象)

注意：不论是哪种方式创建出来的组件，组件的template属性指向的模板内容，只能有唯一的一个根元素

**创建组件的方式三( 推荐 )：**

````javascript
<div id="app">
    <mycom3></mycom3>
</div>

<!-- 在被控制的 元素app外面使用template元素来定义组件的HTML模板结构 -->
<template id="tmp1">
    <h1>这种是通过template元素在外部定义组件的结构</h1>
	<h3>好用，推荐！</h3>
</template>

<script>
        Vue.component('mycom3',{
        template: '#tmp1'
    })
</script>
````

#### 自定义私有组件

````javascript
<div id="app1">
    <mycom3></mycom3>
</div>

<div id="app2">
    <mycom3></mycom3>
	<login></login>
</div>
<!-- 定义的全局组件的HTML模板结构 -->
<template id="tmp1">
    <h1>这种是通过template元素在外部定义组件的结构</h1>
</template>

<script>
//全局组件
 Vue.component('mycom3',{
        template: '#tmp1'
 })
//控制app1
var vm1 = new Vue({
    el:'#app1',
    data: {
    },
    methods: {},
    filters: {},
    directives:{},   
})
//控制app2
var vm2 = new Vue({
    el:'#app2',
    data: {
    },
    methods: {},
    filters: {},
    directives:{},
    components:{
        //定义实列内部私有的组件
        login:{
            template: '<h1>这是私有的login组件</h1>'
        }
    },
    
    beforeCreate(){},
    created(){},
    beforeMount(){},
    mounted(){},
    beforeUpdate(){},
    updated(){},
    beforeDestroy(){},
    destroyed(){}
})
</script>
````

注意：内部的私有组件，也可以同全局组件一样，把模板定义到外面去，然后用#temp2去关联它。

````javascript
<div id="app2">
	<login></login>
</div>

<!-- 定义的局部组件的HTML模板结构 -->
<template id="tmp2">
    <h1>这是私有的login组件</h1>
</template>

<script>
var vm2 = new Vue({
    el:'#app2',
    data: {
    },
    methods: {},
    filters: {},
    directives:{},
    components:{
        //定义实列内部私有的组件
        login:{
            template: '#tmp2'
        }
    }
})
</script>
````

### 组件中的data和methods

````javascript
<div id="app">
	<mycom1></mycom1>
</div>

<script>
    Vue.component('mycon1',{
    template:'<h1>这是全局组件 --- {{msg}}</h1>',
    data:function(){//必须要return里面定义的对象，如果把外面的对象return出去，会导致各个组件都指向同一个对象，数据会错误。
        return {
            msg: '这是组件中的data定义的数据'
        }
    },
    methods:{
        //组件的方法
        add(){}
    }
})
var vm = new Vue({
    el:'#app',
    data: {
    },
    methods: {},
    filters: {},
    directives:{}
})
</script>
````

1.组件可以有自己的data数据

2.组件的data和实列的data不一样，实列中的data可以为一个对象，但是组件中的data必须是一个方法；

3.组件中的data内部还必须返回一个对象;

4.组件中的data数据的使用方式和实列中的data使用方式完全一样

### 组件的切换

方法一：使用v-if和v-else结合flag进行切换

````javascript
<div id="app">
	<a href="" @click.prevent="flag=true">登录</a>
	<a href="" @click.prevent="flag=false">注册</a>
	<login v-if="flag"></login>
	<register v-else="flag"></register>
</div>

<script>
Vue.component('login',{
        template:'<h3>登录组件</h3>'
    })
Vue.component('register',{
        template:'<h3>注册组件</h3>'
    })
var vm = new Vue({
    el:'#app',
    data: {
    },
    methods: {},
    filters: {},
    directives:{}
})
</script>
````

缺点是只能切换两个组件，三个以上就很难用flag来弄。

方法二：

component标签结合它的 :is 属性

````javascript
<div id="app">
    <!-- Vue提供了component来展示对应名称的组件,是一个占位符，is属性用来指定要展示的组件的名称 -->
    <a href="" @click.prevent="comName='login'">登录</a>
	<a href="" @click.prevent="comName='register'">注册</a>
	<component :is="comName"></component>
</div>

<script>
Vue.component('login',{
        template:'<h3>登录组件</h3>'
    })
Vue.component('register',{
        template:'<h3>注册组件</h3>'
    })
var vm = new Vue({
    el:'#app',
    data: {
        comName:'login'//当前component中的:is绑定的组件的名称
    },
    methods: {},
    filters: {},
    directives:{}
})
</script>
````

Vue提供了component来展示对应名称的组件,是一个占位符，is属性用来指定要展示的组件的名称

总结:当前学习了几个Vue提供的标签了？ component, template, transition, transitionGroup

### 组件切换动画

多个组件的过渡简单很多，不需要使用key特性，只需要使用动态组件，用transition把component包裹起来就可以了。

还可以通过transition的mode属性设置组件切换时候的模式

````:british_virgin_islands:
<style>
.v-enter,.v-leave-to{
    opacity:0;
    transform:translateX(150px);
}
.v-enter-active,v-leave-active{
    transition:all 0.5s ease;
}

</style>

<div id="app">
    <a href="" @click.prevent="comName='login'">登录</a>
	<a href="" @click.prevent="comName='register'">注册</a>
	
	<!--可以通过mode属性设置组件切换时候的模式 -->
	<transition mode="out-in>
		<component :is="comName"></component>
	</ransition>
	
</div>

<script>
Vue.component('login',{
        template:'<h3>登录组件</h3>'
    })
Vue.component('register',{
        template:'<h3>注册组件</h3>'
    })
var vm = new Vue({
    el:'#app',
    data: {
        comName:'login'//当前component中的:is绑定的组件的名称
    },
    methods: {},
    filters: {},
    directives:{}
})
</script>
````



### 父组件向子组件传值

通过属性绑定来实现

````javascript
<div id="app">
    <!-- 父组件通过子组件绑定属性的形式把数据传递给子组件 -->
	<com1 v-bind:parentmsg="msg"></com1>
</div>

<script>
var vm = new Vue({
    el:'#app',
    data: {
        msg: '122啊这是父组件中的数据'
    },
    methods: {},
    components:{
        //结论:子组件默认无法访问到父组件的data上的数据和methods上的方法
        com1: {
            data(){//注意：子组件中的data数据并不是父组件传递过来的，而是自己私有的,例如子组件通过Ajax请求过来的数据;子组件的数据是可读可写的；
                return {
                    title: '123,',
                    content: 'qqq'
                }
            },
            template: '<h1>这是子组件 --- {{parentmsg}}',
            //注意：组件中的所有props中的数据都是父组件传递给子组件的；props的数据都是只读的。
            props:['parentmsg'],//这是唯一的一个数组形式，把父组件传递过来的parentmsg属性，先在props数组中定义一下,这样才能使用这个数据
            directives:{},
            filters:{},
            components:{}
        }
    }
})
</script>
````

子组件默认无法访问到父组件的data上的数据和methods上的方法

1,父组件可以在引用子组件的时候通过属性绑定（v-bind:)的形式吧需要传递给子组件的数据已属性绑定的形式传递到子组件的内部同子组件使用;

2,把父组件传递过来的属性，先在props数组中定义一下,这样子组件才能使用这个数据.

### 父组件把方法传递给子组件

````javascript
<div id="app">
    <!-- 父组件向子组件传递方法，使用的是事件绑定机制； v-on,当我们自定义了一个事件属性，子组件就能够通过某些方式来调用传递进去的这个方法了 -->
	<com2 v-on:func123='show'></com1>
</div>

<template id="tmp1">
    <div>
    	<h1>这是子组件</h1>
		<input type="botton" value="这是子组件中的按钮 -点击会触发父组件传递过来的func方法" @click="myclick">
    </div>
</template>

<script>

var com2 = {
     template: '#tmp1',
    methods:{
        myclick(){
            //当点击子组件的 按钮的时候，如何拿到父组件传递过来的func方法呢？
            //通过这个方法this.$emit('事件函数名'，传参1),可以有多个传递参数,也可以不传参数
            this.$emit('func123',123)
        }
    }
}
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {
        show(data1){
            console.log('调用了父组件的show方法:---' + data1)
        }
    },
    components:{
        com2: com2
    }
    directives:{},
    filters:{},
    }
})
</script>
````

当子组件通过 this.$emit('父组件方法函数名',传参)调用父组件的方法的时候，可以通过传参把子组件身上的data数据当做参数传递进去，这样在调用父组件身上方法的时候，父组件就拿到了子组件身上的数据

````javascript
<div id="app">
    <!-- 父组件向子组件传递方法，使用的是事件绑定机制； v-on,当我们自定义了一个事件属性，子组件就能够通过某些方式来调用传递进去的这个方法了 -->
	<com2 v-on:func123='show'></com1>
</div>

<template id="tmp1">
    <div>
    	<h1>这是子组件</h1>
		<input type="botton" value="这是子组件中的按钮 -点击会触发父组件传递过来的func方法" @click="myclick">
    </div>
</template>

<script>

var com2 = {
    template: '#tmp1',
    data(){//子组件身上的数据
        return{
            sonmsg:{ name:'小儿子'，age：6}
        }
    }
    methods:{
        myclick(){
            //子组件身上的data数据通过参数传递进去
            this.$emit('func123',this.sonmsg)
        }
    }
}
var vm = new Vue({
    el:'#app',
    data: {
        //父组件上用来存储来自于子组件身上的data数据
        datamsgFormSon: null
    },
    methods: {
        show(data1){
            //console.log('调用了父组件的show方法:---' + data1)
            //console.log(data1);
            this.datamsgFormSon = data1;
        }
    },
    components:{
        com2: com2
    }
    directives:{},
    filters:{},
    }
})
</script>
````

### 使用this.$refs来获取DOM元素和组件引用

实列vm上有一个$refs属性

1.获取元素

````javascript
<div id="app">
    <input type="button" value="获取元素" @click="getElement" ref="mybtn">
	<h3 ref='myh3'>哈哈哈</h3>
</div>


<script>

var vm = new Vue({
    el:'#app',
    data: {},
    methods: {
        getElement(){
            console.log(this.$refs.myh3.innerText)
        }
    }
})
</script>
````

$refs上有很多个，想获取那个元素的dom，就给它一个ref属性，这样就可以通过$refs.属性值来获取到这个元素节点

 同样的方法可以获取到组件。

````javascript
<div id="app">
    <input type="button" value="获取元素" @click="getElement" ref="mybtn">
	<h3 ref='myh3'>哈哈哈</h3>
	<hr>
    <login ref="mylogin"></login>
</div>


<script>
 var login = {
        template:'<h1>登录组件</h1>'，
     data(){
         return {
             msg: 'son msg'
         }
     },
      methods:{
          show(){
              console.log('调用了子组件的方法')
          }     
     }
}

var vm = new Vue({
    el:'#app',
    data: {},
    methods: {
        getElement(){
            //通过refs来获取到子组件的数据
            console.log(this.$refs.mylogin.msg)
            //通过refs来获取子组件的方法
            this.$refs.mylogin.show()
        }
    },
    components:{
        login
    }
})
</script>
````

### 前端路由

在单页面应用程序中，通过hash（#号）改变来切换页面的方式，称为前端路由。区别于后端路由。

使用vue-router第三方包来实现

在项目所在目录运行：

`npm install vue-router --save`



当导入vue-touter包治好，在window全局对象中就有了一个路由的构造函数，VueRouter

````javascript
<div id="app">
    <!-- router-link默认渲染a标签, tag属性用来指定渲染成什么标签-->
    <router-link to="/login" tag="span">登录</router-link>
	<router-link to="/register">注册</router-link>
    <!-- 这是vue-router提供的元素，用来做占位符，将来放路由规则匹配到的组件 -->
    <router-view></router-view>
</div>
<script>
    var login = {
        template: '<h1>登录组件</h1>'
    }
 var register = {
        template: '<h1>注册组件</h1>'
    }
var routerObj = new VueRouter({
    routes:[//路由匹配规则
        //注意 component的属性值必须是一个组件的模板对象，不能是组件的引用名称,否则就会报错
        {path: '/', redirect:''},//这里的redirect和node中的redirect是两码事，重定向。
        {path: '/login', componet:login},
        {path: '/rigister', componet:register}
    ],
    mode:"histroy"//干掉网页路径中#，多余的，干掉才方便
})
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {},
    router:routerObj//将路由规则对象注册到vm实例上，用来监听url地址的变化来展示对应的组件
})
</script>
````

每个路由规则都是一个对象，这个对象有两个必须的属性：

属性1是path，表示监听哪个路由链接地址

属性2是component表示如果路由是前面匹配到的path，则展示component属性对应的按个组件

注意 component的属性值必须是一个组件的模板对象，不能是组件的引用名称,否则就会报错

> **注意:如果要干掉http路径中的#号，就需要添加mode:'history'这个属性**

### 设置选中路由高亮的两种方式

方式一：

直接给类.router-link-active设置样式就可以了。

方式二：

通过构造函数配置选项设置，默认值可以通过路由的构造选项linkActiveClass来全局配置

````javascript
var routerObj = new VueRouter({
    routes:[
        {path: '/', redirect:''},
        {path: '/login', componet:login},
        {path: '/rigister', componet:register}
    ],
    linkActiveClass:'myactive'
})
````

然后给myactive设置样式就可以了 。

### 路由传参

方式一：使用query方式传递参数

如果在路由中，使用查询字符串给路由传递参数，则不需要修改路由规则的path属性.

````javascript
<div id="app">
    <!--使用查询字符串给路由传递参数，则不需要修改路由规则的path属性. -->
    <router-link to="/login?id=10&name=bob" tag="span">登录</router-link>
	<router-link to="/register">注册</router-link>
    <router-view></router-view>
</div>
<script>
    var login = {
        template: '<h1>登录组件---{{$route.query.id}}---{{$route.query.name}}</h1>',
        created(){//组件的生命周期函数
        	//console.log(this.$route)
            //成功拿到？id后面的值10
            console.log(this.$route.query.id)
        }
    }
 var register = {
        template: '<h1>注册组件</h1>'
    }
var routerObj = new VueRouter({
    routes:[
        {path: '/', redirect:''},
        {path: '/login', componet:login},
        {path: '/rigister', componet:register}
    ]
})
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {},
    router:routerObj
})
</script>
````

想拿到路径地址?id后面的值，可以使用$route对象上的.query属性来获取到。多个参数都可以拿到。

方式二：使用params方式传递参数

````javascript
<div id="app">
    <!--login/10后面传递参数10-->
    <router-link to="/login/10" tag="span">登录</router-link>
	<router-link to="/register">注册</router-link>
    <router-view></router-view>
</div>
<script>
    var login = {
        template: '<h1>登录组件</h1>',
        created(){//组件的生命周期函数
            //这里通过params拿到了路由后面的参数10
            console.log(this.$route.params.id)
        }
    }
 var register = {
        template: '<h1>注册组件</h1>'
    }
var routerObj = new VueRouter({
    routes:[
        {path: '/', redirect:''},
        //把路由改造成这样子，:id的值通过url后面传递过来，这个例子是10
        {path: '/login/:id', componet:login},
        {path: '/rigister', componet:register}
    ]
})
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {},
    router:routerObj
})
</script>
````

### 路由的嵌套

使用childrend属性来实现路由的嵌套

注意点：子路由的path前面不要带/，否则永远以根路径开始请求

````javascript
<div id="app">
	<router-link to="/account">Account</router-link>
    <router-view></router-view>
</div>
<template id="tmp1">
    <div>
    <h1>这是Account组件</h1>
	<router-link to="/account">登录</router-link>
	<router-link to="/account">注册</router-link>
	</div>
</template>
<script>

var account = {
    template: '#tmp1'
	}
var login = {
     template: '<h1>登录组件</h1>'
    }
var register = {
        template: '<h1>注册组件</h1>'
    }
var routerObj = new VueRouter({
    routes:[
        {path: '/account',
         componet:account,
         children:[
             //注意这里没有斜线了
            {{path: 'login', componet:login},
        	{path: 'rigister', componet:register}}
         ]}
    ]
})
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {},
    router:routerObj
})
</script>
````

下面这样做行不通

````javascript
routes:[
        {path: '/account',componet:account},
        {path: '/account/login', componet:login},
        {path: '/account/rigister', componet:register}
    ]
````

### 路由-使用命名视图实现经典布局

````javascript
<div id="app">
    //没有name就找默认的default
    <router-view></router-view>
	//命名left就在path里去找left属性对应的东东
	<router-view name="left"></router-view>
	////命名main就在path里去找main属性对应的东东
	<router-view name="main"></router-view>
</div>
<script>

var header = {
    template: '<h1>Header头部区域</h1>'
	}
var leftBox = {
     template: '<h1>Left侧边栏区域</h1>'
    }
var mainBox = {
        template: '<h1>mainBox主要区域</h1>'
    }
var routerObj = new VueRouter({
    routes:[
        //注意这里的是componets而不是componet了。
        path:'/', components:{
        	'default': header,
        	'left':leftBox,
        	'main':mainbox
        }
    ]
})
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {},
    router:routerObj
})
</script>
````

### watch属性

使用这个属性可以监视data中指定的数据变化，然后触发watch中对应的function处理函数。

譬如这个属性可以用来监视路由的变化。

````javascript
<div id="app">

</div>
<script>
var vm = new Vue({
    el:'#app',
    data: {
        firstname:'',
        lastname:'',
        fullname:''
    },
    methods: {},
    watch:{
        //可以传递两个newVal，oldVal参数
        'firstname':function(newVal,oldVal){
            this.fullname = newVal + '-' + this.lastname
        }
    }
})
</script>
````

### computed计算属性

在computed中可以定义一些属性叫计算属性，本质就是一个方法，我们在使用这些计算属性的时候，是把它们的名称当做属性来使用的，并不会把计算属性当做方法去调用。

````javascript
<div id="app">
	<input type='text' v-model="firstname"> +
    <input type='text' v-model="lastname"> =
        <!-- 注意，计算属性在引用的时候后面不要加（）去调用，直接当做普通属性去使用 -->
    <input type='text' v-model="fullname">
</div>
<script>
var vm = new Vue({
    el:'#app',
    data: {
        firstname:'',
        lastname:''
    },
    methods: {},
    computed:{
        'fullname': function(){
            return this.firstname + '-' + this.lastname
        }
    }
})
</script>
````

注意点：

1.        计算属性在引用的时候后面不要加（）去调用，直接当做普通属性去使用
2.        计算属性内部所用到的任何data中的数据发生了变化，就会重新计算这个计算属性
3.        计算属性的求值结果会被缓存起来，方便下次直接使用，如果计算属性方法中，所有的数据都没有发生过变化，则不会重新对计算属性求值。

### watch、computed和methods之间的对比

1.computed属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当做属性来使用；

2.methods方法表示一个具体的操作，主要书写业务逻辑;

3.watch一个对象，键是需要观察的表达式，值是对应的回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看做是computed和methods的结合体

### nrm的安装使用

作用：提供了一些最常用的NPM包镜像地址

1.运行`npm i nrm -g`全局安装nrm包

2.使用`nrm ls`查看当前所有可用的镜像源地址及当前所使用的镜像源地址

3.使用`nrm use npm `或`nrm use taobao`切换不同的镜像源地址

> 注意： nrm只是单纯的提供了几个常用的下载包的URL地址，可用很方便的进行切换。

## Webpack

网页中引入的静态资源多了后会出现的问题

1.网页加载速度慢，因为发起了很多的二次请求

2.需要处理错综复杂的包的依赖关系

如何解决

1.合并，压缩，精灵图，图片的Base64编码

2.可以使用requireJs，webpack

什么是webpack

是前端的一个项目构建工具，是基于Node.js开发出来的一个前端工具

webpack和Gulp的区别：webpack 是基于项目进行构建的。Gulp是基于task任务的。

### webpack安装的两种方式

1.运行`npm i webpack -g`全局安装，这样就能在全局使用webpack命令

2.在项目根目录中运行`npm i webpack --save-dev`安装到项目依赖中

### webpack可以做什么事情？

1.webpack能够处理JS文件的相互依赖关系

2.webpack能够处理JS的兼容问题，把高级的浏览器不识别的语法转换为低级的

````shell
//打包的命令
webpack 要打包的文件路径 打包好的输出文件路径
````

### webpack配置文件使用

需要在项目根目录下添加一个webpack.config.js文件，里面的内容为：

````javascript
const path = require('path')
//这个配置文件就是一个js文件，通过Node中的模块操作，向外暴露了一个配置对象
module.exports = {
    entry:path.join(__dirname,'./src/main.js'),//入口，表示要使用webpack打包哪个文件
    output:{//输出文件的相关配置
        path:path.join(__dirname,'./dist'),//指定打包好的文件输出到哪个目录中去
        filename: 'bundle.js'//指定输出文件的名称
    }
}
````

注意：如果不弄这个配置文件的话，就需要每次都自己输入命令`webpack 要打包的文件路径 打包好的输出文件路径`来打包

### webpack-dev-server工具

手动执行打包编译太麻烦，可以使用这个工具来实现自动打包编译的功能。它会自动监听我们的代码。

1.安装：运行`npm i webpack-dev-server -D`把这个工具安装到项目的本地开发依赖

2.安装完毕后，这个工具的用法和webpack命令的用法完全一样

3.因为我们是在项目中本地安装的，不是全局安装，所以不能在powershell中直接运行。

4.这个工具想要正常运行，必须在本地项目中安装webpack。哪怕全局有安装webpack都是不行的。

5.webpack-dev-server帮我们打包生成的bundle.js文件并没有存放到实际的物理磁盘上，而是托管到了电脑的内存中，所以我们在项目根目录中找不到这个打包好的bundle.js文件

6.我们可以认为，webpack-dev-server把打包好的文件以一种虚拟的形式托管到了我们的项目根目录中，虽然我们看不到它，它放到了内存里了。

webpack-dev-server配置命令的方式1（推荐）

在package.json里面配置

````javascript
{
    "name": "webpack-study",
        //等等其他的
     "scripts":{
         //自定义dev用来代替的shell里面需要输入的webpack-dev-serve
        "dev": "webpack-dev-serve --open --port 8080 ---contentBase src --hot"
         //open自动帮我们启动浏览器打开网址
         //port 指定端口8080
         //contentBase 指定打开网页去哪里找需要打开的页面
         //hot是热加载，不需要每次都打包一个新的，只需要更新修改的部分。
      }
}
````

第二种方式，在webpack.config.js里面设置

````javascript
const path = require('path')
//启用热更新的第二步
const webpack = require('webpack')
module.exports = {
    entry:path.join(__dirname,'./src/main.js'),
    output:{
        path:path.join(__dirname,'./dist'),
        filename: 'bundle.js'
    },
    devServer:{
        open:true,//自动打开浏览器
        port:8080,//设置端口
        contentBase:'src',//指定托管的根目录
        hot:true//启用热更新的第一步
    },
    plugins:[//配置插件的节点
        new webpack.HotModuleReplacementPlugin()//第三步:new 一个热更新的模块对象
    ]
}
````

### html-webpack-plugin插件

作用

1.自动在内存中根据指定页面生成一个内存的页面

2自动把打包好的bundle.js追加到页面中去

### loader处理css样式表插件

1.如果要打包处理css文件，需要安装`cnpm i style-loader css- loader -D`

2.打开webpack.config.js配置文件，新增一个配置节点，module。这个mudule对象上，有个rules属性，是个数组，是所有第三方模块的匹配规则

````javascript
const path = require('path')
//启用热更新的第二步
const webpack = require('webpack')
module.exports = {
    entry:path.join(__dirname,'./src/main.js'),
    output:{
        path:path.join(__dirname,'./dist'),
        filename: 'bundle.js'
    },
    devServer:{
        open:true,//自动打开浏览器
        port:8080,//设置端口
        contentBase:'src',//指定托管的根目录
        hot:true//启用热更新的第一步
    },
    plugins:[//配置插件的节点
        new webpack.HotModuleReplacementPlugin()//第三步:new 一个热更新的模块对象
    ],
    mudule:{//配置所有的第三方模块加载器
        rules:[//所有第三方模块的匹配规则
            {test:/\.css$/,use:['style-loader','css-loader']}//意思是配置用后面的插件来处理所有的.css文件
        ]
    }
}
````



### 配置处理less文件的loader

先装插件`cnpm i less - D`然后装`cnpm i less-loader -D`

因为后面的插件需要依赖前面的，所有前面的也要装

````javascript
const path = require('path')
//启用热更新的第二步
const webpack = require('webpack')
module.exports = {
    entry:path.join(__dirname,'./src/main.js'),
    output:{
        path:path.join(__dirname,'./dist'),
        filename: 'bundle.js'
    },
    devServer:{
        open:true,//自动打开浏览器
        port:8080,//设置端口
        contentBase:'src',//指定托管的根目录
        hot:true//启用热更新的第一步
    },
    plugins:[//配置插件的节点
        new webpack.HotModuleReplacementPlugin()//第三步:new 一个热更新的模块对象
    ],
    mudule:{//配置所有的第三方模块加载器
        rules:[//所有第三方模块的匹配规则
            {test:/\.css$/,use:['style-loader','css-loader']},//意思是配置用后面的插件来处理所有的.css文件
            { test:/\.less$/,use:['style-loader','css-loader','less-loader']}//配置处理.less文件的第三方loader规则
        ]
    }
}
````

### 配置处理.scss文件的loader

先装包`cnpm i sass-loader - D`,还要装依赖包`cnpm i node-sass - D`

配置一下

````javascript
const path = require('path')
//启用热更新的第二步
const webpack = require('webpack')
module.exports = {
    entry:path.join(__dirname,'./src/main.js'),
    output:{
        path:path.join(__dirname,'./dist'),
        filename: 'bundle.js'
    },
    devServer:{
        open:true,//自动打开浏览器
        port:8080,//设置端口
        contentBase:'src',//指定托管的根目录
        hot:true//启用热更新的第一步
    },
    plugins:[//配置插件的节点
        new webpack.HotModuleReplacementPlugin()//第三步:new 一个热更新的模块对象
    ],
    mudule:{//配置所有的第三方模块加载器
        rules:[//所有第三方模块的匹配规则
            {test:/\.css$/,use:['style-loader','css-loader']},//意思是配置用后面的插件来处理所有的.css文件
            { test:/\.less$/,use:['style-loader','css-loader','less-loader']}，//配置处理.less文件的第三方loader规则
            {test:/\.scss$/, use['style-loader','css-loader','sass-loader']}//配置处理.sass文件的规则
        ]
    }
}
````

### url-loader的使用

默认情况下，webpack无法处理css中的url地址，不管是图片还是字体库

安装`cnpm i url-loader file-loader -D`后面是依赖包

````javascript
const path = require('path')
//启用热更新的第二步
const webpack = require('webpack')
module.exports = {
    entry:path.join(__dirname,'./src/main.js'),
    output:{
        path:path.join(__dirname,'./dist'),
        filename: 'bundle.js'
    },
    devServer:{
        open:true,//自动打开浏览器
        port:8080,//设置端口
        contentBase:'src',//指定托管的根目录
        hot:true//启用热更新的第一步
    },
    plugins:[//配置插件的节点
        new webpack.HotModuleReplacementPlugin()//第三步:new 一个热更新的模块对象
    ],
    mudule:{//配置所有的第三方模块加载器
        rules:[//所有第三方模块的匹配规则
            {test:/\.css$/,use:['style-loader','css-loader']},//意思是配置用后面的插件来处理所有的.css文件
            { test:/\.less$/,use:['style-loader','css-loader','less-loader']}，//配置处理.less文件的第三方loader规则
            {test:/\.scss$/, use['style-loader','css-loader','sass-loader']},//置处理.sass文件的规则
    		{test:/\.(jpg|png|gif|bmp|jpeg)$/,use:'url-loader?limit=图片大小&name=[hash:8}-[name].[ext]'}
    //处理图片路径的loader,?后面可以传递参数
        ]
    }
}
````

limit给的的值，是图片的大小，单位是byte,如果引用的图片大于或是等于给定的值，则不会转为base64图片;

name=[hash:8}-[name].[ext]用来把名字改为前面8位哈希值-原来的名字

### url-loader处理字体文件

配置

````javascript
const path = require('path')
//启用热更新的第二步
const webpack = require('webpack')
module.exports = {
    entry:path.join(__dirname,'./src/main.js'),
    output:{
        path:path.join(__dirname,'./dist'),
        filename: 'bundle.js'
    },
    devServer:{
        open:true,//自动打开浏览器
        port:8080,//设置端口
        contentBase:'src',//指定托管的根目录
        hot:true//启用热更新的第一步
    },
    plugins:[//配置插件的节点
        new webpack.HotModuleReplacementPlugin()//第三步:new 一个热更新的模块对象
    ],
    mudule:{//配置所有的第三方模块加载器
        rules:[//所有第三方模块的匹配规则
            {test:/\.css$/,use:['style-loader','css-loader']},//意思是配置用后面的插件来处理所有的.css文件
            { test:/\.less$/,use:['style-loader','css-loader','less-loader']}，//配置处理.less文件的第三方loader规则
            {test:/\.scss$/, use['style-loader','css-loader','sass-loader']},//置处理.sass文件的规则
    		{test:/\.(jpg|png|gif|bmp|jpeg)$/,use:'url-loader?limit=图片大小&name=[hash:8}-[name].[ext]'},
    //处理图片路径的loader,?后面可以传递参数
    {test:/\.(ttf|eot|svg|woff|woff2)$/, use:'url-loader'}//处理字体文件的loader
        ]
    }
}
````

### webpack中babel的配置

在webpack中默认只能处理一部分ES6的新语法，一些更高级的语法处理不来，这个时候就需要借助于第三方的loader来帮助webpack处理这些高级语法，把高级语法转为低级语法后，再把结果交给webpack去打包的bundle.js中。

通过Babel可以帮我们将高级语法转换为低级语法

1.有两套包，两种安装方式来安装Babel相关的loader功能

第一套包：`cnpm i babel-core babel-loader babel-plugin-transform-runtime -D`

第二套包：`cnpm i babel-preset-env babel-preset-stage-0 -D`

> 第1套包是工具，是转换器，第2套包是字典，是语法插件，

2.打开webpack配置文件，在module节点下的rules数组中加一个新的匹配规则

````javascript
{test:/\.js$/, use: 'babel-loader', exclude:/node_modules/}//配置Babel转换高级ES语法的loader，exclude排除掉node_modules目录的js文件排除掉
````

> 注意：在配置babel的loader规则的时候，必须把node_modules目录，通过exclude排除掉，原因是不排除node_modules就会把第三方js都打包编译了，会非常消耗资源，打包速度非常慢，此外，就算转换了，项目也还是无法运行的。
>
>

3、在项目的根目录，新建一个.babelrc的babel配置文件，这个配置文件是JSON格式，所以里面不能写注释，字符串必须用双引号， .babelrc写如下配置：

````javascript
{
    "presets": ["env","stage-0"],
    "plugins": ["transform-runtime"]
}
````

presets是语法，plugins是插件,就算第一步里面安装的那两套包

目前，我们安装的babel-preset-env是比较新的ES语法，之前安装的是babel-preset-es2015, 现在出了一个新的babel-preset-env，它包含了所有的es相关的新语法

### 使用vue实例的render方法渲染组件

````javascript
<div id="app">

</div>
<script>
    var login = {
        template:'<h1>这是登录组件</h1>'
    }
var vm = new Vue({
    el:'#app',
    data: {},
    methods: {},
    render: function(createElements){
        return createElements(login)
        //注意：这里的return结果会替换页面中el指定的那个容器
    }
})
</script> 
````

createElements是一个方法，调用它能够把指定的组件模板渲染为html结构

### 区别webpack导入vue和script引入vue的区别

普通网页中用直接使用link引入vue.js

webpack中，需要import Vue from '../node_modules/vue/dist/vue.js',如果使用import Vue form 'vue'导入的功能会不完整，只提供runtime-only的方式，是有问题的。

如果非要使用import Vue form 'vue'这种形式，那就需要改一下webpack.config.js的配置文件

main.js中：

````javascript
import Vue from 'vue'
````

webpack.config.js中：

````javascript

module.exports = {
    entry:path.join(__dirname,'./src/main.js'),//入口，表示要使用webpack打包哪个文件
    output:{//输出文件的相关配置
        path:path.join(__dirname,'./dist'),//指定打包好的文件输出到哪个目录中去
        filename: 'bundle.js'//指定输出文件的名称
    },
    plugins:[],//所有webpack插件的配置节点
    module:{},//配置所有第三方loader模块的
    resolve:{
        alias:{
            "vue$":"vue/dist/vue.js"//这个就是当前小节需要配置的地方
        }
    }
}
````

### 在vue中结合runder函数渲染指定的组件到容器中

默认情况下，webpack无法打包.vue文件，需要安装相关的loader

`cnpm i vue-loader vue-template-compiler - D`

在配置文件中，新增loader的配置项{test:/\.vue$/, use:'vue-loader'}

在webpack中如果想通过vue把一个组件放到一个页面中去展示，vm实例中的render函数可以实现

````javascript
import login form './login.vue'
var vm = new Vue({
    el:'#app',
    data:{
        msg:'123'
    },
    render:function(createElements){//在webpack中如果想通过vue把一个组件放到一个页面中去展示，vm实例中的render函数可以实现
        return createElements(login)
    }
})
````

可以简写成：

````javascript
import login form './login.vue'
var vm = new Vue({
    el:'#app',
    data:{
        msg:'123'
    },
    render: c => c(login)//箭头函数的形式
})
````

### 总结webpack中如何使用vue

1.安装vued的包：`cnpm i vue -s`

2.由于在webpack中推荐使用.vue这个组件模板文件定义组件，所有需要安装能够解析这种文件的loader ：`cnpm i vue-loader vue-template-complier - D`

3.在main.js中导入vue模块：import Vue from 'vue'

4.定义一个.vue结尾的组件，其中组件有三部分组成：template script style

5.使用import导入这个组件: import login from './login.vue'

6.创建vm的实例，var vm = new Vue({ el: '#app', render: c => c(login) })

7.在页面中创建一个id为app的div元素作为vm实例控制的区域

### export default和export的使用方式

node中向外暴露成员的方式module.exports = { }

导入var 名称 = require('模块标识符')

在ES6中也规定了导入和导出模块

ES6中导入模块：使用import 模块名称 from '模块标识符'  或是import '表示路径'

ES6中导出模块：使用export default和export向外导出模块

注意：1.export default向外暴露成员，可以使用任意的变量来接收；

2.在一个模块中export default只允许向外暴露一次；

3.在一个模块中，可以同时使用export default和export；

4.export向外暴露成员只能使用{}的形式来接收，这种形式叫按需导出;

5.export可以向外暴露多个成员，同时，如果某些成员我们在import的时候不需要，则可以不在{}中定义;

6.使用export导出成员的时候，必须严格按照导出的是的名称来使用{}来接收;

7.如果使用export导出的成员换个名称的话，可以在接收的时候使用as来起别名,例如{title as title123}

### 结合webpack使用vue-router

````javascript
import Vue from 'vue'
//1.导入vue-router包
import VueRouter from 'vue-router'
//2.手动安装VueRouter
Vue.use(VueRouter)
import app from './App.vue'

//3.创建路由对象
var router = new VueRouter({
    routes:[
        {path: '/account', component: account},
        {path: '/goodslist', component: goodslist}
    ]
})
var vm = new Vue({
    el:'#app',
    render: c => c(app),
    router //4.将路由对象挂载到vm上
})
````

注意，render会把el指定的容器中的所有内容清空覆盖，所有不要把路由的router-view 和router-link直接写到el所控制的元素中

### 组件中style标签lang属性和scoped属性

组件中的style标签只支持普通的样式，如果想要启用scss或less，需要为style元素设置lang属性

````javascript
<template></template>
<script></script>
<style lang="scss">
    body{
        div{
            font-style:italic;
        }
    }
</style>
````

组件中如果想使用私有样式，就必须给style标签添加scoped属性

````javascript
<template></template>
<script></script>
<style scopted>
    color:red;
</style>
````

> 如果咱们的style标签是在.vue组件中定义的，那么推荐都为style标签开启scoped属性；

样式的scoped属性的实现原理：是通过css的属性选择器实现的，它偷偷给div加了一个自定义属性，然后给这个属性设置样式。

### 抽离路由模块

````javascript
//导入vue-router包
import VueRouter from 'vue-router'
//导入Account组件
import account from './main/Account.vue'
import goodlist from './main/GoodsList.vue'
//导入Account的两个子组件
import login from './subcom/login.vue'
import register from './subcom/register.vue'
//3.创建路由对象
var router = new VueRouter({
    routes:[
        {path: '/account', component: account},
        {path: '/goodslist', component: goodslist}
    ]
})
//把理由对象暴露出去
export default router
````

最后在main.js里通过import rotuer from './router.js'来导入自定义路由模块

### Vuex的使用

1.装包`cnpm i vuex -S`

2.导入包 import Vuex from 'vuex'

3.注册包 Vue.use(Vuex)

4.new一个实例，得到数据仓储对象 

````javascript
var store = new Vuex.Store({
    state:{//相当于data，用来存储数据
        //如果在组件中想要访问store中的数据，只能通过this.$store.state.xxx来访问
       count:0 //实例数据
    },
    mutations:{
        //注意：如果要操作store中的state值，只能通过调用mutations提供的方法才能操作，不要在子组件中直接操作state中的数据，一旦有错误，容易导致数据紊乱，很难找到出错的地方。
        increment(state){
            state.count++
        },
        //如果子组件想要调用mutations中的方法，只能通过使用this.$store.commit('方法名')
        subtract(state, c) {
            state.count -= c
        }
        //mutations的函数参数列表中最多值支持两个参数，参数1是state状态，参数2是通过commit提交过来的参数
    },
    getters: {
        //这里的getters只负责提供数据，不修改数据，要修改，请找mutations
        optCount: function(state) {
            return '当前最新的count值是：' + state.count
        }
        //getters中的方法，和组件中的过滤器比较类似，都没有修改原数据，只对原始数据进行包装提供给调用者。也和computed比较相似，只要state中的数据发生变化，如果getters引用了这个数据，就会触发getters重新求值
    }
})

var vm = new Vue({
    el:'#app',
    render: c => c(app),
    store:store //5.将store对象挂载到vm上,任何组件都能使用store来存取数据
})

````

总结：1 state中的数据不能直接修改，想要修改，必须通过mutations

2.如果组件想要从state获取数据，需要this.$store.state.xxx

3.如果组件要修改数据，必须使用mutations提供的方法，需要通过this.$sote.commit('方法名'，唯一的一个参数) 参数也可以省略

4.如果store中的state上的数据在对外提供的时候需要做一层包装，那么推荐使用getters,如果需要使用getters，则用this.$store.getters.xxx来使用



