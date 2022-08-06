# Vue

## 概述

**MVVM模式的实现者**

+ Model：模型层，在这里表示JavaScript对象
+ View：视图层，表示DOM（HTML操作的元素）
+ ViewModel：连接视图和数据的中间件，Vue.js就是MVVM中的ViewModel层的实现者

在MVVM架构中，不允许**数据**和**视图**直接通信，只能通过ViewModel来通信，而ViewModel就是定义了一个Observer观察者

+ ViewModel能够观察到数据的变化，并对视图对应的内容进行更新
+ ViewModel能够观察到视图的变化，并能够通知数据发生改变

Vue.js就是MVVM的实现者，它的核心就是实现了DOM监听与数据绑定

观察得知

1. data中所有的属性，最后都出现在了vm身上
2. vm身上所有的属性以及Vue原型上的所有属性，在Vue模板中都可以直接使用

**为什么要是用Vue.js**

1. 轻量级，体积小

2. 移动优先，更适合移动端，比如移动端的touch事件

3. 易上手，学习曲线平稳，文档齐全

4. 吸取了Angular（模块化）和React（虚拟DOM） 的长处，并拥有自己的独特功能，比如：计算属性

5. 开源，社区活跃度高

**createApp()和mount()方法**

```html
<body>
<div id="app"></div>
</body>
<script>
    const app = Vue.createApp({ // 此花括号为参数
        data(){
          return {
              message:'Vue'
          }
        },
        template:`
          <div>{{message}}</div>
        `
    })

    const vm = app.mount("#app") // 挂在app实例
    console.log(vm) // MVVM     M->model    V->view     VM->ViewModel   数据视图连接层
</script>
```

**第一个Vue**

```html
<!--  导入Vue.js-->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
	or
	<script src="https://unpkg.com/vue@next"></script>
```

```html
<body>
<!--Vue层 模板-->
<div id="app">
  {{massage}}
</div>


<script>
  var vue = new Vue({
    el: "#app",
    //model层：数据
    data:{
      massage: "Hello,Vue!",
    }
  });
</script>
</body>
```

```html
<body>
  <div id="app"></div>
</body>
<script>
  Vue.createApp({ // 创建Vue实例
    template: '<div>Hello World</div>'
  }).mount("#app")
</script>
```

el和data的两种写法

+ el的两种写法
  + new Vue的时候配置el属性
  + 先创建Vue实例，随后再通过vm.$mount('#app')指定el的值
+ data的两种写法
  + 对象式
  + 函数式
+ other：
  + 由Vue管理的函数，不能写箭头函数，否则函数里的this就不是Vue实例了

```js
const app = new Vue({
    el: "#app",//第一种绑定元素方法
    data: {
        name: "Vue",
    }
})
// app.$mount('#app')//第二种绑定元素的方法

const app = new Vue({
    el:'#app',
    // data:{//data的第一种写法：对象式
    //   name:'Vue',
    // }
    data:function(){//可以简写为 data(){}
        //data的第二种写法：函数式
        return{
            name:'Vue',
        }
    }
})
```



```html
<body>
    <div id="app"></div> 
    <!--每秒加一计数器-->
</body>
<script>
    Vue.createApp({
        data(){
          return {
              counter:1
          }
        },
        mounted(){
            setInterval(()=>{
                this.counter++; // 等于 this.$date.counter +=1;
            })
        },
        template:'<div>{{counter}}</div>' // 插值表达式{{该括号中可以写任意的js表达式}}
        // e.g.{{3 + 1}};{{"Volerde" + ".com"}};{{count > 3 ? true : false}}
    }).mount('#app')
</script>
```

## 数据代理

Object.defineProperty

```javascript
let number = 19
let person = {
    name: 'John',
    sex: 'M',

}
Object.defineProperty(person, 'age',{//此方法添加的属性默认不可枚举，不可修改，不可删除
    // value: 30,
    // enumerable: true,//添加此属性后可枚举
    // writable: true,//添加此属性后可修改
    // configurable: true,//添加此属性后可删除

    //当有人调用person的age属性时，getter就会被调用
    get(){
        return number
    },
    //当有人修改person的age属性时，setter就会被调用，且会收到修改的值
    set(value){
        console.log(value)
        number = value
    }
})
console.log(person)何为数据代理
```

何为数据代理

```javascript
 <!--数据代理：通过一个对象代理对另外一个对象属性的操作-->
 <script>
   let obj = {x:100}
   let obj2 = {y:300}

   Object.defineProperty(obj2, "x",{
     get(){
       return obj.x
     },
     set(value){
        obj.x = value
     }
   })
```

数据代理的Vue实现

1. Vue中的数据代理

   通过vm对象来代理data对象中属性的操作

2. Vue中数据代理的好处

   更加方便的操作data中的数据

3. 基本原理

   通过Object.defineProperty()把data对象中所有的属性添加到vm上

   为每一个添加到vm上的属性指定一个getter/setter

   在getter/setter内部去操作data中对应的属性

![1652706136854.png](http://lsky.volerde.space/i/2022/05/16/62824b5a3563d.png)

## 基础语法

### 指令

`v-bind`

```html
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
```

```html
<body>
<!--Vue层 模板-->
<div id="app">
  <span v-bind:title="massage">
    鼠标悬停此处几秒，查看此处的动态信息
  </span>
</div>


<script>
  var app = new Vue({
    el: "#app",
    //model层 数据
    data:{
      massage: "Hello!",
    }
  });
</script>
</body>
```

v-bind等被称为指令，指令带有前缀 v-，以表示他们是Vue提供的特殊特性。他们会在渲染的DOM上应用特殊的响应式行为。

在这里，该指令的意思为：“将这个元素节点的title特性和Vue实例的massage属性绑定”。

 v-html

```html
<script>
    const app = Vue.createApp({
        data(){
            return {
                message: '<i>Hello, world</i>'
            }
        },
        methods: {
          messageChangeClick(){
              this.message = this.message === 'Hello, world' ? 'Vue!' : 'Hello, world'
          }
        },
        template:`
          <div
              @click="messageChangeClick"
              v-html="message"
              v-once
          ></div> 
          <!--使用v-html可以将message中的HTML的标签正常编译出来-->
          <!--v-once使用后,该标签不再改变-->
        `
    });
    app.mount("#app");
</script>
```

v-once

v-once这个指令不需要任何表达式，它的作用就是定义它的元素或组件只会渲染一次，包括元素或者组件的所有字节点。首次渲染后，不再随着数据的改变而重新渲染。也就是说使用v-once，那么该块都将被视为静态内容。

### 事件

`v-on`监听事件

事件有vue的事件，前端本身的事件。

```html
<body>
<div id="app">
    <!--v-on绑定click事件-->
    <button v-on:click="sayhi">Click Me</button>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      massage: "Volerde"
    },
    methods: {//方法必须定义在methods对象中
        sayhi: function(){
          alert(this.massage)
        }
    }
  })
</script>
</body>
```

```html
<body>
	<!--案例-->
    <div id="app"></div>
</body>
<script>
    Vue.createApp({
        data() {
            return {
                message: "",
                showStatus: true
            }
        },
        methods: {
            comeBtnClick() {
                this.message = "ようこそ"
            },
            goBtnClick() {
                this.message = "Long May The Sunshine"
            },
            showStatusBtn() {
                this.showStatus = !this.showStatus
            }
        },
        template:`
                  <div>
                    <button @click="showStatusBtn">show</button>
                    <div v-if="showStatus">{{message}}</div>
                    <button @click="comeBtnClick">come</button>
                    <button @click="goBtnClick">go</button>
                  </div>
                `
    }).mount("#app");
</script>
```

## 条件渲染

条件渲染

+ v-if
  + 写法：
    1. v-if = "表达式"
    2. v-else-if = "表达式"
    3. v-else = "表达式"
  + 适用于：切换频率较低的场景
  + 特点：不展示的DOM元素会被直接删除
  + 注意：v-if可以和v-else-if,v-else一起使用，但是要求结构不能被打断
+ v-show
  + 写法：v-show = "表达式"
  + 适用于：切换频率较高的场景
  + 特点：不展示的元素不会被删除，会被使用样式隐藏掉
+ 备注：使用v-if时，元素可能无法被获取到，使用v-show一定可以获取到

`v-if，v-else-if，v-else`

```html
<body>
<!--Vue层 模板-->
<div id="vm">
<h1 v-if="type==='A'">YES</h1>
<h1 v-else-if="type==='B'">YES</h1>
<h1 v-else>C</h1>
</div>
<script>
  var vm = new Vue({
    el: "#vm",
    data:{
      type: "A"
    }
  });
</script>
</body>
```



v-show&v-if区别

1. v-show通过控制css样式来控制隐藏显示元素,而v-if则通过重新渲染DOM来控制隐藏显示元素 
2. v-if更加灵活,可以v-if,v-else,v-else-if,进行业务逻辑判断,而v-show则不能进行业务逻辑判断
3. 频繁使用v-if反复进行DOM渲染则会给用户造成卡顿的使用体验

二者选择:

+ 当需要经常切换隐藏显示时,如用户选择隐藏与显示某些元素,使用v-show

## 列表渲染

### v-for

1. 用于展示列表数据
2. 语法：v-for = "(item,index) in xxx" :key = "yyy"
3. 可遍历：数组，对象，字符串（很少），指定次数（很少）

```html
<body>
<!--Vue层 模板-->
<div id="array">
  <ul>
    <li v-for="item in items">
      {{item.massage}}
    </li>
  </ul>
</div>
<script>
  let array = new Vue({
    el: "#array",
    data: {
      items: [
        {massage: 'Volerde'},
        {massage: 'SunDog'}
      ]
    }
  });
</script>
</body>
```

```javascript
const app = Vue.createApp({
    data(){
        return {
            listArray:[
                'Ruby',
                'Blake',
                'Yang'
            ],
            listObject:{
                GirlOne:'Ruby',
                GirlTwo:'Blake',
                GirlThree:'Yang'
            }
        }
    },
    methods: {
      handleChangeBtnClick() {
          this.listArray.push('Vue')
      }
    },
    template:`
        <ul>
            <li
                v-for="(item,index) in listArray"
                :key="index+item"
            >
                [{{ index }}]{{ item }}
            </li>
        </ul>
        <button @click="handleChangeBtnClick">Click to change</button>
        <ul>
            <li
                v-for="(value,key,index) in listObject"
                :key="key"
            >
              [{{ index }}]{{ key }}:{{ value }}
            </li>
        </ul>
        <span v-for="number in 99">{{ number }} |</span>
		<!--Vue还可以用于循环数字-
    `
})
app.mount("#app")
```

**v-for循环时**

+ 循环数组: v-for = "(item,index) in listArray"
+ 循环对象: v-for = "(value,key,index) in listObject"

点击按钮时，页面上加了一个新的内容，实际整个列表都被重新渲染了。在实际工作中，这样的代码是不被允许的，它会降低页面的性能，在数据量变多的时候，用户用起来会变的卡顿。

这时，可以加唯一性`key`值，增加后Vue就会辨认出哪些内容被渲染后并没有变化，而只渲染新变化的内容。

```jsx
<ul>
    <li
        v-for="(item,index)  in listArray"
        :key="index+item"
    >
        [{{index}}]{{item}}
    </li>
</ul>
```

官方不建议使用索引`index`为key值，但此时又为了保持唯一性，所以这里使用了`index+item`进行绑定key值

**v-for&v-if优先级对比**

+ 非兼容:二者作用在同一个元素上时,`v-if`会拥有比`v-for`更高的优先级
+ 2.x 版本中在一个元素上同时使用 `v-if` 和 `v-for` 时，`v-for` 会优先作用。
+ 3.x 版本中 `v-if` 总是优先于 `v-for` 生效。

```vue
<ul>
    <template v-for="(value,key,index) in listObject">
      <li
          :key="key"
          v-if="value !== 'Ruby'"
      >
        [{{ index }}]{{ key }}:{{ value }}
      </li>  
    </template>
</ul>
```

故,可以使用如上方法解决v-for&v-if不兼容的问题.<template>标签属于Vue中的业务逻辑标签,不在HTML中进行展示

### React和Vue中的key有什么作用？(key的内部原理)

1. 虚拟DOM中key的作用：

   key是虚拟DOM对象的标识，当状态中的数据发生变化时，Vue会根据[新数据源]生成[新虚拟DOM]，随后Vue会进行[新虚拟DOM]和[旧虚拟DOM]的差异比较，比较规则如下：

2. 比较规则

   1. 旧虚拟DOM中找到了与新虚拟DOM中相同的key：

      1. 若虚拟DOM中的内容没变，直接使用之前的真实DOM
      2. 若虚拟DOM中的内容变了，则生成新的虚拟DOM，随后替换之前页面中的真实DOM

   2. 就虚拟DOM中未找到与新虚拟DOM中相同的key

      1. 创建新的真实DOM，随后渲染到页面

   3. 用index作为key可能会引发的问题：

      1. 若对数据进行：逆序添加、逆序删除等破坏顺序的操作；

         会产生没有必要的真实DOM更新 ==> 页面效果没问题，但效率低

      2. 如果结构中还包含输入类的DOM

         会产生错误DOM更新 ==>界面有问题

   4. 开发中如何选择key？

      1. 最好使用每条数据的唯一标识作为key，比如id，手机号，身份证号，学号等唯一标识
      2. 如果不存在对数据的逆序添加、逆向删除等破坏顺序的操作，仅用于渲染列表进行展示，使用index作为key是没有问题的

### 列表过滤示例

```javascript
    //watch实现
    var vm = new Vue({
        el: '#app',
        data(){
          return{
            keyworld:'',
            persons:[
              {id: '001', name: '马冬梅', age: 16,sex:'女'},
              {id: '002', name: '周冬雨', age: 26,sex:'女'},
              {id: '003', name: '周杰伦', age: 36,sex:'男'},
              {id: '004', name: '温兆伦', age: 36,sex:'男'},
            ],
            filterPersons:[],
          }
        },
        watch: {
          keyworld:{
            immediate: true,
            handler: function(val){
              this.filterPersons = this.persons.filter((p)=>{
                return p.name.indexOf(val) != -1;
              })
            }
          }
        }
    })

    //computed实现
    var vm = new Vue({
      el: '#app',
      data(){
        return{
          keyworld:'',
          persons:[
            {id: '001', name: '马冬梅', age: 16,sex:'女'},
            {id: '002', name: '周冬雨', age: 26,sex:'女'},
            {id: '003', name: '周杰伦', age: 36,sex:'男'},
            {id: '004', name: '温兆伦', age: 36,sex:'男'},
          ]
        }
      },
      computed:{
        filterPersons(){
          return this.persons.filter((p)=>{
            return p.name.indexOf(this.keyworld) != -1
          })
        }
      }
    })
```

### 列表排序示例

```javascript
    var vm = new Vue({
      el: '#app',
      data(){
        return{
          sortType: 0,//0原顺序 1降序 2升序
          keyworld:'',
          persons:[
            {id: '001', name: '马冬梅', age: 18,sex:'女'},
            {id: '002', name: '周冬雨', age: 16,sex:'女'},
            {id: '003', name: '周杰伦', age: 26,sex:'男'},
            {id: '004', name: '温兆伦', age: 36,sex:'男'},
          ]
        }
      },
      computed:{
        filterPersons(){
          const array = this.persons.filter((p)=>{
            return p.name.indexOf(this.keyworld) != -1
          })
          if(this.sortType){
            array.sort((p1,p2)=>{
              return this.sortType == 1 ? p2.age - p1.age : p1.age - p2.age
            })
          }
          return array
        }
      }
    })
```

### Vue的数据监视

1. Vue监视data中所有层次的数据

2. 如何监视对象中的数据？

   通过setter实现监视，且要在new Vue就传入要监视的数据。

    	1.	对象中后追加的属性，Vue默认不做响应式处理
    	2.	如需给后加入的属性做响应式，使用如下API：
    	 	1.	Vue.set(target,propertyName/index,value)
    	 	2.	vm.$set(target,propertyName/index,value)

3. 如何监视数组中的数据？

   通过包裹数组更新元素的方法实现，本质就是做了两件事：

   1. 调用原生的方法对数组进行更新
   2. 重新解析模板，进而刷新页面

4. 在Vue中修改数组中的某个元素一定用以下方法

   1. 使用这些API：push()、pop()、shift()、unshift()、splice()、sort()、reverse()
   2. Vue.set()或vm.$set()

**注：**Vue.set()和vm.$set()不能给vm或vm的根目录对象添加属性

## 双向绑定

### 双向数据绑定

Vue.js是-个MVVM框架，即数据双向绑定，即当数据发生变化的时候，视图也就发生变化，当视图发生变化的时候,数据也会跟着同步变化。这也算是Vue.js的精髓之处了。

值得注意的是，数据双向绑定，一定是对于UI控件来说的，非UI控件不会涉及到数据双向绑定。单向数据绑定是使用状态管理工具的前提。如果我们使用vuex ，那么数据流也是单项的，这时就会和双向数据绑定有冲突。

### 为什么要实现数据的双向绑定

在Vue.js中,如果使用vuex，实际上数据还是单向的，之所以说是数据双向绑定，这是用的UI控件来说，对于我们处理表单，Vue.js的双向数据绑定用起来就特别舒服了。即两者并不互斥,在全局性数据流使用单项，方便跟踪;局部性数据流使用双向，简单易操作。

### 在表单中使用数据双向绑定

可以用`v-model`指令在表单<input>、<textarea> 及<select> 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但v-model本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

**`v-model`会忽略所有表单元素的value、checked、 selected特性的初始值而总是将Vue实例的数据作为数据来源。应该通过JavaScript在组件的data选项中声明初始值!**

输入实时显示

```html
<div id="app">
    <input type="text" v-model="massage">{{massage}}
</div>

<script>
  var app = new Vue({
    el: "#app",
      data: {
        massage:""
      }
  })
```

单选内容显示

```html
<body>
<div id="app">
    sex:
    <input type="radio" name="sex" value="man" v-model="massage"> man
    <input type="radio" name="sex" value="woman" v-model="massage"> woman
    
    <p>
        checked:{{massage}}
    </p>
</div>

<script>
  var app = new Vue({
    el: "#app",
      data: {
        massage:''
      }
  })
</script>
</body>
```

复选框显示

```html
<body>
<div id="app">
    <select v-model="selected">
        <option value="" disabled>--请选择--</option>
        <option>A</option>
        <option>B</option>
        <option>C</option>
        <!--为支持iOS多端适配，加入一个属性为disable的选项-->
    </select>

    <p>
        checked:{{selected}}
    </p>
</div>

<script>
  var app = new Vue({
    el: "#app",
      data: {
        selected:''
      }
  })
</script>
</body>
```

```vue
const app = Vue.createApp({
    data(){
        return {
            message:'',
            check:false,
            checked:[],
            witchOne:''
        }
    },
    template:`
        <div>
            {{ message }}
            <input type="text" v-model="message">
            <textarea v-model="message" />
            <!--Vue3实现了使用单个textarea＋v-model的形势来进行数据绑定-->
            <div>
              {{ check }}<input type="checkbox" v-model="check">
            </div>
            <div>
                {{ checked }}
                Ruby<input type="checkbox" v-model="checked" value="Ruby">
                Blake<input type="checkbox" v-model="checked" value="Blake">
                Yang<input type="checkbox" v-model="checked" value="Yang">
                <!--多选，checked为数组形式变量，被选中的标签的value值通过v-model绑定到checked中-->
            </div>
            <div>
                {{ witchOne }}
                Ruby<input type="radio" v-model="witchOne" value="Ruby">
                Blake<input type="radio" v-model="witchOne" value="Blake">
                Yang<input type="radio" v-model="witchOne" value="Yang">
                <!--单选，使用v-model绑定withOne变量，当被选中时，withOne的值改为选中的标签的value值-->
            </div>
        </div>
    `
}).mount("#app")
```
**注意:如果`v-model`表达式的初始值未能匹配任何选项，<select> 元素将被渲染为"未选中”状态。在iOS中，这会使用户无法选择第一个选项。因为这样的情况下，ios 不会触发change事件。因此,更推荐像上面这样提供一个值为空的禁用选项。**

```vue
    Vue.createApp({
        data(){
            return {
                list:['Ruby', 'Blake', 'yang'],
                name:""
            }
        },
        methods: {
          handleAddCharacter() {
              this.list.push(this.name);
              this.name = '';
          }
        },
        template:`
          <div>
            <button @click="handleAddCharacter">Add Character</button>
            <input type="text" v-model="name"/>
          <ul>
              <li v-for="(item,index) of list"><{{index+1}}>{{item}}</li>
              <!--()中的顺序不能改变-->
            </ul>
          </div>
        `
    }).mount("#app");
```

```vue
const app = Vue.createApp({
    data(){
        return {
            checked: false,
            name:'Unchecked'
        }
    },
    template:`
        <div>
            <div>
                {{ checked }}
                <input
                    type="checkbox"
                    v-model="checked"
                >
            </div>
            <div>
                {{ name }}
                <input
                    type="checkbox"
                    v-model="name"
                    true-value="Hello, world!"
                    false-value="Vue!"
                >
                <!--通过使用true-value&false-value来绑定选中与未选中时此标签的值，代替了默认的值“true&false”-->
            </div>
        </div>
    `
})
app.mount("#app")
```

### 表单双向绑定中的修饰符

```vue
const app = Vue.createApp({
    data(){
        return {
            checked: false,
            name:'Unchecked',
            message:''
        }
    },
    template:`
        <div>
            <div>
                {{ checked }}
                <input
                    type="checkbox"
                    v-model="checked"
                >
            </div>
            <div>
                {{ name }}
                <input
                    type="checkbox"
                    v-model="name"
                    true-value="Hello, world!"
                    false-value="Vue!"
                >
            </div>
            <div>
                <input type="text" v-model.lazy="message">
                <!--lazy修饰符，只有在标签失去焦点时数据才会变化-->
                {{ message }}
                {{ typeof message}}
              <!--typeof message显示当前的message的类型-->
                <input type="text" v-model.number="message">
              <!--输入框中的默认为string字符串类型，number修饰符可以将输入的数字转换为number类型-->
                <input type="text" v-model.trim="message">
                <!--自动将输入框中的空格取消-->
            </div>
        </div>
    `
})
app.mount("#app")
```

## 组件

组件是可复用的vue实例，就是一组可以重复使用的模板，和JSTL的自定义标签、Thymeleaf的`th:fragment`等框架有异曲同工之妙。通常，一个应用会以一棵嵌套的组件树的形式来组织

![1652706136853.png](http://lsky.volerde.space/i/2022/05/16/62824b59388d9.png)

组件编写：

```html
<body>
<div id="app">
    <my-first-component v-for="item in items" v-bind:target="item"></my-first-component>
    <!--v-bind:targetのtarget相当于绑定了target,item则相当于实参-->
</div>

<script>
    // 定义Vue组件component
    Vue.component("my-first-component",{
        // 使用props属性传递参数，props的参数默认不能大写
        props:['target'],// props这里的"target"相当于方法＋形参
        template:'<li>{{target}}</li>'// 相当于将形参的内容显示出来
    })

  let app = new Vue({
    el: "#app",
      data: {
        items:['JAVA','JAVASCRIPT','LINUX']
      }
  })
</script>
</body>
```

+ `Vue-component()`:注册组件
+ `my-first-component`:自定义组件名
+ `template`:组件模板
+ `v-for="item in items"`:遍历Vue实例中定义的名为`items`的数组，并创建同等数量的组件
+ `v-bind:target="item"`：将遍历的item项绑定到组件中`props`定义的名为`target`的属性上；`=`左边的`target`为`props`定义的属性名，右边为`item in items`中遍历的item项的值

```html
<body>
<div id="app"></div>
</body>
<script>
    const app = Vue.createApp({
        data(){
            return {
                list:['Ruby', 'Blake', 'yang'],
                name:""
            }
        },
        methods: {
            handleAddCharacter() {
                this.list.push(this.name);
                this.name = '';
            }
        },
        template:`
          <div>
            <myTitle/>
            <button @click="handleAddCharacter">Add Character</button>
            <input type="text" v-model="name"/>
          <ul>
              <myCharacters
                v-for="(item,index) of list"
                v-bind:item="item"
                v-bind:index="index+1"
              />
              <!--()中的顺序不能改变-->
            </ul>
          </div>
        `
    })
    app.component('myTitle',{
        template:`<h1 style="text-align:center">rwby</h1>`
    })
    app.component('myCharacters',{
        props: ['index', 'item'],
        template:`
            <li><{{index}}>{{item}}</li>
        `
    })
    app.mount("#app");
</script>
```

### 全局组件定义和复用性

**全局组件**定义

```vue
const app = Vue.createApp({
    template:`
        <Website />
        <Description />
    `
});
app.component('Website',{
    template:`
        <h2>Hello World</h2>
    `
})
app.component('Description',{
    template:`
        <div>Vue</div>
    `
})
```

全局组件

+ 优
  + 定义在全局中，在任何地方都可以使用，方便简单
+ 缺
  + 需要加载完全部全局组件才能打开界面，因此网页打开会变慢
  + 当页面加载有就一直存在，会对性能产生一定影响

**复用性**

每个组件单独运行，互不干扰

```vue
const app = Vue.createApp({
    template:`
        <Website />
        <Description />
        <count />
        <count />
        <count />
		<!--三个count互不干扰，彼此独立运行-->
    `
});
app.component('Website',{
    template:`
        <h2>Hello World</h2>
    `
})
app.component('Description',{
    template:`
        <div>Vue</div>
    `
})
app.component('count',{
    data(){
        return {
            count:'0'
        }
    },
    template:`
        <div @click="count++">
            {{ count }}
        </div>
    `
})
const vm = app.mount("#app");
```

### 局部组件创建和注册方法

局部组件需要注册才能使用

> 通过此处进行局部组件的创建

```vue
const count = {
    data(){
        return {
            count:0
        }
    }, 
    template:`
        <div @click="count++">
            {{ count }}
        </div>
    `
}
```

>  通过此处进行局部组件的注册，此为简写法

```vue
const app = Vue.createApp({
    components:{count},
    template:`
        <Website />
        <count />
    `
});
```

> 通过键值对进行注册

```vue
const app = Vue.createApp({
    components:{
		click-add-one:count
	},
    template:`
        <Website />
        <click-add-one />
		<!--需要注意的是，对于驼峰命名法有潜规则为：采用键值加横杠的形式，如“click-add-one”-->
    `
});
```

### 父子组件的静态和动态传值

**静态传值**

```vue
    const app = Vue.createApp({
        template:`
            <div>Vue</div>
            <son name="Vue" />
        `
    });
    app.component('son',{
        props:['name'],
        template:`
            <div>{{ name }} div</div>
        `
    })
    const vm = app.mount("#app");
```

+ 父组件使用name关键字将值传给子组件，子组件通过props以数组的形式绑定name关键字，并在模板中{{name}}应用
+ 静态传值只能传字符串类型的值

**动态传值**

```vue
const app = Vue.createApp({
    data(){
        return {
            name:'Hello, world'
        }
    },
    template:`
        <div>Vue</div>
        <son :name="name" />
    `
});
app.component('son',{
    props:['name'],
    template:`
        <div>{{ name }} div</div>
    `
})
const vm = app.mount("#app");
```

+ 使用v-bind绑定父组件中的name属性，在data（）中定义name

+ 动态传值可以传非字符串类型的值

+ 动态传值可以传入函数

  ```vue
      const app = Vue.createApp({
          data(){
              return {
                  name:'Hello, world',
                  pay:()=>{
                      alert("Long May The Sunshine")
                  }
              }
          },
          template:`
              <div>Vue</div>
              <son :name="name" :pay="pay" />
          `
      });
      app.component('son',{
          props:['name','pay'],
          methods:{
              handleClick() {
                  alert("money money money")
                  this.pay()
              }
          },
          template:`
              <div @click="handleClick">{{ name }} div</div>
          `
      })
      const vm = app.mount("#app");
  ```

  使用v-bind:pay=“pay”绑定data（）中的pay函数，子函数中，用props接收pay

### 组件传值的校验

**对传入的值的类型进行校验**

```vue
    app.component('son',{
        props:{ // 不使用数组形式，而是使用对象的形式对传入的值进行校验
            name:String, // 类型的校验 支持 Sting Boolean Array Object Function Symbol
        },
        template:`
            <div>{{ name }}</div>
        `
    })
```

**同时进行类型与非空等多个校验**

```vue
    app.component('son',{
        props:{
            name:{
                type:String, // 类型判断
                required:true // 非空判断
				default:'Volerde' // 默认值
				validator:function(value){
					// 任意逻辑代码
					return value.search("Volerde") != -1 // search：JavaScript查找是否出现“Volerde”，返回值为起始下标
     			}
            },
        },
        template:`
            <div>{{ name }}</div>
        `
    })
```

使用{}，以对象的形式进行多个校验

### 单向数据流机制

```vue
const app = Vue.createApp({
    data(){
        return {
            count:0
        }
    },
    template:`
        <div>Vue</div>
        <counter :count="count"/>
    `
})
app.component('counter',{
    props:['count'],
    template:`
        <div @click="count++">{{ count }}</div>
    `
})
const vm = app.mount("#app")
```

此时并不能实现点击加一的效果。这就是单向数据流机制限制的结果

**什么是单向数据流**

> 所有的prop都使得其父子prop之间形成了一个单向下行绑定：父级prop的更新会向下流动到子组件中，反过来则不行。——Vue官方

简单来说就是

+ 数据从父级组件传递给子组件，只能单向绑定。子组件内部不能直接修改从父组件传递过来的数据。

这就解释了为什么上述的程序不能正常工作的原因了。

```vue
app.component('counter',{
    props:['count'],
    data(){
        return {
            newCount: this.count
        }
    },
    template:`
        <div @click="newCount++">{{ newCount }}</div>
    `
})
```

这样就可以进行修改了

总结：

+ 单向数据流就是父组件可以向子组件传递数据，但是子组件不能修改数据

**为什么要有单向数据流机制**

单向数据流的最终目的就是为了降低组件的耦合段，增加独立性。当页面中同时调用类三个<counter />组件时，没有单向数据流的机制，很容易变成一个组件数值变化，其他的组件的数值也跟着变化的现象。这会使得页面中组件的数据耦合在一起，没办法单独使用。

### non-props使用

**什么是non-props**

子组件没有接收父组件传递过来的参数，而子组件完全不变的复制了父组件的属性到自己的标签上，就叫做non-props属性

```vue
const app = Vue.createApp({
	template:`
      <Hello msg="Hello"/>
    `
})
app.component('Hello',{
    template:`
        <div>Hello World</div>
    `
})
```

如上述，`msg="Hello"`传给了子组件,但是子组件并没有接收,则此时,`msg="Hello"`这个属性就会出现在子组件的标签上

```vue
const app = Vue.createApp({
    template:`
      <Hello msg="Hello"/>
    `
})
app.component('Hello',{
    props:['msg'],
    template:`
        <div>Hello World</div>
    `
})
```

现在给子组件加上了`props:['msg']`后,子组件接收父组件传来的值,`msg="Hello"`这个属性不会出现在子组件的标签上

```vue
app.component('Hello',{
    inheritAttrs:false,
    template:`
        <div>Hello World</div>
    `
})
```

可以通过在子组件中设置`inheritAttrs:false`的方式,来禁止non-props.此属性默认为`true`

**一些应用**

通过non-props给子组件设置样式

```vue
const app = Vue.createApp({
    template:`
      <Hello style="color: red"/>
    `
})
app.component('Hello',{
    template:`
        <div>
            <div>Hello World!</div>
            <div>Hello World!</div>
            <div>Hello World!</div>
        </div>
    `
})
```

给子组件中某个特定标签传递属性

```vue
const app = Vue.createApp({
    template:`
      <Hello style="color: red"/>
    `
})
app.component('Hello',{
    template:`
            <div v-bind="$attrs">Hello World!</div>
            <div>Hello World!</div>
            <div>Hello World!</div>
    `
})
```

通过使用`v-bind="$attrs"`,将父组件传给子组件的全部属性都绑定到某个特定的标签上

```vue
const app = Vue.createApp({
    template:`
      <Hello style='color: red' msg="Hello"/>
    `
})
app.component('Hello',{
    template:`
            <div v-bind:style="$attrs.style">Hello World!</div>
            <div>Hello World!</div>
            <div>Hello World!</div>
    `
})
```

通过`v-bind:style="$sttrs.style"`的方式可以只将style的值传给特定的标签

**在业务逻辑中使用non-props**

```vue
const app = Vue.createApp({
    template:`
      <Hello style='color: red' msg="Hello"/>
    `
})
app.component('Hello',{
    mounted(){
        console.log(this.$attrs.msg)
    },
    template:`
            <div v-bind:style="$attrs.style">Hello World!</div>
            <div>Hello World!</div>
            <div>Hello World!</div>
    `
})
```

`this.$attrs`是Vue的一个常用的属性

### 子组件调用父组件方法,传值和校验

**子组件调用父组件**

```vue
methods:{
	handleAddCounter(){
		this.count++
	}
},
template:`
	<div>Vue</div>
	<counter :count="count" @add="handleAddCounter"/>
`

app.component('counter',{
    props:['count'],
    emits:['add'],
    methods:{
        handleClick(){
            this.$emit('add')
        }
    },
    template:`
        {{ count }}
        <div @click="handleClick">Click me</div>
    `
})
```

父组件中声明了方法:`handleAddCounter`

子组件通过在`template`中编写一个`click`事件,然后调用**子组件的`handleClick`方法**,在子组件的方法中通过`$emit`调用父组件的响应事件`add`.使用`emit`需要先在子组件中进行声明

`add`为响应事件,在父组件的`template`中,添加一个`add`响应事件,然后响应事件再去调用方法`handleAddCounter`.

**子组件向父组件传递参数**

```vue
methods:{
    handleClick(){
        this.$emit('add',2)
    }
},
```

如上述,在`$emit('add',2)`加入了2这个参数,在父组件中可以使用在方法中接收这个参数`param`

```vue
methods:{
    handleAddCounter(param){
        this.count +=param
    }
},
```

此时就完成了子组件向父组件传值的操作.

实际操作中更推荐将业务逻辑写在子组件中,只将最后的结果传递给父组件

> 父组件

```vue
methods:{
    handleAddCounter(param){
        this.count = param
    }
},
```

> 子组件

```vue
methods:{
    handleClick(){
        this.$emit('add',this.count+2)
    }
},
```

**对传递值进行校验**

```vue
emits:{
    add:(value)=>{
        return value < 20
    }
},
```

通过`emit`进行传递值的校验,上述为校验传递到`add`的值不大于20,返回值为布尔型

### 组件中插槽的使用(Slot)



## 生命周期

Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载DOM、渲染-->更新-->渲染、卸载等一系列过程，我们称这是Vue的生命周期。即Vue实例从创建到销毁的过程，就是生命周期。

在Vue的生命周期内，它提供了一些列的事件，可以让我们在事件触发时创建JS方法，可以让我们创建的JS控制整个大局，让这些方法响应中的this指向的是Vue的实例。

![1652706136855.png](http://lsky.volerde.space/i/2022/05/16/62824b5a22103.png)

```html
<body>
  <div id="app"></div>
</body>
<script>
  // 生命周期函数->在某些时刻会自动执行的函数
  const app = Vue.createApp({
    data(){
      return {
        message: 'Hello World!'
      }
    },
    methods: {
      handleItemClick(){
        this.message = this.message === 'Hello World!' ? 'Vue' : 'Hello World!';
      }
    },
    beforeCreate(){
      console.log("beforeCreate,在实例生成之前会自动执行的函数")
    },
    created(){
      console.log("created,实例生成之后会自动执行的函数")
    },
    beforeMount(){
      console.log("beforeMount,在模板渲染完成之前执行的函数")
    },
    mounted(){
      console.log("mounted,在模板渲染完成之后执行的函数")
    },
    beforeUpdate(){
      console.log("beforeUpdate,在更新前")
      console.log(document.getElementById('app').innerHTML) // 打印出来为Hello World,为改变之前的值
      // console.log(this.message) // 此处不得使用console.log打印this.message   会报错,原因未知->待解决
    },
    updated(){
      console.log("updated,在更新后")
      console.log(document.getElementById('app').innerHTML) // 打印出来为Vue,为改变之后的值
      // console.log(this.message) // 此处不得使用console.log打印this.message   会报错,原因未知->待解决
    },
    beforeUnmount(){
      console.log("beforeUnmount,在VUe应用卸载完成前执行的函数")
      console.log(document.getElementById('app').innerHTML) // 能打印出来结果

    },
    unmounted(){
      console.log("unmounted,在Vue应用卸载完成后执行")
      console.log(document.getElementById('app').innerHTML) // 打印不出来,结果为空

    },
    template:'<div @click="handleItemClick">{{message}}</div>'
  })
  app.mount("#app")
</script>
```

## 模板

### 动态参数和阻止默认事件

```javascript
    const app = Vue.createApp({
        data() {
            return {
                message: 'Volerde',
                name: 'title2',
                event: 'click'
            }
        },
        methods: {
            alertClick() {
                alert("Long May The Sunshine")
            },
            handleClick() {
                alert("Vue")
            }
        },
        template: `
          <div
              v-bind:title="message"
              v-on:[event]="alertClick"
          >
          {{ message }}
          </div>
          <!--v-bind:title&v-on:click均可以简写,以下为简写方式-->
          <div
              :[name]="message"
              @click="alertClick"
          >
          {{ message }}
          </div>
          <form action="https://www.volerde.space" @click.prevent="handleClick">
            <button type="submit">submit</button>
          </form>
          <!--此处name&event为data中的name&event-->
        `
    })
    app.mount("#app")
```

### 条件判断

使用三目运算符进行判断,从而改变class

```html
<script>
    const app = Vue.createApp({
        data() {
            return {
                message: 'Hello, world'
            }
        },
        methods: {
            messageChangeClick() {
                this.message = this.message === 'Hello, world' ? 'Vue!' : 'Hello, world'
            }
        },
        template: `
          <div
              @click="messageChangeClick" :class="message === 'Hello, world' ? 'one' : 'two'"
          >
              {{ message }}
          </div>
        `
    });
    app.mount("#app");
</script>
<style>
    .one {
        color: red
    }

    .two {
        color: green
    }
</style>
```

### 样式绑定

```html
<style>
    .color-red {
        color:red !important;
    }
    .color-green {
        color:green
    }
    .background-color-orange {
        background-color:orange
    }
</style>
<script>
    const app = Vue.createApp({
        data(){
          return { // 通过定义变量来操控css样式
              colorSelect:"color-red", // 直接使用字符串控制css
              colorObject:{ // 使用对象来控制css
                  'color-red':false,
                  'background-color-orange':true
              },
              colorArray:[ // 使用数组＋嵌套对象的方式
                  'color-green',
                  'background-color-orange',
                  {
                      'color-red':true
                  }
              ]
          }
        },
        methods:{
          colorChange() {
            this.colorSelect = this.colorSelect === "color-red" ? "color-green" : "color-red";
          }
        },
        template:`
            <h2
                @click="colorChange"
                :class="colorArray"
            >
                Volerde
            </h2>
        `
    }).mount("#app")
</script>
```

```javascript
    const app = Vue.createApp({ // createApp生成的一般叫做父组件
        template:`
            <DIYComponent />
        `
    })
    app.component('DIYComponent',{ // 被调用的,一般称为子组件
        template:`
            <div>So,now you can see this component</div>
        `
    })
    app.mount("#app")
```

**子组件的坑:**

修改一下子组件，再写一个`<div>`进去，里边写上`Vue`

```jsx
app.component('sonCom',{
    template:`
        <div>SonCom</div>
        <div>VUe</div>
    `
})
```

结果:两个`<div>`的样式都不起作用了.

解决方法:在两个并列的`<div>`外层，加上一个包括性的标签就可以了.即让子组件的最外层只有一个根元素。

```jsx
app.component('sonCom',{
    template:`
        <div>
            <div>SonCom</div>
            <div>Vue</div>
        </div>
    `
})
```

### 行内样式

```javascript
const app = Vue.createApp({
    data(){
        return {
            styleString:'color:orange', // 字符串形式
            styleObject:{ // 对象样式
                color: 'orange',
                background:'yellow'
            }
        }
    },
    template:`
        <div :style="styleObject">Volerde</div>
        <!--通过v-bind:style来绑定行内css样式-->
    `
})
app.mount("#app")
```

## 计算属性

1. 定义：要用的属性不存在，需要通过已有属性计算得来
2. 原理：底层借助了Object.defineproperty方法提供的getter和setter
3. get的调用时机
   1. 初次读取fullName时
   2. 所依赖的数据发生变化时
4. 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便
5. 备注：
   1. 计算属性最终会出现在vm上，直接读取即可
   2. 如果计算属性要被修改，那必须写setter去响应修改，且setter中要引起计算时依赖的数据发生改变

### 什么是计算属性

计算属性的重点突出在属性二字上，首先是属性，其次是计算的能力，此计算为一个函数：即这是一个能够将计算结果缓存起来的一个属性，暂且可以理解为缓存

计算属性的特性,当计算属性以来的内容发生改变时,才会重新计算

```javascript
    const app = Vue.createApp({
        data(){
            return {
                message:'Hello, world',
                price:10,
                count:2
            }
        },
        computed:{ // 计算属性,当计算属性以来的内容发生改变时,才会重新计算
        // 当首次渲染页面时,也会计算一遍属性
            total(){
              return this.price * this.count // 计算属性必须返回一个值
            }
        },
        methods:{
            messageChangeClick(){
                this.message = this.message === 'Hello, world' ? 'Vue!' : 'Hello, world'
            },
            getTotal(){ // 只要页面重新渲染,就会重新执行此方法
                return this.price * this.count
            },
            addCount(){
                this.count++
            }
        },
        template:`
            <h2 @click="messageChangeClick">{{ message }}</h2>
            <div>{{ total }}</div>
            <div>{{ getTotal() }}</div>
            <button @click="addCount">get one</button>
        `
    });
    app.mount("#app");
```

## 侦听器(监听器)-watch

1. 当被监视的属性变化时，回调函数自动调用，进行相关操作
2. 监视的属性必须存在，才能进行监视
3. 监视的两种写法：
   1. new Vue时传入watch配置
   2. 通过vm.$watch监视 
4. 深度监视：
   1. Vue中的watch默认不检测对象内部值的改变
   2. 配置deep:true可以检测对象内部值的改变
5. 备注：
   1. Vue自身可以检测对象内部值的改变，但Vue提供的watch默认不可以
   2. 使用watch时根据数据的具体结构，决定是否采用深度监视

```javascript
const app = Vue.createApp({
    data(){
        return {
            price:10,
            count:2
        }
    },
    watch:{ // 当监听的值发生改变时调用
        // 与computed的区别:computed在首次进入页面时会调用一次,而watch只有在监听的数值改变时才会调用
        count(current,prev){ // 监听哪个值,就写哪个值对应的方法
            // 监听器可以不用返回值
            console.log("count changed")
            console.log("现在的值:"+current+"\n"+"以前的值:"+prev)
        }
    },
    computed:{
        total(){
            return this.price * this.count
        }
    },
    methods:{
        addCount(){
            this.count++
        }
    },
    template:`
        <div>{{ total }}</div>
        <button @click="addCount">get one</button>
    `
});
app.mount("#app");
```

```javascript
watch:{
    //正常写
     isHot:{
       immediate:true,//初始化时让handler调用一下
       handler(val,prev){
         console.log("change",val,prev)
       }
     },
    //简写
    isHot(val,prev){
    console.log("change",val,prev)
    }
}
//正常写
vm.$watch('isHot',{
    immediate:true,
    deep:true,
    handler(val,prev){
        console.log("change",val,prev)
    }
})
简写
vm.$watch('isHot',function(val,prev){
    console.log("change")
})
```

**method,watch,computed三者使用的优先级**

当三者可以实现某个具体的功能时,他们的优先级

+ `computed`和`method`都能实现的功能,建议使用`computed`,因为有缓存,不用渲染页面就刷新
+ `computed`和`watch`都能实现的功能,建议使用`computed`,因为更加简洁

computed和watch之间的区别

1. computed能完成的功能，watch都能完成
2. watch能完成的功能，computed不一定能完成，例：watch可以完成异步操作
3. 注意：
   1. 所有被Vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象
   2. 所有不被Vue管理的函数（定时器的回调函数，Ajax的回调函数，promise的回调函数等），最好写成箭头函数，这样this的指向才是vm或组件实例对象

## 样式绑定

1. class样式

   + 写法

     :class = "xxx" xxx可以是字符串、对象、数组

     字符串写法，适用于样式的类名不不确定，需要动态指定串

     数组写法，适用于要绑定的样式个数不确定，名字也不确定

     对象写法，适用于要绑定的样式个数确定，名字也确定,但要动态决定用不用

2. style样式

   + :style = "{fontSize: xxx}"其中xxx是动态值
   + :style = "[a,b]"其中a、b是样式对象

   

### Class样式绑定

```html
  <!-- 绑定class样式  字符串写法，适用于样式的类名不不确定，需要动态指定 -->
  <div class="container" :class="normal" @click="Change">{{name}}</div>

  <!-- 绑定class样式  数组写法，适用于要绑定的样式个数不确定，名字也不确定 -->
  <div class="container" :class="classArray">{{name}}</div>

  <!-- 绑定class样式  对象写法，适用于要绑定的样式个数确定，名字也确定,但要动态决定用不用 -->
  <div class="container" :class="classObject">{{name}}</div>
```

### Style样式绑定

```html
  <!-- 绑定style样式  对象写法 -->
  <div class="container" :style="styleObject">{{name}}</div>
  
  <!-- 绑定style样式  数组写法 -->
  <div class="container" :style="styleArray">{{name}}</div>
```



## 绑定事件

事件的基本使用：

1. 使用v-on:xxx或@xxx绑定事件，其中xxx是事件名
2. 事件的回调需要配置在methods对象中，最终会在vm上
3. methods中配置的函数，不要用箭头函数，否则this就不是vm了
4. methods中配置的函数，都是被Vue所管理的函数，this的指向是vm或组件实例对象
5. @click="demo"和@click="demo($event)"效果一致，但后者可以传参

### 方法和参数

```javascript
const app = Vue.createApp({
    data(){
        return {
            count:0
        }
    },
    methods: {
        addCountClick(num,event) {
            this.count+=num
            console.log(event.target)
        },
        alertBtnClick1(){
            alert(1)
        },
        alertBtnClick2(){
            alert(2)
        }
    },
    template:`
      <div>{{ count }}</div>
      <button @click="addCountClick(2,$event)">Click</button>
      <button @click="count++">Click</button><!--可以直接用表达式-->
      <button @click="alertBtnClick1(),alertBtnClick2()"></button>
    `
})
app.mount("#app")
```

@click="addCountClick(2,$event)"

+ 无参数时,写为addCountClick,不用加括号,且在methods中的方法加入参数event即可传入event
+ 有参数时,写为addCountClick(参数1,参数2...),需要传入event时需用"$",写为"\$event"
+ 要触发两个事件时,用","隔开,并**需要加上()**

### 事件修饰符

1. prevent：阻止默认事件
2. stop：阻止事件冒泡
3. once：事件只触发一次
4. capture：使用事件的捕获模式
5. self：只有event.target是当前的操作对象时才会触发事件
6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕

>  冒泡事件			.stop

```vue
template:`
    <div @click="alertBtnClick">
        <div>{{ count }}</div>
        <button @click="addCountClick">Click</button>
    </div>
`
```

当点击button时,程序会向上冒泡,先触发addCountClick事件,然后触发alertBtnClick事件

**解决方法:@click后面加上.stop阻止冒泡事件**

```vue
template:`
    <div @click="alertBtnClick">
        <div>{{ count }}</div>
        <button @click.stop="addCountClick">Click</button>
    </div>
`
```

> .self

```vue
template:`
    <div @click.self="alertBtnClick">
        点击本元素才会触发.self,即alertBtnClick事件只会响应本元素的触发,不会响应子元素的触发
        <div>{{ count }}</div>
        <button @click.stop="addCountClick">Click</button>
    </div>
`
```

> .prevent

```vue
        template:`
          <form action="https://www.volerde.space" @click.prevent="handleClick">
            <button type="submit">submit</button>
          </form>
        `
```

阻止默认事件的发生

> .capture

```vue
template:`
    <div @click.capture="alertBtnClick">
        <div>{{ count }}</div>
        <button @click="addCountClick">Click</button>
    </div>
`
```

.capture和冒泡事件正好相反,冒泡事件是先执行字元素中的事件,再执行父元素的事件;而.capture则是先执行父元素的事件,再去执行子元素的事件

> .once

```vue
template:`
    <div @click="alertBtnClick">
        <div>{{ count }}</div>
        <button @click.once="addCountClick">Click</button>
    </div>
`
```

.once只会使事件在第一次执行时成功,此后则会阻止事件发生,即只能点击一次

> .passive
>
> passive主要用在移动端的scroll事件，来提高浏览器响应速度，提升用户体验。因为passive=true等于提前告诉了浏览器，touchstart和touchmove不会阻止默认事件，手刚开始触摸，浏览器就可以立刻给与响应；否则，手触摸屏幕了，但要等待touchstart和touchmove的结果，多了这一步，响应时间就长了，用户体验也就差了。

### 按键与鼠标修饰符

1. Vue中常用的按键别名
   + 回车 => enter
   + 删除 => delete（捕获“删除”和“退格”键）
   + 退出 => esc
   + 空格 => space
   + 换行 => tab
   + 上 => up
   + 下 => down
   + 左 => left
   + 右 => right
2. Vue未提供别名的按键，可以使用按键原始的key值去绑定，但要注意转换为kebab-case（短横线命名）
3. 系统修饰键（用法特殊）：ctrl，alt，shift，meta
   1. 配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才会被触发
   2. 配合keydown使用：正常触发事件
4. 也可以使用keycode去指定具体的按键（不推荐）
5. Vue.config.keycodes.自定义键名 = 键码，可以去定制按键别名

> 按键修饰符

```vue
template:`
    <div>
        <input @keydown.enter="handleKeyDown">
    </div>
`
// 按键修饰符    enter    tab    delete    esc    up    down    left    right
```

> 鼠标修饰符

```vue
template:`
    <div>
        <div @click.right="handleKeyDown">Click with right</div>
    </div>
`
// 鼠标修饰符    left    right    middle 
```



## Axios异步通信

Axios是一个开源的可以用在浏览器端和NodeJS的异步通信框架，它的主要作用就是实现Ajax异步通信

功能特点：

+ 从浏览器中创建`XMLHttpRequests`
+ 从NodeJS中创建http请求
+ 支持PromiseAPI[JS中链式编程]
+ 拦截请求和相应
+ 转换请求数据和响应数据
+ 取消请求
+ 自动转换JSON数据
+ 客户端支持防御XSRF(跨站请求伪造)

**为什么使用Axios**

`Vue.js`是一个视图层框架，并且作者(尤雨溪)严格遵守SoC(关注度分离原则)，所以`Vue.js`不包括AJAX通信功能。为了解决通信问题，作者单独开发了一个名为`vue-resource`的插件，不过在进入2.0版本后停止了对该插件的维护，并推荐使用`Axios`框架。少用jQuery，因为它操作DOM太频繁。

**第一个Axios应用程序**

开发的接口大部分采用JSON格式，创建一个data.json的文件，并填入以下内容，放在项目的根目录下

```json
{
  "name": "Volerde",
  "url": "http://www.volerde.space",
  "page": 1,
  "isNoProfit": true,
  "address": {
    "street": "灵川",
    "city": "桂林",
    "country": "China"
  },
  "links": [
    {
      "name": "bilibili",
      "url": "http://www.bilibili.com"
    },
    {
      "name": "狂神说JAVA",
      "url": "http://www.kuangstudy.com"
    },
    {
      "name": "baidu",
      "url": "http://www.baidu.com"
    }
  ]
}
```

```html
<body>

<div id="app" v-cloak>
    <div>
        {{info.name}}
    </div>
    <a v-bind:href="info.url">Click Me</a>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    var vm = new Vue({
        el:'#app',
        data(){
            return{
                // 请求的返回字符串格式必须和JSON字符串中的一样,貌似可以自动对整格式
                info:{}
            }
        },
        mounted(){// 钩子函数，链式编程 基于ES6的箭头函数编写
            axios.get('../data.json').then(response=>(this.info=response.data));
            axios.get('../data.json').then(function (response){// 无箭头函数版
                this.info=response.data;
            });
        }
    });
</script>
</body>
```

实例：

1. 在这里使用v-bind将a:href的属性值与Vue实例中的数据进行绑定
2. 使用Axios的get方法请求Ajax并自动将数据封装进了Vue实例的数据对象中
3. 在data中的数据结构必须要和Ajax响应回来的数据结构匹配



## 不同版本的Vue

1. Vue.js和Vue.runtime.js的区别：
   1. Vue.js是完整版的Vue，包含：核心功能和模板解析器
   2. Vue.runtime.xxx.js是运行版的Vue，只包含核心功能，不包含模板解析器
2. 因为Vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体的内容