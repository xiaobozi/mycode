# v-html
你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎
你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值。

# v-bind
v-bind 指令可以用于响应式地更新 HTML attribute

# 使用 JavaScript 表达式
有个限制就是 每个绑定都只能包含单个表达式 (三元表达式支持)

# 指令
指令 (Directives) 是带有 v- 前缀的特殊 attribute。指令 attribute 的值预期是单个 JavaScript 表达式
 (v-for 是例外情况)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM

# 动态参数
从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数
    对动态参数的值的约束(可用于事件reomve)
        动态参数预期会求出一个字符串，异常情况下值为 null。这个特殊的 null 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告
    对动态参数表达式的约束
        因为某些字符，如空格和引号，放在 HTML attribute 名里是无效
        变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。
    在DOM中使用模板时,需要避免使用大写字符来命名键名，因为浏览器会把 attribute 名全部强制转为小写

# 修饰符 .
    特殊后缀，用于指出一个指令应该以特殊方式绑定

# 缩写
@ === v-on
: === v-bind

# 计算属性

在计算属性中,我们提供的函数的函数名将用于 vm实例的property 
且该函数将作为该property 的 getter 函数
计算属性的 getter 函数是没有副作用 (side effect) 的，这使它更易于测试和理解。

# 计算属性缓存 vs 方法 (提高性能)

我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。
然而，不同的是计算属性是基于它们的响应式依赖进行缓存的。
只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 message 还没有发生改变，
多次访问该计算属性会立即返回之前的计算结果，而不必再次执行函数。

计算属性默认只有 getter，不过在需要时你也可以提供一个 setter
当计算属性进行setter时,只有视图有该属性的依赖就一定会getter

# 侦听器

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。
watch 选项允许我们执行异步操作，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态

# class 和 style
用 v-bind 处理它们：只需要通过表达式计算出字符串结果即可
表达式结果的类型除了字符串之外，还可以是对象或数组。
可以绑定一个返回对象的计算属性

绑定多个class 可以用数组
当在一个自定义组件上使用 class property 时，这些 class 将被添加到该组件的根元素上面。这个元素上已经存在的 class 不会被覆盖。

v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上

注意，v-show 不支持 <template> 元素，也不支持 v-else

# v-for 遍历对象的话有3个参数 value key index
如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序而是重新刷新每个元素

v-for 与 v-if 一同使用

当它们处于同一节点，v-for 的优先级比 v-if 更高,这意味着 v-if 将分别重复运行于每个 v-for 循环中

# 事件
需要获取evevt对象是,可以用$event获取内置对象

.stop 阻止冒泡
.prevent 阻止默认事件
.capture 事件捕获
.self   事件源本身
.once   事件只处发一次
.passive  滚动事件的默认行为 (即滚动行为) 将会立即触发
使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生
不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略，
同时浏览器可能会向你展示一个警告。请记住，.passive 会告诉浏览器你不想阻止事件的默认行为。

# v-model
v-model 会忽略所有表单元素的 value、checked、selected,
attribute 的初始值而总是将 Vue 实例的数据作为数据来源,需要现在data中声明初始值

v-model 表达式的初始值未能匹配任何选项
<select> 元素将被渲染为“未选中”状态
默认初始值设置为空,且首个选项disabled
更推荐像上面这样提供一个值为空的禁用选项。
多选时 (绑定到一个数组),但是length永远为 1

对于单选按钮，复选框及选择框的选项，v-model 绑定的绑定的返回值是先查找自定义的value

复选框
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
这里的 true-value 和 false-value attribute 并不会影响输入控件的 value 
因为浏览器在提交表单时并不会包含未被选中的复选框

#  修饰符
在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步
可以添加 lazy 修饰符，从而转为在 change 事件_之后_进行同步
使得v-medol 的队列为最后

如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：
这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 parseFloat() 解析，则会返回原始的值。

如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：

# props
若想传递一个对象的所有property,可总结v-bind=对象

对于绝大多数 attribute 来说，从外部提供给组件的值会替换掉组件内部设置好的值。
庆幸的是，class 和 style attribute 会稍微智能一些,不会破坏原有的类样式,而是进行合并

禁用 Attribute 继承 可在参数选项中 inheritAttrs : false

# 事件
不同于组件和 prop，事件名不存在任何自动化的大小写转换,始终使用 kebab-case 的事件名。

# 自定义组件的v-model
一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件，
但是像单选框、复选框等类型的输入控件可能会将 value attribute 用于不同的目的。model 选项可以用来避免这样的冲突：

# 将原生事件绑定到组件
想要在一个组件的根元素上直接监听一个原生事件。这时，你可以使用 v-on 的 .native 修饰符：
过在你尝试监听一个类似 <input> 的非常特定的元素时, 组件可能做了重构，所以根元素不是一个input
这时，父级的 .native 监听器将静默失败但是其类似foucs的处理函数不会被调用。
Vue 提供了一个 $listeners property，它是一个对象，里面包含了作用在这个组件上的所有监听器    
配合 v-on="$listeners" 将所有的事件监听器指向这个组件的某个特定的子元素。
在计算属性中, Object.assign({},this.$listeners,{自定义事件},进行合并对象,添加自定义事件

# 插槽
父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

<slot> 元素有一个特殊的 attribute：name。这个 attribute 可以用来定义插槽：
使用 v-slot: 指令可以指定插槽
注意 v-slot 只能添加在 <template> 上

由于在父插件使用的插槽模板,所以其内的模板可以访问不到子插件的作用域,
在新版本中,可在子插件的<slot>上v-bind一个自定义的 attribute=你想传递的数据

使用默认插槽时
在父插件中使用<template>时调用 v-slot:default='自定义变量'

使用具名插槽时
在父插件中使用<template>时调用 v-slot:名字='自定义变量'
其自定义变量是一个对象,其中有在子插件的<slot>定义的attribute,就可以访问在子插件的数据

因为作用域插槽的内部工作原理是将插槽内容包括在一个传入单个参数的函数中
意味着v-solt 的值实际上可以任何能够作为函数定义的参数的JS表达式
所以可以使用 解构赋值
简写的话 要把传递的数据名 === attrbute === 父插件的自定义变量 
v-slot:名字='{自定义名}'
甚至可以定义后备内容,用于插槽的prop是undefined的情况
v-slot:名字='{自定义名}={后备内容}'
可以优雅的写代码了

# 动态插槽名
v-slot:[dynamicSlotName]
动态指令参数也可以用在 v-slot 上，来定义动态的插槽名

# 具名插槽的缩写
s-slot:  === #

插槽 prop 允许我们将插槽转换为可复用的模板，这些模板可以基于输入的 prop 渲染出不同的内容。
在设计封装数据逻辑同时允许父级组件自定义部分布局的可复用组件时是最有用的。

# 动态组件与异步组件
在动态组件上使用 <keep-alive>
每次切换新标签时,Vue都会重新创建一个新的实例
用一个<keep-alive>元素将其动态组件包裹起来,可实现缓存效果
注意这个 <keep-alive> 要求被切换到的组件都有自己的名字，
不论是通过组件的 name 选项还是局部/全局注册。

# 访问子组件实例或子元素
元素标签也可以设置
可以通过 ref 这个 attribute 为子组件赋予一个 ID 引用。
this.$refs.usernameInput 进行get
当 ref 和 v-for 一起使用的时候，你得到的 ref 将会是一个包含了对应数据源的这些子组件的数组。
$refs 只会在组件渲染完成之后生效，并且它们不是响应式的。
这仅作为一个用于直接操作子组件的“逃生舱”——你应该避免在模板或计算属性中访问 $refs。

# 依赖注入
实例选项：provide 和 inject。
provide 选项允许我们指定我们想要提供给后代组件的数据/方法
然后在任何后代组件里，我们都可以使用 
inject 选项来接收指定的我们想要添加在这个实例上的 property：

依赖注入还是有负面影响的。
它将你应用程序中的组件与它们当前的组织方式耦合起来，使重构变得更加困难。
同时所提供的 property 是非响应式的

# 程序化的事件侦听器
通过 $on(eventName, eventHandler) 侦听一个事件
通过 $once(eventName, eventHandler) 一次性侦听一个事件
通过 $off(eventName, eventHandler) 停止侦听一个事件

注意 Vue 的事件系统不同于浏览器的 EventTarget API。
尽管它们工作起来是相似的，但是 $emit、$on, 和 $off 
并不是 dispatchEvent、addEventListener 和 removeEventListener 的别名。

# 通过 v-once 创建低开销的静态组件
渲染普通的 HTML 元素在 Vue 中是非常快速的，
但有的时候你可能有一个组件，这个组件包含了大量静态内容。
你可以在根元素上添加 v-once attribute 以确保这些内容只计算一次然后缓存起来

# 单元素/组件的过渡
* 过渡的类名
v-enter：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
v-enter-to：2.1.8 版及以上定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。
v-leave：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除
v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
v-leave-to：2.1.8 版及以上定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。
对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 <transition>，则 v- 是这些类名的默认前缀。如果你使用了 <transition name="my-transition">，那么 v-enter 会替换为 my-transition-enter。

# V-Router

结构
    new VueRouter({
        routers : [{
            path : '/...',
            component/components : 组件 ,
            redirect : '/…',
            meta : {},
            props : []
        }]
    })
path 表hash值,路径
component 表组件
components 对象写法,显示多个组件
redirect 表重定向
meta 表源信息
props 表传递数据(可以书写 函数 ,布尔值) 

需要设置初始的hash值
跟 Vuex一样需要挂载在vue实例的根组件上
<router-view>//路由动态组件
<router-link to=''>//路由的入口

路由组件的通信
可通过<router-view>向所有的路由组件传递数据,所有数据共享一个数据

给组件通过设置name,可在<router-view name=> 指定组件

##路由守卫

路由守卫是什么?为什么要用路由守卫?
路由守卫都是一些特定的函数,用于路由跳转拦截

全局路由守卫
    beforeEach(fn(to,from,next)) 全局的路由进入的钩子
路由独享守卫
    如果一个路由对应一个组件,则和组件独享守卫一样
    
组件独享守卫
    在组件中设置钩子函数:
    路由进入 beforeRouteEnter(to,from,next) 
    路由修改(动态路由切换     ) beforeRouteUpdate   
    路由离开 beforeRouteLeave(to,from,next)     
    to 目标路由对象($route) from 上个路由对象 
    next 函数传参填入布尔值,是否跳转,不使用next()也会跳转失败,在next中可传入一个回调,形参就是该路由
        也可写对象,表示跳转到的路由

如何做到路由发生了切换?
watch 监听$route对象
有keep-alive 用created监听
没有keep-alive 用activated监听
动态路由 用beforeRouteUpdate

完整的导航解析流程
1, 导航被触发
2, 在失活的组件里调用离开守卫
3, 调用全局的beforeEach守卫
4, 在重用的组件里调用beforeRouteUpdate守卫
5, 在路由配置里调用beforeEnter
6, 解析异步路由组件
7, 在被激活的组件里调用beforeRouteEnter守卫
8, 调用全局的beforeResolve守卫
9, 导航被确认
10, 调用全局的afterEach钩子
11, 触发DOM更新
12, 用创建好的实例调用beforeRouterEnter守卫中传给next的回调函数



