---
title: react 生命周期
date: 2018-03-03 10:04:41
type: "tags"
tags: [ React, 前端 ]
---

### 相关函数
简单地说，React Component通过其定义的几个函数来控制组件在生命周期的各个阶段的动作。
在ES6中，一个React组件是用一个class来表示的[（具体可以参考官方文档）](https://link.jianshu.com/?t=http://facebook.github.io/react/docs/reusable-components.html#es6-classes)，如下：

```
// 定义一个TodoList的React组件，通过继承React.Component来实现
class TodoList extends React.Component {
  ...
}
```
这几个生命周期相关的函数有：

```
void componentWillMount()
```
在组件挂载之前调用一次。如果在这个函数里面调用setState，本次的render函数可以看到更新后的state，并且只渲染一次。
```
void componentDidMount()
```
在组件挂载之后调用一次。这个时候，子主键也都挂载好了，可以在这里使用refs。
```
void componentWillReceiveProps(nextProps)
```
props是父组件传递给子组件的。父组件发生render的时候子组件就会调用componentWillReceiveProps（不管props有没有更新，也不管父子组件之间有没有数据交换）。
```
bool shouldComponentUpdate(nextProps, nextState)
```
组件挂载之后，每次调用setState后都会调用shouldComponentUpdate判断是否需要重新渲染组件。默认返回true，需要重新render。在比较复杂的应用里，有一些数据的改变并不影响界面展示，可以在这里做判断，优化渲染效率。
```
void componentWillUpdate(nextProps, nextState)
```
shouldComponentUpdate返回true或者调用forceUpdate之后，componentWillUpdate会被调用。
```
void componentDidUpdate()
```
除了首次render之后调用componentDidMount，其它render结束之后都是调用componentDidUpdate。

componentWillMount、componentDidMount和componentWillUpdate、componentDidUpdate可以对应起来。区别在于，前者只有在挂载的时候会被调用；而后者在以后的每次更新渲染之后都会被调用。
```
ReactElement render()
```
render是一个React组件所必不可少的核心函数（上面的其它函数都不是必须的）。记住，不要在render里面修改state。
```
void componentWillUnmount()
```
组件被卸载的时候调用。一般在componentDidMount里面注册的事件需要在这里删除。

### 更新方式

在react中，触发render的有4条路径。

#### 以下假设shouldComponentUpdate都是按照默认返回true的方式。
1. 首次渲染Initial Render
2. 调用this.setState （并不是一次setState会触发一次render，React可能会合并操作，再一次性进行render）
3. 父组件发生更新（一般就是props发生改变，但是就算props没有改变或者父子组件之间没有数据交换也会触发render）
4. 调用this.forceUpdate

下面是我对React组件四条更新路径地总结：

![image](http://p0mejgdza.bkt.clouddn.com//blog/1814354-4bf62e54553a32b7.png)

##### ==注意，如果在shouldComponentUpdate里面返回false可以提前退出更新路径。==

### 一个React组件生命周期的测试例子

代码比较简单，没有逻辑，只是在每个相关函数里面alert一下。<h6>[点击链接来试试这个例子](https://link.jianshu.com/?t=http://wximg.gtimg.com/shake_tv/test/lifeCycle2113.html)。</h6>
源码：

```path
class LifeCycle extends React.Component {
    constructor(props) {
        super(props);
        alert("Initial render");
        alert("constructor");
        this.state = {str: "hello"};
    }

    componentWillMount() {
        alert("componentWillMount");
    }

    componentDidMount() {
        alert("componentDidMount");
    }

    componentWillReceiveProps(nextProps) {
        alert("componentWillReceiveProps");
    }

    shouldComponentUpdate() {
        alert("shouldComponentUpdate");
        return true;        // 记得要返回true
    }

    componentWillUpdate() {
        alert("componentWillUpdate");
    }

    componentDidUpdate() {
        alert("componentDidUpdate");
    }

    componentWillUnmount() {
        alert("componentWillUnmount");
    }

    setTheState() {
        let s = "hello";
        if (this.state.str === s) {
            s = "HELLO";
        }
        this.setState({
            str: s
        });
    }

    forceItUpdate() {
        this.forceUpdate();
    }

    render() {
        alert("render");
        return(
            <div>
                <span>{"Props:"}<h2>{parseInt(this.props.num)}</h2></span>
                <br />
                <span>{"State:"}<h2>{this.state.str}</h2></span>
            </div>
        );
    }
}

class Container  extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            num: Math.random() * 100
        };
    }

    propsChange() {
        this.setState({
            num: Math.random() * 100
        });
    }

    setLifeCycleState() {
        this.refs.rLifeCycle.setTheState();
    }

    forceLifeCycleUpdate() {
        this.refs.rLifeCycle.forceItUpdate();
    }

    unmountLifeCycle() {
        // 这里卸载父组件也会导致卸载子组件
        React.unmountComponentAtNode(document.getElementById("container"));
    }

    parentForceUpdate() {
        this.forceUpdate();
    }

    render() {
        return (
            <div>
                <a href="javascript:;" className="weui_btn weui_btn_primary" onClick={this.propsChange.bind(this)}>propsChange</a>
                <a href="javascript:;" className="weui_btn weui_btn_primary" onClick={this.setLifeCycleState.bind(this)}>setState</a>
                <a href="javascript:;" className="weui_btn weui_btn_primary" onClick={this.forceLifeCycleUpdate.bind(this)}>forceUpdate</a>
                <a href="javascript:;" className="weui_btn weui_btn_primary" onClick={this.unmountLifeCycle.bind(this)}>unmount</a>
                <a href="javascript:;" className="weui_btn weui_btn_primary" onClick={this.parentForceUpdate.bind(this)}>parentForceUpdateWithoutChange</a>
                <LifeCycle ref="rLifeCycle" num={this.state.num}></LifeCycle>
            </div>
        );
    }
}

ReactDom.render(
    <Container></Container>,
    document.getElementById('container')
);

```

作者：linjinhe
链接：https://www.jianshu.com/p/4784216b8194
來源：简书
