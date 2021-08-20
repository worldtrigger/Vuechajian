# Vue 中使用数学计算

- 计算的话推荐用 Math.js

## 安装包

> Math.js 是专门为 Js 和 Node 提供的一个广泛的数学库.他具有灵活的表达式解析器,支持符号计算,配有大量内置函数和常量,并提供集成解决方案来处理不同的数据类型

```javascript
cnpm i mathjs
```

## 使用

```javascript
import * as math from 'mathjs';
```

> 具体使用

- 开方 math.sqrt(4)

- 加法 math.add(1,2)

- 减法 math.subtract()

- 除法 math.divide()

- 乘法 math.multiply()

### 加法

```javascript
console.log(math.add(0.1, 0.2)); //0.30000000000000004
console.log(math.format(math.add(math.bignumber(0.1), math.bignumber(0.2)))); //'0.3'
```

### 表达式也可以

```javascript
math.eval('sqrt(4) + 2');
```

# 数据格式化

- 千分位

- 金额

- 保留几位小数

- 舍去舍入

## 安装

```javascript
npm i accounting
```

## 使用

```javascript
// Default usage:
accounting.formatMoney(12345678); // $12,345,678.00

// European formatting (custom symbol and separators), can also use options object as second parameter:
accounting.formatMoney(4999.99, '€', 2, '.', ','); // €4.999,99

// Negative values can be formatted nicely:
accounting.formatMoney(-500000, '£ ', 0); // £ -500,000

// Simple `format` string allows control of symbol position (%v = value, %s = symbol):
accounting.formatMoney(5318008, { symbol: 'GBP', format: '%v %s' }); // 5,318,008.00 GBP
```

### formatNumber

- 第一个参数是要划分的结果

- 第二个参数是按照多少位划分

- 第三个参数是插入的符号

```javascript
accounting.formatNumber(5318008); // 5,318,008
accounting.formatNumber(9876543.21, 3, ' '); // 9 876 543.210
```

### toFixed

```javascript
let num1 = 0.615;

num1.toFixed(2); //结果0.61
accounting.toFixed(0.615, 2); //0.62
```

### unformat

```javascript
accounting.unformat('£ 12,345,678.90 GBP'); // 12345678.9
```
