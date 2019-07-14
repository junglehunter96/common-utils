## 初识TypeScript 

TypeScript 作为 JavaScript 语言的超集，它为 JavaScript 添加了可选择的类型标注，大大增强了代码的可读性和可维护性。同时，它提供最新和不断发展的 JavaScript 特性，能让我们建立更健壮的组件。

### 特点

- 始于JavaScript，归于JavaScript

TypeScript 可以编译出纯净、 简洁的 JavaScript 代码，并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或更高版本）的JavaScript 引擎中。

- 强大的工具构建大型应用程序

类型允许 JavaScript 开发者在开发 JavaScript 应用程序时使用高效的开发工具和常用操作比如静态检查和代码重构。

类型是可选的，类型推断让一些类型的注释使你的代码的静态验证有很大的不同。类型让你定义软件组件之间的接口和洞察现有 JavaScript 库的行为。

- 先进的JavaScript

TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件。

这些特性为高可信应用程序开发时是可用的，但是会被编译成简洁的 ECMAScript3（或更新版本）的JavaScript。

### 使用和编写TypeScript

在命令行工具输入如下命令:

```
npm install typescript -g
tsc -V
```

如果你是mac电脑，记得使用`sudo npm install typescript -g`指令来进行安装。

###类型注解

生成项目目录

1. 初始化项目：进入你的编程文件夹后，可以使用`npm init -y`来初始化项目，生成package.json文件。
2. 在终端中输入`tsc --init`：创建`tsconfig.json`文件，它是一个`TypeScript`项目的配置文件，可以通过读取它来设置`TypeScript`编译器的编译参数。
3. 安装@types/node,使用`npm install @types/node --dev-save`进行安装。这个主要是解决模块的声明文件问题。

在当前目录下编写`hello.ts`如下:

```
function greeter(person:string) {
return `Hello,${Person}`
}
let Person = 'Lin'
console.log(greeter(Person))
```

TypeScript里面的类型注解是一种轻量级的为函数或者变量添加类型约束的方式。

在上面这个函数里面，我们希望greeter的函数接受参数类型为字符串，当我们尝试改变`greeter`的调用为一个数字时:

```
function greeter(person:string) {
return `Hello,${Person}`
}
let Person = [1,2,3]
console.log(greeter(Person))
```

重新编译，你会看到产生一个错误了：

```
error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
```

TypeScript提供了静态的代码分析，它可以分析代码结构和提供的类型注解。

### 接口

我们可以使用接口来描述一个拥有`firstName`和`lastName`字段的对象。在`TypeScript`里，只在两个类型内部的结构兼容，那么这两个类型就是兼容的。

这就允许我们在实现接口时候只要保证包含了接口要求的结构就可以了，而不必明确地使用`implements`语句。

```
interface Person{
fistName:string
lastName:string
}
function greeter(person:Person) {
return `Hello,${person.firstName}${person.lastName}`
}
let Name = {
fistName:'zhou',
lastName:'xin'
}
console.log(greeter(Name))
```

### 类

最后，我们可以使用类改写这个例子。

```
class User {
  firstName:string
  lastName:string
  fullName:string
  constructor(firstName:string,lastName:string) {
    this.firstName = firstName
    this.lastName = lastName
    this.fullName = `${firstName} ${lastName}`
  }
}

interface Person {
  firstName:string
  lastName:string
}

function greeter(person:Person) {
  return `Hello,${person.firstName} ${person.lastName}`
}
let user = new User('zhou','xin')
console.log(greeter(user))
```

运行`tsc hello.ts`，我们可以打开编译后的JavaScript文件查看

````
var User = /** @class */ (function () {
    function User(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.fullName = firstName + " " + lastName;
    }
    return User;
}());
function greeter(person) {
    return "Hello," + person.firstName + " " + person.lastName;
}
var user = new User('zhou', 'xin');
console.log(greeter(user));
````

nice!

