## 常用运算符

### 空值合并运算符 '??'

写法：

```
result = a ?? b;
```

等同于如下

```
result = (a !== null && a !== undefined) ? a : b;
```

可以理解为：如果第一个参数不是 `null/undefined`，则 `??` 返回第一个参数。否则，返回第二个参数

举例：在这里，如果 `user` 的值不为 `null/undefined` 则显示 `user`，否则显示 `匿名`

```javascript
let user;
alert(user ?? "匿名"); // 匿名（user 未定义）
```

在下面这个例子中，我们将一个名字赋值给了 `user`：

```javascript
let user = "John";
alert(user ?? "匿名"); // John（user 已定义）
```

### 或运算符’||‘

与？？运算符不同的是，`||` 无法区分 `false`、`0`、空字符串 `""` 和 `null/undefined`。它们都一样 —— 假值（falsy values）。如果其中任何一个是 `||` 的第一个参数，那么我们将得到第二个参数作为结果。

例如，考虑下面这种情况：

```javascript
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

### 与运算符&&

写法：

```javascript
result = a && b;
```

在传统的编程中，当两个操作数都是真值时，与运算返回 `true`，否则返回 `false`：

```javascript
alert( true && true );   // true
alert( false && true );  // false
alert( true && false );  // false
alert( false && false ); // false
```

**与运算寻找第一个假值**

给出多个参加与运算的值：

```javascript
result = value1 && value2 && value3;
```

与运算 `&&` 做了如下的事：

- 从左到右依次计算操作数。
- 在处理每一个操作数时，都将其转化为布尔值。如果结果是 `false`，就停止计算，并返回这个操作数的初始值。
- 如果所有的操作数都被计算过（例如都是真值），则返回最后一个操作数。

**换句话说，与运算返回第一个假值，如果没有假值就返回最后一个值。**

### 非运算符！

写法：

```javascript
result = !value;
```

逻辑非运算符接受一个参数，并按如下运作：

1. 将操作数转化为布尔类型：`true/false`。
2. 返回相反的值。

例如：

```javascript
alert( !true ); // false
alert( !0 ); // true
```

两个非运算 `!!` 有时候用来将某个值转化为布尔类型：

```javascript
alert( !!"non-empty string" ); // true
alert( !!null ); // false
```

也就是，第一个非运算将该值转化为布尔类型并取反，第二个非运算再次取反。最后我们就得到了一个任意值到布尔值的转化。

## Object常用属性和方法

### 常用属性

**数据属性**

所谓数据属性就是对象的每一个属性名，数据属性都有四个特征，如下：

```vbnet
configurable:表示能否对该属性 进行配置，比如使用delete删除，能否修改属性特性等。默认为true
enumerable:表示是否能被枚举，比如是否可以用for-in循环获取属性,默认为true。
writable:表示是否可写，是否能修改属性的值。默认为true。
value:包含属性的数据值，默认为undefined。
```

可以使用`object.getOwnPropertyDescriptor(person,name)`来获取指定的数据属性的描述。

**访问器属性Object.defineProperty**

访问器属性不包括数据值，它包括一对getter和setter函数，通过定义访问器属性，可以实现设置一个属性的值会导致其他属性发生变化。

```js
let person = {
    name:"jack"
}
let temp = null;
Object.defineProperty(person,'name',{
    get:function(){
        console.log('被读取')
        return temp;
    },
    set:function(val){
        console.log('被设置')
        temp = val
    }
})
console.log(person.name) // null
person.name = 'lucy';
console.log(person.name) // lucy
```

### 原型方法

**Object.prototype.hasOwnProperty(‘属性名’)**

​	此方法返回一个布尔值，表示这个对象中是否有这个属性。不会判断原型链上继承到的属性，类似Object.key()，不同于for in 。

**Object.prototype.isPrototypeOf()**

此方法主要是判断`prototypeObj.isPrototypeOf(object)`,也就是判断对象prototypeObj是否在**对象object的原型链上**。也可以理解为对象object是否继承自对象prototypeObj。

**Object.prototype.toString()**方法返回一个表示该对象的字符串。可以用来检测对象类型。

### 对象方法

**Object.assign(obj)**

将一个对象的所有可枚举属性复制到一个新的对象中，是**浅拷贝**。

**Object.entries(),key(),value()**

返回给定对象的可遍历属性的键值对

这里有个重点需要注意一下就是`Object.getOwnPropertyNames()`，`Object.keys()`,`for-in`都是可以获取到对象的属性的，但是他们是有区别的这里简单总结一下。

`for in`:使用for..in循环时，返回的是`所有能够通过对象访问的、可枚举的属性`，既包括存在于实例中的属性，也包括存在于原型中的属性。 

`Object.keys()`:用于获取对象`自身所有的可枚举的属性值`，但`不包括原型中的属性`，然后返回一个由属性名组成的数组 

`Object.getOwnPropertyNames()`:方法返回对象的`所有自身属性的属性名`（`包括不可枚举的属性`）组成的数组，但不会获取原型链上的属性 差异主要在属性`是否可枚举`，`是来自原型`，`还是实例`。

**object.getPrototypeOf(obj)**

返回指定对象的原型

`Object.setPrototypeOf()`方法设置一个指定的对象的原型为另一个对象或null。

```
//构造函数
function Person(name) {
    this.name = name;
}
var p = new Person("zhenglijing");

//等同于将构造函数的原型对象赋给实例对象p的属性__proto__
p.__proto__ = Object.setPrototypeOf({},Person.prototype);
Person.call(p,"zhenglijing");
```

### 小技巧

1、计算数组中的最大值，可以用Math.max(...array)；计算数组中最小的值Math.min(…array)
2、可以使用Object.entries(array)将对象转换成该对象所有 [key, value] 键值对的数组，具体结构变为:
[[key1, value1],[key1, value1],[key1, value1]]
3、数组去重 const arr =[...new Set(array)]
4、过滤数据 const arr = array.filter((item:any)=>item.id)
5、解构数组的完整语法：
let [item1 = default, item2, ...rest] = array
数组的第一个元素被赋值给 item1，第二个元素被赋值给 item2，剩下的所有元素被复制到另一个数组 rest。
6、合并变量声明 
当我们声明多个同类型的变量时，可以像下面这样简写。
 let test1, test2 = 1;
合并变量赋值 
当我们处理多个变量并将不同的值分配给不同的变量时。
短 let [test1, test2, test3] = [1, 2, 3];
7、JSON.stringify 将对象转换为 JSON。
JSON.parse 将 JSON 转换回对象。
8、阻止事件冒泡、默认行为
event.stopPropagation(); 
事件处理过程中，阻止了事件冒泡，但不会阻击默认行为（执行超链接的跳转） 
return false; 
事件处理过程中，阻止了事件冒泡，也阻止了默认行为（不执行超链接的跳转） 
还有一种与冒泡有关的： 
event.preventDefault(); 
事件处理过程中，不阻击事件冒泡，但阻击默认行为（它只执行所有弹框，却没有执行超链接跳转）

9、if多条件判断

```javascript
// 冗余
if (x === 'abc' || x === 'def' || x === 'ghi' || x ==='jkl') {}

// 简洁
if (['abc', 'def', 'ghi', 'jkl'].includes(x)) {}
```

10. switch条件

```javascript
// 冗余
switch (data) {
  case 1:
    test1();
  break;

  case 2:
    test2();
  break;

  case 3:
    test();
  break;
  // so on...
}

// 简洁
var data = {
  1: test1,
  2: test2,
  3: test
};

data[anything] && data[anything]();
```

数组降维度

**二维数组**

```jsx
let arr = [ [1], [2], [3] ];
arr = Array.prototype.concat.apply([], arr);
console.log(arr);// [1, 2, 3]

let array = [ [1], [2], [3] ];
array = array.flat(2);
console.log(array); // [1, 2, 3]
复制代码
```

**多维数组**

```jsx
let arrMore = [1, 2, [3], [[4]]];
arrMore = arrMore.flat(Infinity);
console.log(arrMore);
```

222