## ES6中的变量声明

ES6中新增了let和const来定义变量:

```var```: ES5和ES6中,定义全局变量(是variable的简写)

```let```: 定义局部变量, 替代var

```const```: 定义常量(定义后, 不可修改)

### var局部变量

用var声明的全局变量,有时候会污染整个js的作用域

### let局部变量

```javascript
var a = 2;
{
    let a = 3;
}
console.log(a);
```

上方代码的输出结果是2. 用let声明的变量,只在局部(块级作用域内)起作用

let是防止数据污染, 我们来看下面这个例子

用var声明变量:(可以打印结果,说明循环体外的变量i被污染了)

```
for (var i = 0; i < 10; i++) {
    console.log('循环体中'+ i);
}
console.log('循坏体外' + i);
```

用let声明变量不能打印结果

```
for (let i = 0; i < 10; i++) {
    console.log('循环体中'+ i);
}
console.log('循坏体外' + i);
```

总结: 我们要习惯用let声明,减少var声明带来的**污染全局空间**

为了进一步说明let不会带来污染,需要说明的是:当我们定义了```let a=1```时, 如果我们在同一个作用域内继续定义```let a = 2,```是会报错的

###const:声明变量

在程序开发中, 有些变量是希望声明后,在业务层就不再发生变化, 此时可以用 const来定义

举例:

```
const name = 'junglehunter';//定义常量
```

###变量的结构赋值

ES6允许我们,通过数组或者对象的方法, 对一组变量进行赋值, 这被称为解构

解构赋值在实际开发中可以大量减少我们的代码量,并且让程序结构更清晰

### 数组的结构赋值

举例:

通常情况下,我们在为一组变量赋值时,一般是这样写的:

```
let a = 0;
let b = 1;
let c = 2;
```

现在我们可以通过数组解构的方式进行赋值:

```
let[a,b,c] = [1,2,3];
```

二者的效果是一样的

**解构的默认值**

在解构赋值时,是允许使用默认值的.举例如下:

```javascript
 {
     //一个变量时
     let [foo = true] = [];
     console.log(foo);//输出结果:true
 }
 {
    //两个变量时
    let [a, b] = ['森林小猎人']   //a 赋值为：森林小猎人。b没有赋值
    console.log(a + ',' + b); //输出结果：森林小猎人,undefined
}


{
    //两个变量时
    let [a, b = 'junglehunter'] = ['森林小猎人']   //a 赋值为：森林小猎人。b 采用默认值 junglehunter
    console.log(a + ',' + b); //输出结果：森林小猎人,junglehunter
}
```

`undefined`和`null`的区别：

```javascript
{
    let [a, b = 'smyhvae'] = ['生命壹号', undefined]; //b 虽然被赋值为 undefined，但是 b 会采用默认值
    console.log(a + ',' + b); //输出结果：生命壹号,smyhvae
}

{
    let [a, b = 'smyhvae'] = ['生命壹号', null];  //b 被赋值为 null
    console.log(a + ',' + b); //输出结果：生命壹号,null
}
```

上方代码分析:

underfined: 相当于什么都没有,此时b采用默认值

null: 相当于有值,值为null

### 对象的解构赋值

我们同样可以针对对象,进行解构赋值

**举例如下**

```javascript
let {foo, bar} = {bar: '我是bar的值',foo: '我是foo的值'};
console.log(foo +','+bar);//输出结果:我是键foo的值,我是键bar的值
```

上方代码可以看出, 对象的解构和数组的解构,有一个重要的区别:数组的元素是按次序排序的, 变量的取值由它的位置决定;而对象的属性没有次序,是根据键的取值来决定的

圆括号的使用:

如果变量foo在解构之前就已经定义了,此时你再去解构,就会出现问题. 下面是错误的代码, 编译会报错:

```
let foo = 'haha';
{ foo } = { foo: 'junglehunter'};
console.log(foo);
```

要解决报错,只要在解构的语句外边,加一个圆括号即可:

```
let foo = 'haha';
({ foo } ={ foo: 'junglehunter'});
console.log(foo);//输出结果:junglehunter


```

###字符串解构

字符串也可以解构,这是因为,此时字符串被转化为一个类似数组的对象.举例如下:

```
const [a,b,c,d] = 'junglehunter';
console.log(a);
console.log(b);
console.log(c);
console.log(d);
console.log(typeof a);//输出结果:string
```



## 扩展运算符和rest运算符

扩展运算符的格式为```...```

rest运算符的格式为```...变量名```

#### 扩展运算符

有了ES6,当我们在定义一个方法时, 但是不确定其参数的个数时,我们就可以用扩展运算符作为参数

以前, 我们在定义方法时, 参数要确定个数, 如下: (程序会报错)

```javascript
function fn(a,b,c) {
    console.log(a);
    console.log(b);
    console.log(c);
    console.log(d);
    
}
fn(1,2,3);
```

上方代码中, 因为方法的参数是三个, 但使用时用得了四个参数,所以会报错

现在，我们有了扩展运算符，就不用担心报错的问题了。代码可以这样写：

```javascript
function fn(...arg) {   //当不确定方法的参数时，可以使用扩展运算符
    console.log(arg[0]);
    console.log(arg[1]);
    console.log(arg[2]);
    console.log(arg[3]);
}

fn(1, 2, 3); //方法中定义了四个参数，但只引用了三个参数，ES6 中并不会报错。

```

注意:上方代码中,arg参数之后,不能再加别的参数,否则编译报错

**举例：**数组赋值的问题

我们来分析一段代码：（将数组 arr1 赋值给 arr2）

```javascript
	let arr1 = ['www', 'smyhvae', 'com'];
	let arr2 = arr1;          // 将 arr1 赋值给 arr2，其实是让 arr2 指向 arr1 的内存地址
	console.log('arr1:' + arr1);
	console.log('arr2:' + arr2);
	console.log('---------------------');

	arr2.push('你懂得');  //往arr2 里添加一部分内容
	console.log('arr1:' + arr1);
	console.log('arr2:' + arr2);
```

运行结果：

![](http://img.smyhvae.com/20180304_1950.png)

上方代码中，我们往往 arr2 里添加了`你懂的`，却发现，arr1 里也有这个内容。原因是：`let arr2 = arr1;`其实是让 arr2 指向 arr1 的地址。也就是说，二者指向的是同一个内存地址。

如果不想让 arr1 和 arr2 指向同一个内存地址，我们可以借助扩展运算符来做：

```javascript
	let arr1 = ['www', 'smyhvae', 'com'];
	let arr2 = [...arr1];  //arr2 会重新开辟内存地址
	console.log('arr1:' + arr1);
	console.log('arr2:' + arr2);
	console.log('---------------------');

	arr2.push('你懂得');  //往arr2 里添加一部分内容
	console.log('arr1:' + arr1);
	console.log('arr2:' + arr2);
```

运行结果：

![](http://img.smyhvae.com/20180304_1951.png)

我们明白了这个例子，就可以避免开发中的很多业务逻辑上的 bug。

### ```rest```运算符

rest在英文中是指剩余部分.举个例子

```javascript
function fn(first,second,...arg) {
    console.log(arg.length);
}
fn(0,1,2,3,4,5,6);//调用函数之后,输出结果为5
```

上方代码中的输出结果为5,调用fn()时,里面有七个参数,而arg指的是剩下部分

从上方例子可看出,```reset```运算符适用于:知道前面的一部分参数的数量,但对于后面剩余的参数数量未知的情况.

##for...of循环

ES6中如果我们要遍历一个数组,可以这样做:

```
let arr1 = [1,2,3,4,5];
for(let value of arr1) {
    console.log(value);
}
```

输出结果:

![](http://img.smyhvae.com/20180304_2016.png)

for...of的循环可以避免我们开拓内存空间, 增加代码运行效率, 所以建议大家在以后的工作中使用for...of循环

```for...of```既可以遍历数组,也可以遍历Map对象

## 模版字符串

我们以前让字符串进行拼接的时候, 是这样做的:

```
var name = 'junglehunter';
var age = '23';
console.log('name'+name+'age'+age);//传统写法

```

这种写法,比较繁琐,而且容易出错

现在有了ES6语法,字符串拼接可以这样写:

```
 var name = 'junglehunter';
 var age = 23;
 console.log(`name:${name},age:${age}`);//ES6写法
```

注意,上方代码中字符串拼接使用的是反引号

## 箭头函数

需要说明的是, ES6中, 函数新增了很多特性.例如:

- 参数默认值
- 扩展运算符
- rest参数
- 箭头函数
- this绑定
- 尾调用

这一段,我们来讲一下箭头函数

ES6中定义和调用箭头函数

```
var fn2 = (a,b) => a+b;
console.log(fn(1,2));//输出结果为3
```

上方代码中, 箭头后面的内容,就相当于return的内容(**返回值**)

在箭头函数中,如果方法体内有两句话,那就需要在方法外边加上{}括号如下:

```JavaScript
var fn2 = (a,b) => {
    console.log('haha');
    return a + b;
};
console.log(fn(1,2 ));//输出结果3
```

从上面的箭头函数中,我们可以很清楚的找到函数名, 参数名, 方法体

### 参数默认值

当然在ES6中定义方法时,我们还可以给方法里面的参数加上一个**默认值**(缺省值):

- 方法被调用时,如果没有给参数赋值, 那就是用默认值
- 方法被调用时, 如果给参数赋值了新的值,那就用新的值

如下:

```javascript
var fn2 = (a,b =5) => {
    console.log('haha');
    return a + b;
}
console.log(fn2(1)); //第二个参数使用默认值5,输出结果为:6
console.log(fn2(1,8)); //输出结果为:9
```

需要提醒的是:默认值的后面不能再有没有默认值的变量 比如```(a,b,c)```这三个参数,如果给b设置了默认值, 那么就一定要给c设置默认值

#### this的指向

ES5中, this指向的是函数被调用的对象;而ES6的箭头函数中, this指向的是函数被定义时的外层函数的调用对象

## 模块化

**模块化的意义**：

比如说，当我需要用到jQuery库时，我会把jQuery.js文件引入到我自己代码的最前面；当我需要用到vue框架时，我会把vue.js文件引入到我自己代码的最前面。

可是，如果有20个这样的文件，就会产生20次http请求。这种做法的性能，肯定是不能接受的。

如果把20个文件直接写在一个文件里，肯定是不方便**维护**的。可如果写成20个文件，这20个文件又不好排序。这就是一个很矛盾的事情，于是，模块化就诞生了。

**模块化历程**：commonJS、AMD规范（RequireJS）、CMD规范（SeaJS）；import & export

**export：**

静态化：必须在顶部，不能使用条件语句，自动采用严格模式。（静态化有利于性能以及代码的稳定性）