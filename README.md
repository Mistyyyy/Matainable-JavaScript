# 前端开发规范

> 程序是写给人读的，只是偶尔让计算机执行一下&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; --Donald Knuth

## 为什么有代码规范

 - 软件生命周期中80%的成本消耗在了维护上

 - 大多数时间里，你面对的是别人已经写好的代码

 - 编码规范提高了代码的可读性和可维护性

 - 在团队开发中，所有的代码风格看起来风格一致是极其重要的


## 1. JavaScript书写规范

### **1.基本的格式化**

  > **该文档大部分内容基于《Maintainable JavaScript》，author：Nicholas Zakas**

#### 1.1 缩进的层级

- 使用空格符缩进，目前团队选择主要有两种，一种是4个空格的缩进，一种是2个空格的缩进
  *（个人建议用四个空格缩进，这种情况下的代码展示是最优的）*
---

#### 1.2 语句结尾

- 推荐不要省略分号，原因如下

- 因为JavaScript有自动分号插入机制(Automatic Semicolon Insertion)，代码省略分号也可以正常工作,ASI会自动寻找应当使用分号的但实际没有使用分号的位置，并插入分号。

- 大多数场景下ASI会正确插入分号，但其规则非常复杂切很难记住。

```
//这样的情况下，ASI会在return后面插入分号，导致返回的结果是undefined
function getDate() {

    // 错误写法
    return 
    {
        title:'book'
    }
}
```
---
#### 1.3行的长度

- 推荐编辑器单行不出现横向滚动条。行长度限定在**80**个字符内
---

#### 1.4 关于换行

- 在运算符后换行，下一行增加两个层级的缩进
```
// 假定缩进为4个字符。逗号是运算符，应对作为前一行的行尾（因为ASI的规则）
callFunction(document,element.window,'some String value',true,123,
        navigator);
```
- 有一个例外，就是给变量赋值的时候，第二行的位置应当和赋值运算符符位置保持对其

```

let result = something + anotherThing + yetAnotherThing + somethingElse+
             anotherSomethingElse;
```
---
#### 1.5 空行

- 在方法之间有一个空行

- 在方法的局部变量和第一条语句之间

- 在多行或单行注释之前

- 在方法的逻辑片段之间插入空行，提高可读性
---
#### 1.5.1 关于间距

- 运算符:前后各留一个空格

``` 
sName = 'harley';

if (value === item);

if(found && (count > 10))

```

- 括号间距:使用括号时,紧接左括号和紧接右括号之间不应该有空格

```
if (condition) {
    ```````````
}

```

- 标识符后留一个空格

```
let sName = 'harley';
function getName() {

}
```

- 函数的形参和实参逗号之后留一个空格

```
function getName(myName, yourName) {

}
getName('my', 'your');
```

- 块语句的括号左右各留一个空格

```
  if (condition) {

  }
  switch (whichCase) {

  }
  while (true) {

  }
```

- 函数的括号右侧留一个空格
---

#### 1.6 命名

- 变量和函数名遵循小驼峰（camelCase）

- 变量名命名前缀应该是名词。函数名前缀应该是动词。匿名函数给变量赋值遵循这一规律。尽量符合语义。如
```
  let count = 10;
  let myName = 'harley'
  let found = true;

  function getName() {

  }
```
- 常量，使用大写字母和下划线来命名
```
const MAX_COUNT = 10;
```
- 构造函数，采用大驼峰（PascalCase）
```
function Person() {

}
```
---
#### 1.7 直接量（包含字符串，数字，null，undefined）

- 字符串:建议使用es6的字符串模板，支持换行。避免了传统字符串拼接，嵌套，换行，带来的一系列问题
```
let count = 10;
let sName = `the count is ${count}`
```
- 数字：注意八进制的数字写法，开发过程中应禁止使用
```
let num = 010;
```
- null: null是一个特殊值。（null == undefined 为true）因此，常常遭到误解。我们在下列场景应该使用null。 **理解null的最好方式是将它当做对象的占位符**

   - 用来初始化一个变量，这个变量可能赋值为一个对象

   - 用来和一个已经初始化的变量比较,这个变量可以是也不可以是一个对象

   - 当函数的参数期望是对象时，用作参数传入

   - 当函数的返回值是对象时，用作返回值传出
```
let person = null;

function getPerson() {
    if (condition) {
        return new Person('harley');
    } else {
        return null;
    }
}

if(person !== null){
  doSomething()
}
```
- undefined：undefined也是一个特殊值，那些没有被初始化的变量都有一个初始值，即undefined，表示这些变量等待被赋值.但是未声明的变量和值为undefined的变量还是已经声明但没有赋值的变量，typeof 都是 undefined。所以代码中应该禁止使用。
```
  let sName = undefined;    // typeof sName = undefined
  let sAnotherName;    // typeof sAnotherName = undefined
```

- 对象直接量：直接在花括号内添加属性名，每个属性的名值都独占一行，并保持一个缩进。常常第一行包括左花括号。最后右花括号也要独占一行。

```
  let book = {
    title: 'Maintainable JavaScript',
    author: 'Nicholas C. Zakas'
  }
```
- 数组直接量： 跟对象直接量类似，使用方括号来将数组初始元素包裹起来。
---
***注意:不要在初始化数组里面，出现空位，即[a,,b]。因为空位在es5和es6的api里处理方式不同，使用的话需要hack，所以应该避免出现空位***

- forEach(),filter(),every(),some()都会跳过空位
- map() 会跳过空位，但会保留这个值
- join（） 和 toString() 会将这个空位视为undefined，而undefined和null会被处理为空字符串
- es6会将空位处理为undefined。包含 Array.from(), ... , fill(), keys() , values() , find(), findIndex()。

```
 Array(3)   // [,,]
 0 in [undefined,undefined,undefined]    // true
 0 in [,,]    // false
```
---
### 2.注释

#### 2.1 单行注释

  - 以 // 开始，以行尾结束，而且建议在**双斜杠后给一个空格**

  - 独占一行的时候，用于解释下一行的代码，**这样的注释之前应该要有一个空行**。缩进级别应该和下一行代码保持一致。

  - 在代码行的尾部的注释，**代码结束到注释之间至少有一个缩进** ,如果注释超过当前行的最大字符数限制，就将这条注释放置在代码行的上方。

  ```
  // good job
  if (condition) {
    
      // allow
      allowed();
  }
  ```
---
#### 2.2 多行注释

- 多行注释应该在每一行加上*号,第一行和最后一行独占一行。注释与星号之间有一个空格。多行注释之前应当有一个空行。且缩进与下面的代码一致

```
  if (condition) {
      
      /*
       * 如果代码执行到这里 
       * 说明通过了所有的安全检测
       */
      allowed();
  }
```
---
#### 2.3 使用注释

- 使用注释的通用原则是，当代码不够清晰时，添加注释。当代码很明了时不应添加注释。

- 一般原则是：在需要让代码变得更清晰时添加注释。

---

#### 2.4 文档注释（vscode有自动生成的插件 Document this。）

- 我觉得很有不要添加文档注释，由我们自定义来设置具体格式。但应该向一下内容添加文档注释。

- 所有的方法，应该对方法，期望的参数，和可能的返回值添加注释描述

- 所有的构造函数，应当对自定义构造函数类型和期望的参数添加注释描述

- 所有包含文档化方法的对象。即如果一个对象包含一个或多个附带文档注释的方法，那么这个对象也应当适当的针对文档生成工具添加文档注释。 

```
/**
 * 
 *
 * @export
 * @param {*} requestArray 接受合并请求的任意多个参数
 * @returns 没有返回值
 */
export function allRequest (...requestArray) {
    requestArray.forEach((element, index) => {
        requestArray[index] = $axios({method: element.method, url: element.url})
  })
```
---
### 3. 语句和表达式

> 无论块语句包含多行代码还是单行代码，都应该总是使用花括号，包括if, for, while, do...while..., try...catch...finally 

- ES6中新增了块级作用域.也就是说块语句也会形成自己的作用域.

- 花括号的对齐方式，建议为讲左花括号放置在块语句中第一句代码的末尾
```
if (conditation) {
    doSomething()
} else {
    doSomeOthering()
}
```
- 块语句的间隔： **推荐在左圆括号之前和右圆括号之后各添加一个空格**

```
if (condition) {
    doSomething()
}
```
- switch语句：建议
  - 每条case语句都相对于switch关键字都缩进一个层级。

  - 从第二条语句开始时，每条case语句前后各有一个空行。增加可读性

  - 每一条case语句都要有break，return或throw做结尾。避免出现连续执行的情况（特殊情况应该给以注释）

  - 建议在没有默认行为的时候，不使用default来做结尾。
```
switch (condition) {
    case 'first':
          //代码
          break;

    case 'second':
          //代码
          break;

    case 'first':
          //代码
          break;
}
```
- 强烈建议代码中应该禁止使用with()语句和eval()语句

- for循环(补充遍历方式)

> ES5中有两种for循环（for in）和 传统for循环。ES6有 (for of) 表明，任何具有迭代器属性的对象都是可以遍历循环的 

  - 应避免在传统for循环中，使用continue语句。

  - for in 循环用来遍历对象的属性的。(包含对象自身的属性和从原型对象继承来的属性)，出于这个原因，建议使用实例方法hasOwnProperty()属性来进行过滤对象自身的属性。但是不应该使用此方法来遍历数组。

  ```
let key;

for (key in object) {
      if (object.hasOwnProperty(key)) {
          console.log(object)
      }
}
  ```
---
## 4.变量，函数和运算符

### 4.1 变量声明

> 介绍不同点以及使用理由

- ES6声明变量的6中方式，包含ES5的var,function命令。ES6新增4种，分别是let,const,import,class。

- 普通变量声明建议使用let,const。代码中不应该出现var。理由如下：

- var会变量提升，即编译阶段，编译器就将其加入到当前的作用域中。所以，变量可以在声明之前就可以使用，不符合语义化。而let const 不会出现变量提升 还会造成当前作用域暂时性死区，只要声明了该变量，在声明之前调用，就会产生错误。

- 在全局作用域下，var声明的变量会被当做全局window对象的属性。且无法使用delete语句进行删除。let const不会

- var变量可以重复声明，而let const 不可以重复声明，否则报错

- let const 可以在块级作用域使用，且作用域就在当前的块级作用域，var无此特性。

> 建议使用的规范

- 所有的变量必须在使用前进行声明，禁止出现未声明就赋值的操作，否则，会影响父级作用域甚至造成全局变量的污染

- 变量使用let 声明，常量使用const声明

- 告别var声明变量的方式

- 推荐在函数顶部或者块级作用域顶部 使用单let,const语句。每个变量的初始化独占一行。赋值运算符对其

- 建议在块级作用域或者函数局部的作用域，把局部变量的声明作为函数内的第一条语句，对于没有初始值的变量，应该出现在声明语句的底部。

- 避免全局变量,全局环境是用来定义JavaScript内置对象的地方.而不是用来盛放全局变量的地方.

```
function dosomethingOthers() {
    let value = 10,
        result = 20,
        i,
        len;

    function doSomething() {

    }
}
```
---
### 4.2 函数声明

- 函数声明也会被浏览器引擎执行代码前提前编译，提升到当前作用域的最顶端。

> 推荐

- 函数内部的局部函数或者块级作用域的函数应该紧接着变量声明之后声明.

- 函数声明不应该出现在语句块之内

- 函数应该在调用之前进行声明.

```
if (condition) {
    function doSomething() {

    }
} else {
    function doSomething() {

    }
}
```
---
### 4.3 函数调用间隔

- 在函数名和左括号之间没有空格.这样为了和块语句区分开来 

```
// 函数调用
doSomething()

// 块语句
while (true) {
  
}
```
---
### 4.4 关于立即执行函数的使用与否

- 建议关于使用立即执行函数进行赋值操作的时候,可以使用.但是应该使用(function(){}())这样的格式

- 使用代码块来取代立即执行函数,防止全局变量的污染情况,可以减少命名冲突.让变量成为局部变量,这样的代码是最容易维护的

```
// 形成块级作用域,防止全局变量的污染.
{
    //使用这样的立即执行函数来给变量进行赋值操作,如果有必要的话.
    let name = (function(){

    }())
}
```
---
### 4.5 关于相等运算符

- 优先使用 === 和 !== 来进行两个值的判断.因为,js具有强制类型转换机制转换,所以,建议弃用 == 和 != .(除特殊情况)

- 该方法的缺陷如下,如有必要,请自行hack,hack的手段是

```
{
    NaN === NaN    // false
    +0 === -0    // true
    
    //使用ES6新增的方法Object.is()来进行hack
    Object.is(+0,-0)    // false
    Object.is(NaN,NaN)    // true
}
```
- 建议禁止使用原始包装类型 
---


# 参考[知识点]
>> **下列内容是自己根据实践和参考书做的比较综合的总结**

## **编程实践**

### 第一 : UI层的松耦合

>>>包含的3个层,即结构(HTML), 样式(CSS),  行为(JavaScript)

#### 1.关于耦合性的说明,什么是松耦合

- 如果修改一个组件的逻辑,另一个组件也要进行修改.那么,这两个组件就是紧密耦合的.

- 如果能做到修改一个组件而不需要(或很少量的)更改其他组件时,这做到了松耦合.

- 此外,无法达到无耦合,因为总有一些信息是需要共享的.

#### 2.将JavaScript从CSS中抽离

- 早期的版本存在这个问题,因为那时候,存在css表达式,现在不用考虑这个问题

#### 3.将CSS从JavaScript中抽离

- 目前较多的用法是修改DOM的style属性.这样就产生了耦合性(因为出现后期出现样式问题,先从样式表找起,殊不知,实在js文件里面做了更改)

- 此外,应该杜绝cssText这一种蹩脚的方式.

- 样式应该包含在样式表里,而如果改变样式,最佳方法是操作CSS的className

- JavaScript不应该直接操作样式,以便保持和CSS的松耦合

#### 4.将JavaScript从HTML中抽离

- 很多人都是在进行事件操作的时候,直接在HTML上的on属性来进行事件的绑定,这样会导致行为和结构的耦合.此外,还会导致JavaScript错误的发生

- 对于这种情况,应该采用2级DOM的addEventListener方法来解决

#### 5.将HTML从JavaScript抽离

- 原生js来书写,使用innerHTML来进行属性赋值.这是不好的实践 

**解决办法**

- 从服务器加载

- 简单客户端模板

- 复杂客户端模板

---

### 第二: 避免使用全部变量

#### 1.全局变量带来的问题

- 命名冲突,当全局变量太多的时候,你可能无意中就使用了一个已经声明了的变量.

>>>注:全局变量是用来定义JavaScript内置对象的地方

- 代码的脆弱性:依赖全局变量的函数,任何别的函数可能无意间在修改全局变量,导致代码极其脆弱

- 难以测试 : 因为需要依赖一些全局变量才能正常工作

#### 2.意外的全局变量

- 没有通过声明,直接赋值,导致修改父级作用域或者流入了全局变量

#### 3.单全局变量方式

- 只创建一个全局变量.类似于jQuery(实际有两个全局变量,一个是$,一个是jquery).

- 使用命名空间,即通过简单的全局对象的单一属性表示来进行功能性的分组:
```
// 将同类的功能归分到同一分组,这样多人开发还是单人开发,可以规避命名冲突问题.逻辑清晰
let myGlobal = {
    DOM: {},
    Event: {},
    init: {}
}
```
- 提供一个创建命名空间的方法  [此方法来源于Maintainable JavaScript ]

```
let myGlobal = {
    namespace (ns) {
        let part = ns.split('.'),
            object =  this,
            i,
            len;
        
        for (i=0,len=part.length; i<len; i++) {
            if (!object[part[i]]) {
                object[part[i]] = {};
            }
            object = object[part[i]];
        }
        return object;
    }
}

myGlobal.namespace('agent.dom.query')
```
#### 4. 零全局变量

- 使用ES6的块级作用域或者立即执行函数,这里不多做阐述

---

### 第三: 事件处理

#### 1.典型用法

- 事件触发时,通常都要用事件对象event来作为参数回传给回调函数,用作事件处理的程序中,event对象包含了所有和事件相关的信息.而通常在很多场景下,我们只需要用到事件对象的很少一部分信息

```
// 不好的写法
function handle(event) {
    let div = document.getElementById('div');
    div.style.left = event.clientX + 'px';
    div.style.top = event.clientY + 'px';
    div,className = 'al-main'
}

elem.addEventListener('click',handle);

```

---

#### 2.遵循的规则 1: 隔离应用逻辑

- 上述案例中:事件处理程序包含了两方面,一个是用户行为的处理**即点击事件的处理函数**,一个是应用逻辑的处理**即给指定的元素设置属性,类名**

> 应用逻辑是和应用相关的功能性代码,而不是和用户行为相关的.

- ***将应用逻辑从所有事件处理程序中抽离的做法是一种最佳实践***,理由如下
     
     - 因为说不定在其他地方会触发同一段逻辑或者相似的逻辑
     
     - 测试时,如果要直接触发功能型代码,还需要模拟事件的发生,这不是测试的最佳方法

     - 最佳方法是调试功能型代码,单个函数的调用.

```
// 好的做法-拆分应用逻辑
let myGlobal = {
    handle: function(event) {
        this.showPop(event);
    },

    showPop: function(event) {
        let div = document.getElementById('div');
        div.style.left = event.clientX + 'px';
        div.style.top = event.clientY + 'px';
        div,className = 'al-main';
    }
}

elem.addEventListener('click',function(event) {
    myGlobal.handle(event);
})
```  
- 现在,事件处理程序只做了一件事,就是调用 myGlobal.handle().也就是-用户行为的处理只做了一件事.应用逻辑被剥离出去.从而,同一段功能代码的调用就可以在多点发生.

- 也就是说事件处理程序应该专注于 处理用户行为.而所有的应用逻辑 都放在行为逻辑的函数中来处理.这就是隔离应用逻辑.让事件专注于处理行为逻辑.

---

#### 3.规则2: 不要分发事件对象

- 上述代码存在的问题,虽然应用逻辑被剥离出来,**但是事件对象却被无节制的分发**.即使应用逻辑只使用了事件对象的其中两个属性.

- **应用逻辑不应当依赖于event对象来正确完成功能**,理由如下:
  
  - 方法的接口并没有表明哪些数据是必要的.将event对象作为参数并不能告诉你event的哪些属性是有用的,用来干什么

  - 如果进行单元测试,还要重新创建一个event对象传入

  - 这样代码就有点不清晰,容易导致bug

- 最佳的实践方法是,让事件处理程序使用event来处理用户的行为逻辑,然后拿到具体需要的属性来处理应用逻辑,改写上述代码

```
// 好的做法
let myGlobal = {
    handle: function(event) {
        this.showPop(event.clientX,event.clientY);
    },

    showPop: function(x,y) {
        let div = document.getElementById('div');
        div.style.left = x + 'px';
        div.style.top = y + 'px';
        div,className = 'al-main';
    }
}

elem.addEventListener('click',function(event) {
    myGlobal.handle(event);
})
```

-***处理事件时,最好让事件处理程序成为接触到event对象的唯一的函数,事件处理函数在进入应用逻辑之前针对event对象执行任何必要的操作,包括阻止默认事件或阻止事件冒泡.***

```
// 好的做法
let myGlobal = {
    
    // 行为逻辑
    handle: function(event) {
        event.preventDefault();
        event.stopProgation();
        this.showPop(event.clientX,event.clientY);
    },

    // 应用逻辑
    showPop: function(x,y) {
        let div = document.getElementById('div');
        div.style.left = x + 'px';
        div.style.top = y + 'px';
        div,className = 'al-main';
    }
}

elem.addEventListener('click',function(event) {
    myGlobal.handle(event);
})
```

---

### 第四: 避免 **'空比较'**

#### 1. 检测原始值

- typeof 用来检测 number string boolean undefined symbol

- null 直接用 === 和 !=== 检测 建议检测null

```
let elem = document.getElementById('div');
if (elem !== null) {
    // code
}
```

---


#### 2. 检测引用值

- 在JavaScript中,除了原始值之外的都是引用值.如:Object Array Date Error Function

- 使用instanceof .

> 补充 instanceof 的原理是: 先检测这个实例的构造器,之后在该实例的原型链上进行检测,一旦发现该对象,即返回true,否则返回false

```
function Person() {
    this.name = 'harley';
}
function Male() {
    this.name = 'male';
}
let p1 = new Person();

Object.setPrototypeof(Person,Male.prototype);

let p2 = new Person();
p1 instanceof Person    // true
p1 instanceof Male    // false

p2 instanceof Person    // true
p2 instanceof Male    // true
```

---

#### 3.检测数组

- 直接使用Array,isArray()

```
// 原理
Object.prototype.toString().call(value) === '[object Array]'
```

---

#### 4.检测属性

- 推荐使用 in 操作符


- 执行更快,因为只判断属性是否存在,而不会去尝试读取属性的值,所以可以规避一些属性为 ' ' 0 false null undefined 的值


---

### 第五: 将配置数据从代码中抽离

#### 1.什么是配置数据

- 配置数据是应用中写死(hardcoded)的值,且将来可能会被修改

- URL 

- 需要展示给用户的字符串

- 重复的值

- 需要进行设置的值

- 任何可能发生变更的值

---


#### 2.抽离配置数据

```
// 将配置数据抽离出来
let config = {
    MSG_INVALID_VALUE: 'Invalid value',
    URL_INVALID: '/errors/invalid.php',
    CSS_SELECTED: 'selected',
};

function validate(value) {
    if (!value) {
        alert(config.MSG_INVALID_VALUE);
        location.href = config.URL_INVALID;
    }
}
```

---


#### 3.保存配置数据

- 清晰的分隔数据和应用逻辑

- 方法有很多种,也有很多插件可供使用,自行谈论选择如: JSONP 等

---

### 第六: 抛出自定义错误

---

### 第七: 不是你的对象请不要修改它

---

### 第八: 浏览器的嗅探

---

## 自动化工程

### 1.








