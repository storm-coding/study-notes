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
