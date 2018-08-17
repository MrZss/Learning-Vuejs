## 1.let 和 const 命令
在es5时，只有两种变量声明，var 和function。在es6中新增了四种let和const，以及另外两种声明import和class。 
## 新添方法
### 2.统一输出格式 padStart()，padEnd()
ES6两个字符串追加方法String.prototype.padStart和String.prototype.padEnd

```
 var a='abc'
 var b='abcd'
 var c='abcd'
 console.log(a.padStart('10',0)); 
 console.log(b.padStart('10',0));
 console.log(c.padStart('10',0));
 //0000000abc
 //000000abcd
 //00000abcde

```
```
var a='abc'
 var b='abcd'
 var c='abcde'
 console.log(a.padEnd(10,'-------')); 
 console.log(b.padEnd(10,'-------'));
 console.log(c.padEnd(10,'-------'));
 //abc-------
 //abcd------
 //abcde-----

```
```
var obj={
  name:'wangcai',
  car:'BMW',
  wife:'fatgirl'
}
for(var item in obj){
  console.log(item.padEnd(10,'-')+'value:'+obj[item].padStart(10,'**'));
}
//name------value:***wangcai
//car-------value:*******BMW
//wife------value:***fatgirl
。
```

### 3.includes()值存在判断
在es6之前使用indexof也可以进行值存在判断。includes与indexof既可以进行字符串的判断，也可以进行数组值的判断，
但是indexof在对NaN进行判断时会出现不准确。
```
var text = [1,NaN]
console.log(text.indexOf(NaN));//-1
```
复制代码另外，indexof的返回结果为-1||0，includes为true||false.

#### 4.repeat()字符串重复
a.repeat(n) 返回新字符串 重复n次 字符串a
```
var a = 'la'
console.log(a.repeat(3)); // lalala
```
n会自动取整 n<=-1||n==infinaty（无穷大）都会报错。

## 模板字符串 ( `   `)
```
var age=22;
var name='lang'
var say=()=>{
  return 'hello'
}
var str=`myname is${name} my age is ${age} and i can say ${say()}`
console.log(str);  //myname islang my age is 22 and i can say hello
```
在模板字符串的 ${} 中可以写任意表达式，但是同样的，对 if / else 判断、循环语句无法处理。

但在很多时候我们需要去使用if或者循环。我们可以先在外部使用逻辑处理语句，然后生成一个我们想要的模板，在用``进行转义

```
var age=22;
var name='lang'
var name2='Lang'
var str=''
var say=()=>{
  return 'hello'
}
if(age>=18){str=name2}
var str=`myname is${str} my age is ${age} and i can say ${say}`
console.log(str);  //myname isLang my age is 22 and i can say hello
```

当然，我们也可以使用数组，将各个模板片段存入数组之中，最后通过Array.join('')将其拼接为最终的模板。

## 函数扩展

### 函数默认值
```
function say(x,y=5){
    console.log(x,y);  //1,5
    y=10;
    console.log(x,y);  //1,10
  }
  say(1)

```
需要注意以下两点

.使用参数默认值时，函数不能有同名参数。

.不可使用let，const重复声明
```
// 不报错
function foo(x, x, y) {
  // ...
}

// 报错
function foo(x, x, y = 1) {
  // ...
}
// SyntaxError: Duplicate parameter name not allowed in this context
function say(x,y=5){
  let y=10;
  console.log(x,y);  //SyntaxError: Identifier 'y' has already been declared
 }
 say(1)

```

### rest 参数 扩展运算符
```
function say(...res) {
   for (var item of res) {
     console.log(item);
   }
 }
 say(1, 2, 3)  
 //1
 //2
 //3

```

### 箭头函数
```
 var f=(x,y)=>{ return x+y}
 console.log(f(1,2));  //3

```
假如（x，y）只有一个参数，我们可以省略（），同样返回语句中，若只有一条语句，也可以省略。
```
 var f=x=>x+10
 console.log(f(1));  //11
```

## 解构赋值
```
var x = [1, 2, 3, 4, 5];
var [y, z] = x;
console.log(y); // 1
console.log(z); // 2
```
### 数组的结构
变量申明并赋值时的结构
```
var foo = ["one", "two", "three"];

var [one, two, three] = foo;
console.log(one); // "one"
console.log(two); // "two"
console.log(three); // "three"
Link to section变量先声明后赋值时的解构
```
通过结构分离变量的声明，可以为一个变量赋值
```
var a, b;

[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
Link to section默认值

```
## **交换两个变量的值
通过结构赋值 交换两个变量值  

没有解构赋值的情况下，交换两个变量需要一个临时变量（或者用低级语言中的XOR-swap技巧）
```
var a = 1;
var b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1
Link to section解析一个从函数返回的数组

```
```
function f() {
  return [1, 2];
}

var a, b; 
[a, b] = f(); 
console.log(a); // 1
console.log(b); // 2

```
### 对象的解构
```
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true
```
## 对象结构 获取对象中对应值

可以从一个对象中提取变量并赋值给和对象属性名不同的新的变量名。
```
var o = {p: 42, q: true};
var {p: foo, q: bar} = o;
 
console.log(foo); // 42 
console.log(bar); // true

```
### 默认值
默认值

变量可以先赋予默认值。当要提取的对象没有对应的属性，变量就被赋予默认值。
```
var {a = 10, b = 5} = {a: 3};

console.log(a); // 3
console.log(b); // 5

```
混合解构（嵌套对象和数组） 解构嵌套对象和数组
```
var metadata = {
    title: "Scratchpad",
    translations: [
       {
        locale: "de",
        localization_tags: [ ],
        last_edit: "2014-04-14T08:43:37",
        url: "/de/docs/Tools/Scratchpad",
        title: "JavaScript-Umgebung"
       }
    ],
    url: "/en-US/docs/Tools/Scratchpad"
};

var { title: englishTitle, translations: [{ title: localeTitle }] } = metadata;

console.log(englishTitle); // "Scratchpad"
console.log(localeTitle);  // "JavaScript-Umgebung"

```