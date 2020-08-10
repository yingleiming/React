目前 React 16.8+的生命周期分为三个阶段，分别是**挂载阶段、更新阶段、卸载阶段**。分别对应组件生命周期的三个状态：**1.Mounting**：已插入真实DOM；**2.Updating**：正在被重新渲染；**3.Unmounting**：已移出真实DOM

**React 的生命周期的函数有哪些**：

1. 组件将要挂载时触发的函数：**componentWillMount**
2. 组件挂载完成时触发的函数：**componentDidMount**
3. 是否更新数据时触发的函数：**shouldComponentUpdate**
4. 将要更新数据时触发的函数：**componentWillUpdate**
5. 数据更新完成时触发的函数：**componentDidUpdate**
6. 组件将要销毁时触发的函数：**componentWillUnmount**
7. 父组件中改变了props传值时触发的函数：componentWillReceiveProps

其中在React 16之后 **componentWillMount**、**componentWillReceiveProps**、**componentWillUpdate**这三个生命周期函数将会被废弃，请使用新增的生命周期函数。

### 挂载阶段：

constructor：构造函数，最先被执行，我们通常在构造函数里初始化 state 对象或者给自定义方法绑定 this

getDerivedStateFromProps：这是个静态方法，当我们接收到新的属性想去修改 state ，可以使用此方法

render：是个纯函数，只返回需要渲染的东西，不应该包含其他的业务逻辑，可以返回原生的 DOM、REACT组件、Fragment、Portals、字符串和数字、Boolean和 null 等内容

componentDidMount：组件装载之后调用，此时我们可以获取 DOM 节点并操作，比如对 canvas、svg的操作，服务器请求，订阅都可以写在这里面，但是记得在componentWillUnmount中取消订阅

### 更新阶段：

getDerivedStateFromProps：此方法在更新挂载阶段都可能会调用

shouldComponentUpdate：shouldComponentUpdate(nextProps,nextState)，有两个参数nextProps和nextState，表示新的属性和变化之后的 state，返回一个布尔值，true表示会触发重新渲染，false表示不会触发重新渲染，默认返回true，我们通常利用此生命周期来优化 React 程序性能

render：更新阶段也会触发此声明周期

getSnapshotBeforeUpdate：getSnapshotBeforeUpdate(preProps,preState)，该方法在 render 之后，componentDidUpdate之前调用，有两个参数 preProps 和 preState，表示之前的属性和之前的state，这个函数有一个返回值，会作为第三个参数传给 componentDidUpdate，如果你不想要返回值，可以返回 null，此生命周期必须与 componentDidUpdate 搭配使用

componentDidUpdate：componentDidUpdate(preProps,preState,snapShot) ,该方法在 getSnapshotBeforeUpdate方法之后调用，有三个参数 preProps,preState,snapShot ，表示之前的 props，之前的 state，和 snapShot。第三个参数是 getSnapshotBeforeUpdate 返回的，如果触发某些回调函数时需要用到 DOM 元素的状态，则将对此或计算的过程迁移至 getSnapshotBeforeUpdate，然后在 componentDidUpdate 中统一触发回调或更新状态。

### 卸载阶段：

componentWillUnmount：会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除定时器，取消网络请求或清除在 componentDidMount() 中创建的订阅，清理无效的 DOM 元素等垃圾清理工作

#### 异常处理：

static getDerivedStateFromError：此生命周期会在渲染阶段后代组件抛出错误后被调用。它将抛出的错误作为参数，并返回一个值以更新 state

componentDidCatch：此生命周期会在后代组件抛出错误后被调用。它接收两个参数：1.error：抛出的错误，2.info：带有 componentStack key 的对象，其中包含有关组件引发错误的栈信息。componentDidCatch 会在“提交”阶段被调用，因此允许执行副作用。它应该用于记录错误之类的情况。