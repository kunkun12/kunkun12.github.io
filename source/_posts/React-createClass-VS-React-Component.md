title: React.createClass VS React.Component
date: 2016-03-05 11:16:42
tags:
---
创建react组件有两种方式，使用用 `React.createClass` 以及用es6模块化的方式`extends React.Component` 通过集成 `Component` 组件类的形式来实现。两者的作用类似，后者是新的语法也是趋势。于是赶紧对比下如何升级到新的语法

### React.createClass

－ creatClass创建的组件赋值为`const`类型的变量，在组件内部实现必要的 `render `函数

        import React from 'react';

        const Contacts = React.createClass({
          render() {
            return (
              <div></div>
            );
          }
        });
        export default Contacts;

### React.Component
通过ES6的类继承模式在 构造函数里面要 先通过`super(props)`调用父组件的构造函数

        import React from 'react';
        class Contacts extends React.Component {
          constructor(props) {
            super(props);
          }
          render() {
            return (
              <div></div>
            );
          }
        }
        export default Contacts;

## propTypes and getDefaultProps
### React.createClass

        import React from 'react';
        const Contacts = React.createClass({
          propTypes: {

          },
          getDefaultProps() {
            return {
              
            };
          },
          render() {
            return (
              <div></div>
            );
          }
        });
        export default Contacts;

### React.Component 
给类增加属性的方式发生了变化，`defaultProps` 来替代 `getDefaultProps()` 似乎简单了不少

        import React from 'react';
        class Contacts extends React.Component {
          constructor(props) {
            super(props);
          }
          render() {
            return (
              <div></div>
            );
          }
        }
        Contacts.propTypes = {
        };
        Contacts.defaultProps = {
        };
        export default Contacts;

## 状态定义 State

        import React from 'react';
        const Contacts = React.createClass({
          getInitialState () {
            return {
              
            };
          },
          render() {
            return (
              <div></div>
            );
          }
        });
        export default Contacts;

定义默认的状态在构造函数中实现来取代了 `getInitialState`,语法更简单

        import React from 'react';
        class Contacts extends React.Component {
          constructor(props) {
            super(props);
            this.state = {
            };
          }
          render() {
            return (
              <div></div>
            );
          }
        }
        export default Contacts;

### this的含义不同

在 `React.createClass`中，给组件绑定事件的函数中的 `this` 指向当前的组件 ，在 `React.Component` 中事件函数中的 `this` 默认指向为空

        import React from 'react';
        const Contacts = React.createClass({
          handleClick() {
            console.log(this); // 但前的组件实例
          },
          render() {
            return (
              <div onClick={this.handleClick}></div>
            );
          }
        });
        export default Contacts;

        import React from 'react';
        class Contacts extends React.Component {
          constructor(props) {
            super(props);
          }
          handleClick() {
            console.log(this); // null
          }
          render() {
            return (
              <div onClick={this.handleClick}></div>
            );
          }
        }
        export default Contacts;

可以在绑定事件的时候使用 JS中的bind来绑定this 对象 `<div onClick={this.handleClick.bind(this)}></div>`也可以在 构造函数里面手动的 bind `this.handleClick = this.handleClick.bind(this);`

        import React from 'react';
        class Contacts extends React.Component {
          constructor(props) {
            super(props);
            this.handleClick = this.handleClick.bind(this);
          }
          handleClick() {
            console.log(this); // React Component instance
          }
          render() {
            return (
              <div onClick={this.handleClick}></div>
            );
          }
        }
        export default Contacts;


### Mixins
使用mixins可以提高复用性，`React.Component` 不再支持 `Mixins`。

        import React from 'react';
        var SomeMixin = {
          doSomething() {
          }
        };
        const Contacts = React.createClass({
          mixins: [SomeMixin],
          handleClick() {
            this.doSomething(); // use mixin
          },
          render() {
            return (
              <div onClick={this.handleClick}></div>
            );
          }
        });
        export default Contacts;

在 React.Component 官方推荐 [HOC(高阶组件)的形式](https://leozdgao.me/chushi-hoc/) 来替换，如果一定要使用 mix的话 也可以使用 [react-mixin](https://github.com/brigand/react-mixin)