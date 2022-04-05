# Vue3

## 1. Vue基础语法：



### 1. 第一个Vue程序 -- HelloWorld

1. 步骤：

   1. 创建一个html文件，使用CDN的方式引入Vue3。

   2. ```html
      <script src="https://unpkg.com/vue@next"></script>
      ```

   3. 在body中创建一个div标签作为被挂载的对象。

   4. ```html
      <div id="app"></div>
      ```

   5. 创建Vue实例并挂载，Vue.createApp()的参数是一个对象。

   6. ```javascript
      Vue.createApp({   //创建一个Vue实例，简单理解就说，我要使用Vue了
          template: '<div>Hello World</div>'   // template是模板的意思，就是在JS里写html代码
      }).mount("#app")   //这个模板需要放一个位置，也就是说挂载，挂载到`id=app`的DOM上
      ```

      

****



### 2. Vue3编写计数器

1. 重点：数字是个**变量**。

2. 在Vue的template(模板)中使用**变量**，需要用到字面量标识{{}}双大括号。

3. mounted() 是一个生命周期钩子函数。页面加载完成(页面渲染完成)后自动执行。

4. ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>计数器</title>
       <script src="https://unpkg.com/vue@next"></script>
   </head>
   <body>
       <div id="app"></div>
       
   </body>
   <script>
       Vue.createApp({
           data() {
               return {
                   counter: 1,
               }
           },
           mounted() {
               setInterval(() => {
                   this.counter++;
               }, 1000)
           },
           template: '<div>{{counter}}</div>'
       }).mount("#app");
   </script>
   </html>
   ```

5. 启示：使用vue开发相较于原生开发，不需要自己编写DOM，从面向DOM编程转为面向数据编程。



****



### 3. 绑定事件和方法

1. 给标签绑定**点击**事件：v-on:click="方法名"

2. ```html
   <button v-on:click="welcome">顾客前来</button>
   ```

3. 给标签绑定**是否显示**该DOM元素   v-if=“布尔变量”

4. ```html
   <div v-if="isShowMeal" >{{setMeal}}</div>
   ```



****



### 4. 循环展示和双向数据绑定

1. 循环展示某个标签   v-for

2. ```html
   <li v-for="(item,index) of list">[{{index}}]{{item}}</li>
   ```

3. 双向数据绑定一般会涉及到输入框 v-model

4. ```html
   <input v-model="inputValue" />
   ```

5. 完整代码：

6. ```html
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta http-equiv="X-UA-Compatible" content="IE=edge" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <title>循环和双向绑定</title>
       <script src="https://unpkg.com/vue@next"></script>
     </head>
     <body>
       <div id="app"></div>
     </body>
     <script>
       Vue.createApp({
         data() {
           return {
             inputValue: "",
             content: "可是区域",
           };
         },
         mounted() {},
         methods: {
           changeContent() {
             this.content = this.inputValue;
             this.inputValue = "";
           },
         },
         template: `<div>
               <div>
               <button v-on:click="changeContent">将输入的内容展示到下面的区域</button>&nbsp;
               <input v-model="inputValue" @keyup.enter="changeContent"/>
           </div>
               <div>{{content}}</div>
           </div>`,
       }).mount("#app");
     </script>
   </html>
   ```

7. v-model的修饰符：

   1. lazy修饰符 --- 懒更新

      1. ```html
         <div>     {{message}}<input v-model="message" /> </div>
         ```

      2. 上面这行代码的效果：当你在文本框中输入任何内容的时候，插值表达式也会跟着改变。如果你不想马上显示，就可以使用`lazy`修饰符，这样就可以实现当输入完成后，失去焦点再进行改变。

      3. ```html
         <div>
             {{message}}<input v-model.lazy="message" />
         </div>
         ```

   2. number修饰符：

      1. `<input />`输入的内容无论是数字还是字母，最终都会变为`字符串`。如果想最终输入的变成数字，你就可以使用`number`修饰符了。
      2. 加上`number`修饰符后，**你输入的值只要是数字**，就变成了number类型。（也就是说，如果你输入的是字母，它还会是字符串类型）

   3. trim修饰符：

      1. `trim`修饰符大家一定不陌生，它是用来消除`input`框输入内容前后的空格的。现在我们再字符串上输入空格，其实它会在DOM元素上进行增加空格的，这个可以在控制台清楚的看出(详细请看视频操作)。 加入`trim`修饰符后，Vue就会自动给我们去除前后的空格。



****



### 5. 编写组件

1. 组件的意义：降低代码的耦合性，每个人写不同的模块，写好后拼起来更加简单。

2. props:[ ] 是写在子组件中的，表示这些变量是父组件传进来的

3. v-bind:属性名="需要传给子组件的变量的变量名"

4. ```html
   <!DOCTYPE html>
   <html lang="en">
   
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>佳丽列表</title>
       <script src="https://unpkg.com/vue@next"></script>
   </head>
   
   <body>
       <div id="app"></div>
   </body>
   <script>
       const app = Vue.createApp({
           data() {
               return {
                   inputValue: '',
                   list: []
               }
           },
           methods: {
               handleAddItem() {
                   this.list.push(this.inputValue)
                   this.inputValue = ''
               }
           },
           template: `
               <div>
                   <my-title />
                   <input v-model="inputValue" />
                   <button v-on:click="handleAddItem">增加佳丽</button>
                   <ul>
                       <my-jiali 
                           v-for="(item,index) of list"  
                           v-bind:item="item" 
                           v-bind:index="index"  
                       />
                   </ul>
               </div>
   
           `
       })
   
       app.component('my-title', {
           template: '<h1 style="text-align:center">象牙山洗脚城</h1>'
       })
   
       app.component('my-jiali', {
           props: ['item', 'index'],
           template: ` <li >[{{index}}]-{{item}}</li>`
       })
       app.mount("#app")
   </script>
   
   </html>
   ```

5. 



****



### 6.createApp()和mount()方法

1. Vue.createApp() --- 创建一个Vue的应用。
2. mounted() --- 挂载到指定的DOM元素。



****



### 7. MVVM设计模式

1. m: model数据       v：view视图     vm：viewModel视图数据连接层
2. Vue做的事情就是将我们定义的数据和我们定义的模板**自动**进行关联。



****



### 8. 生命周期函数

1. 生命周期函数就是在**某一时刻**会**自动执行**的函数。
2. 四个关键 点：创建、渲染、更新、销毁。



****



### 9. 两个模板语法：

1. 插值表达式：{{}}双大括号就是插值表达式，利用这种形式可以使data中的变量展示在模板中。

   1. 插值表达式输出html标签使用v-html指令。

      1. 注意：这时template需要用反引号抱起来。

      2. ```html
         template: `<h2 v-html="message"> </h2>`
         ```

      3. ```javascript
         return {
             message: '<i>jspang.com</i>'
         }
         ```

      4. 插值表达式中可以使用js表达式，比如三元运算符、加号，但不能使用if等js语句。

   2. 插值表达式只做一次渲染使用v-once指令。加入v-once后无论data中的数据如何变化，模板也不会再重新渲染。

2. v-bind数据绑定：

****



### 10.简写

1. v-on用于绑定**事件**，可以使用@取代。
2. v-bind用于绑定**属性**，可以用：取代。



****



### 11. 模板动态参数

1. 属性动态绑定：

   1. [ ] 方括号加上data中变量名的形式

   2. ```javascript
      const app=Vue.createApp({ 
          data(){
              return{
                  message:'jspang.com' ,
                  name:'title'
              }
          },
          //.........
          template:`
              <h2 
                  @click="hanldClick"
                  :[name]="message"
              >
              {{message}}
              </h2>
          `
      
      })
      ```

2. 事件动态绑定：

   1. 下面的代码的功能类似于绑定了一个点击事件

   2. ```javascript
      // data中这样写
      return{
          message:'jspang.com' ,
          name:'title',
          event:'click'
      }
      // 模板中这样写
      template:`
          <h2 
              @[event]="hanldClick"
              :[name]="message"
          >
          {{message}}
          </h2>
      `
      ```



****



### 12. 阻止默认事件

1. prevent是阻止默认事件的修饰符。最常用的场景就是一个属性为submit的按钮，当点击按钮时，表单就会默认提交到对应的网址。但我不想它这样做，我想自定义点击该按钮的触发事件，这时就可以用@click.prevent="自定义事件名"。



****



### 13.计算属性 - computed

1. 计算属性的特性是：当**计算属性依赖的内容发生变更**时，才会重新执行计算。
2. methods和mounted的区别：
   1. 方法methods：只要页面重新渲染就会重新执行方法。
   2. 计算属性mounted：只有计算属性依赖的内容发生改变才会重新执行计算。



****



### 14. Vue中的侦听器 / 监听器 --- watch

1. 作用：侦听data中的值的变化。

2. 侦听器中的方法可以接收两个参数：一个是现在的值（current），一个是变化之前的值（prev）

   1. 须特别注意两个参数的先后顺序：

   2. ```js
      watch:{
          count(current,prev){
              console.log('watch changed')
              console.log('现在的值：',current)
              console.log('变化前的值：',prev)
          }
      },
      ```

      

3. watch（侦听器）和computed（计算属性）的区别：

   1. computed必须要返回一个值，而且在页面渲染的同时会执行里边的业务逻辑，也就是会执行一遍你写的业务逻辑。而`watch`只有在发生变化时才会执行。
   2. 当然watch也能实现computed的效果，但computed显然更简洁。

****



### 15. method、watch和computed三者使用优先级

- `computed` 和 `method`都能实现的功能，建议使用computed,因为有缓存，不用渲染页面就刷新。
- `computed` 和 `watch` 都能实现的功能，建议使用 computed，因为更加简洁。



****



### 16. v-show和v-if的差别

1. v-show控制DOM元素显示其实是控制css样式，也就是`display:none`。如果显示和隐藏的状态切换比较频繁，并且没有什么复杂的业务逻辑，建议使用v-show。



****



### 17. v-for

1. 为了提高循环时性能，在数组其中一项变化后，整个数组不进行全部重新渲染，Vue提供了绑定key值的使用方法，目的就是增强渲染性能，避免重复渲染。

2. 官方不建议使用索引`index`为key值，但此时又为了保持唯一性，所以这里使用了`index+item`进行绑定key值。

3. 注意：v-for的优先级是高于v-if的。

   1. 下面这段代码的v-if不起作用，因为v-for优先于v-if

   2. ```html
      <ul>
          <li 
              v-for="(item,index) in listArray"
              :key="index+item"
              v-if="item != '谢大脚'"
          >
              [{{index}}]{{item}}
          </li>
      </ul>
      ```

   3. 正确的写法是v-if的标签独立出来(但这种方式会使DOM结构有问题，明明循环出两项，但却有三个div标签)

   4. ```html
      <ul>
          <div
              v-for="(item,index) in listArray"
              :key="index+item"
          >
          <li v-if="item != '谢大脚'">
              [{{index}}]{{item}}
          </li>
          </div>
      </ul>
      ```

   5. 为解决上面这个问题，Vue为我们提供了template标签。所以真正合适的vue代码应该是下面这样的：

   6. ```html
      <ul>
          <template
              v-for="(item,index) in listArray"
              :key="index+item"
          >
          <li v-if="item != '谢大脚'">
              [{{index}}]{{item}}
          </li>
          </template>
      </ul>
      ```

   7. 



****



### 18. 事件的event对象

1. 在编写响应事件时，是可以接收一个event参数的，这个参数是关于响应事件的一些内容，我们可以打印出来event。

   1. ```js
      methods:{
          addCountClick(event){
              this.count++;
              console.log(event)
          },
      }
      ```

2. 会发现它的内容比较多，下面讲一些工作中实际会用到的：

   1. event.target: 可以看到是哪个DOM元素触发的事件。

   2. 有参数的情况下怎么使用event呢？ --- 方法的参数增加`$event`。

      1. ```js
         methods:{
             addCountClick(num,event){
                 this.count+=num;
                 console.log(event.target)
             },
         },
         template:`
             <div>目前已点佳丽数量{{count}}.</div>
             <button @click="addCountClick(2,$event)">增加一位佳丽</button>
             ` 
         })
         ```

3. 一个按钮调用两个方法：用`,`逗号把事件隔开，每个事件名后边必须加上`( )`才能起作用。



****



### 19. 事件修饰符

1. 6个Vue中的修饰符：分别是：`stop`,`prevent`,`capture`,`self`,`once`和`passive`。

2. `冒泡：`形成冒泡效果，就是有嵌套的DOM元素时，两个都有绑定事件，JS会自动向上传递事件。

   1. 比如下面这段代码：加入点击button按钮，不仅会触发button绑定的点击事件，还会触发其父元素div中绑定的点击事件。这就是**冒泡**效果。

   2. ```js
      template:`
              <div @click="handleBtnClick1">
                  <div>目前已点佳丽数量{{count}}.</div>
                  <button @click=" addCountClick()">增加一位佳丽</button>
             </div>
              ` 
          }) 
      ```

   3. ![image-20220405140514404](https://gitee.com/nothingimpossible/study-notes-img/raw/master/marktext_img/image-20220405140514404.png)

3. 其他修饰符：
   1. prevent修饰符：阻止默认行为，比如阻止`form`表单的默认提交行为。
   2. capture修饰符：改成捕获模式，默认的模式都是冒泡模式，也就是从下到上，但是你用capture之后，是从上到下的。
   3. once修饰符：点击此按钮时，事件只执行一次。
   4. passive修饰符：解决滚动时性能的修饰符。



****



### 20. 按键、鼠标修饰符

1. 按下任意按键皆可触发事件：@keydown="事件名"

   1. ```js
      template:`
          <div">
              <input @keydown="handleKeyDown"/>
          </div>
          `
      ```

2. 按下某个按键才会触发事件：

   1. ```js
      template:`
          <div">
              <input @keydown.enter="handleKeyDown"/>
          </div>
          ` 
          })
      ```

3. 鼠标修饰符最常用的就是: left、right、middle：

   1. ```html
      <div @click.right="handleClick">JSPang.com</div>
      ```



****







## 2.组件：



### 1. 一张图了解组件：

<img src="https://gitee.com/nothingimpossible/study-notes-img/raw/master/marktext_img/image-20220405144948899.png" alt="image-20220405144948899" style="zoom:80%;" />



****



### 2. 根组件：

1. ```html
   <script>
       const app=Vue.createApp({ })
       const vm=app.mount("#app")
   </script>
   ```

2. 这时候的`Vue.createApp`实际是建立一个Vue的实例，也就是相当于刚才说的第一个`根组件`。你可以通过对象属性的形式（实际上就是方法接受一个对象形式的参数）。



****



### 3. 全局组件的注册：

1. 注册组件：

   1. ```html
      <script>
          app.component('website',{
              template:` <h2>JSPang.com</h2>`
          })
          app.component('describe',{
              template:` <h2>技术胖的博客</h2>`
          })
      </script>
      ```

2. 组件注册好之后就可以在根组件上使用了：

   1. ```js
      const app=Vue.createApp({
          template:`
              <website />
              <describe />
          `
      })
      ```



****



### 4. 组件的可复用性：

1. ```js
   const app=Vue.createApp({
       template:`
           <website />
           <describe />
           <count/>
                   <count/>
                   <count/>
       `
   })
   ```

2. 然后在浏览器中进行预览，你会发现`<count />`是**互不干扰**的，就是因为这个特性，Vue中的组件就具有了复用性。



****



### 5. 全局组件的弊端：

1. 全局组件编写起来确实非常方便，当时全局组件就是你一旦定义了，就会占用系统资源。它是一直存在的，你在任何地方都可以使用这个全局组件。这势必会对性能产生影响，比如一个真实的项目，会有上千个组件，这些组件由不同人编写，如果全部是全局组件，那这个应用打开速度一定是极慢的，而且流畅度也会受到影响。

   全局组件的概括：**只要定义了，处处可以使用，性能不高，但是使用起来简单。**



****



### 6. 局部组件：

