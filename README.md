## vue day01 ##

### vue 简介 ###

1. 简单易用, 上手快
2. 热度最高

#### 对比jQuery的区别 ####

- 以前使用jQuery结合模板引擎(art-template), 每一次进行页面渲染都需要将整个节点渲染, 而如果使用vue进行渲染, 只会将改变的内容渲染一次, 大大提高了用户体验!!!

- jQuery只是一个JavaScript库, 对项目的侵入性较小, 开发过程中可以很轻易的切换成其他库或框架来继续开发

- jQuery一直以来最强大的功能就是选择器和便捷的DOM操作, 处理了浏览器兼容性问题

- Vue等MVVM思想设计的框架, 都有数据绑定的特点, 当Model层的数据更新, 会通过VM层自动同步刷新到View层, 从此程序员只需要关注业务逻辑, 而不需要操作DOM

- Vue和Angular都提供了双向数据绑定, 除了可以从Model同步到View层, 还可以在View层数据发生变化时反向同步到Model层

### Vue的基本使用 ###

1. 引入vue.js文件

		<script src="./lib/vue.js"></script>

2. 引入vue.js后, 全局作用域中就有一个 `Vue` 的构造函数, 通过此构造函数创建`Vue`实例

	新创建出来的实例对象, 就是MVVM中的VM层

		let vm = new Vue()

3. 在VM实例创建时, 可以传入一个配置对象

	html结构:

		...
		<body>
			<div id="app"></div>
		</body>
		...

	js代码:

		let vm = new Vue({
			el: '#app', // vm实例托管的区域
			data: {
				test: 'Hello Frontend 23!!!'
			}
		})

4. 当创建了VM实例后, 在VM实例托管的范围内, 就可以使用模板语法引用data中的数据(只要data中的数据修改了, 页面会自动同步)

	html结构:

		<div id="app">
			{{ test }}
		</div>

### VSCode编写Vue的插件 ###

1. Vetur
2. Vue 2 Snippets

### Vue的指令 ###

> 在Vue中, 只要是以 `v-` 开头的属性, 都是指令

#### v-cloak ####

为了解决Vue模板渲染时的闪烁问题

闪烁:

> 当标签内使用小胡子`{{}}`语法进行模板渲染时, 由于Vue文件在HTML结构之后才引入, 会导致`{{}}`模板代码在HTML中短暂停留, 然后再渲染成真正的数据, 用户体验不够好

使用`v-cloak`指令结合CSS将元素默认隐藏起来, 当数据变化时进行渲染, 会自动显示, 无需手动干预

其他解决方案: 不要在HTML后面引入vue文件, 应该在head区域引入

#### 插值表达式 ####

同`art-template`, 将data中的数据渲染到`{{}}`中, `{{}}` 内部写的是表达式(可以写js代码的地方)

	{{ test }}

**注意: 在Vue中插值表达式只能用于元素内部文本的渲染, 不能用于属性渲染**

#### v-text ####

将表达式的结果渲染到标签的 `innerText` 属性上

后面的内容会拼接到test数据后面

	<p>{{ test }}后面的内容</p>

后面的内容会被**覆盖**, 等同于 `p.innerText = test`

	<p v-text="test">后面的内容</p>

#### v-html ####

用法同`v-text`, 区别在于 `v-html` 可以解析带HTML标签的结构, `v-text` 会将HTML解析成纯文本进行渲染

	<p v-html="test"></p>

#### v-bind ####

Vue中提供用于属性绑定的指令

	<input v-bind:value="val + 'aaa'" type="text">

由于v-bind太长了, 所以Vue提供了简写 `:`

v-bind支持非标准属性的绑定

	<input :aaa="val" :value="val + 'aaa'" type="text">

### v-on ###

Vue中提供用于事件绑定的指令

	<input id="inp" :value="msg" v-on:keyup="keydownHandler" type="text">

data与methods的代码:

	data: {
		msg: '你好啊!'
	},
	methods: {
		keydownHandler() {
		  // console.log('我被调用了!')
		  // console.log(document.querySelector('#inp').value)
		  this.msg = document.querySelector('#inp').value
		}
	}

**注意: 由于还没有学习到双向数据绑定的指令, 所以暂时操作了一下DOM, 后期实现该功能不需要操作DOM**

v-on的简写: `@`

	<input id="inp" :value="msg" @keyup="keydownHandler" type="text">

### 事件修饰符 ###

Vue中为了方便开发人员处理一些简单的逻辑, 例如阻止冒泡或者阻止浏览器默认行为等操作

特意提供了一系列事件修饰符完成:

1. `.stop`
2. `.prevent`
3. `.capture`
4. `.self`
5. `.once`
6. `.passive`

		<!-- 阻止单击事件继续传播 -->
		<a v-on:click.stop="doThis"></a>
		
		<!-- 提交事件不再重载页面 -->
		<form v-on:submit.prevent="onSubmit"></form>
		
		<!-- 修饰符可以串联 -->
		<a v-on:click.stop.prevent="doThat"></a>
		
		<!-- 只有修饰符 -->
		<form v-on:submit.prevent></form>
		
		<!-- 添加事件监听器时使用事件捕获模式 -->
		<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
		<div v-on:click.capture="doThis">...</div>
		
		<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
		<!-- 即事件不是从内部元素触发的 -->
		<div v-on:click.self="doThat">...</div>

### github提交代码时报错解决方案 ###

报错信息:

> permisstion denied

> access denied

1. 检查公钥是否配置正确

2. 检查当前仓库关联的远端仓库是否使用的 `ssh` 协议

	查看当前仓库的关联详细信息

		git remote --verbose

	将本地的仓库关联到远端的 git@github.com:TianchengLee/test.git

		git remote add origin git@github.com:TianchengLee/test.git

	如果之前关联的是https, 非ssh, 可以删除
		
	删除关联

		git remote remove origin

	重新添加

		git remote add origin git@github.com:TianchengLee/test.git

## vue day02 ##

### 指令 ###

#### v-model ####

Vue提供的双向数据绑定的指令, 在Vue中也只有唯一这一个属性可以提供双向数据绑定

	<!-- v-bind只能实现单向绑定 -->
    <!-- <input :aaa="msg" :value="msg" type="text"> -->

    <!-- 为什么v-model不需要手动指定要绑定的属性呢??? -->
    <!-- 由于用户与input交互时, 修改的都是value属性的值, 所以v-model只能绑定value属性 -->
    <!-- v-model 除了可以从M层绑定到V层, 还可以在用户修改value属性时 自动同步回M层 -->
    <input v-model="msg" type="text">


#### v-for ####

在Vue中可以迭代的类型有:

	数组

	对象

	数字

1. 迭代数组

	数组中有多少个元素, 就会循环创建多少个p

	item表示数组中的每一项

		<p v-for="item in list">{{ item }}</p>

	index表示数组中的每一项对应的索引

		<p v-for="(item, index) in list">{{ item }}</p>

2. 迭代对象

	对象中有多少个属性, 就会循环创建多少个p

		<p v-for="(value, key) in obj">{{ key }} --- {{ value }}</p>

3. 迭代数字

	迭代的数字有多大, 就会循环创建多少个p

	**注意: v-for迭代数字时, num从1开始, 到10结束**

		<p v-for="num in 10">{{ num }}</p>

注意事项: v-for中的key属性, 在v-for循环渲染列表后, 如果每个单项中都有状态类型的表单元素, 例如: checkbox, 此时数据顺序发生变化后, 默认不会记录每个单项的状态, 会导致checkbox勾选状态出现混乱

#### v-for的原理 ####

> 当 Vue.js 用 `v-for` 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。

key的作用就是将当前的数据与当前对应的DOM元素进行绑定, 以后如果数据顺序发生变化, Vue会在内部重新排序 然后渲染

如果实在无法理解原理, 那就记住 以后只要用v-for, 都使用 `:key`, 给key绑定一个唯一标识, 可以是 `number/string`

#### v-if和v-show ####

v-if和v-show都是用于控制元素的显示隐藏的

区别在于: v-if有较高的切换性能消耗, v-show有较高的初始渲染消耗

因为, v-if如果表达式是false, 元素压根不会创建, 而v-show如果表达式结果是false, 元素也会创建只是把display设为了none

	<h3 v-if="flag">这是用v-if控制的元素</h3>
    <h3 v-show="flag">这是用v-show控制的元素</h3>

### 样式操作 ###

以前在DOM阶段操作样式的两种方法:

	dom.style.backgroundColor // 行内样式

	dom.className // 设置类样式名, 结合css选择器完成

在Vue中同样也是两种方法操作样式:

#### 在Vue中动态绑定类样式 ####

不用属性绑定, 设置固定的类样式

	<p class="red active"></p>

如果需要动态修改类样式, 则需要使用属性绑定

属性绑定类样式可以为数组或对象

1. 绑定数组

	现在虽然是使用属性绑定了, 但是依然是写死了, 没有实现的动态的切换

		<p :class="['red', 'active']"></p>

	动态切换有两种方式, 可以使用三元表达式来决定是否要动态添加数组元素

		<p :class="['red', flag ? 'active' : '']"></p>

	推荐以下方法:

		<p :class="['red', { active: flag }]"></p>

2. 绑定对象

	当flag1为true时应用red类样式, 否则不应用

	当flag2位true时应用active类样式, 否则不应用

		<p :class="{ red: flag1, active: flag2 }"></p>

**总结: 当类样式较多, 而需要动态切换的类名只有一个时, 推荐使用绑定数组的方式来实现动态切换, 反之, 当类样式较少, 而都需要动态切换类名时, 使用对象较为方便**

#### 操作行内样式 ####

给 `style` 属性进行 v-bind 绑定

	<p :style="{ 'background-color': 'pink', color: 'black' }"></p>

通过绑定数组, 可以同时绑定多个样式对象

	<p :style="[ 样式对象1, 样式对象2 ]"></p>

### 过滤器 ###

过滤器的基本使用方法:

1. 先定义好全局过滤器

		// 定义全局过滤器
	    // 参数1: 过滤器名称
	    // 参数2: 回调函数
	    //   回调函数中第一个参数是管道符左边的数据, 第二个参数起都是过滤器调用时传入的参数
	    Vue.filter('msgFilter', function(data, str) {
	      return data.replace('Helloworld', str)
	    })

2. 在插值表达式或v-bind中使用, v-text等其他指令中无法使用

	    <p>{{ msg | msgFilter('你好世界') }}</p>
	
	    <input type="text" :value="msg | msgFilter('你好世界')">

注意: 过滤器可以串联使用, 例如

	<p>{{ msg | msgFilter('你好世界') | test }}</p>


## vue day03 ##

### 私有过滤器 ###

不同于全局过滤器, 私有过滤器只能在当前vm实例内部使用, 定义方式也是在当前vm实例的配置对象中加入一个filters的节点, 与methods和data等节点同级:

	filters: {
        // 方法名就是过滤器名 
        // 方法就是过滤器
        // 就近原则 如果当前vm实例的私有过滤器和全局过滤器同名了 就会优先使用私有过滤器
        msgFormat() {
          
        }
      }

### 按键修饰符 ###

当用户输入完数据后, 每次都需要点击添加按钮才可以将数据录入表中, 用户体验不佳, 最好能够当用户输入完数据后, 直接按回车立即录入表中

Vue提供了按键修饰符来解决这个问题:

	<input @keyup.enter="add" type="text" class="form-control" v-model="name">

`@keyup.enter` 的意思是给input绑定键盘抬起事件, 并且只有在`回车键`抬起时才会触发

也就是当用户抬起回车时就会调用add方法 进行添加的逻辑操作

所有**键盘事件**都有按键修饰符, 本质上其实是点的keyCode, 只不过Vue为了方便大家记忆, 内置了一些别名:

	.enter
	.tab
	.delete (捕获“删除”和“退格”键)
	.esc
	.space
	.up
	.down
	.left
	.right

如果需要自定义别名, 可以使用全局的`config.keyCodes`对象来添加:

	// 可以使用 `v-on:keyup.f1`
	Vue.config.keyCodes.f1 = 112

### 自定义指令 (了解内容) ###

在Vue内部提供了很多内置指令: v-text, v-html, v-if, v-show, v-model ... 等等, 一切以v-开头的都是指令

除了内部提供的这些指令外, 开发者还可以自定义指令

因为Vue的作者考虑到, 有些情况还是需要操作DOM元素的, 通过指令可以对一些DOM元素操作进行封装

1. 定义全局指令

		// 定义全局指令
	    // 参数1: 指令名称, 不需要 v-
	    // 参数2: 对象, 对象中有可以有5个函数 bind, inserted, update, componentUpdated, unbind
	    //     5个函数就是所谓的钩子函数, 就是当指令应用到标签身上整个过程, 每个阶段所调用的函数
	    Vue.directive('focus', {
	      // 只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
	      bind(el) {
	        // console.log(el)
	        // console.log('我被绑定了')
	        // el.focus()
	      },
	      // 被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
	      inserted(el) {
	        console.log(el)
	        // console.log('我insert到父节点了')
	        el.focus()
	      },
	      // 所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前
	      update() {}
	    })

2. 使用指令

		<input v-focus type="text" class="form-control" v-model="keywords">

**注意: 定义指令时不需要加 `v-` 使用指令时必须要加 `v-`**


### 定义私有指令 ###

同私有过滤器, 在vm配置对象中, 和methods以及data同级的位置, 加入一个`directives`的属性:

	
	directives: {
		focus: {
		  bind() {
		
		  },
		  inserted() {
		    
		  }
		}
	}

### 生命周期函数 ###

生命周期函数是指, Vue实例创建的过程中, 从出生到死亡每个阶段所执行的函数

一共有8个:

	beforeCreate: 实例完全创建之前, 此时data和methods等数据都没有初始化, 不能使用

	created: 实例已经创建完毕了, 此时data和methods等数据都可以使用了, 实例对象也可以访问

	beforeMount: 模板在内存中编译完成了, 但是还未渲染到页面上

	mounted: 编译好的模板完全渲染到页面, 用户最终看到的样子, 此时DOM元素也是最新的, 如果想操作DOM元素, 最好在这个生命周期函数中进行

	beforeUpdate: 当data中数据改变时会触发, 此时页面上的数据并没有重新渲染, 只是data中的数据更新了

	updated: 当data中数据改变后, 并将页面上的数据也更新完成后会触发, 此时data中的数据和页面上的数据是同步的

	beforeDestroy: 当实例进入销毁阶段时执行的钩子函数, 此时Vue实例中的data/methods/filters/directives等都还可以使用

	destroyed: 实例上的所有成员已经完全销毁, 无法使用了


### 域名更新 ###

课程中所有接口的域名都替换为: `vue.lovegf.cn:8899`

## vue day04 ##

### 动画 ###

在Vue中使用动画, 一般是用于帮助用户理解应用程序而存在, 并非为了做出炫酷的特效

核心目标在于掌握基本动画的实现即可

在Vue中实现动画, 需要经历6个类名(只需要掌握四个), 如图所示:

![动画过渡类名图解](https://cn.vuejs.org/images/transition.png)

根据动画图示, 将四个类名分为两组:

动画执行之前和结束后元素的状态:

	v-enter
	v-leave-to

执行过渡动画的样式:

	v-enter-active
	v-leave-active

1. v-enter：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。

2. v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。

3. v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。

4. v-leave-to: 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。


案例:

1. 先将要执行动画的元素使用 `<transition></transition>` 标签包裹

		<input type="button" value="toggle" @click="flag=!flag">
	    <!-- 需求： 点击按钮，让 h3 显示，再点击，让 h3 隐藏 -->
	    <transition>
	      <h3 v-show="flag">这是一个H3</h3>
	    </transition>

2. 给四个过渡类名加样式, 完成动画

		.v-enter,
		.v-leave-to {
			opacity: 0;
			transform: translateY(200px);
		}
		
		.v-enter-active,
		.v-leave-active {
			transition: all .8s ease-in-out;
		}

点击按钮即可进行动画过渡切换显示隐藏了!

整个过程不需要操作DOM元素, 只靠标签+CSS类样式即可完成, 非常方便!

如果项目中有多个地方需要使用动画, 而默认情况下, 动画的类名都是 `v-` 开头, 所以需要自定义前缀

通过给`transition`标签添加`name`属性, 可以自定义`v-`的前缀

	<transition name="my">
      <h6 v-if="flag2">这是一个H6</h6>
    </transition>

style样式:

	.my-enter,
    .my-leave-to {
      opacity: 0;
      transform: translateY(70px);
    }

    .my-enter-active,
    .my-leave-active{
      transition: all 0.8s ease;
    }

### 列表动画 ###

不同于单元素动画, 需要使用`transition-group`标签进行包裹

1. 不同于 `<transition>`，它会以一个真实元素呈现：默认为一个 `<span>`。你也可以通过 `tag` 特性更换为其他元素。

2. 过渡模式不可用，因为我们不再相互切换特有的元素。

3. 内部元素 总是需要 提供唯一的 `key` 属性值。

4. 通过`appear`属性可以让列表加载时执行动画


案例:

	<transition-group appear tag="ul">
      <li v-for="(item, i) in list" :key="item.id" @click="del(i)">
        {{item.id}} --- {{item.name}}
      </li>
    </transition-group>

style:

	.v-enter,
    .v-leave-to {
      opacity: 0;
      transform: translateY(80px);
    }

    .v-enter-active,
    .v-leave-active,
    .v-move {
      transition: all 6s ease;
    }

    .v-leave-active {
      position: absolute;
    }

如果想让删除时的动画更加平滑, 删除一个元素后, 其他元素一起执行动画, 可以使用Vue内部封装FLIP动画队列后的 `v-move` 类样式来设置过渡效果

**注意: 当执行删除动画时, 元素依然占位置, 所有需要在v-leave-active中设置绝对定位, 让元素脱离文档流实现完美的动画效果!!!**

### 组件化开发 ###

组件化: 对视图层的逻辑划分, 提高了视图层的复用性

模块化: 对Controller的划分, 提高了业务代码的复用性


### 组件的创建 ###

注意事项:

- 组件必须要注册完成后, 才可以使用!

- 组件名如果注册时用了驼峰命名, 使用时需要改成`-`连接

- 组件的模板`template`中有且只能有一个根节点

创建组件的方法:

1. Vue.extend方法(不推荐, 太麻烦了)

		// 创建组件, 第一种方式 (不推荐使用)
	    // let com = Vue.extend({
	    //   template: '<h1>com组件的模板代码</h1>'
	    // })
	
	    // 注册组件  不论通过哪一种方式创建出来的组件, 注册都是如此
	    // 参数1: 组件名, 如果注册时用了驼峰命名, 使用时需要改成-连接
	    // 参数2: 组件对象
	    // Vue.component('com', com)
	    // Vue.component('com', Vue.extend({
	    //   template: '<h1>com组件的模板代码</h1>'
	    // }))

2. 直接使用带有`template`属性的对象

	    // 创建组件的第二种方式  直接使用对象即可
	    Vue.component('com', {
	      template: '<h1>com组件的模板代码</h1>'
	    })


3. 在app区域外定义好模板后, 直接通过template属性引用

		  <template id="tmp">
		    <div>
		      <p>你是一个p</p>
		      <h1>你是一个h1</h1>
		    </div>
		  </template>

		// 创建组件的第三种方式  同样是对象, 但是template属性指向一个模板ID即可
	    // 需要在app区域外定义好模板, 这种做法有智能提示, 非常方便!
	    Vue.component('com', {
	      template: '#tmp'
	    })

    
4. computed
类型：{ [key: string]: Function | { get: Function, set: Function } }

详细：

计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。

注意如果你为一个计算属性使用了箭头函数，则 this 不会指向这个组件的实例，不过你仍然可以将其实例作为函数的第一个参数来访问。

computed: {
  aDouble: vm => vm.a * 2
}
计算属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。注意，如果某个依赖 (比如非响应式属性) 在该实例范畴之外，则计算属性是不会被更新的。

示例：
// 

		var vm = new Vue({
			data: { a: 1 },
			computed: {
				// 仅读取
				aDouble: function () {
					return this.a * 2
				},
				// 读取和设置
				aPlus: {
					get: function () {
						return this.a + 1
					},
					set: function (v) {
						this.a = v - 1
					}
				}
			}
		})
		vm.aPlus   // => 2
		vm.aPlus = 3
		vm.a       // => 2
		vm.aDouble // => 4

		
watch类型：{ [key: string]: string | Function | Object | Array }

详细：

一个对象，键是需要观察的表达式，值是对应回调函数。
值也可以是方法名，或者包含选项的对象。
Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性
:
## vue day05 ##

### 组件的data ###

组件内部的成员, 基本上和vm实例类似, 有data/methods/filters/directives/八个生命周期函数等 都是一样的

但是组件中的data和vm实例中的data是有区别的

组件中的data必须是一个 函数 , 并且内部必须返回一个对象

原因:

> 为了避免多个组件实例内部共享同一个对象

### 组件切换动画 ###

和单元素动画一样, 不过由于是组件的切换, 所以还有一个mode属性, 用于设置切换时的模式, 例如先出后进或是先进后出

	<transition mode="out-in">
      <component :is="comName"></component>
    </transition>

### 父组件向子组件传值 ###

核心原理: 属性绑定 v-bind 指令

1. 在父组件中定义好数据后, 在子组件身上绑定数据(v-bind)

		<login :pmsg="msg" v-if="flag"></login>

2. 在子组件中通过 `props` 定义一下`pmsg`

		Vue.component('login', {
	      props: ['pmsg'],
	      template: '<h3>登录组件----{{ pmsg }}</h3>'
	    })

**注意: props中定义的数据是单向下行绑定的**

特点就是为了避免, 子组件中修改数据导致父组件或其他子组件数据同时篡改, 所以Vue设计的时候就考虑的**单向下行**的特点, 仅允许父组件修改数据, 修改数据后会同步到所有引用了该数据的子组件中, 而不允许子组件反向同步(子组件中修改props的数据其他组件都不会受到影响)

### 子组件向父组件传值 ###

核心原理: 自定义事件 v-on 指令绑定, 将函数传递给子组件, 子组件找个合适的时机触发事件并携带数据

1. 先给子组件绑定事件, `gotIt` 函数需要在父组件的 `methods` 中定义好

		// foo = gotIt
		<login @foo="gotIt" :pmsg="msg" v-if="flag"></login>

2. 在子组件内部, 某个时机调用`$emit()`方法触发父组件绑定的foo事件

	$emit()方法有多个参数

	参数1: 要触发的事件名

	参数2及以后: 事件处理函数的参数

		this.$emit('foo', this.username)

### ref(了解内容) ###

ref : reference 引用

Reference Error 引用错误

Vue在设计之初也考虑到了, 很难完全避免DOM操作, 只有在一些基础的业务处理上可以省去DOM操作, 但是在一些特定场景下还是需要用到DOM操作, 所以保留了一个`ref`的设计, 来获取DOM元素

可以通过给元素添加 `ref` 属性, 将来就可以在vm或组件实例身上通过 `$refs` 属性获取到所有添加了`ref`属性的DOM对象

### 前端路由 ###

SPA (Single Page Application)单页面应用程序的核心就是源于`#` 锚点的切换, 实现的组件切换

### 路由的基本使用 ###

1. 引入路由的包

		<!-- 1. 安装 vue-router 路由模块 -->
	  	<script src="./lib/vue-router-3.0.1.js"></script>

2. 创建路由对象

		// 组件的模板对象
	    var login = {
	      template: '<h1>登录组件</h1>'
	    }
	
	    var register = {
	      template: '<h1>注册组件</h1>'
	    }
	
	    // 注册组件  Vue.component这种方式
	    // 目的是为了在模板中使用 <> 方式使用组件
	    // Vue.component('login', login)
	
	    // 2. 创建路由对象
	    let router = new VueRouter({
	      routes: [
	        // { path: '/', component: login },// 虽然可以让首页显示login组件, 但是不推荐
	        { path: '/', redirect: '/login' }, // 路由重定向, 当用户访问 / 路径时 自动跳转到 /login 路径
	        { path: '/login', component: login }, // 一个路由匹配规则
	        { path: '/register', component: register },
	      ],
	      // linkActiveClass: 'mui-active'
	    })

3. 将路由对象挂载到VM实例的配置对象中

	    // 创建 Vue 实例，得到 ViewModel
	    var vm = new Vue({
	      el: '#app',
	      data: {},
	      methods: {},
	      router // 3. 将创建的路由对象挂载到vm实例中
	    });

4. 在app区域内放置一个`router-view`组件占位

		<router-view></router-view>

5. 通过地址切换即可显示不同路由, 也可以使用`ruoter-link` 组件来渲染`a`标签

	tag属性用于指定渲染的标签, 默认渲染a标签
	to属性用于指定跳转的路由, 不需要加`#`

		<router-link tag="span" to="/login">登录</router-link>
    	<router-link to="/register">注册</router-link>

6. 使用`router-link`后会自动给当前的路由对应的a标签加上一个默认的类名: `router-link-active` 用于做当前路由高亮

7. 通过给`router-view`包裹`transition`组件来实现组件的切换动画, 同单元素动画的做法
		
## vue day06 ##

### 路由传参 ###

### watch ###

监视数据变化

使用方法:

在于data/methods同级的地方, 添加一个watch属性, 值为对象

在对象中添加方法, 方法名就是要监视的数据(该数据必须存在, 可以是data中的数据, 也可是是vm实例上的数据例如:$route)

	watch: {
		$route(to, from) {
	      // console.log(to, from)
	      // console.log(to.path)
	      // console.log(from.path)
	      // console.log(newVal + ' --- ' + oldVal)
	      // 客户端版本信息  BOM
	      // console.log(window.navigator.userAgent)
	      if (to.path === '/login') {
	        console.log('欢迎进入登录页面')
	      } else if (to.path === '/register') {
	        console.log('欢迎进入注册页面')
	      }
	    }
	}

### computed ###

注意事项:

1. 计算属性，在引用的时候，一定不要加 () 去调用，直接把它 当作 普通 属性去使用就好了；

2. 只要 计算属性，这个 function 内部，所用到的 任何 data 中的数据发送了变化，就会 立即重新计算 这个 计算属性的值

3. 计算属性的求值结果，会被缓存起来，方便下次直接使用； 如果 计算属性方法中，所以来的任何数据，都没有发生过变化，则，不会重新对 计算属性求值；

4. 一旦在data中定义了属性, 就不能在computed中定义同名属性了 会冲突

5. computed计算属性默认是只读的

使用方法:

在data或methods同级的地方加入一个computed属性

	computed: { // 在 computed 中，可以定义一些 属性，这些属性，叫做 【计算属性】， 计算属性的，本质，就是 一个方法，只不过，我们在使用 这些计算属性的时候，是把 它们的 名称，直接当作 属性来使用的；并不会把 计算属性，当作方法去调用；
        'fullname': function () {
          console.log('ok')
          return this.firstname + '-' + this.middlename + '-' + this.lastname
        }
      }

## vue day07 ##

### 简介 ###

四个核心概念:

1. 入口
2. 输出
3. loaders (加载器)
4. plugins (插件)

### webpack基本使用 ###

webpack官方有两种用法:

1. 终端使用(需要全局安装webpack)
2. Node.js的配置文件(可以全局安装也可以在项目中安装)

以下是终端使用的方式, 学习阶段可以尝试一下:

1. 全局安装webpack(不推荐)

		npm i webpack webpack-cli -g

2. 调用webpack指令打包编译js文件

	在webpack4中必须通过`-o`手动指定output

	webpack后第一个参数是入口文件(要打包的文件)

	-o 表示output, 即输出文件

	-d 表示development, 即开发模式, 不压缩混淆

	-p 表示production, 即生产模式, 开启压缩混淆

	如果不指定-d或-p, 则默认使用 -p 开启压缩混淆并在控制台给出警告提示

		webpack ./src/main.js -o ./dist/bundle.js -d

**注意: 在控制台中使用webpack不是其官方推荐的方法, 所以可以不需要全局安装webpack, 学习阶段使用一下即可**

以下是配置文件的使用方式:

此方式不需要全局安装webpack, 但是需要在每个项目中的开发依赖安装webpack和webpack-cli

1. 安装webpack和webpack-cli

		npm i webpack webpack-cli -D

2. 在项目根目录下新建`webpack.config.js`的配置文件, 进行基本配置(入口和输出)

		const path = require('path');

		module.exports = {
		  entry: './src/main.js', // entry是指定打包文件的入口, 可以使用相对路径
		  output: {
		    path: path.join(__dirname, 'dist'), // output是指输出的目录, 必须是绝对路径
		    filename: 'bundle.js'
		  },
		  mode: 'development'
		}

3. 在package.json中的scripts节点下, 可以编写一些项目中用的脚本, 在这个地方可以执行项目依赖的指令, 不需要全局安装, 只需要本地安装即可
		
		"scripts": {
			"test": "echo \"Error: no test specified\" && exit 1",
			"build": "webpack"
		},

4. 当配置好scripts之后, 就可以运行`npm run build`进行项目打包了

		npm run build

### webpack-dev-server的使用 ###

以下简称devServer

出现的目的是因为每次修改了文件后, 都需要手动运行`npm run build`或者全局执行`webpack`指令重新编译打包

在开发阶段修改代码, 是非常频繁的一个操作, 如果每次修改完代码不能用最高效的方式查看结果, 那么开发起来体验非常不好

所以devServer出现的目的就是为了解决上述问题的, 当开发者修改完代码后自动编译打包, 刷新浏览器, 提高开发效率!!!

1. 安装`webpack-dev-server`包在项目的开发依赖

		npm i webpack-dev-server -D

	在安装devServer时, 可能会出现如下警告:

		npm WARN webpack-dev-server@3.1.14 requires a peer of webpack@^4.0.0 but none is installed. You must install peer dependencies yourself.
		npm WARN webpack-dev-middleware@3.4.0 requires a peer of webpack@^4.0.0 but none is installed. You must install peer dependencies
		yourself.

	**注意: 如果在项目的开发依赖中没有安装 `webpack@^4.0.0` 就会产生以上警告, 发现了该警告一定要注意安装 `webpack`**

		npm i webpack webpack-cli -D

	如果开始就没有安装webpack在开发依赖, 则可以一次性一起安装

		npm i webpack-dev-server webpack webpack-cli -D

2. 在`package.json`的`scripts`节点下新建一个脚本

		"scripts": {
		    "test": "echo \"Error: no test specified\" && exit 1",
		    "dev": "webpack-dev-server",
		    "build": "webpack"
		  },

3. 可以直接在终端运行`npm run dev`开启devServer

		npm run dev

devServer的原理其实是, 在运行的第一次时将入口文件打包编译, 把最终编译的结果放到内存中, 挂载到服务器的根目录下与输出文件名同名的文件, 将来只要代码改变, devServer会自动把结果编译到内存中的输出文件, 同时自动刷新浏览器

#### devServer的参数 ####

默认开启devServer不会自动打开浏览器, 而且采用的是8080端口, 默认devServer托管的是项目根目录, 而index.html在src目录, 每次修改代码后都会重新编译生成完整的bundle.js

以上问题都可以通过指定不同的参数来解决

1. 自动打开浏览器

		--open

2. 设置服务器端口

		--port <端口号>
		--port 3000

3. 设置默认托管的目录

		--contentBase <路径>
		--contentBase ./src

4. 开启热替换模块

		--hot

只需要在scripts中的脚本后加上以上参数即可:

	"scripts": {
		"test": "echo \"Error: no test specified\" && exit 1",
		"dev": "webpack-dev-server --port 3000 --open --contentBase ./src --hot",
		"build": "webpack"
	},

设置devServer参数的第二种方式

修改配置文件:

1. 导入webpack

		// 热模块替换的插件  HMR  在webpack中内置了
		const webpack = require('webpack')

2. 配置devServer节点

		devServer: {
		    contentBase: "./src", // 托管的根目录
		    hot: true, // 我要开启或关闭HMR
		    open: true, // 自动打开浏览器
		    port: 3000 // 设置devServer的端口号
		  },

3. 如果想开启HMR(热模块替换) 还需要安装一个插件
	
		  plugins: [
		    // 装了插件表示当前项目有资格开启HMR
		    new webpack.HotModuleReplacementPlugin()
		  ],

在webpack的配置中, 装插件的方式都一样, 在plugins节点中创建插件对象即可

### html-webpack-plugin插件 ###

目前bundle.js已经托管到内存中了, 但是index.html还是物理磁盘的文件, 而且每次还需要通过contentBase来指定托管的根目录, 程序员还需要操心bundle.js的路径问题

html-webpack-plugin插件主要功能有如下两点:

1. 在内存中会根据指定的模板生成一份index.html直接托管在服务器的根目录中
2. 并且还会自动追加一个bundle.js的引用, 在index.html中

所以程序员不需要再操心bundle.js的引用问题了, 写HTML的时候不需要引入bundle.js同样也可以实现js的效果

使用方法:

1. 装包

		npm i html-webpack-plugin -D

2. 在webpack.config.js中引入

		const HtmlWebpackPlugin = require('html-webpack-plugin')

3. 安装插件

		plugins: [
		    // 装了插件表示当前项目有资格开启HMR
		    new webpack.HotModuleReplacementPlugin(),
		    // 安装插件
		    // 如果不传入任何配置选项, 默认也会创建一个index.html托管在服务器根路径
		    // 只不过这个HTML是空的, title默认是webpack app
		    new HtmlWebpackPlugin({
		      // title: '传智大法好!!!', // 如果模板中有title, 会覆盖这里的配置
		      template: './src/index.html'
		    })
		  ],

注意: 如果需要了解更详细的html-webpack-plugin的使用, 请查阅官方文档, 在github或npm都可以搜索到官方仓库

### loader的使用 ###

在webpack中默认是只能打包js文件的, 如果需要引入css或其他文件, 则会报错

而且为了减少css的二次请求, webpack允许在main.js中直接import css文件, 将css打包到bundle.js中

但是由于webpack默认不能打包css文件, 所以需要安装第三方loader来加载css文件

#### css-loader的使用 ####

1. 安装css-loader和style-loader

		npm i style-loader css-loader -D

2. 在webpack.config.js文件中进行配置

		module: {
		    rules: [
		      {
		        test: /\.css$/,
		        // use: [
		        //   { loader: 'style-loader' },
		        //   {
		        //     loader: 'css-loader',
		        //     options: {
		        //       modules: true
		        //     }
		        //   }
		        // ]
		        // css-loader 用于解析css文件
		        // style-loader 用于将css代码 使用js动态的插入到html中, 减少二次请求
		        // use使用loader时  顺序是固定的从右到左的加载
		        use: ['style-loader', 'css-loader']
		      }
		    ]
		  },

#### less-loader的使用 ####

1. 装包

	需要less-loader和less两个包才可以

		npm i less-loader less -D

2. 配置

		module: {
		    rules: [
		      {
		        test: /\.css$/,
		        // use: [
		        //   { loader: 'style-loader' },
		        //   {
		        //     loader: 'css-loader',
		        //     options: {
		        //       modules: true
		        //     }
		        //   }
		        // ]
		        // css-loader 用于解析css文件
		        // style-loader 用于将css代码 使用js动态的插入到html中, 减少二次请求
		        // use使用loader时  顺序是固定的从右到左的加载
		        use: ['style-loader', 'css-loader']
		      },
		      {
		        test: /\.less$/,
		        use: ['style-loader', 'css-loader', 'less-loader']
		      }
		    ]
		  },

#### sass-loader的使用 ####

sass早期是.sass文件

后来更新了一版, 变成了.scss文件

1. 装包

	注意: node-sass在使用npm时很多时候可能会安装失败, 所以可以选择cnpm或yarn来安装

		npm i node-sass sass-loader -D

2. 配置

		 module: {
		    rules: [
		      {
		        test: /\.css$/,
		        // use: [
		        //   { loader: 'style-loader' },
		        //   {
		        //     loader: 'css-loader',
		        //     options: {
		        //       modules: true
		        //     }
		        //   }
		        // ]
		        // css-loader 用于解析css文件
		        // style-loader 用于将css代码 使用js动态的插入到html中, 减少二次请求
		        // use使用loader时  顺序是固定的从右到左的加载
		        use: ['style-loader', 'css-loader']
		      },
		      {
		        test: /\.less$/,
		        use: ['style-loader', 'css-loader', 'less-loader']
		      },
		      {
		        test: /\.scss$/,
		        use: ['style-loader', 'css-loader', 'sass-loader']
		      }
		    ]
		  },

#### url-loader的使用 ####

webpack也无法解析图片或字体等文件

所以需要使用url-loader来加载

**注意: url-loader是file-loader的升级版, 内部依赖了file-loader, 所以需要安装url-loader和file-loader!**

1. 装包

		npm i url-loader file-loader -D

2. 配置

		 module: {
		    rules: [
		      {
		        test: /\.css$/,
		        // use: [
		        //   { loader: 'style-loader' },
		        //   {
		        //     loader: 'css-loader',
		        //     options: {
		        //       modules: true
		        //     }
		        //   }
		        // ]
		        // css-loader 用于解析css文件
		        // style-loader 用于将css代码 使用js动态的插入到html中, 减少二次请求
		        // use使用loader时  顺序是固定的从右到左的加载
		        use: ['style-loader', 'css-loader']
		      },
		      {
		        test: /\.less$/,
		        use: ['style-loader', 'css-loader', 'less-loader']
		      },
		      {
		        test: /\.scss$/,
		        use: ['style-loader', 'css-loader', 'sass-loader']
		      },
		      {
		        test: /\.(png|jpg|gif|bmp|jpeg)$/,
		        use: [
		          {
		            loader: 'url-loader',
		            options: {
		              limit: 81920 // 字节 Byte 如果在8192个字节(8KB)内  就使用base64编码
		            }
		          }
		        ]
		      },
		      {
		        test: /\.(eot|svg|ttf|woff|woff2)$/,
		        use: [
		          {
		            loader: 'url-loader'
		          }
		        ]
		      }
		    ]
		  },

	
	

## vueday08 ##

### babel的配置 ###

1. 安装babel的核心包和loader, 和语法预设

	由于babel7目前使用还有很多问题

	所以选择使用babel6

	babel-preset-env 核心语法包

	babel-preset-stage-0 ES2015的语法包合集

		npm i babel-loader@7 babel-core babel-preset-env babel-preset-stage-0 -D

2. 配置loader

		module: {
		  rules: [
		    { test: /\.js$/, exclude: /node_modules/, loader: "babel-loader" }
		  ]
		}

3. 配置babel

	在.babelrc中进行配置

		{
		  "presets": ["env", "stage-0"]
		}

### vue-loader的配置 ###

总结梳理： webpack 中如何使用 vue :

1. 安装vue的包：  cnpm i vue -S
2. 由于 在 webpack 中，推荐使用 .vue 这个组件模板文件定义组件，所以，需要安装 能解析这种文件的 loader    cnpm i vue-loader vue-template-complier -D
+ 配置vue-loader
+ 安装VueLoaderPlugin在webpack.config.js文件中
3. 在 main.js 中，导入 vue 模块  import Vue from 'vue'
4. 定义一个 .vue 结尾的组件，其中，组件有三部分组成： template script style
5. 使用 import login from './login.vue' 导入这个组件
6. 创建 vm 的实例 var vm = new Vue({ el: '#app', render: c => c(login) })
7. 在页面中创建一个 id 为 app 的 div 元素，作为我们 vm 实例要控制的区域；


### CommonJS模块化规范 ###

导入:

	require()

导出:

	module.exports = {}

### ES6模块化规范 ###

1. 导入(可以导入js/css等其他文件):

	导入js:
	
		import 变量名 from 包名/路径
	
	导入css:
	
		import 路径

2. 导出

	export

	export default {}

### vue-cli 脚手架 ###

由Vue官方提供的一键生成webpack工程化模板项目

目前最新的版本是v3.0, 18年8月份发布的正式版

我自己电脑中安装的是v2.9.6

1. 全局安装vue-cli

	注意: vue-cli包最新的版本是v2.9.6, 如果需要安装v3.0以后的版本, 包名更换为了 @vue/cli 自行去官网查阅文档

		npm i vue-cli -g

2. 执行vue init初始化创建一个项目

		vue init webpack vue-cli-demo

3. 配置项目

		? Project name vue-cli-demo
		? Project description 这是一个牛逼的vue项目
		? Author TianchengLee <ltc6634284@gmail.com>
		? Vue build runtime
		? Install vue-router? Yes
		? Use ESLint to lint your code? No
		? Pick an ESLint preset Standard
		? Set up unit tests No
		? Setup e2e tests with Nightwatch? No
		? Should we run `npm install` for you after the project has been created? (recommended) npm

4. 当配置好后立即会开始使用npm装包

	由于速度较慢, 建议 ctrl + c 停止

	重新使用yarn或cnpm等工具进行安装会更快

		cd vue-cli-demo
		cnpm i

