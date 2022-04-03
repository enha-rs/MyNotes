# Vue3

## 1. 第一个Vue程序 -- HelloWorld

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

   7. 

   8. 

****

## Vue3编写计数器

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

## 3. 绑定事件和方法

1. 给标签绑定**点击**事件：v-on:click="方法名"

2. ```html
   <button v-on:click="welcome">顾客前来</button>
   ```

3. 给标签绑定**是否显示**该DOM元素   v-if=“布尔变量”

4. ```html
   <div v-if="isShowMeal" >{{setMeal}}</div>
   ```

5. 

****

## 4. 循环展示和双向数据绑定

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

7. 