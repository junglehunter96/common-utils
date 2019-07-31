1. 定时器的this，默认指向谁？

   - 首个参数为字符串，执行对象为表达式（IIFE)，在当前环境下执行，因此this指向当前调用对象(可作为参数传入)
   - 首个参数为函数，定时器触发线程在定时完成之后，将任务加入task，在被调入调用栈时执行，此时this默认指向window
2. 数字0 属于布尔类型的那一种 
   - undefined,null,0,NaN在隐式转换的情况下,对应的boolean类型为false
3. 变量a等于null，typeof a；输出什么
   - [Object] ; JS的历史遗留问题
4. Null转数字类型,输出什么
	- 0
5. Undefined转数字类型,输出什么
  - NaN
6. Js通过什么来实现继承
   - 原型
7. 怎么判断两个对象相等
   - === , 实际上判断的是两变量是否是同一对象，即引用地址是否相同，如果要判断对象的键值对是否相等，可以采用for in循环对键值对进行遍历判断
8. 怎么用原生js获取浏览器的滚动距离

- document.documentElement.scrollHeight