# 前端性能优化

> lecture-1 

## JavaScript 代码级优化

### 1. if 多条件判断

```javascript
const day = '星期二';
if (day === '星期二' || day === '星期三' || day === '星期四') {
    console.log(day);
}

// 优化
if (['星期二', '星期三', '星期四'].includes(day)) {
    console.log(day);
}
```

### 2. if...else

当有不包含复杂逻辑的使用 `if...else` 条件时，可以三元运算符来简化。

```javascript
// 优化前
let test;
if (x > 100) {
    test = true;
} else {
    test = false;
}
// 优化后
let test = x > 10 ? true : false;
// 或简写如下
let test = x > 10;
```

当有嵌套条件时，可以采用下面的方式：

```javascript
const x = 300;
const test2 =
    x > 100 ? 'greater 100' : x < 50 ? 'less 50' : 'between 50 and 100';
console.log(test2); // 'greater than 100'
```

### 3. 变量声明

当要声明两个具有相同值或相同类型的变量时，可以使用简写方式。

```javascript
// 优化前
let number1;
let number2 = 1;

// 优化后
let number1, number2 = 1;
```

### 4. null、undefined 和空值检查

当创建新的变量或者调试 API 提供的数据时，需要检查数据是否为 null、undefined 和空值。Javascript 新语法空值合并运算符 `??` 有一个很好的快捷方式来实现这个功能。

```javascript
// 优化前
if (test1 !== null || test1 !== undefined || test1 !== '') {
    const test2 = test1;
}
// 优化后
const test2 = test1 || '';
// 新语法
const foo = null ?? 'default string';
console.log(foo);
```

### 5. null 或者 undefined 检查并设置默认值

```javascript
const test1 = null;
const test2 = test1 || '';

const test3 = undefined;
const test4 = test3 || '';

// 使用空值合并运算符
const test = null ?? 'default';
```

### 6. 声明多个变量并赋值

当处理多个变量并进行赋值的时候，使用“解构”可以使得代码更加友好。

```javascript
// 优化前
let test1, test2, test3;
test1 = 1;
test2 = 2;
test3 = 3;
// 优化后
let [test1, test2, test3] = [1, 2, 3];
```

### 7. 赋值运算符

在项目开发中，要处理大量的算术运算符，很多方式可以进行简写。

```javascript
// 优化前
test1 = test1 + 1;
test2 = test2 - 1;
test3 = test3 * 10;
// 优化后
test1++;
test2--;
test3 *= 10;
```

### 8. if 存在的简写

```javascript
// 优化前
if (test1 === true) or if (test1 !== '') or if (test1 !== null){

}
// 优化后
if (test1){

}
```

### 9. if 执行函数

如果判断一个条件满足就执行某个函数，则可以利用和运算符`&&`。

```javascript
// 优化前
if (test1) {
    callMethod();
}
// 优化后
test1 && callMethod();
```

### 10.for循环

```javascript
// 优化前
for (var i = 0; i < testData.length; i++)

// 优化后
for (let i in testData) or  for (let i of testData)
```

如果是数组，可以使用`foreach`。

```javascript
[11, 24, 32].forEach((element, index, array) => {
    console.log('test[' + index + '] = ' + element);
});
```

### 11.条件返回

```javascript
// 优化前
let test;
function callMe(val) {
    console.log(val);
}

function checkReturn() {
    if (!(test === undefined)) {
        return test;
    } else {
        return callMe('test');
    }
}
var data = checkReturn();
console.log(data);
// 优化后
function checkReturn() {
    return test || callMe('test');
}
```

### 12. 箭头函数

在项目中还是建议多用箭头函数，简洁而且可以避免 `this` 的问题。

```javascript
// 优化前
function add(a, b) {
    return a + b;
}
// 优化后
const add = (a, b) => a + b;

function callMe(name) {
    console.log('Hello', name);
}
// 箭头函数的简写
callMe = name => console.log('Hello', name);
```

### 13.条件调用

```javascript
// 优化前
function test1(title) {
    console.log(title);
}
function test2(title) {
    console.log('我的名称是：' + title);
}
let test3 = 1;
let title = 'DevPoint';
if (test3 === 1) {
    test1(title);
} else {
    test2(title);
}

// 优化后
(test3 === 1 ? test1 : test2)(title);
```

### 14.switch

优化的方式可以将条件保存在键值对象中，根据条件调用。

```javascript
// 优化前
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
}

// 优化后
var data = {
    1: test1,
    2: test2,
    3: test,
};

data[something] && data[something]();
```

### 15. 隐式返回

使用箭头函数，可以直接返回值，而不必使用 `return` 语句。

```javascript
// 优化前
function calculate(diameter) {
    return Math.PI * diameter;
}
// 优化后
const calculate = diameter => Math.PI * diameter;
```

### 16.十进制数值指数优化

```css
// 优化前
for (let i = 0; i < 10000; i++) {}

// 优化后
for (let i = 0; i < 1e4; i++) {}
```

### 17.参数默认值

```javascript
// 优化前
function add(test1, test2) {
    if (test1 === undefined) {
        test1 = 1;
    }
    if (test2 === undefined) {
        test2 = 2;
    }
    return test1 + test2;
}
// 优化后
add = (test1 = 1, test2 = 2) => test1 + test2;
add();
```

### 18. `...` 运算符

展开运算符 `...` 是一个 es6 / es2015 特性，它提供了一种非常方便的方式来执行浅拷贝，这与  `Object.assign()` 的功能相同。

```javascript
// 优化前
const data = [1, 2, 3];
const test = [4, 5, 6].concat(data);
// 优化后
const data = [1, 2, 3];
const test = [4, 5, 6, ...data];


// 数组对象浅拷贝
const map1 = {
        title: 'DevPoint',
    },
    map2 = {
        info: 'Develop',
    };
const newMap = { ...map1, ...map2 };
console.log(newMap); // { title: 'DevPoint', info: 'Develop' }
const test1 = [1, 2, 3];
const test2 = [...test1];
```

### 19.模板字符串

对于拼接字符串很实用，可以更加友好。

```javascript
// 优化前
const welcome = 'Hi ' + test1 + ' ' + test2 + '.';
// 优化后
const welcome = `Hi ${test1} ${test2}`;
```

多行字符串的拼接。

```javascript
// 优化前
const data =
    'abc abc abc abc abc abc\n\t' + 'test test,test test test test\n\t';
// 优化后
const data = `abc abc abc abc abc abc
         test test,test test test test`;
```

### 20.对象属性赋值

```javascript
let test1 = 'a';
let test2 = 'b';
// 优化前
let obj = { test1: test1, test2: test2 };
// 优化后
let obj = { test1, test2 };
```

### 21.字符串转为数字

```javascript
// 优化前
let test1 = parseInt('123');
let test2 = parseFloat('12.3');
// 优化后
let test1 = +'123';
let test2 = +'12.3';
```

### 22. 解构变量

```javascript
// 优化前
const test1 = this.data.test1;
const test2 = this.data.test2;
const test2 = this.data.test3;
// 优化后
const { test1, test2, test3 } = this.data;
```

### 23.Array.find()

当确实有一个对象数组并且我们想要根据对象属性查找特定对象时，`find`方法确实很有用。

```javascript
const data = [
    {
        type: 'test1',
        name: 'abc',
    },
    {
        type: 'test2',
        name: 'cde',
    },
    {
        type: 'test1',
        name: 'fgh',
    },
];
function findtest1(name) {
    for (let i = 0; i < data.length; ++i) {
        if (data[i].type === 'test1' && data[i].name === name) {
            return data[i];
        }
    }
}
// 优化后
filteredData = data.find(
    (data) => data.type === 'test1' && data.name === 'fgh'
);
console.log(filteredData); // { type: 'test1', name: 'fgh' }
```

### 24.条件执行

如果有代码来检查类型，并且基于类型需要调用不同的方法，可以选择使用多个 `else if` 或进行切换，可以使用对象键值优化。

```javascript
// 优化前
if (type === 'test1') {
    test1();
} else if (type === 'test2') {
    test2();
} else if (type === 'test3') {
    test3();
} else if (type === 'test4') {
    test4();
} else {
    throw new Error('Invalid value ' + type);
}
// 优化后
var types = {
    test1: test1,
    test2: test2,
    test3: test3,
    test4: test4,
};

var func = types[type];
!func && throw new Error('Invalid value ' + type);
func();
```

