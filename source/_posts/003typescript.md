---
title: TypeScript 之旅
tag: [TypeScript, JavaScript, 前端]
date: 2024-10-21
categories: [语言学习]
---

# TypeScript 语言学习

## What is TypeScirpt

TypeScript 是由微软进行开发和维护的一种开源的编程语言。 TypeScript 是 JavaScript 的严格语法超集，提供了可选的静态类型检查。 **_--维基百科_**

TypeScript 顾名思义，即 Typed JavaScript , 是 JavaScript 的一种超集。TypeScript 的目标是成为 JavaScript 程序的静态类型检查器 - 换句话说，它是一个在代码运行之前运行的工具（静态），并确保程序的类型正确（类型检查）。

### TypeScript 的特性

- **始于 JS ，终于 JS**

  TypeScript 代码需要通过编译器（tsc）转换为纯 JavaScript 才能运行。这一过程允许在编写代码时进行类型检查，提前发现错误。

- **静态类型检查**

  允许在代码运行前发现错误并提示修改

- **And so on**

  更多其他内容将在文档中详细介绍

## TypeScript 基础

### 静态类型检查

很多人在执行代码的时候都不想看到任何错误，毕竟这都是 bug，而 ts 在这里就扮演了一个静态类型检查器的角色，它会在代码执行之前抛出这个错误以提醒我们进行修改：

```TypeScript
const msg = "Helloworld";

msg(); //Error , Type 'String' has no call signatures.
```

如上，由于一个字符串类型常量并不具有调用自身的方法，ts 会告诉我们这是一个错误的代码执行。同时，ts 还有会捕捉到各种 js 认为不会引起错误的“有效”代码，如示：

```JavaScript
const user = {
  name = "Tony" ,
  userID = "01020304" ,
}
user.location // return undefined
```

```TypeScript
const user = {
  name = "Tony" ,
  userID = "01020304" ,
}
user.location // Error , Property 'location' does not exist on type '{ name: string; age: number; }'.
```

同样的 demo，在 JavaScript 中由于 ECMAScript 规范明确规定不会报错，而 TypeScript 则会指出由于 location 没有定义，代码中存在一个错误。类似的，TypeScript 还会捕捉到其他许多"合法"的错误，如函数调用时的函数名拼写错误，逻辑错误等等。

### TypeScript 编译器 —— tsc

TypeScript 的类型检查器由其编译器 tsc 来完成

首先，通过 npm 安装

```bash
$ npm install -g typescript
```

然后，编写运行我们的第一个 ts 应用 **hello.ts**

```ts
console.log("Helloworld");
```

```bash
tsc hello.ts
```

此时会发现当前目录下多了一个同名的 .js 文件，这是 tsc 编译转换 hello.js 后输出得到的纯 js 文件。在一般情况下，即使 tsc 编译报错，其对应的 js 文件仍然会发生改动，此时可以开启 noEmitOnError 编译选项，这样当编译器编译 ts 文件报错时，其对应的 js 文件便不会发生改动。

### 降级

TypeScript 可以将高版本的代码重写为低版本的代码，以兼容更古老的浏览器。但是一般情况下可以通过使用 --target es2015 参数进行编译，因为大部分浏览器已经支持 ES2015 了。

## TypeScript 常见类型

### 基本类型 : string , number , boolean

- **string：** 表示字符串值 , 如"Hello World"
- **number：** 表示数字，js 中没有类似其他语言中的 float，int 等类型，一切数字表示均为 number
- **boolean：** 表示布尔值，即只有 true | false 两种取值

### 数组

**number[]** 可用于指定数组的类型，该语法同样适用于其他类型(eg: **string[]**)。同时也可以通过 Array\<number\>指定数组类型。

### 特殊类型 any

当你不希望某个特定值而出错时，可以采用 any 的显式注释，此时这个特定值可以访问他的任何属性，ts 默认这一特定值不会出错

#### noImplicitAny

当未明确指定类型，且 ts 无法从上下文推断一个值的类型时，ts 会隐式默认这个值的类型为 any，这时可以通过 **noImplicitAny** 来标志这个隐式的 any 为错误

### 函数

函数时 TypeScript 传递数据的一种方式，ts 允许对传入值与输出值做类型注解如示：

```ts
function greet(name: string) {
  console.log("hello " + name + " !");
}
function add(num1: number, num2: number): number {
  return num1 + num2;
}
```

通常情况下不需要对返回值做类型注解，因为 ts 会根据 return 语句和上下文自动推断返回值类型

#### 匿名函数

对于匿名函数而言，函数出现在 TypeScript 可以断定其类型的位置时，参数将自动被赋予类型，如以下代码块，由于 foreach 的存在，ts 将自动推断 s 所应该具有的类型为 string

```ts
const names = ["Alice", "Bob", "Eve"];

names.forEach(function (s) {
  console.log(s.toUppercase());
  //Error , Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});
```

### 对象类型

这是除了基本类型之外最常见的一种类型，这是指具有任意属性的 javascript 值，因此，要定义一个对象类型，我们只需要列出其的属性及类型。同时对象还可以执行其部分或全部属性是可选的，只需要在属性名称后添加 **？** 即可实现此操作：

```ts
function printName(obj: { first: string; last?: string }) {
  // ...
}
// Both OK
printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });
```

### 联合类型

TypeScript 的类型系统允许你用任何运算符来从现有类型构造新类型，其中联合类型是由两个或多种类型组合而成的类型，其表示的值可以是其中任意一种类型

### 类型别名

顾名思义，类型别名即为类型的一种名称，其语法为

```ts
type Point = {
  x: number;
  y: number;
};
type userID = number | string;
```

如上所示，类型别名不仅可以用来表示对象类型，还可以用来表示联合类型，但是别名只是别名，如上代码中，使用 **userID** 为类型定义的值完全可以直接适用于与参数类型要求为 **number | string** 的函数方法中

### 接口（Interfaces）

接口是声明对象类型的另一种方式，大多数情况下接口所能实现的功能都能在类型别名中实现，他们具体有以下几点区别（以下内容直接摘录于 TypeScript 官方文档）：

- 在 TypeScript 4.2 版之前，类型别名可能会出现在错误消息中，有时会代替等效的匿名类型（这可能是也可能不是所希望的）。接口将始终在错误消息中命名。
- 类型别名可能不参与声明合并，但接口可以。
- 接口只能用于声明对象的形状，而不能重命名原语。
- 接口名称将始终以其原始形式出现在错误消息中，但前提是按名称使用它们。
- 对于编译器来说，使用接口通常比使用类型别名和交集 extends 更高效

### 类型断言

在某些时候，当你认为你比编译器本身更了解这一行代码时，可以使用类型断言的语法，此时 typescript 会认为你已经在这一行代码中做好了检查

```ts
const sth: any = "hello world";
const strlength_a = (sth as string).length;
const strlength_b = (<string>sth).length;
```

如上所示，类型断言分别有 **<>** 与 **as** 两种语法，但是当在 tsx 文件中，由于语法冲突，不建议使用 **<>** 实现类型断言的目的。同时，为了项目本身的安全性，也不建议类型断言语法本身的使用。

类型断言只可存在于向更高级或更低级转化，不可实现同级转换，如果一定有相关需求，可以借助特殊类型 any 实现相关操作如示：

```ts
const data: number = 43;
const str1 = data as string; //Error
const str2 = data as any as string; // True , but not recommend
```

### Null & Undefined

在 JavaScript 中有两个类型代表着空和未被定义：Null & Undefined
而在 TypeScript 中，这两种类型的代码表现取决于你是否启动了一个选项：strictNullchecks

#### strictNullchecks

这个选项具有 off & on 两个值，当值为 off 时，你可以不对 null & undefined 做任何检查，但是正因为没有检查，才是导致错误的主要来源。当其值为 on 时，则需要做出规范检查如示：

```ts
function greet(str: null | string) {
  if (str === null) {
    //do nothing
  } else {
    console.log(`hello`, str);
  }
}
```

所以一般情况下，为避免类似问题导致错误，我们一般会启动 strictNullchecks

#### 非空运算判断符 ！

在 ts 中还有一种方法可以免去像上方代码块类似的显式判断语句，这就是 **！语法**
如当代码中出现 **str!** 时，会自动删除 str 的 null 和 undefined 部分

## Narrowing

在 TypeScript 中，有一种可以通过类型保护，条件判断等方式来提高代码类型安全性的操作，我们称之为 **_缩小_**

### 真实性缩小

真实性缩小即我们可以在 if ，&& ，|| ，！等布尔判断中使用任何表达式。

- ''
- null
- undefined
- 0
- NaN

这些表达在类似 if 结构中会被强行转化为 false，其他的则被转化为 true
还有运算符 in ，instanceof 等也可以通过缩小原理维护代码类型安全，不再细述

### 类型谓词
