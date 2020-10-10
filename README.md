##### 1、请简述 Vue 首次渲染的过程。

Vue的首次渲染是通过调用this._init(options)实现的。具体步骤如下：
1. 判断当前组件是不是vue实例
2. 判断vue实例是不是组件，如果不是组件将我们传入的options 和vue构造函数的options进行合并
3. 调用vm.$mount()把模板编译成render函数，编译好之后把模板存到options中
4. 调用runtime/index.js里的 $mount方法，重新获取el
5. 调用mountComponent(this,el),首先判断选项中是否有render选项，如果没有但是传入了模板，开发环境会报警告，然后触发beforeMount，定义updateComponent ，在次方法中调用 vm_update(vm._render(),...)(负责渲染虚拟dom),以及vm._update()(负责更新，将虚拟dom转化为真实dom)
6. 创建watcher对象，调用get方法
7. 触发mounted
8. 返回vm实例

##### 2、请简述 Vue 响应式原理。
initstate() => initData() => oberver() 将对象转化为响应式对象
1. observer(value) 先判断value是否是对象，不是对象就直接返回，如果是对象判断是否有__ob__如果有直接返回，如果没有就创建observer对象如果是数组就对数组进行处理，如果是对象就调用work方法。在work中调用defineReactive方法
2. defineReactive为每一个属性创建一个dep属性，如果当前的属性是对象就调用observer，调用geter收集依赖，定义setter派发通知，调用dep.notify()
3. watcher 数据变化时dep.notify调用update()方法，

##### 3、请简述虚拟 DOM 中 Key 的作用和好处。
作用：跟踪每个节点的身份，便于重用和替换元素。
好处：避免多次操作dom, 渲染更加高效

##### 4、请简述 Vue 中模板编译的过程。
complieToFunctions(template) 先从缓存中加载编译好的render函数，如果没有就调用 complie(template,options)开始编译

complie(template,options) 合并options ，调用baseComplie(template.trim,finalOptions)

baseComplie(template.trim,finalOptions) 先调用parse把template转换成AST 语法树，然后调用optimize()检查静态语法树，设置标记，然后generater将AST tree生产js代码

然后回到入口函数 complieToFunctions(template）调用 createFunction()把上一步生成的字符串js代码转化为函数，当函数创建完毕后将函数挂在到options上的对应的选项上

