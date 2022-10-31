# 一、JS基本语法

## 1.1、注释

单行注释

```js
//  单行注释
```

多行注释

```js
/*
	多行注释
*/   
```

## 1.2、输入输出语句

| 方法             | 描述                           |
| ---------------- | ------------------------------ |
| alert(msg)       | 浏览器弹出警示框               |
| console.log(msg) | 浏览器控制台打印输出信息       |
| prompt(info)     | 浏览看弹出输入框，用户可以输入 |

## 1.3、变量

```js
//使用var关键字来声明一个变量
var a;  
//为变量赋值
a = 1; 
//声明和赋值同时进行
var a = 1;   
//同时声明多个变量时，只需要写一个 var，多个变量名之间使用英文逗号隔开。
var 1, b ='abc',c=2;
var age; console.log(age);//只声明，不赋值	undefined
console.log(age)//不声明 不赋值 直接使用	报错
age = 10;console.log(age);//不声明 只赋值	10
```

**变量的命名规范**

- 由字母(A-Z,a-z)，数字(0-9)，下划线(_)，美元符号($)组成，如:usrAge,num01,__name
- 严格区分大小写。

- 不能以数字开头。

- 不能是关键字，保留字。

- 遵循驼峰命名法。


## 1.4、数据类型

JavaScript **是一种弱类型或者说动态语言。**这意味着不用提前声明变量的类型，在程序运行过程中，类型会被自动确定。

```js
var a = 10;//这是一个数字型
var bk = 'abc';	//这是一个字符串
```

在代码运行时，变量的数据类型是由 JS引擎 根据 = 右边变量值的数据类型来判断 的，运行完毕之后， 变量就确定了数据类型。

JavaScript 拥有动态类型，同时也意味着相同的变量可用作不同的类型

```js
var x = 1;		//x为数字
var x = "abc";	//x为字符串
```

JS 把数据类型分为两类：

- 基本数据类型(Number,String,Boolean,Undefined,Null)
- 引用数据类型(Object)

### 1.4.1 数字型Number

JavaScript 数字类型既可以用来保存整数值，也可以保存小数(浮点数）。

```js
var a = 1;		//整数
var a = 1.22;	//小数

JS中数值的最大值：Number.MAX_VALUE 1.7976931348623157e+308
JS中数值的最小值：Number.MIN_VALUE 5e-324

特殊的数字：能赋值给变量
Infinity 正无穷 打印输出isFinite() { [native code] }
-Infinity 负无穷 打印输出NaN
NaN 非法数字 打印输出NaN

其他进制的数字的表示：
0b开头表示二进制，但是不是所有的浏览器都支持
0开头表示八进制
0x开头表示十六进制
```

- 
  isNaN( )这个方法用来判断非数字，并且返回一个值，如果是数字返回的是false，如果不是数字返回的是true
- 使用typeof检查一个Number类型的数据时，会返回"number"（包括NaN 和 Infinity）

### 1.4.2 字符串型String

字符串型可以是引号中的任意文本，其语法为 “双引号” 或 "单引号’’推荐使用单引号。

```js
var str = "abc";//使用双引号表示字符串
var str = 'abc';//使用单引号表示字符串
```

在字符串中使用\作为转义字符

```js
\'  ==> '  
\"  ==> "  
\n  ==> 换行  
\t  ==> 制表符  
\\  ==> \	  
```

JS可以用 单引号嵌套双引号，或者用 双引号嵌套单引号

```js
var a = '我很"帅"'; //输出：我很'帅'
var b = "我很'帅'";//输出：我很"帅"
var c = '我很'帅'';//报错
var d = "我很"帅"";//报错
```

利用String的length属性可以获取字符串长度

```js
console.log('abc'.length)//3
```

任何数据类型与字符串拼接都是字符串型

```js
console.log('abc' + 13)//abc13
```

使用typeof运算符检查字符串时，会返回"string"

### 1.4.3 布尔型Boolean

布尔类型有两个值：true 和 false 。
布尔型和数字型相加的时候， true 的值为 1 ，false 的值为 0。

```js
var flag = true;
console.log(flag + 1); // 2
```

使用typeof检查一个布尔值时，会返回"boolean"

### 1.4.4 未定义undefined

一个声明后没有被赋值的变量会有一个默认值 undefined 

```js
var a;
console.log(a);//undefined
console.log(a + 'abc'); //undefinedabc
console.log(a + NaN); //NaN 
console.log(a + false); //NaN 
console.log(a + 18); //NaN 
```

使用typeof检查一个Undefined类型的值时，会返回"undefined"

### 1.4.5 空值null

一个声明变量给 null 值，里面存的值为空

```js
var space = null;
console.log(space + 'abc'); //spaceabc
console.llog(space + 1); // 1 
```

**使用typeof检查一个Null类型的值时会返回"object"**

## 1.4、数据类型转换

类型转换就是指将其他的数据类型，转换为String Number 或 Boolean

### 1.4.1 转换为String

```js
var num = 1; 
//toString()
console.log(num.toString());
//String()
console.log(String(num));
//字符串拼接
console.log(num+"");
```

注意：**这个方法不适用于null和undefined**
由于这两个类型的数据中没有方法，所以调用toString()时会报错

### 1.4.2 转换为数字型

**Number()函数**

```js
//String-->Number
console.log(Number("123"));//123
console.log(Number("12.123"));//12.123
console.log(Number("1abc"));//NaN
console.log(Number("abc"));//NaN
console.log(Number(""));//0
console.log(Number("   "));//0
//Boolean-->Number
console.log(Number(true));//1
console.log(Number(false));//0
//null-->Number
console.log(Number(null))//0
//undefined-->Number
console.log(Number(undefined))//NaN
```

**parseInt()函数**

它会**先将其转换为String**然后在操作 parseInt()
可以将**一个字符串中的有效的整数位**提取出来，并转换为Number

```js
//String-->Number
console.log(parseInt("123"));//123
console.log(parseInt("12abc"));//12
console.log(parseInt("12.123"));//12
console.log(parseInt("abc"));//NaN
console.log(parseInt(""));//NaN
console.log(parseInt("   "));//NaN
//Boolean-->Number
console.log(parseInt(true));//NaN
console.log(parseInt(false));//NaN
//null-->Number
console.log(parseInt(null))//NaN
//undefined-->Number
console.log(parseInt(undefined))//NaN
```

**parseFloat()函数**

```js
//String-->Number
console.log(parseFloat("123"));//123
console.log(parseFloat("12abc"));//12
console.log(parseFloat("12.123"));//12.123
console.log(parseFloat("abc"));//NaN
console.log(parseFloat(""));//NaN
console.log(parseFloat("   "));//NaN
//Boolean-->Number
console.log(parseFloat(true));//NaN
console.log(parseFloat(false));//NaN
//null-->Number
console.log(parseFloat(null))//NaN
//undefined-->Number
console.log(parseFloat(undefined))//NaN
```

**隐式转换**

```js
console.log('12'-0);  // 12
console.log('123' - '120');  //3
console.log('123' * 1);  // 123
```

### 1.4.3 转换为布尔型

**Boolean()**

否定的值会被转换为false，如 ’ ’ , 0, NaN , null , undefined

其余的值都会被被转换为true

```js
console.log(Boolean('')); //false
console.log(Boolean(0));  //false
console.log(Boolean(NaN)); //false
console.log(Boolean(null)); //false
console.log(Boolean(undefined)); //false
console.log(Boolean('小白')); //true
console.log(Boolean(12));   //true
```

**隐式类型转换**

**为任意的数据类型做两次非运算，即可将其转换为布尔值**

```js
var a = "hello";
console.log(!!a)//true
```

## 1.5、运算符

### 1.5.1 算术运算符

```js
10 + 20 =30
10 - 20 =-10
10 * 20 =200
10 / 20 =0.5
9 % 2 =1
a++;
++a;
```

浮点数的精度问题
浮点数值的最高精度是17位小数，但在进行算数计算时其精确度远远不如整数

```js
var result = 0.1 +0.2; //结果不是0.3，0.30000000000000004
console.log(0.07 * 100); //结果不是7，而是7.000000000000001
```

所以不要直接判断两个浮点数是否相等

### 1.5.2 关系运算符

```
1 > 2	false
2 >= 2	true
3 <= 2	false
==	判断	判断两边值是否相等(注意此时有隐士转换)
===	全等	判断两边的值和数据类型是否完全相同
37 == 37	true
37 != 37	false
37 === '37'	false
37 !== '37' true
```

### 1.5.3 逻辑运算符

**非运算（!）**
非运算可以对一个布尔值进行取反，true变false false边true
当对非布尔值使用!时，会先将其转换为布尔值然后再取反
我们可以利用!来将其他的数据类型转换为布尔值

**逻辑与（&&）**
&&可以对符号两侧的值进行与运算
只有两端的值都为true时，才会返回true。只要有一个false就会返回false。
与是一个短路的与，如果第一个值是false，则不再检查第二个值
对于非布尔值，它会将其转换为布尔值然后做运算，并返回原值
规则：
1.如果第一个值为false，则返回第一个值
2.如果第一个值为true，则返回第二个值

**逻辑或（||）**
||可以对符号两侧的值进行或运算
只有两端都是false时，才会返回false。只要有一个true，就会返回true。
或是一个短路的或，如果第一个值是true，则不再检查第二个值
对于非布尔值，它会将其转换为布尔值然后做运算，并返回原值
规则：
1.如果第一个值为true，则返回第一个值
2.如果第一个值为false，则返回第二个值

### 1.5.4 三元运算符：

语法：条件表达式?语句1:语句2;
执行流程：
先对条件表达式求值判断，
如果判断结果为true，则执行语句1，并返回执行结果
如果判断结果为false，则执行语句2，并返回执行结果

## 1.6、流程控制

### 1.6.1 分支结构

**if语句**

```js
if (条件表达式) {
    //条件成立执行的代码语句
}
```

**if ...else... 语句**

```js
if(条件表达式){
    //条件成立执行的代码
}
else{
   //条件不成立执行的代码
}
```

**if... else if ...else...语句**

```js
if(条件表达式1){
  语句1;
}
else if(条件表达式2){
   语句2;
}
else if(条件表达式3){
  语句3;
}
else{
   //上述条件都不成立执行此处代码
}
```

**switch语句**

```js
switch(表达式){
  case value1:
     //表达式等于 value1 时要执行的代码
     break;
  case value2:
     //表达式等于value2 时要执行的代码
     break;
  default:
     //表达式不等于任何一个value时要执行的代码
     break;
}
```

### 1.6.2 循环结构

**for循环**

```js
for(初始化变量;条件表达式;操作表达式){
   //循环体
}
```

**while循环**

```js
while(条件表达式){
  //循环体
}
```

**do while循环**

```js
do {
  //循环体
}while(条件表达式);
```

先执行一次循环体代码再判断条件表达式
**continue 关键字**
continue 关键字用于立即跳出本次循环，继续下一次循环
**break关键字**
break 关键字用于立即跳出整个循环

## 1.7 数组

### 1.7.1 创建数组

```js
// 利用new关键字
var arr = new Array(); 
// 利用数组字面量方式创建数组 
var arr =[];
var arr =['小白','小黑','小黄','瑞奇'];
// 数组中可以存放任意类型的数据，例如字符串，数字，布尔值等
var arrStus =['小白'，12,true,28.9];
```

### 1.7.2 数组赋值

索引 (下标) ：用来访问数组元素的序号（数组下标从 0 开始）

使用“数组名.length”可以访问数组元素的数量（数组长度）

可以通过修改 length 长度来实现数组扩容的目的

数组默认值就是 undefined

```js
var arr = ['red', 'green', 'blue', 'pink'];
arr.length = 7;
console.log(arr);
console.log(arr[4]);
console.log(arr[5]);
console.log(arr[6]);
```

### 1.7.3 遍历数组

```js
var arr = ['red','green', 'blue'];
for (var i = 0; i < arr.length; i++){
    console.log(arrStus[i]);
}
```

使用forEach()方法来遍历数组（不兼容IE8）

```js
数组.forEach(function(value , index , obj){  
  
});  
```

- forEach()方法需要一个回调函数作为参数，
- 数组中有几个元素，回调函数就会被调用几次，
- 每次调用时，都会将遍历到的信息以实参的形式传递进来，我们可以定义形参来获取这些信息。
  - value:正在遍历的元素
  - index:正在遍历元素的索引
  - obj:被遍历对象

### 1.7.4 数组的方法

| functionName | function                                                     |
| ------------ | ------------------------------------------------------------ |
| push()       | 用来向数组的末尾添加一个或多个元素，并返回数组新的长度       |
| pop()        | 用来删除数组的最后一个元素，并返回被删除的元素               |
| unshift()    | 向数组的开头添加一个或多个元素，并返回数组的新的长度         |
| shift()      | 删除数组的开头的一个元素，并返回被删除的元素                 |
| reverse()    | 可以用来反转一个数组，它会对原数组产生影响                   |
| concat()     | 可以连接两个或多个数组，它不会影响原数组，而是新数组作为返回值返回 |

**lice(sart,[end])**

```
 可以从一个数组中截取指定的元素  
 该方法不会影响原数组，而是将截取到的内容封装为一个新的数组并返回  
 参数：  
	1.截取开始位置的索引（包括开始位置）  
	2.截取结束位置的索引（不包括结束位置）  
		 第二个参数可以省略不写，如果不写则一直截取到最后  
	 参数可以传递一个负值，如果是负值，则从后往前数  
```

**splice()**

```
 可以用来删除数组中指定元素，并使用新的元素替换  
	该方法会将删除的元素封装到新数组中返回  
 参数：  
	1.删除开始位置的索引  
	2.删除的个数  
	3.三个以后，都是替换的元素，这些元素将会插入到开始位置索引的前边  
```

**join([splitor])**

```
可以将一个数组转换为一个字符串
参数：
需要一个字符串作为参数，这个字符串将会作为连接符来连接数组中的元素
如果不指定连接符则默认使用,
```

**sort()**

```
可以对一个数组中的内容进行排序，默认是按照Unicode编码进行排序
调用以后，会直接修改原数组。
可以自己指定排序的规则，需要一个回调函数作为参数：

我们可以自己来指定排序的规则
我们可以在sort()添加一个回调函数，来指定排序规则，
回调函数中需要定义两个形参,
浏览器将会分别使用数组中的元素作为实参去调用回调函数
使用哪个元素调用不确定，但是肯定的是在数组中a一定在b前边

浏览器会根据回调函数的返回值来决定元素的顺序，
    如果返回一个大于0的值，则元素会交换位置
    如果返回一个小于0的值，则元素位置不变
    如果返回一个0，则认为两个元素相等，也不交换位置

    如果需要升序排列，则返回 a-b
    如果需要降序排列，则返回b-a
```

# 二、函数

函数：就是封装了一段可被重复调用执行的代码块。通过此代码块可以实现大量代码的重复使用。

## 2.1、 函数的使用

函数在使用时分为两步：声明函数和调用函数

```js
//函数的两种声明方式
//命名函数
function 函数名(){
     //函数体代码
}
//匿名函数
var f = function(){
     //函数体代码
}
//立即执行函数
(function(){  
    //函数体代码
})(); 
//调用函数
函数名();
f();
```

- 声明函数本身并不会执行代码，只有调用函数时才会执行函数体代码。
- 命名函数调用可以在函数声明之前，也可以之后。匿名函数调用只能在函数声明之后。

## 2.2、 函数的参数

1.3.1、形参和实参🔥
在声明函数时，可以在函数名称后面的小括号中添加一些参数，这些参数被称为形参，而在调用该函数时，同样也需要传递相应的参数，这些参数被称为实参。

```js
// 带参数的函数声明
function 函数名(形参1, 形参2 , 形参3...) { // 可以定义任意多的参数，用逗号分隔
  // 函数体
}
// 带参数的函数调用
函数名(实参1, 实参2, 实参3...); 
```


函数调用的时候实参值是传递给形参的

当形参和实参个数不匹配时

- 实参个数等于形参个数：输出正确结果
- 实参个数多于形参个数：只取到形参的个数
- 实参个数小于形参个数：多的形参定义为undefined

注意：在JavaScript中，**形参的默认值是undefined**

## 2.3、 函数的返回值

```js
// 声明函数
function 函数名（）{
    ...
    return  需要返回的值;
}
// 调用函数
函数名();    // 此时调用函数就可以得到函数体内return 后面的值
```

- 在使用 return 语句时，函数会停止执行，并返回指定的值

- 如果函数没有 return ，返回的值是 undefined

- 
  return 只能返回一个值。如果用逗号隔开多个值，以最后一个为准

## 2.4 、arguments的使用

当我们不确定有多少个参数传递的时候，可以用 arguments 来获取。在 JavaScript 中，arguments 实际上它是当前函数的一个内置对象。所有函数都内置了一个 arguments 对象，arguments 对象中存储了传递的所有实参。

- arguments存放的是传递过来的实参

- arguments展示形式是一个伪数组，因此可以进行遍历。伪数组具有以下特点

  - 具有 length 属性


  - 按索引方式储存数据


  - 不具有数组的 push , pop 等方法


```js
// 函数声明
function fn() {
    console.log(arguments);  //里面存储了所有传递过来的实参
    console.log(arrguments.length); // 3
    console.log(arrguments[2]); // 3
}

// 函数调用
fn(1,2,3);
```

## 2.5、作用域

通常来说，一段程序代码中所用到的名字并不总是有效和可用的，而限定这个名字的可用性的代码范围就是这个名字的作用域。作用域的使用提高了程序逻辑的局部性，增强了程序的可靠性，减少了名字冲突。

### 2.5.1 函数作用域

- 全局作用域：作用于所有代码执行的环境(整个 script 标签内部)或者一个独立的 js 文件
- 局部（函数）作用域：作用于函数内的代码环境，就是局部作用域。 因为跟函数有关系，所以也称为函数作用域
- JS 中没有块级作用域(在ES6之前)

### 2.5.2 变量的作用域

全局变量

- 在全局作用域下声明的变量叫做全局变量（在函数外部定义的变量）
- 全局变量在代码的任何位置都可以使用

- 在全局作用域下 var 声明的变量 是全局变量

- 特殊情况下，在函数内不使用 var 声明的变量也是全局变量（不建议使用）


局部变量

- 在局部作用域下声明的变量叫做局部变量（在函数内部定义的变量）
- 局部变量只能在该函数内部使用

- 在函数内部 var 声明的变量是局部变量

- 函数的形参实际上就是局部变量


3.3、区别

- 全局变量：在任何一个地方都可以使用，只有在浏览器关闭时才会被销毁，因此比较占内存
- 局部变量：只在函数内部使用，当其所在的代码块被执行时，会被初始化；当代码块运行结束后，就会被销毁，因此更节省内存空间


## 2.6 、预解析

首先来看几段代码和结果：

```js
console.log(num);  //会报错 num is undefined
/**********************************************/
console.log(num);  // undefined
var num = 10;   
/**********************************************/
// 命名函数(自定义函数方式):若我们把函数调用放在函数声明上面
fn();			//11
function fn() {
    console.log('11');
/**********************************************/
// 匿名函数(函数表达式方式):若我们把函数调用放在函数声明上面
fn();
var  fn = function() {
    console.log('22'); // 报错
}
//相当于执行了以下代码
var fn;
fn();      //fn没赋值，没这个，报错
var  fn = function() {
    console.log('22'); //报错
}
```


JavaScript 代码是由浏览器中的 JavaScript 解析器来执行的。JavaScript 解析器在运行 JavaScript 代码的时候分为两步：预解析和代码执行。

- 预解析：js引擎会把js里面所有的 var 还有 function 提升到当前作用域的最前面

- 代码执行：从上到下执行JS语句


### 2.6.1 变量预解析(变量提升)

变量预解析也叫做变量提升、函数提升

变量提升: 变量的声明会被提升到当前作用域的最上面，变量的赋值不会提升

```js
console.log(num);  // 结果是多少？
var num = 10;   
// undefined

//相当于执行了以下代码
var num;		// 变量声明提升到当前作用域最上面
console.log(num);
num = 10;		// 变量的赋值不会提升
```

### 2.6.2 函数预解析(函数提升)

函数提升： 函数的声明会被提升到当前作用域的最上面，但是不会调用函数。

```js
fn();				//11
function fn() {
    console.log('11');
}
// 匿名函数(函数表达式方式):若我们把函数调用放在函数声明上面
fn();
var  fn = function() {
    console.log('22'); // 报错
}

//相当于执行了以下代码
var fn;
fn();      //fn没赋值，没这个，报错
var  fn = function() {
    console.log('22'); //报错
}
```

# 三、对象

对象是由属性和方法组成的：

## 3.1 对象的创建与调用

```js
//利用字面量创建对象
var star = {
    name : 'li',
    age : 18,
    sex : '男',
    sayHi : function(){
        alert('大家好啊~');
    }
};
对象里面的属性调用 : 对象.属性名 
对象里面属性的另一种调用方式 : 对象[‘属性名’]
对象里面的方法调用：对象.方法名() 
console.log(star.name);     // 调用名字属性
console.log(star['name']);  // 调用名字属性
star.sayHi();   //调用方法

//利用 new Object 创建对象
var obj = new Object(); //创建了一个空的对象
obj.name = '张三丰';
obj.age = 18;
obj.sex = '男';
obj.sayHi = function() {
    console.log('hi~');
}

//利用构造函数创建对象
构造函数用于创建某一类对象，其首字母要大写
构造函数要和 new 一起使用才有意义
function Star(uname,age,sex) {
    this.name = uname;
    this.age = age;
    this.sex = sex;
    this.sing = function(sang){
        console.log(sang);
    }
}
var ldh = new Star('刘德华',18,'男');
console.log(typeof ldh) //object
console.log(ldh.name); //刘德华
ldh.sing('冰雨'); //冰雨
```

## 3.2 new关键字

new 在执行时会做四件事:

- 在内存中创建一个新的空对象。
- 让 this 指向这个新的对象。
- 执行构造函数里面的代码，给这个新对象添加属性和方法
- 返回这个新对象（所以构造函数里面不需要return）

## 3.3 遍历对象的属性

```js
for(var k in obj) {
    console.log(k);		//这里的 k 是属性名
    console.log(obj[k]);//这里的 obj[k] 是属性值
}
```

## 3.4 内置对象

### 3.4.1 Math对象

Math 对象不是构造函数，不用实例化就可以使用

| 方法          | 描述                   |
| ------------- | ---------------------- |
| Math.PI       | 圆周率                 |
| Math.floor()  | 向下取整               |
| Math.ceil()   | 向上取整               |
| Math.round()  | 四舍五入               |
| Math.abs()    | 绝对值                 |
| Math.max()    | 最大值                 |
| Math.min()    | 最小值                 |
| Math.random() | 随机返回一个[0，1)的数 |

### **3.4.2 Data对象**

Date 对象和 Math 对象不一样，他是一个构造函数，所以我们需要实例化后才能使用

```js
var now = new Date();
console.log(now);//Mon Oct 31 2022 20:53:06 GMT+0800 (中国标准时间)
```

**Date()构造函数的参数**

- 如果Date()不写参数，就返回当前时间
- 如果Date()里面写参数，就返回参数里面的时间

```js
var now =  new Date('2019/5/1');//Wed May 01 2019 00:00:00 GMT+0800 (中国标准时间)
var now =  new Date('2019-5-1');//Wed May 01 2019 00:00:00 GMT+0800 (中国标准时间)
console.log(now);
```

**Data中的方法**



方法名	|说明
-|-
getFullYear()|	获取当年
getMonth()|	获取当月(0-11)
getDate()|	获取当天日期	
getDay()|	获取星期几(周日0到周六6)
getHours()|	获取当前小时	
getMinutes()|	获取当前小时	
getSeconds()	|获取当前秒钟	
date.valueOf()	|得到现在时间距离1970.1.1总的毫秒数	
date.getTime()	|得到现在时间距离1970.1.1总的毫秒数	

## 3.5、字符串对象

| 方法                    | 说明                                                         |
| ----------------------- | ------------------------------------------------------------ |
| indexOf(str,begin)      | 返回指定内容在元字符串中的位置，如果找不到就返回-1，开始的位置是index索引号 |
| lastIndexOf()           | 从后往前找，只找第一个匹配的                                 |
| charAt(index)           | 返回指定位置的字符(index字符串的索引号)                      |
| charCodeAt(index)       | 获取指定位置处字符的ASCII码(index索引号)                     |
| concat(str1,str2,str3…) | 拼接字符串                                                   |
| substr(start,length)    | 从 start 位置开始(索引号), length 取的个数。                 |
| slice(start,end)        | 从 start 位置开始，截取到 end 位置 ，end 取不到 (两个都是索引号) |
| substring(start,end)    | 从 start 位置开始，截取到 end 位置 ，end 取不到              |
| replace()               | 在字符串中用一些字符替换另一些字符                           |
| split()                 | 方法用于切分字符串，它可以将字符串切分为数组。在切分完毕之后，返回的是一个新数组。 |
| toUpperCase()           | 转换大写                                                     |
| toLowerCase()           | 转换小写                                                     |

# 四、DOM

## 4.1、DOM简介

文档对象模型（Document Object Model，简称 DOM），是 W3C 组织推荐的处理可扩展标记语言（HTML或者XML）的标准编程接口

W3C 已经定义了一系列的 DOM 接口，通过这些 DOM 接口可以改变网页的内容、结构和样式。

- 文档：一个页面就是一个文档，DOM中使用doucument来表示
- 元素：页面中的所有标签都是元素，DOM中使用 element 表示
- 节点：网页中的所有内容都是节点（标签，属性，文本，注释等），DOM中使用node表示
- DOM 把以上内容都看做是对象

## 4.2、获取元素

| 方法                                    | 描述                             |
| --------------------------------------- | -------------------------------- |
| doucument.getElementByld('id')          | 根据ID获取                       |
| doucument.getElementsByTagName(标签名') | 根据标签名获取                   |
| document.getElementsByClassName('类名') | 根据类名获取                     |
| document.querySelector('选择器')        | 根据指定选择器返回第一个元素对象 |
| document.querySelectorAll('选择器')     | 根据指定选择器返回所有元素对象   |
| document.body                           | 返回body元素对象                 |
| document.documentElement;               | 返回html元素对象                 |

 注意：querySelector 和 querySelectorAll 里面的选择器需要加符号,比如: document.querySelector('#nav');

## 4.3、事件基础

事件三要素

- 事件源(谁)
- 事件类型(什么事件)
- 事件处理程序(做啥)

执行事件的步骤

- 获取事件源
- 注册事件(绑定事件)
- 添加事件处理程序(采取函数赋值形式)

### 4.3.1 鼠标事件

事件	|触发条件
-|-
onclick|	鼠标点击左键触发
onmouseover|	鼠标经过触发
onmouseout	|鼠标离开触发
onfocus	|获得鼠标焦点触发
onblur	|失去鼠标焦点触发
onmousemove|	鼠标移动触发
onmouseup	|鼠标弹起触发
onmousedown	|鼠标按下触发

### 4.3.1 操作元素

**改变元素内容**

```js
//从起始位置到终止位置的内容，但它去除html标签，同时空格和换行也会去掉。
element.innerText

//起始位置到终止位置的全部内容，包括HTML标签，同时保留空格和换行
element.innerHTML
```

**改变元素属性**

```js
img.src = "xxx";

input.value = "xxx";
input.type = "xxx";
input.checked = "xxx";
input.selected = true / false;
input.disabled = true / false;
```

**改变样式属性**

```js
div.style.backgroundColor = 'pink';
div.style.width = '250px';
```

**获取属性值**

```js
element.属性;
```

**获取自定义的属性**

```js
element.getAttribute('属性');
```

**设置属性值**

```js
element.属性 = '值';
```

**设置自定义的属性**

```js
element.setAttribute('属性','值');
```

**移除属性**

```js
element.removeAttribute('属性');
```

### 4.3.2 节点操作

一般的，节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性。

元素节点：nodeType 为1
属性节点：nodeType 为2
文本节点：nodeType 为3(文本节点包括文字、空格、换行等)

#### 父子节点

**父级节点**

```js
node.parentNode
```

parentNode属性可以返回某节点的父结点，注意是最近的一个父结点
如果指定的节点没有父结点则返回nul
**子结点**

```js
parentNode.childNodes(标准)
```

parentNode.childNodes 返回包含指定节点的子节点的集合，该集合为即时更新的集合
返回值包含了所有的子结点，包括元素节点，文本节点等
如果只想要获得里面的元素节点，则需要专门处理。所以我们一般不提倡使用childNodes

```js
parentNode.children(非标准)
```

parentNode.children 是一个只读属性，返回所有的子元素节点
它只返回子元素节点，其余节点不返回
虽然 children 是一个非标准，但是得到了各个浏览器的支持，因此我们可以放心使用
**第一个子结点**

```js
parentNode.firstChild
```

firstChild 返回第一个子节点，找不到则返回null
同样，也是包含所有的节点
**最后一个子结点**

```js
parentNode.lastChild
```

lastChild 返回最后一个子节点，找不到则返回null
同样，也是包含所有的节点

**第一个子结点(兼容性)**

```js
parentNode.firstElementChild
```

firstElementChild 返回第一个子节点，找不到则返回null
有兼容性问题，IE9以上才支持
**最后一个子结点(兼容性)**

```js
parentNode.lastElementChild
```

lastElementChild 返回最后一个子节点，找不到则返回null
有兼容性问题，IE9以上才支持
**解决方案**

- 如果想要第一个子元素节点，可以使用 parentNode.chilren[0]

- 如果想要最后一个子元素节点，可以使用数组元素个数减1 就是最后一个元素的索引号


#### 兄弟节点

**下一个兄弟节点**

```js
node.nextSibling
```

nextSibling 返回当前元素的下一个兄弟元素节点，找不到则返回null
同样，也是包含所有的节点
**上一个兄弟节点**

```js
node.previousSibling
```

previousSibling 返回当前元素上一个兄弟元素节点，找不到则返回null

同样，也是包含所有的节点

**下一个兄弟节点(兼容性)**

```js
node.nextElementSibling
```

nextElementSibling 返回当前元素下一个兄弟元素节点，找不到则返回null
有兼容性问题，IE9才支持
**上一个兄弟节点(兼容性)**

```js
node.previousElementSibling
```

previousElementSibling 返回当前元素上一个兄弟元素节点，找不到则返回null
有兼容性问题，IE9才支持

#### 创建节点

```js
document.createElement('tagName');
```

document.createElement() 方法创建由 tagName 指定的HTML 元素
因为这些元素原先不存在，是根据我们的需求动态生成的，所以我们也称为动态创建元素节点

#### 添加节点

```js
node.appendChild(child)
```

node.appendChild() 方法将一个节点添加到指定父节点的子节点列表末尾。类似于 CSS 里面的 after 伪元素。

```js
node.insertBefore(child,指定元素)
```

node.insertBefore() 方法将一个节点添加到父节点的指定子节点前面。类似于 CSS 里面的 before 伪元素。

#### 删除节点

```js
node.removeChild(child)
```

node.removeChild()方法从 DOM 中删除一个子节点，返回删除的节点

#### 克隆节点

```js
node.cloneNode()
```

node.cloneNode()方法返回调用该方法的节点的一个副本。 也称为克隆节点/拷贝节点
如果括号参数为空或者为 false ，则是浅拷贝，即只克隆复制节点本身，不克隆里面的子节点
如果括号参数为 true ，则是深度拷贝，会复制节点本身以及里面所有的子节点

## 4.4、事件高级

### 4.4.1 绑定事件

给元素添加事件，称为注册事件或者绑定事件。

注册事件有两种方式：传统方式和方法监听注册方式

| 传统注册方式                                                 | 方法监听注册方式                                      |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| 利用 on 开头的事件 onclick                                   | w3c 标准推荐方式                                      |
| <button onclick = "alert("hi")"></button>                    | addEventListener() 它是一个方法                       |
| btn.onclick = function() {}                                  | IE9 之前的 IE 不支持此方法，可使用 attachEvent() 代替 |
| 特点：注册事件的唯一性                                       | 特点：同一个元素同一个事件可以注册多个监听器          |
| 同一个元素同一个事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数 | 按注册顺序依次执行                                    |

#### addEventListener事件监听方式

- eventTarget.addEventListener()方法将指定的监听器注册到 eventTarget（目标对象）上
- 当该对象触发指定的事件时，就会执行事件处理函数

```js
eventTarget.addEventListener(type,listener[,useCapture])
```

该方法接收三个参数：

- type:事件类型字符串，比如click,mouseover,注意这里不要带on
- listener：事件处理函数，事件发生时，会调用该监听函数
- useCapture：可选参数，是一个布尔值，默认是 false。

```js
<body>
    <button>传统注册事件</button>
    <button>方法监听注册事件</button>
    <button>ie9 attachEvent</button>

    <script>
        var btns = document.querySelectorAll('button');
        // 1. 传统方式注册事件
        btns[0].onclick = function() {
            alert('hi');
        }
        btns[0].onclick = function() {
                alert('hao a u');
            }
            // 2. 事件监听注册事件 addEventListener 
            // (1) 里面的事件类型是字符串 所以加引号 而且不带on
            // (2) 同一个元素 同一个事件可以添加多个侦听器（事件处理程序）
        btns[1].addEventListener('click', function() {
            alert(22);
        })
        btns[1].addEventListener('click', function() {
                alert(33);
            })
            // 3. attachEvent ie9以前的版本支持
        btns[2].attachEvent('onclick', function() {
            alert(11);
        })
    </script>

</body>
```

#### attachEvent事件监听方式(兼容)

eventTarget.attachEvent()方法将指定的监听器注册到 eventTarget（目标对象） 上
当该对象触发指定的事件时，指定的回调函数就会被执行

```js
eventTarget.attachEvent(eventNameWithOn,callback)
```

该方法接收两个参数：

- eventNameWithOn：事件类型字符串，比如 onclick 、onmouseover ，这里要带 on
- callback： 事件处理函数，当目标触发事件时回调函数被调用

ie9以前的版本支持

### 4.4.2 删除事件(解绑事件)

#### removeEventListener删除事件方式

```js
eventTarget.removeEventListener(type,listener[,useCapture]);
```

该方法接收三个参数：

- type:事件类型字符串，比如click,mouseover,注意这里不要带on
- listener：事件处理函数，事件发生时，会调用该监听函数
- useCapture：可选参数，是一个布尔值，默认是 false。

#### detachEvent删除事件方式(兼容)

```js
eventTarget.detachEvent(eventNameWithOn,callback);
```

该方法接收两个参数：

- eventNameWithOn：事件类型字符串，比如 onclick 、onmouseover ，这里要带 on
- callback： 事件处理函数，当目标触发事件时回调函数被调用

ie9以前的版本支持

### 4.4.3 事件对象的常见属性和方法

方法|	描述
-|-
e.target	|返回触发事件的对象 标准
e.srcElement|	返回触发事件的对象 非标准 ie6-8使用
e.type	|返回事件的类型 比如click mouseover 不带on
e.cancelBubble|	该属性阻止冒泡，非标准，ie6-8使用
e.returnValue	|该属性阻止默认行为 非标准，ie6-8使用
e.preventDefault()|	该方法阻止默认行为 标准 比如不让链接跳转
e.stopPropagation()|	阻止冒泡 标准


e.target 和 this 的区别：

this 是事件绑定的元素， 这个函数的调用者（绑定这个事件的元素）
e.target 是事件触发的元素。

# 五、BOM

## 5.1 BOM概述

- BOM = Browser Object Model 浏览器对象模型

- 它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是 window

- BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性

- BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA, DOM 的标准化组织是 W3C, BOM最初是Netscape 浏览器标准的一部分

DOM|	BOM
-|-
文档对象模型|	浏览器对象模型
DOM 就是把 文档 当作一个对象来看待	|BOM把浏览器当作一个对象来看待
DOM 的顶级对象是 document	|BOM 的顶级对象是 window
DOM 主要学习的是操作页面元素|	BOM 学习的是浏览器窗口交互的一些对象
DOM 是 W3C 标准规范	|BOM 是浏览器厂商在各自浏览器上定义的，兼容性较差

1.1、BOM的构成

- 
  BOM 比 DOM 更大。它包含 DOM。

- window 对象是浏览器的顶级对象，它具有双重角色

- 它是 JS 访问浏览器窗口的一个接口

- 它是一个全局对象。定义在全局作用域中的变量、函数都会变成 window 对象的属性和方法

- 在调用的时候可以省略 window，前面学习的对话框都属于 window 对象方法，如 alert()、prompt()等。


```js
// 定义在全局作用域中的变量会变成window对象的属性
var num = 10;
console.log(window.num);
// 10

// 定义在全局作用域中的函数会变成window对象的方法
function fn() {
    console.log(11);
}
console.fn();
// 11

var name = 10;  //不要用这个name变量,window下有一个特殊属性window.name
console.log(window.num);
```

## 5.2 window 对象的常见事件

### 5.2.1 窗口加载事件

window.onload是窗口（页面）加载事件，当文档内容完全加载完成会触发该事件（包括图像，脚本文件，CSS文件等），就调用的处理函数。

```js
window.onload = function(){
    
};

// 或者
window.addEventListener("load",function(){});
```

- 
  有了window.onload就可以把JS代码写到页面元素的上方

- 因为onload是等页面内容全部加载完毕，再去执行处理函数

- window.onload 传统注册事件方式，只能写一次

- 如果有多个，会以最后一个window.onload为准

- 如果使用addEventListener 则没有限制


```js
document.addEventListener('DOMContentLoaded',function(){})
```


接收两个参数：

- DOMCountentLoaded事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等

- 如果页面的图片很多的话, 从用户访问到onload触发可能需要较长的时间，交互效果就不能实现，必然影响用户的体验，此时用 DOMContentLoaded事件比较合适。


2.1.1、区别
load等页面内容全部加载完毕，包括页面dom元素，图片，flash，css等
DOMContentLoaded 是DOM加载完毕，不包含图片 flash css 等就可以执行，加载速度比load更快一些

### 5.2.2 调整窗口大小事件

window.onresize 是调整窗口大小加载事件，当触发时就调用的处理函数

```js
window.onresize = function() {}

// 或者
window.addEventListener('resize',function(){});
```

只要窗口大小发生像素变化，就会触发这个事件
我们经常利用这个事件完成响应式布局。window.innerWidth 当前屏幕的宽度

### 5.2.3 定时器

#### setTimeout()定时器

setTimeout()方法用于设置一个定时器，该定时器在定时器到期后执行调用函数。

```js
window.setTimeout(调用函数,[延迟的毫秒数]);
```

- window可以省略

- 这个调用函数可以直接写函数，或者写函数名，或者采取字符串 ‘函数名()’ （不推荐）
- 延迟的毫秒数省略默认是0，如果写，必须是毫秒
- 因为定时器可能有很多，所以我们经常给定时器赋值一个标识符
- setTimeout() 这个调用函数我们也称为回调函数 callback
- 普通函数是按照代码顺序直接调用，而这个函数，需要等待事件，事件到了才会去调用这个函数，因此称为回调函数。

#### clearTimeout()停止定时器

clearTimeout()方法取消了先前通过调用 setTimeout()建立的定时器

```js
window.clearTimeout(timeoutID)
```

- window可以省略
- 里面的参数就是定时器的标识符

#### setInterval()定时器

setInterval()方法重复调用一个函数，每隔这个时间，就去调用一次回调函数

```js
window.setInterval(回调函数,[间隔的毫秒数]);
```

- 
  window可以省略
- 这个回调函数:可以直接写函数或者写函数名或者采取字符 ‘函数名()’
- 第一次执行也是间隔毫秒数之后执行，之后每隔毫秒数就执行一次

#### clearInterval()停止定时器

clearInterval ( ) 方法取消了先前通过调用 setInterval() 建立的定时器

```
clearInterval(timer);
```

- window可以省略
- 里面的参数就是定时器的标识符



### 5.3 location对象方法

location对象方法|	返回值
-|-
location.assign()|	跟href一样，可以跳转页面（也称为重定向页面）
location.replace()|	替换当前页面，因为不记录历史，所以不能后退页面
location.reload()	|重新加载页面，相当于刷新按钮或者 f5 ，如果参数为true 强制刷新 ctrl+f5

### 5.4 navigator对象

navigator 对象包含有关浏览器的信息，它有很多属性
我们常用的是userAgent,该属性可以返回由客户机发送服务器的user-agent头部的值
下面前端代码可以判断用户是用哪个终端打开页面的，如果是用 PC 打开的，我们就跳转到 PC 端的页面，如果是用手机打开的，就跳转到手机端页面

```js
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
    window.location.href = "";     //手机
 } else {
    window.location.href = "";     //电脑
 }
```

### 5.5 history对象

window 对象给我们提供了一个 history 对象，与浏览器历史记录进行交互
该对象包含用户（在浏览器窗口中）访问过的 URL。
history对象方法|	作用
-|-
back()|	可以后退功能
forward()	|前进功能
go(参数)	|前进后退功能，参数如果是 1 前进1个页面 如果是 -1 后退1个页面
