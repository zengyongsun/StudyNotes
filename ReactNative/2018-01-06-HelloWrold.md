# Hello World
```js
import React, { Component } from 'react';
import { Text } from 'react-native';

export default class HelloWorldApp extends Component {
  render() {
    return (
      <Text>Hello world!</Text>
    );
  }
}
```
上面代码定义了一个名为`HelloWorldApp`的新`组件（Component）`你在编写 React Native 应用时，肯定会写出很多新的组件。而一个App的最终界面，其实也就是各式各样的组件的组合。组件本身结构可以非常简单——唯一必须的就是在`render`方法中返回一些用于渲染结构的JSX语句。
# Props（属性）
以常见的基础组件`Image`为例，在创建一个图片时，可以传入一个名为`source`的prop来指定要显示的图片的地址，以及使用名为`style`的prop来控制其尺寸。
```js
import React, { Component } from 'react';
import { Image } from 'react-native';

export default class Bananas extends Component {
  render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    };
    return (
    //{}括号内部为一个js变量表达式，需要执行后取值
      <Image source={pic} style={{width: 193, height: 110}} />
    );
  }
}
```
自定义的组件也可以使用`props`。通过在不同的场景使用不同的属性定制，可以尽量提高自定义组件的复用范畴。只需在`render`函数中引用`this.props`，然后按需处理即可。下面是一个例子：
```js
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class Greeting extends Component {
  render() {
    return (
      <Text>Hello {this.props.name}!</Text>
    );
  }
}

export default class LotsOfGreetings extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
        <Greeting name='Rexxar' />
        <Greeting name='Jaina' />
        <Greeting name='Valeera' />
      </View>
    );
  }
}
```
我们在`Greeting`组件中将`name`作为一个属性来定制，这样可以复用这一组件来制作各种不同的“问候语”。上面的例子把`Greeting`组件写在JSX语句中，用法和内置组件并无二致——这正是React体系的魅力所在。
# state 状态
我们使用两种数据来控制一个组件：`props`和`state`。`props`是在父组件中指定，而且一经指定，在被指定的组件的生命周期中则不再改变。 对于需要改变的数据，我们需要使用`state`。
一般来说，你需要在`constructor`中初始化`state`（译注：这是ES6的写法，早期的很多ES5的例子使用的是getInitialState方法来初始化state，这一做法会逐渐被淘汰），然后在需要修改时调用`setState`方法。
```js
class Blink extends Component {
    //初始化 state
    constructor(props) {
        super(props);
        this.state = {showText: true};
        //每秒对showText状态做一次取反操作
        setInterval(() => {
            this.setState(previousState => {
                return {showText: !previousState.showText};
            });
        }, 1000);
    }

    render() {
        //根据当前showText的值决定是否显示text内容
        let display = this.state.showText ? this.props.text : '';
        return (
            <Text>{display}</Text>
        );
    }
}
```
实际开发中，我们一般不会在定时器函数（setInterval、setTimeout等）中来操作state。典型的场景是在接收到服务器返回的新数据，或者在用户输入数据之后。你也可以使用一些“状态容器”比如Redux来统一管理数据流（译注：但我们不建议新手过早去学习redux）。

