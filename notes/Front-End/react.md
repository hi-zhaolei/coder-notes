# React

## 生命周期

### 初始化

*getDefaultProps
*getInitialState
*componentWillMount
*render
*componentDidMount

*tips* **componentDidMount**会等待**同级component**都执行完**render**方法之后，再按照**先子后父**,**代码前后**顺序依次执行。

### 存在期

组件已存在时的状态改变

*componentWillReceiveProps
*shouldComponentUpdate
*componentWillUpdate
*render
*componentDidUpdate

### 销毁&清理期

componentWillUnmount

*tips* **componentWillUnmount**会按照**先父后子**,**代码前后**顺序依次执行。

## react的setState

### setState是异步的

setState() 调用后不会立刻改变 this.state，而是创建一个即将处理的 state 转变，也就是说, setState 是异步的，在调用该方法之后访问 this.state 会返回现有的值。

组件在还没有渲染之前, this.setState 还没有被调用，真正的调用是在 render 时, 这里的 state 才会改变。这么做的目的是为了提升性能, 批量执行 State 转变时让 DOM 渲染更快.

### setState会造成不必要的渲染

每次调用都会造成重新渲染。很多时候，这些重新渲染是不必要的。你可以用 React performance tools 中的 printWasted 来查看什么时候会发生不必要渲染。比如:

*新的 state 其实和之前的是一样的。这个可以通过 shouldComponentUpdate 来解决。

*通常发生改变的 state 是和渲染有关的，但是也有例外。比如，有些数据是根据某些状态来显示的。

*有些 state 和渲染一点关系都没有。有一些 state 可能是和事件、 timer ID 有关的。

所以并不是所有的组件状态都应该用 setState 来进行保存和更新的。
虽然复杂的组件可能会有各种各样的状态需要管理，但是用 setState 来管理这些状态不但会造成很多不需要的重新渲染，也会造成相关的生命周期钩子一直被调用。

### setState回调函数

setState 方法接收一个 function 作为回调函数。这个回掉函数会在 setState 完成以后直接调用，这样就可以获取最新的 state 。

也可以在 setState 使用 setTimeout 来让 setState 先完成以后再执行里面内容。

## react的diff算法
