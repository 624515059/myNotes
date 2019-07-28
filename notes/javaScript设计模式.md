

### 工厂模式

#### 介绍

- 将 new 操作单独封装
- 遇到new时，就要考虑是否应该使用工厂模式了

#### 示例

- 购买汉堡，直接点餐、取餐，不需要亲自做
- 商店‘封装’好汉堡的工作，直接给消费者

#### 演示

``` JavaScript

class Fn {
    constructor(name){
        this.name = name;
    }
    init(){
        console.log('init');
    }
}
class Creator {   
    create(name){
        console.log('工厂模式,隐藏了new的过程，统一入口');
        return new Fn(name);
    }
}

let creator = new Creator();
let p = creant.create('name');

```

#### 场景

``` JavaScript
// jquery
window.$  = function (selector) {
    return new JQuery(selector);
}

// react
React.createElement(..,..,..);

class Vnode (tag, attrs, children) {
    // 代码......
}
React.createElement = function (tag, attrs, children){
    return new Vnode(tag, attrs, children);
}
```

#### 设计原则

- 构造函数和创建者分离
- 符合开放封闭原则


### 单例模式

#### 介绍

> 单例模式的定义：保证一个类仅有一个实例，并提供一个访问它的全局访问点。实现的方法为先判断实例存在与否，如果存在则直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。

- 系统中被唯一使用
- 一个类只有一个实例

#### 示例

> 适用场景：一个单一对象。比如：弹窗，购物车。无论点击多少次，只应该全局被创建一次。

#### 演示

``` javaScript
class Fn2 {
    login(){
        console.log('单例模式');
    }
}

Fn2.getInstance= (function (){
    let instance;
    return function() {
        if(!instance){
            instance = new Fn2() ; 
        };
        return instance
    }
})();

let p1 = Fn2.getInstance()
let p2 = Fn2.getInstance()
p1.login()
p2.login()

console.log(p1 === p2);
```

#### 场景

- jquery只有一个$
- 只有一个模拟登陆框

``` javaScript
if(window.jQuery != null) {
    return window.jQuery
}else{
    // 初始化
}
```

#### 设计原则验证

- 符合单一职责原则，值实例化唯一的对象


### 观察者模式

#### 介绍

- 发布 & 订阅
- 一对N

#### 示例

- 点咖啡，点好之后坐等被叫就好

#### 演示

``` javaScript
// 观察者模式
// 主题，保存状态，状态变化后触发所有观察者对象

class Subject {
    constructor() {
        this.state = 0;
        this.observers = [];
    }
    getState() {
        return this.state;
    }
    setState(state) {
        this.state= state;
        this.notifyAllObserrvers();
    }
    notifyAllObserrvers() {
        this.observers.forEach(observer => {
            observer.update();
        })
    }
    attach(observer){
        this.observers.push(observer);
    }
}

// 观察者
class observer {
    constructor(name,subject){
        this.name = name;
        this.subject = subject;
        this.subject.attach(this)
    }
    update() {
        console.log(`${this.name} update, state: ${this.subject.getState()}`)
    }
}

// 测试
let s = new Subject()
let o1 = new observer('o1',s);
s.setState(1);
```
#### 场景

- 网页事件绑定
- Promise
- nodejs 自定义事件
- ...

``` javaScript
$('#btn').click(function () {
    console.log(1)
})
$('#btn').click(function () {
    console.log(2)
})

```

#### es5 es6 [实现方法](https://blog.csdn.net/lm278858445/article/details/78287492)

``` JavaScript
// es5
// 创建对象
var targetObj = {
    age: 1
}
// 定义值改变时的处理函数
function observer(oldVal, newVal) {
	// 其他处理逻辑...
    console.info('name属性的值从 '+ oldVal +' 改变为 ' + newVal);
}

// 定义name属性及其set和get方法
Object.defineProperty(targetObj, 'name', {
    enumerable: true,
    configurable: true,
    get: function() {
        return name;
    },
    set: function(val) {
        //调用处理函数
        observer(name, val)
        name = val;
    }
});

targetObj.name = 'Martin';
targetObj.name = 'Lucas';
console.info('targetObj:', targetObj)


// es6
class TargetObj {
    constructor(age, name) {
        this.name = name;
        this.age = age;
    }
    set name(val) {
        observer(name, val);
        name = val;
    }
}

let targetObj = new TargetObj(1, 'Martin');

// 定义值改变时的处理函数
function observer(oldVal, newVal) {
	// 其他处理逻辑...
    console.info('name属性的值从 '+ oldVal +' 改变为 ' + newVal);
}
targetObj.name = 'Lucas';
console.info(targetObj)

```