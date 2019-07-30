#### 模板中的插入变量是如何渲染到DOM上的？

initMixin(Vue)->_init->`$options`->	`$mount()`当执行该挂载方法时DOM变化

#### 为什么可以通过this访问到data里面的数据？

initstate(vm)->initData()->proxy(vm,`_data`,key)代理函数
所以我们也可以同过this._data.dataName获取到数据
![图片描述](https://img.mukewang.com/5bb2a1a000017c3a07530292.png)

#### $mount的实现

$mount->处理e（编译，转化成render函数）->mountComponent()->updateComponent()->渲染Wather

#### vm._render的实现

_render->从vm.options拿到render->render.call(vm._renderProxy,vm.$createElement)->initProxy->hasHandler判断元素如果不在target上，则会报错warnNonPresent-> 返回vnode

#### 虚拟Dom

1. VNodeData定义在flow/vnode.js （创建虚拟DomTree）
2. create-element–>
3. 参数重载->
4. _createElement->
5. 对data校验（如果是响应式的 return create EmptyVnode()	(vnode.js)）->
6. 对children做normalizetionChildren(normalize-chiildren.js) 多维数组变一维数组->
7. 对tag进行判断（字符串还是组件）->
8. 创建Dom

------

1. _update定义在src/core/instance/lifecycle.js （**渲染**）

2. `vm.__patch__`

3. patch

4. createPatchFunction ( 内部定义了一系列的辅助方法，最终返回了一个 patch 方法，这个方法就赋值给了 vm._update 函数里调用的

    

   ```
   vm.__patch__
   ```

   )

    

   函数柯里化

   1. createElm （通过虚拟节点创建真实的 DOM 并插入到它的父节点中）
   2. createChildren
   3. invokeCreateHooks

### 组件化

#### createCompment

- createElement
- _createElement(对Tag判断)
- createComponent

![图片描述](https://img.mukewang.com/5bb46be100019af210240318.png)

#### patch

##### 整体流程

##### 重要属性

- activeInstance
- vm.$vnode
- vm._vnode

##### 嵌套组件插入顺序

### Vuex

![图片描述](https://img.mukewang.com/5c037ba200014f7409310591.png)

### Vue组件

- 巧用Vue标签`is`属性，解决模板标签出现bug问题
- 子组件定义data,必须是一个函数
- ref 操作dom
- 父组件通过**属性**向子组件传递数据

### 动画

#### transition

> 通过自动操纵transition中的元素的class实现

![图片描述](https://img.mukewang.com/5b8c964d0001551711770564.png)

#### 同时使用过渡和动画

- 通过设置type=“transition”（过渡）来设置根据过渡还是动画显示时长
- 通过appear实现页面初试动画

```
	<link rel="stylesheet" href="./animate.css">
	<script src="./vue.min.js"></script>
	<style>
	  .fade-enter,.fade-leave-to{
	  	opacity: 0;
	  }
	  .fade-enter-active,
	  .fade-leave-active{
	  	transition: opacity 3s;
	  }
	</style>
</head>
<body>
	<div id="app">
		<!-- type="transition" 放在transition里面指定指行的时间以animate或者transition为准-->
		<transition
		 :duration="{enter: 5000, leave: 10000}"
		 name="fade"
		 appear
		 enter-active-class="animated swing fade-enter-active"
		 leave-active-class="animated shake fade-leave-active"
		 appear-active-class="animated swing"
		>
		<div v-show="show">hello meijing</div>
		</transition>
		<button @click="handleClick">切换</button>
	</div>

	<script>
	

	var vm = new Vue({
		el: '#app',
		data: {
			show: true
		},
		methods: {
			handleClick: function(){
				this.show = !this.show
			}
		}
	})
	</script>
```

#### Js 动画与 Velocity.js 的结合

```
	<link rel="stylesheet" href="./animate.css">
	<script src="./vue.min.js"></script>
	<script src="./velocity.min.js"></script>
</head>
<body>
	<div id="app">
		<transition
		 name="fade"
		 @before-enter="handleBeforeEnter"
		 @enter="handleEnter"
		 @after-enter="handleAfterEnter"
		>
		<!-- <transition
		 name="fade"
		 @before-leave="handleBeforeEnter"
		 @leave="handleEnter"
		 @after-leave="handleAfterEnter"
		> -->
		<div v-show="show">hello meijing</div>
		</transition>
		<button @click="handleClick">切换</button>
	</div>

	<script>
	

	var vm = new Vue({
		el: '#app',
		data: {
			show: true
		},
		methods: {
			handleClick: function(){
				this.show = !this.show
			},
			handleBeforeEnter: function(el){
				el.style.opacity = 0;
			},
			handleEnter: function(el, done){
				Velocity(el, {opacity: 1}, {duration: 1000, complete: done})
			},
			handleAfterEnter: function(el){
				console.log("动画结束")
			}
		}
	})
	</script>
```

### Router

- hash路由并不适合SEO
  - mode: ‘history’
- base 基路径
- router路由样式
  - linkActiveClass 部分匹配
  - linkExactActiveClass 完全匹配
- historyApiFallback 路径映射关系
  - 要包含webpack里的publicPath
- scrollBehavior 记录滚动行为
- parseQuery，stringifyQuery 参数
- fallback 如果页面不支持history路由，自动切换Hash方式
- this.$route获取当前的路由信息，但是并不是所有的信息都有，所以可以使用meta属性
- 同一组件内，不同路由有不同router-view可以使用components替换component，然后给router-view来解决
- 路由守卫（导航狗子） 可以用来验证参数
  - beforeEach
  - beforeResolve
  - afterEach 跳转之后
- 路由配置狗子
  - beforeEnter
- 在组件定义狗子
  - beforeRouteEnter
  - beforeRouteUpdate
  - beforeRouteLeave (大表单确认提醒，安全性)
  - 在next之前拿不到this
  - `next(vm => {console.log(vm.id)})`
- 异步加载，在进入路由中import引入组件
  - 需要插件 babel-plugin-syntax-dynamic-import

### Vuex

- 注意目录结构的划分
- mapState
  - `...mapState({*** : (state) => state.**})`
- mapGetters
  - `...mapState(['**'])`
  - mapActions
  - mapMutation
- mutation只有两个参数，修改多个数据要用对象，使用this.$store.commit(‘方法名’,{})来触发
- action和mutation一样，不过主要用来做异步修改方法。使用this.$store.dispatch(‘方法名’,{参数})来触发
- 模块化 modules
  - namespaced
- 异步加载模块
  - registerModule
  - unregisterModule
- 热加载
  -store.watch((state) => {},()=>{})
  当第一个方法返回值有变化的时候才会调用第二个方法
- store.subscribe(mutation,state)=>{}) 拿到所有mutation的变化，每次变化调用回调函数
- subscribeAction
- plugins 在vue初始化的时候定义

### SSR

![图片描述](https://img.mukewang.com/5cc9a486000114dc09290563.png)



