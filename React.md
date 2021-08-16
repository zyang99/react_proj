# React  

+ 用于构造用户界面的JavaScript库
+ 是一个将数据渲染为HTML试图的开源JavaScript库---FaceBook

**特点**

1. 采用组件化模式、声明式编码，提高开发效率及组件复用率
2. 在React Native中可以使用React语法进行移动端开发
3. 使用**虚拟DOM**+优秀的**Diffing算法**，尽量减少与真实DOM的交互
   + 使用虚拟DOM，不总是直接操作页面真实DOM
   + DOM Diffing算法，最小化页面重绘

### 开始

+ babel---除了将ES6转化为ES5，还可以将JSX转化为JS
+ 使用JS来创建DOM难以实现现实需求--如多层级的元素标签----所以使用JSX

```html
	<!-- 准备一个容器 -->
  <div id="test"></div>

  <!-- 引入JS，有顺序 -->
  <!-- 引入react核心库 -->
  <script type="text/javascript" src="../pacakage_old/react.development.js"></script>
  <!-- 引入react-dom，用于至此react操作dom -->
  <script type="text/javascript" src="../pacakage_old/react-dom.development.js"></script>
  <!-- 引入babel，用于将jsx转化为js -->
  <script type="text/javascript" src="../pacakage_old/babel.min.js"></script>

  <script type="text/babel"> /*此处一定要写babel，表示里面写的是jsx*/
    // 1 创建虚拟DOM
    const VDOM = <h1>hello,React</h1>     //此处一定不要写引号，因为不是字符串 而是JSX
    // 2 渲染虚拟DOM到页面
    ReactDOM.render(VDOM,document.getElementById('test'))
  </script>
```

### JSX

+ 全称：JavaScript XML
+ **jsx语法规则**：
  1. 定义虚拟DOM时，不要写引号。
  2. 标签中混入JS表达式时要使用{}。
     1. {}中只能写JS表达式，不能写语句
     2. 表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方
        1. a
        2. a+b
        3. demo(1)
        4. arr.map()
        5. function test(){}
     3. 语句：
        1. if(){}
        2. for(){}
        3. switch(){case:xxxx}
  3. 样式的类名指定要使用className。
  4. 内联样式，要用style={{key:value}}的形式去写。
  5. 虚拟DOM内必须只有一个根标签（父元素）。
  6. 标签必须闭合。
  7. 标签首字母
     1. 若小写字母开头，则将该标签转为html中同名元素，若无，则报错
     2. 若大写字母开头，则react就去渲染对应的组件，若没有组件，则报错

+ jsx小练习
  + 在react中可以帮你遍历数组，而object不行

### 模块与组件 模块化与组件化

+ 模块
  + what：向外提供特定功能的js程序，一般就是一个js文件
  + why：随着业务逻辑增加，代码越来越多且复杂
  + 复用js，简化js的编写，提高js运行效率

+ 组件
  + what：用来实现局部功能效果的代码和资源的集合（html、css、js、image等）
  + why：一个界面的功能更复杂
  + 复用编码，简化项目编码，提高运行效率
+ 模块化：当应用的js都以模块来编写
+ 组件化：当应用是以多组件的方式来实现

### React面向组件编程

+ 函数式组件

  ```react
  <script type="text/babel">
      //创建函数式组件
      function Demo(){
        console.log(this)  //babel翻译完之后开启严格模式，禁止这里的this指向window，是undefined
        return <h2>我是用函数定义的组件（适用于【简单组件】的定义）</h2>
      }
      //渲染组件到真实页面中
      ReactDOM.render(<Demo/>,document.getElementById('test'))
      // 1 react解析组件标签，找打了Demo组件
      // 2 发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，最后呈现
  </script>
  ```

+ 类式组件

  + 类的基本知识复习---class
  + **总结：**
    1. 类中的构造器不是必须写的，要对实例进行一些初始化的操作，如添加指定属性时才写。
    2. 如果A类继承了B类，且A类中写了构造器，那么A类构造器中的super是必须要调用的。
    3. 类中所定义的方法，都是放在了类的原型对象上。

  ```react
  <script type="text/babel">
      //创建类式组件
      class Demo extends React.Component{
          render(){
              //render放在了Demo的原型对象上
              //render中的this是---Demo的组件的实例对象
              return <h2>我是用类定义的组件（适用于【复杂组件】的定义）</h2>
          }
      }
      //渲染组件到真实页面中
      ReactDOM.render(<Demo/>,document.getElementById('test'))
      /*
      1 react解析组件标签，找到了Demo组件
      2 发现组件是使用类定义的，随后new出该类的实例，并通过实例调用原型上的render方法
      3 将render中返回的虚拟DOM转为真实DOM，随后呈现页面
      */
  </script>
  ```

##### 组件实例的三大核心属性之 state  ---类似vue中的data

+ ```rea
  <script type="text/babel">
      //创建类式组件
      class Weather extends React.Component{
        //构造器调用了几次？  ----- 1次
        constructor(props){
          super(props)
          //初始化状态
          this.state = {isHot: true,}
          // 解决change中的指向问题
          this.changeWeather = this.change.bind(this)
        }
  
        //change调用了几次？ ---点多少次就调用几次
        change(){
          //change放在了Weather原型对象上，供实例使用
          //由于change作为onClick的回调，所以不是通过实例调用的，是直接调用的
          //由于类中方法默认开启了局部的严格模式，所以change中的this为undefined
          
          //严重注意：状态state不可直接更改(如下)，要借助内部一个api
          // this.state.isHot = !this.state.isHot
  
          //严重注意：状态必须通过setState来更新，且更新是一种合并，不是替换
          this.setState({isHot:!this.state.isHot})
        }
  
        // render调用了几次？ ---1+n次   1是初始化那次， n是状态更新的次数
        render(){
          // 读取状态--解构赋值来读取的
          const {isHot} = this.state
          return (
            <h2 onClick={this.changeWeather}>今天天气很{isHot ? '炎热':'凉爽'}</h2>
          )
        }
      }
      //渲染组件到真实页面中
      ReactDOM.render(<Weather/>,document.getElementById('test'))
  </script>
  ```

+ ```react
  <script type="text/babel">
      //创建类式组件
      class Weather extends React.Component{
          //初始化state
          state = {isHot: true,}
          //自定义方法----要用赋值语句+箭头函数
          change = ()=>{
              this.setState({isHot:!this.state.isHot})
          }
          render(){
              const {isHot} = this.state
              return (
                  <h2 onClick={this.change}>今天天气很{isHot ? '炎热':'凉爽'}</h2>
              )
          }
      }
      //渲染组件到真实页面中
      ReactDOM.render(<Weather/>,document.getElementById('test'))
  </script>
  ```

+ **总结state**
  + state是组件对象最重要的属性，值是对象（可以包含多个key-value的组合）
  + 组件被称为“状态机”，通过更新组件的state来更新对应的页面显示（重新渲染组件）
  + 组件中render方法中的this为组件实例对象
  + 组件自定义的方法中this为undefined，如何解决？
    + 通过bind强制绑定this
    + 箭头函数
  + 状态数据，不能直接修改或更新

##### 组件实例的三大核心属性之props

+ ```react
    <script type="text/babel">
      //创建类式组件
      class Person extends React.Component{
        render(){
          const {name,sex,age} = this.props
          //props只读 不可修改
          return (
            <ul>
              <li>姓名：{name}</li>
              <li>性别：{sex}</li>
              <li>年龄：{age+1}</li>
            </ul>
          )
        }
      }
      //限制props的类型 必填选择
      Person.propTypes = {
        name: PropTypes.string.isRequired,
        sex: PropTypes.string,
        age: PropTypes.number.isRequired,
        speak: PropTypes.func, // 限制speak为方法
      }
      // 限制props的默认值（不传的时候）
      Person.defaultProps = {
        sex: '男',
        age: 18,
      }
      //渲染组件到真实页面中
      // 简单传值
      // ReactDOM.render(<Person name='tom' sex='女' age='18'/>,document.getElementById('test1'))
      const p = {name:'tom',age:18,sex:'女'}
      ReactDOM.render(<Person {...p}/>,document.getElementById('test1'))
      ReactDOM.render(<Person name='jerry' sex='女' age={18}/>,document.getElementById('test2'))
      ReactDOM.render(<Person name='jack' sex='男' age={38}/>,document.getElementById('test3'))
  </script>
  ```

+ ```react
      class Person extends React.Component{
        //限制props的类型 必填选择
        static propTypes = {
          name: PropTypes.string.isRequired,
          sex: PropTypes.string,
          age: PropTypes.number.isRequired,
          speak: PropTypes.func, // 限制speak为方法
        }
        // 限制props的默认值（不传的时候）
        static defaultProps = {
          sex: '男',
          age: 18,
        }
        render(){
          const {name,sex,age} = this.props
          //props只读 不可修改
          return (
            <ul>
              <li>姓名：{name}</li>
              <li>性别：{sex}</li>
              <li>年龄：{age+1}</li>
            </ul>
          )
        }
      }
  ```

+ ```react
  constructor(props){
      // 构造器是否接收props，是否传递给super，取决于是否希望在构造器中通过this访问props
      // 基本不会写构造器
      // 写构造器 仅仅两种情况：1 初始化内部的state  2 为事件函数绑定实例
      super(props)
  }
  ```

+ ```react
  <script type="text/babel">
      //创建函数式式组件
      function Person({name,sex,age}){
          return (
              <ul>
                  <li>姓名：{name}</li>
                  <li>性别：{sex}</li>
                  <li>年龄：{age+1}</li>
              </ul>
          )
      }
      //限制 写在外面 Person.propTypes = {}   Person.defaultProps = {}
      // ReactDOM.render(<Person name='tom' sex='女' age='18'/>,document.getElementById('test1'))
      const p = {name:'tom',age:18,sex:'女'}
      ReactDOM.render(<Person {...p}/>,document.getElementById('test1'))
  </script>
  ```

+ **总结props**

  + 每个组件对象都会有props（properties）属性
  + 组件标签的所有属性都保存在props中
  + 通过标签属性从组件外向组件内传递变化的数据
  + 注意：组件内部不要修改props数据

##### 组件实例三大核心属性之 refs

+ ```rea
  //字符串形式refs----尽量避免
  ref='string'
  this.refs.string //得到真实DOM界面
  
  //回调形式的refs----大家都在使用的
  ref={current => this.string = current}
  this.refs.string
  //这种内联回调形式 在更新的时候 会调用两次 第一次会传入null 第二次才是当前节点
  // 可以通过写成class绑定形式：即写成一个方法 放在外面       但是这是无关紧要的 写内联就ok了
  ref={this.setRef}
  setRef = current=>{
  	this.string = current
  }
  
  //createRef形式的refs----官方推荐的
  myRef = React.createRef()  //创建一个容器用来存放节点
  ref={this.myRef} 
  //多个节点需要创建多个容器
  
  ```



+ **react事件处理**
  + 通过onXxxxx属性指定事件处理函数(注意大小写)
    + React使用的是自定义（合成）事件，而不是原生DOM事件-----------为了更好的兼容性
    + React的事件是通过事件委托方式处理的(委托给组件最外层的元素)----------为了高下
  + 通过event.target得到发生事件的DOM元素对象-----就是某些事件发生的时候得到这个DOM元素(就没必要使用ref获取到DOM元素，不要过度使用ref)







