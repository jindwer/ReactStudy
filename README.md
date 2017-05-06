# React的学习
> React的优点?(个人总结) 

  1. 性能出众，但代码并不复杂，结构简洁，开发效率高
  2. 简单易学，能快速上手
  3. React将需要实现的功能以及DOM都是以组件形式封装，重用度高
  4. React发展面越来越广，如React-native就是其衍生出来的侧重移动app的开发，但语法却与React的相差无几，这样不同平台上的开发成本就较低。
  
> React实现渲染的原理?

    React是以组件的形式来构建DOM页面，组件中的数据都是通过props和state进行传递。
    React通过state来将组件实现为一个状态机，通过检测判断状态值的变化来进行渲染的操作。
    React在第一次渲染组件的时候，是先建立一套js形式的虚拟DOM，再将这个虚拟DOM渲染到页面上，
    当状态值被改变时，React会先根据状态的改变生成虚拟DOM，将当前虚拟DOM和前一次的虚拟DOM做diff算法得到不同的部分，
    最后只将不同的这部分DOM更新到真实的DOM中。
    
> React中的合成事件?

    React实现了一个合成事件层，这个事件模型保证了与W3C标准的一致性，并且消除了IE和W3C标准实现之间的差异。
    
> 事件上下文与事件委托?

    React的合成事件自动将事件的处理方法执行的上下文绑定到当前组件，在处理方法中可以直接通过this.setState()设置组件的状态值，从而使组件重新渲染。
    合成事件是以事件委托的方式绑定到组件的最上层，当组件被移除unmount的时候将自动销毁被绑定的事件。
    
> 原生事件的使用?

    在componentDidMount(组件嵌入完成)之后使用addEventListener绑定的事件就是浏览器的原生事件，需要在commponentWillUnmount(组件将要被移除)
    中解除被绑定的原生事件removeEventListener。
   
> 在组件中改变父组件的状态?

    父组件通过props向子组件中传递数据，包括自己的属性和状态值，以及事件处理方法的句柄，
    子组件可以通过props获得父组件传递的改变状态的方法句柄并绑定到自身的事件上
```js
var Child = React.createClass({
    render(){
      return (
        <button onClick={this.props.childEvent}>增加</button>
      );
    }
  });
    var Parent = React.createClass({
      getInitialState(){
        return {
          num:0
        };
      },
      handle(event){
        this.setState({num:++this.state.num});
      },
      render(){
        return (
          <div>
            <h1>{this.state.num}</h1>
            <Child childVal={this.state.num} childEvent={this.handle}></Child>
            <button onClick={this.handle}>父增加</button>
          </div>
        );
      }
    });
    ReactDOM.render(
      <Parent></Parent>,
      document.body
    );
```
> React的生命周期
  
    * React生成和更新组件都是组件的一个生命周期中进行的，生命周期中的时间点都是和React的DOM diff算法相关联，主要分为三个板块:mount,update,unmount
    
    * 接下来是生命周期的详细:(钩子函数)
    
    constructor(组件的构造函数,设置初始属性和类型等) --> getInitialState(设置组件的状态属性) --> componentWillMount(组件即将被创建插入) --> [mounting(创建插入组件)] --> componentDidMount(组件创建插入完成,这之后才会获取到真实的DOM节点) --> componentReciveProps(组件接受变动的新属性值) --> shouldComponentUpdate(通过返回true/false来确定是否要更新组件) --> componentWillUpdate(组件即将被更新) --> [updating(组件正在更新)] --> componentDidUpdate(组件更新完成) --> componentWillUnmount(组件即将被移除,这里需要做一些清除定时，事件绑定等) --> [unmounting(组件正在移除)]
 
> React中的props和state的区别

    props是属性的意思，state是状态的意思。两者都是React组件的对象，都是组件存储数据的载体，但两者在一些方面也存在很大的不同:  
    
      1. 两者作用不同:props主要用于父组件向子组件传递数据用的对象，用于存储页面展示的数据;state主要储存表征组件显示状态的数据。 
      2. 两者数据不同:props是存储子组件展示的数据;state是存储控制子组件展示状态的表征性的数据。 
      3. 工作原理不同:props对象是存储从后台获得的数据并通过数据流从父组件到子组件，完成数据的传递但不一定能展示;state对象是存储状态值，在有新内容需要展示或者需要改变组件结构时候,如需要展示新得到的props的数据的时候,通过setState来设置新的状态值从而导致页面部分重新渲染。
    
