# 1 写在前面

## 1.1 准备工作

- vue官网：[Vue.js - 渐进式 JavaScript 框架 | Vue.js (vuejs.org)](https://cn.vuejs.org/)
- 安装vue前必须安装官方文档安装相应的node
- 这篇笔记记录的是vue3
- vue有两种API：选项式，组合式。该笔记是选项式笔记
- 开发环境为vscode  + volar拓展
- vue项目搭建看[快速上手 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/quick-start.html)

## 1.2 前端页面分析

```html
<div id="testId" class="testClass" onclick="myFunction()" style="color:red;">
    Hello World!
</div>
```

上面是一个HTML标签，其中包括HTML标签、属性、事件、样式、文本内容。如果我们使用js开发代码，会非常麻烦。而使用vue会相对更容易。以后的讨论也是从这几个方面出发

## 1.3 介绍方式

vue中有三件最重要的事情：生态，渲染语法，组件。本笔记先介绍渲染语法，再进行组件介绍。生态请查看其他的笔记

# 2 渲染语法

渲染语法是vue的核心之一，使用一些非常简便和灵活的方式来渲染HTML标签中的文本内容，属性，事件。

## 2.1 vue文件结构

```vue
<template>
	<!--这里写页面布局，即html标签-->
</template>

<script>
    <!--这里写亿写逻辑，即JavaScript代码-->
</script>

<style>
	<!--这里写亿写样式，即css-->
</style>
```

其中，<template> 必须存在，其余两个标签可存在可不存在

## 2.2 渲染文本内容

vue使用{{}}进行文本内容的绑定，详细信息看 [模板语法 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/template-syntax.html)。其中{{}}中的值可以在<script>标签读取到，下面是一个简单的示例

```vue
<template>
	<p>
        {{ msg }}
    </p>
</template>

<script>
    export default{
        data(){
            return{
                msg: "Hello World!"
            }
        }
    }
</script>
```

在这个示例中，<script>中data的msg属性绑定到了<p>标签中

## 2.3 绑定属性

绑定属性需要在标签中使用v-bind指令。具体使用方式见下面

```VUE
<!--标准写法-->
<div v-bind:id="">
    
</div>
<!--简写-->
<div v-bind:id="">
    
</div>
```

其中，v-bind:后接的是属性名，如id。id的取值可以是data中的变量，也可以是methods中的方法求得。下面演示methods计算

```vue
<template>
	<p :id="testAttr()">
        {{ msg }}
    </p>
</template>

<script>
    export default{
        data(){
            return{
                msg: "Hello World!"
            }
        },
        methods: {
            testAttr(){
                console.log('Hello World');
                return "one";
            }   
        }
    }
</script>
```

属性也可以通过对象的方式传递，传递时对象的键必须为属性名

```vue
<template>
	<div v-bind="testAttrs">
        
    </div>
</template>

<script>
    export default{
        data(){
            testAttrs: {
                id: "idTest",
                class: "classtest"
            }
        }
    }
</script>
```

属性都可以使用上述方式进行绑定。其他内容详见[模板语法 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/template-syntax.html)

## 2.4 监听事件

使用v-on指令，可以监听事件。详细请看[事件处理 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/event-handling.html)

```vue
<!--标准写法-->
<p v-on:click="doSomething">
    
</p>
<!--简写-->
<p @click="doSomething">
    
</p>
```

事件监听分为内联事件处理器和方法事件处理器

### 2.4.1 内联事件处理器

使用场景较少，使用方式如下

```vue
<template>
	<button @click="count++">点我加1</button>
	<p>
        Count is: {{ count }}
    </p>
</template>

<script>
    export default{
        data(){
            return{
                count: 1;
            }
        }
    }
</script>
```

### 2.4.2 方法事件处理器

```vue
<template>
	<button @click.stop="warn("FBI open the door",event)">警告按钮</button>
</template>

<script>
    export default{
       methods:{
           warn(message,event)
           {
               alert(message);
           }
       }
    }
</script>
```

以上方法中，可以使用.stop来防止事件冒泡，使用warn()调用方法，还可以进行参数传递。其中event为事件对象，要写在最后

## 2.5 标签优化

vue对HTML中的许多标签做了优化，下面介绍这些标签的优化方式

### 2.5.1 列表渲染

可以使用v-for来将多个元素渲染成一个标签[列表渲染 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/list.html)

```vue
<template>    
	<p v-for='item in values'>
        {{item}}
    </p>
</template>
<script>    
    export default{
        data(){
            return {
                values: ['a','b','c']
            }
        }
    }
</script>
```

使用上述代码，可以渲染3个<p>标签，内容分别为a,b,c

v-for也可以遍历对象

```vue
<template>    
	<li v-for="(value, key, index) in myObject" :key='index'>
  		{{ index }}. {{ key }}: {{ value }}
	</li>
</template>
<script>    
    export default{
        data() {
  			return {
    			myObject: {
      				title: 'How to do lists in Vue',
      				author: 'Jane Doe',
      				publishedAt: '2016-04-10'
    			}
  			}
		}
    }
</script>
```

value，key，index是根据位置来赋值的，不是根据变量名复制的。即第一个位置是对象属性的值，第二个位置是key，第三个是下标

通过key可以对元素进行排序，不使用key，每次更新都是重新渲染。使用key无需重新渲染即可排序

### 2.5.2 表单相关

想要读取输入框，下拉框等值的时候，可以使用v-model进行绑定[表单输入绑定 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/forms.html)

```vue
<template>    
	<input type='text' v-model="test"/>
</template>
<script>    
    export default{
        data() {
  			return {
                test:''
  			}
		}
    }
</script>
```

### 2.5.3 条件渲染

可以使用v-if和v-show来 选择渲染一个元素。v-if是通过渲染来决定元素是否显示，v-show是通过display属性决定的[条件渲染 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/conditional.html)

```vue
<template>    
	<input type='text' v-model="test" v-if="isdisplay"/>
</template>
<script>    
    export default{
        data() {
  			return {
                test:'',
                isdisplay: true
  			}
		}
    }
</script>
```

## 2.6 计算属性

可以在{{}}中使用函数，但和函数不完全一致。计算属性依靠缓存来保证每次渲染时数据不用重新计算函数中的值，而是取得上一次计算的结果。每次响应发生变更时，缓存中的值才会改变[计算属性 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/computed.html)

```vue
<template>    
	<p>
        {{ testes() }}
    </p>
</template>
<script>    
    export default{
        data() {
  			return {
                test: 0
  			}
		},
        computed:{
            testes(){
                return test > 0 ? 1 : 0;
            }
        }
    }
</script>
```

## 2.7 侦听器

vue中可以侦听属性的变化(data()中 的值)，并通过这些变化做出相应的动作[侦听器 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/watchers.html)

```vue
<template>    
	<p>
        {{ testes() }}
    </p>
</template>
<script>    
    export default{
        data() {
  			return {
                test: 0
  			}
		},
        computed:{
            testes(){
                return test > 0 ? 1 : 0;
            }
        },
        watch: {
            test(newValue,oldValue){
                
            }
        }
    }
</script>
```

## 2.7 模板引用

模板引用即在vue中用底层的方式操纵DOM[模板引用 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/template-refs.html)

```vue
<template>    
	<input ref="input"/>
</template>
<script>    
    export default{
        mounted(){
            this.$ref.input.foucus()
        }
    }
</script>
```

需要用js操作dom的标签用ref进行 标注。在script中 用mounted进行访问，访问的固定语法为this.$ref.#ref中的值.方法

# 3 组件语法

## 3.1 组件介绍

我们的网页可以理解为一个树形图，其中root根节点为整个页面。页面中由多个子树组成

vue中，根节点为app.vue，其他的子树为.vue结尾的文件

![components.7fbb3771](Vue.assets/components.7fbb3771.png)

## 3.2 定义组件

```vue
<template>   
</template>
<script>    
</script>
<style>
</style>
```

组件由上面三个部分组成，其中<template>为标签，<script>为逻辑代码，<style>为样式。

其中，组件文件以.vue结尾，组件中必须包含<template>标签

## 3.3 组件注册

想要使用组件，必须在根节点引入相关的节点，相关 节点引用方式如下

```vue
<!--子组件ComponentA-->
<template>   
</template>
<script>    
</script>
<style>
</style>
```

```vue
<!--根组件vue.app-->

<template> 
	<ComponentA />
</template>
<script>   
    import ComponentA from 'ComponentA的相对路径'
    export default {
        components: {
            ComponentA
        }
    }
</script>
```

1. 使用import引入相关组件文件
2. 在components中定义组件
3. 在<template>中引入相关组件

## 3.4 组件传参

有时需要一个组件向另一个组件传递信息，需要组件传参[组件基础 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/essentials/component-basics.html)

```vue
<!--数据源-->
<template> 
	<ComponentA title="Hello World" />
</template>
<script>   
</script>
```

```vue
<!--目标组件-->
<template> 
	<p>
        {{ title  }}
    </p>
</template>
<script>   
    export default {
        props: ['title']
    }
</script>
```

可以在标签中写一些数据，利用props进行传递

## 3.5 组件事件

