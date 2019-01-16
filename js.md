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

##### Object
一组数据和功能的集合，使用new关键字创建对象
Object的属性：

- constructor() 构造函数，保存用于创建当前对象的函数
- hasOwnProperty()  检查给定属性是不是在当前对象实例中
- isPrototypeOf()   检查传入的对象是不是当前对象的原型
- propertyIsEnumerable()  检查传入传入的属性能不能使用for in进行枚举
- toLocaleString()  返回对象的字符串表示，该字符与执行环境地区相对应(比如时间类型)
- toString() 返回对象的字符串表示
- valueOf()  返回对象的字符串、数值或布尔值表示

#### 运算符
##### 布尔操作符
**js中的布尔操作符可以用于任何js类型**
1、逻辑非!表示，规则：

- 对象      false
- 空字符串  true
- 非空字符串 false
- 数值0  true
- 数值非0 false
- null、NaN、undefined true
和Boolean(),的结果刚好相反

2、逻辑与&&,如果有一个不是布尔值的返回规则

- 如果第一个操作数是对象，则返回第二个操作数
- 如果第二个操作数是对象，则只有第一个操作数的求值结果为true时才会返回该对象
- 如果两个操作数都是对象，则但会第二个操作数
- 第一个操作数为null，返回null
- 第一个操作数为NaN，返回NaN
- 第一个操作数为undefined，返回undefined

3、逻辑或||,如果有一个不是布尔值的返回规则

- 如果第一个操作数是对象，则返回第一个操作数
- 如果第一个操作数的求值结果为false，返回第二个操作数
- 如果两个操作数都是对象，则但会第一个操作数
- 两个操作数为null，返回null
- 两个操作数为NaN，返回NaN
- 两个操作数为undefined，返回undefined

##### 相等操作符
1、相等和不相等(==、!=),先转换再比较
转换规则:

- 如果一个操作数是布尔值，会转换为数值：false-0，true-1；
- 如果一个操作数是字符串，另一个操作数是数值，比较之前字符串转换为数值
- 如果一个操作数是对象，另一个操作数不是，调用对象的valueOf()方法。
- null和underfunded相等，比较之前，不能将null和undefined转换成其他任何值
- NaN,相等操作符返回false
- 两个都是对象，比较是不是同一个对象

2、全等和不全等(=== ,!==),不转换进行比较

#### 语句

- if(condition),对于condition中的条件，会调用Boolean()函数转成boolean值
- for-in 枚举对象的属性，对象里面的属性是没有顺序的
- label，在代码中添加标签，一般在循环当中配合break、continue使用
- with，将代码的作用于设置到一个特定的对象中

#### 函数
- 函数的参数对象是arguments，参数个数没有限制(一个函数定义是两个实际上可以传一个三个..),所以这样就不会存在重载的情况