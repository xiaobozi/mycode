# 基本结构
    new Vue ({
        el : "",
        data : {},
        methods : {},
        computed : {},
        watch : {},
        filters : {},
        components : {}

    })
el          表 v 参数为id
data        表 m 数据对象
methods     表事件函数
computed    表计算属性(通过return)
watch       表监听
filters     表过滤器
components  表组件
# 基本指令
v-cloak 解决 插入表达式的闪烁问题,插入表达式只会替换自己的占位符
    [v-cloak]{display: none}

v-text= 是没有闪烁问题,但是会覆盖元素中原本的内容

v-html= 可以解析字符串的html代码后输出

v-bind:(可以快捷写法 : ) 用于绑定属性的键,声明其值是一个变量
        可以进行字符串拼接,计算

v-on:(可以快捷写法 @ ) 事件绑定,绑一个函数名,函数体写在methods中

v-model=  可以实现表单元素和 Model中数据的双向数据绑定(只能在表单元素中)

v-for=   遍历数组     语法 "(e,[i]) in arr" e表元素.i表下标
        遍历对象     语法 "(v,k) in obj" v表value,k表key
        迭代数字     语法 "count in number" number 表迭代次数,count起始为1
        ** 使用v-for时,加上 :key = "e" ** e必须为Num,Str类型 

v-if=    判断布尔值,每次都会重新删除或创建该元素
v-show=  判断布尔值,不会操作DOM,修改该元素样式 display = none
v-once  只会渲染一次,不会更新视图
v-pre   让插值表达式变成字符串

# 事件修饰符
.stop           阻止事件冒泡

.prevent        阻止官方默认事件

.capture        事件捕获(默认事件冒泡)

.self           只触发事件源,且只会阻止自己的冒泡,其余正常冒泡

.once           只触发一次事件处理函数

# class 样式(使用时要加 :)

可以使用数组[],其中可以使用3元表达式或对象
    语法 "['xxx','xxx']" ,对象写法为 {类名 : 布尔值}

可以使用对象{},可不带引号,可以把对象写好放在data中,在传入键名即可
    语法 "{类型 : 布尔值,类型 : 布尔值...},"
行内样式时 若虚传入多个对象样式时可以使用 '[obj,obj]'的写法

### Vue 方法

# 过滤器(就近原则)
* 全局过滤器(需要先声明在调用)
使用过滤器进行文本格式化,先声明在调用,其中可以进行传参,可进行多次过滤.从左向右进行
语法    {{ Str | "过滤器的名称"(arg...)}}    
声明过滤器 Vue.filter("过滤器的名称", function(data,arg...){
        return data
    })
* 私有过滤器
在vm中声明,与data,el,methods平级
语法 filters: {
    过滤器名称 : function(){}
}

# 用户自定义
* 事件键盘修饰符
自定义keycode
Vue.config.keyCode.A = 65
@keyup.A

* 自定义全局指令(定义时不加前缀,调用时加前缀)

Vue.directive("指令名",bingding)
bingding :  一个对象,内含指令的钩子函数(生命周期)
<!-- vnode :      VUE编译生成的虚拟节点 -->
在bingding中的钩子函数,第一个参数都是 指令绑定的DOM元素
自定义指令的生命周期 bind -> inserted -> update -> componentUpdate -> unbind
bingding 的钩子函数(生命周期)
bind : 
    只调用一次，指令第一次绑定到元素时候调用，用这个钩子可以定义一个绑定时执行一次的初始化动作。
inserted : 
    被绑定的元素插入父节点的时候调用
update : 
    被绑定与元素所在模板更新时调用
componentUpdate : 
    被绑定的元素所在模板完成一次更新更新周期的时候调用
unbind : 
    只调用一次，指令元素解绑的时候调用

### 通过$的一些方法与属性
+ $mount（）外部设置el,  vue的挂载点
+ $destroy() 手动销毁
+ $watch() 监听
+ $forceUpdate() 强制更新
+ $set()    添加监听属性
+ $parent 子组件中通过这个属性可以访问父插件
+ $children 父组件访问子子组件,返回一个伪数组
+ $emit() 通过该属性手动触发自定义事件
+ $on(事件名,callback) Vue实例全局设置自定义事件
+ $nextTick(callback) 来监听该数据变化导致的视图变化
+ $refs  一个自定义ref属性ele元素集合的对象
+ $el 新视图的根标签
## 生命周期
* 生命周期钩子 = 生命周期函数 = 生命周期事件

创建期间的生命周期函数:
beforeCreate:        创建实例时  第一个生命周期函数(内存init)
Created:             创建完成后  第二个生命周期函数(内存init)
beforeMount:         页面渲染前  第三个生命周期函数(页面init)
mounted:             页面渲染后  第四个生命周期函数(页面init)

运行期间的生命周期函数:
* 当data数据改变时(这2个事件触发 0到无数次)
beforeUpdate         数据更新后,data更新的,页面未更新(组件运行)
updated              数据更新后,data更新的,页面也是更新的(组件运行)
若要操作DOM节点,最早在mounted中进行

销毁期间的生命周期函数:
beforeDestroy  执行到此函数时,实例处于可用状态,还未销毁
destroyed      执行到此函数时,组件已经完全被销毁

## watch (监听者模式)
简写(一个函数,函数名与监听数据一致)
全写(对象):
immediate : 布尔值
    不管监听数据变不变,默认触发一次handler

deep : 布尔值
    ture 表监听该对象的所有层级

handler : Fanction
    触发的函数体
有2个内置参数(newv,oldv)
    newv 表改变后的值
    oldv 表改变前的值
## computed (计算属性)
如果一个数据是依赖于多个数据的计算,都应该用计算属性
每次当内部的数据变化时,触发该函数体
监听内部所有数据
* data中不能存在跟该函数名一致的数据
简写(一个函数,通过return提供计算属性的值)
全写语法为 变量名(函数名) : {get(){}, set(){}}

### 什么是响应式
具备响应式效果的数据叫响应式数据
一个数据如果想要具备响应式:
    1 数据必须写在视图上
    2 数据必须有监听( get, set)

* 响应式 = 数据变化 -> 视图更新
视图更新,则视图上的所有表达式,变量,函数调用都会重新进行运行求值
* 在vue 中修改数组的元素不会有响应式,只能通过数组方法中能修改原数组的方法进行操作
在vue中,进行响应式主要通过 Object.defineProperty() 进行监听
在vue中,数组元素如果是对象就会监听该对象的key,不是对象就不设监听,
类似[{},{},{}]格式下,若把该数组元素的引用地址修改了,则不再监听该元素(不论该元素是不是对象)
(具有嵌套父子关系,若父对象没有监听,则其所有子数据不会有监听)

在vue中,不存在的属性(比如新添加的属性)如何驱动视图更新?
因为在运行代码时vue就已经为data中的对象设置了监听,而data不存在该属性,
所以不会给该属性设置监听,该属性不具备响应式.
若想其具有响应式可通过修改监听的父级, parentobj = {key : value} 但源数据会被覆盖掉
* vue提供了一个方法 set 方法 全局语法为 Vue.set(parentobj,key,value) 私有语法为 this.$set(parentobj,key,value)
set方法主要用来设置初始值并监听(会触发一次setter)
逻辑图:
Data(getter, setter) -> Watcher(判断是否需要刷新视图) ->  刷新视图(最先触发修改虚拟节点的函数) -> 生成虚拟节点(Data.getter) ->生成虚拟DOM树 
当data的getter时,Watcher 会收集依赖,data的setter会通知Watcher判断,在刷新视图生成虚拟节点时会进行Data的getter,Watacher会重新收集依赖

### 组件(所有的组件都是vue实例)

components : {组件名 : {template : `字符串模板`}}
需要引用时,使用 <组件名></组件名>标签.
组件注册
    通过配置components选项来注册
组件挂载
    通过组件名形成的自定义标签名来挂载
    组件的这个自定义标签不会存在于页面上
    挂载时,可以是单标签,也可以是双标签
组件命名
    组件名不能和html的标签名相同,
    如果使用驼峰式命名,在使用时,需要加上"-"( vNav === v-Nav)
组件模板
    通过template选项设置模板
    一般引号不能换行,使用``字符串模板
    组件的模板需要一个唯一的根元素包裹

组件也是一个vue实例,可以几乎可以配置所有的vue配置项
    
组件选项与new Vue选项的区别:
    1 组件没有el属性
    2 组件选项的data必须写成函数,return一个数据对象
组件流程 
    注册根组件 -> 声明根组件(template,components,data) -> [注册子组件 ->  声明子组件(template,components,data)]+

组件中的属性
    template : String类型. (组件模板)
    components : Object类型. (组件注册)
    data : Fanction类型. (组件的数据变量,通过retren数据对象)
    props : Array类型. 
        简写就是一个Array
        全写是{{}}json格式,可以设置参数,传递数据时可设置type验证等...去官网了解
        props的数据可以通过this进行get,也可以通过watch监听)
组件中有2中数据
    data数据是一般数据,只能在本组件中定义和修改
    props数据是共享数据,在本组件中定义,不能在组件中修改
    computed数据是计算数据,在本组件中定义,不能直接修改,只能修改它依赖的数据,在set中修改依赖的数据

组件的生命周期:


组件的数据通信:    
    组件中的父传子    
        通过props传递数据,子组件的变量在props中进行变量声明,父组件进行赋值操作,受到父组件的限制,不能在子组件中修改
        引用数据类型,引用类型没有修改引用地址则本质上是没有修改
        v-for 不能用在组件的根元素上(根元素没有形成嵌套结构的话,只会生成一个虚拟节点)
    组件中的子传父 
        方法1 父组件用一个函数接收子组件数据,子组件触发父组件的方法,通过传参进行获取
                通过props把父组件的方法传递个子组件
        方法2 用$parent访问到父组件,将子组件的数据传递到父组件上(少用)
        方法3 给子组件绑定一个自定义事件,并且事件的callback设为父组件的方法(推荐)
                语法 @事件名=callback
              用$emit()进行触发事件传递
                语法 $emit(自定义事件,子组件的数据)
### bus 的数据共享
    设置一个bus的空Vue实例
    通过设置全局变量bus的自定义事件(用箭头函数)接收数据,在适当的地方触发事件来传递数据 

### 如何获取某一次数据变化导致的视图更新的视图内容
    vue的视图更新是异步的(数据变化,不会导致视图变化)
    updated()只要视图更新就会触发,监听的是整个视图
    $nextTick()来监听该数据变化导致的视图变化,回调函数会在本次视图更新结束之后触发

### 组件中的属性设置
+ ref=  在refs对象中加入该元素
+ is=   把该标签变为组件挂载点(更直观)
    通过给is绑定一个变量,切换变量的值,可让组件挂载不同的组件(动态组件)

### render 函数(性能高,但是难受)
    render函数用于替代template属性
    render函数跳过了编译的过程,直接生成虚拟节点
    render(h){
        return h(虚拟节点)
    }
    形参h事件全名是 = createElement
    通常用于根组件
### 父子兄弟组件的生命顺序
父子顺序: 父beforCreate -> 父created -> 父beforMount -> 子beforCreate -> 子created -> 子beforMount -> 子mounted -> 父mounted
父子兄弟顺序:
    父beforCreate -> 父created -> 父beforMount ->
    子1beforCreate -> 子1created -> 子1beforMount ->
    子2beforCreate -> 子2created -> 子2beforMount ->
    子1mounted -> 子2mounted -> 父mounted

### vuex (状态管理)

### 组件混合
当组件的某些选项是相同或重复的,可以通过组件混合mixins来进行复用
先全局声明一个对象,再引用到mixins
mixins : [对象]
若个minxins中有的选项与组件本身一致时,以组件的本身为准

### 插槽(结构清晰,父子传递数据)
如果2个组件有相同的template部分,则可以把相同的template部分写成一个组件,其余不同的部分通过插槽插入
写双标签,在里面嵌入不同的部分,在子组件中写入插槽标签
如果需要插入多个插槽,或插槽位置不同,需要给插槽命名做对应
slot= 自定义插槽名字
<slot name=""></slot>

把最下层的组件挂载模板写在最上层的视图内,可把数据传递给下层组件,但是必须通过插槽引入子组件
通过插槽可实现父子传递数据,父子结耦

### vuex 状态管理(数据共享)
什么叫状态管理?
1个共享数据和多个组件的视图组成了一个状态

同一个数据,可以被N个组件使用
这个数据每次变化,都可以在所有组件上看到视图变化 

* 结构
let store = new Vuex.store({
    strict ; bolean,
    state : {
        xxx : xxxx
    },
    mountions:{},
    actions : {}
    getter : {}
})
strict 严格模式
state  数据
mountions 修改数据得句柄
actions  异步修改数据得句柄
getter  计算属性
将store挂载到new Vue 中,所有的组件都能访问

* 如何获取共享数据
通过$store.state.xxx 可以做get,也可以set

* 如何修改共享数据
开启严格模式后,不能在mutation之外的地方修改共享数据,
只能把共享数据的修改逻辑写在mutations的句柄中,
在store中写入mutations属性,其中设置句柄
在组件中 通过 this.$store.commit(句柄,...[args]),来触发mutation的句柄,以修改数据

+ 异步修改共享数据
需要把修改数据的代码放入一个异步的回调函数里
放入异步的回调里,又违背了只能在mutation的句柄内修改的准则
所以得把该函数放入actions中
actions中触发mutations得函数
所有组件触发修改得通过 this.$store.dispath(句柄,...[args]) 

* 流程图
事件源 -> actions(dispath) -> mutations(commit) -> state -> 修改视图(响应式)

 vuex的计算属性通过计算属性才能获取




