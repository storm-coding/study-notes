## ECMAScript学习笔记
### 基本语法
#### 变量
弱类型语言，不需要定义类型
```
{
    var message      局部变量
    message            全局变量
}
```
#### 数据类型：
 基本数据类型 Undefined、null、boolean、number、string
 应用类型  object
##### typeof  
检测变量类型，返回字符串形式
"undefined"  未定义的值
"boolean"  布尔
"string"  字符串
"number"  数值类型
"object"  对象或null
"function"  函数
##### Undefined
声明之后，未初始化。只有一个值(undefined)
不能使用typeof 区分未初始化和未定义的值。即：
```
var name = "zhangsan"
alert(name); // undefined
alert(age); // undefined
```
##### Null
空对象指针，只有一个值(null)
```
var name = null
typeof name // object
null == undefined  //true, 因为undefined派生自unll，undefined是为了区分空对象和未初始化后面引入的
```
##### Boolean
只有两个值：false、true;(区分大小写，True，False不是boolean值，只是标识符)
boolean转换规则：是调用Boolean构造函数进行的
| 数据类型 | true | false |
| ------ | ------ | ------ |
| Boolean | 非空字符串 | 空字符串 |
| Number | 非零数值 | 0、NaN |
| Object | 非null对象 | null |
| Undefined | 无 | Undefined |
**if里面的条件判断就是调用的Boolean()**

##### Number
定义不同进制的数：
```
var num1 = 100  //十进制
var num2 = 020  //八进制 16，以0开头
var num1 = 0xB  //十六进制 11，以0x开头
```
浮点数：数值中包含.
如： var num = 1.1
**不要用浮点数用来判断：if(a + b == 0.3)，因为存在精度丢失，所以永远都不会为true**

数值范围：
Number.MIN_VaLUE - Number.MAX_VaLUE
超过则用-Infinity(负无穷)，Infinity(正无穷)表示

NaN  非数值：
这个用来表示本来要返回一个操作数，但是因为，某种原因倒是不能返回数字
**所有涉及NaN的操作都会返回NaN,NaN与任何操作数都不想等：NaN==Nan(false)**
用isNaN来判断是不是数字，这个会首先调用valueOf,不能转数字再调用toStringz转数字如果都不行就返回true： 
```
isNaN("string") // true
```
数值转换
Number()
-  Boolean  false - 0 true - 1
-  null 0
-  Undefined  NaN
-  字符串： 空字符串: 0, 无法转number的:NaN
-  对象，先调用valueOf，如果是NaN,再调用toString

parseInt,parseFloat 转换中特殊的地方：

- 空字符串NaN
- 会去掉之前的空串，从第一个非空串，到一个非字符结束，如果是字符则返回NaN 

##### String
特点：和java中的string类似。都是不可变类型。
```
var str = 'str';
str = str + 'ing';
```
这里会先创建一个字符串'str'赋值给str，后面再创建一个'string'再赋值给str。
**字符串转换**
有两种方式: 
1、toString()
除了null和undefined都有toString方法。object类型是应为object基类中就有这个方法。所以所有派生类也包含。基本类型中是因为在调用toString的时候会转成包装类型。
2、String()
构造函数方式转换。规则：

- 除了unll和undefined，调用对象的toString方法 
- null  返回null
- undefined 返回undefined