# 教程
https://wangdoc.com/typescript/
# 简介
## 概述
**TypeScript 可以看成是 JavaScript 的超集（superset）**，即它继承了后者的全部语法，所有 JavaScript 脚本都可以当作 TypeScript 脚本（但是可能会报错），此外它再增加了一些自己的语法。

TypeScript 的主要功能是为 JavaScript 添加类型系统。
## 类型的概念
**类型（type）指的是一组具有相同特征的值**。

一旦确定某个值的类型，就意味着，这个值具有该类型的所有特征，可以进行该类型的所有运算。凡是适用该类型的地方，都可以使用这个值；凡是不适用该类型的地方，使用这个值都会报错。

可以这样理解，**类型是人为添加的一种编程约束和用法提示**。主要目的是在软件开发过程中，为编译器和开发工具提供更多的验证和帮助，帮助提高代码质量，减少错误。
## 动态类型与静态类型
JavaScript 的类型系统非常弱，而且没有使用限制，运算符可以接受各种类型的值。在语法上，**JavaScript 属于动态类型语言**。

**TypeScript 引入了一个更强大、更严格的类型系统，属于静态类型语言**。其作用就是为 JavaScript 引入静态类型特征。
## 静态类型的优点 
- **有利于代码的静态分析**：有了静态类型，不必运行代码，就可以确定变量的类型，从而推断代码有没有错误。这就叫做代码的静态分析。这对于大型项目非常重要，单单在开发阶段运行静态检查，就可以发现很多问题，避免交付有问题的代码，大大降低了线上风险。
- **有利于发现错误**：每个值、每个变量、每个运算符都有严格的类型约束，TypeScript 就能轻松发现拼写错误、语义错误和方法调用错误，节省程序员的时间。
- **更好的 IDE 支持，做到语法提示和自动补全**：IDE（集成开发环境，比如 VSCode）一般都会利用类型信息，提供语法提示功能（编辑器自动提示函数用法、参数等）和自动补全功能（只键入一部分的变量名或函数名，编辑器补全后面的部分）。
- **提供了代码文档**：类型信息可以部分替代代码文档，解释应该如何使用这些代码，熟练的开发者往往只看类型，就能大致推断代码的作用。借助类型信息，很多工具能够直接生成文档。
- **有助于代码重构**：类型信息大大减轻了重构的成本。一般来说，只要函数或对象的参数和返回值保持类型不变，就能基本确定，重构后的代码也能正常运行。如果还有配套的单元测试，就完全可以放心重构。越是大型的、多人合作的项目，类型信息能够提供的帮助越大。

综上所述，TypeScript 有助于提高代码质量，保证代码安全，更适合用在大型的企业级项目。
## 静态类型的缺点
- 丧失了动态类型的代码灵活性：动态类型有非常高的灵活性，给予程序员很大的自由，静态类型将这些灵活性都剥夺了。
- 增加了编程工作量：有了类型之后，程序员不仅需要编写功能，还需要编写类型声明，确保类型正确。这增加了不少工作量，有时会显著拖长项目的开发时间。
- 更高的学习成本：类型系统通常比较复杂，要学习的东西更多，要求开发者付出更高的学习成本。
- **引入了独立的编译步骤**：原生的 JavaScript 代码，可以直接在 JavaScript 引擎运行。添加类型系统以后，就多出了一个单独的编译步骤，检查类型是否正确，并将 TypeScript 代码转成 JavaScript 代码，这样才能运行。
- **兼容性问题**：TypeScript 依赖 JavaScript 生态，需要用到很多外部模块。但是，过去大部分 JavaScript 项目都没有做 TypeScript 适配，虽然可以自己动手做适配，不过使用时难免还是会有一些兼容性问题。

总的来说，这些缺点使得 TypeScript 不一定适合那些小型的、短期的个人项目。
# 基本用法
## 类型声明
**类型声明的写法，一律为在标识符后面添加“冒号 + 类型”**。 函数参数和返回值，也是这样来声明类型。
```
function toString(num:number):string {
  return String(num);
}
```
**TypeScript 中没有赋值就被读取，是会报错的**。 JavaScript 允许这种行为，不会报错，没有赋值的变量会返回undefined。
## 类型推断
**类型声明并不是必需的，如果没有，TypeScript 会自己推断类型**。
```
let foo = 123;
foo = 'hello'; // 报错
```
**TypeScript 的设计思想是，类型声明是可选的，你可以加，也可以不加**。即使不加类型声明，依然是有效的 TypeScript 代码，只是这时不能保证 TypeScript 会正确推断出类型。由于这个原因。**所有 JavaScript 代码都是合法的 TypeScript 代码**。
### TypeScript 的编译
JavaScript 的运行环境（浏览器和 Node.js）不认识 TypeScript 代码。所以，**TypeScript 项目要想运行，必须先转为 JavaScript 代码**，这个代码转换的过程就叫做“编译”（compile）。

**TypeScript 官方没有做运行环境，只提供编译器**。 **编译时，会将类型声明和类型相关的代码全部删除，只留下能运行的 JavaScript 代码**，并且不会改变 JavaScript 的运行结果。

因此，**TypeScript 的类型检查只是编译时的类型检查，而不是运行时的类型检查。一旦代码编译为 JavaScript，运行时就不再检查类型了**。
## 值与类型
**“类型”是针对“值”的，可以视为是后者的一个元属性**。每一个值在 TypeScript 里面都是有类型的。

**TypeScript 项目里面，其实存在两种代码，一种是底层的“值代码”，另一种是上层的“类型代码”**。前者使用 JavaScript 语法，后者使用 TypeScript 的类型语法。它们是可以分离的，TypeScript 的编译过程，实际上就是把“类型代码”全部拿掉，只保留“值代码”。
## TypeScript Playground
最简单的 TypeScript 使用方法，就是使用官网的在线编译页面，叫做 [TypeScript Playground](https://www.typescriptlang.org/play)。
## tsc 编译器
TypeScript 官方提供的编译器叫做 tsc，tsc 的作用就是把.ts脚本转变成.js脚本。
### 安装
```
npm install -g typescript // 安装
tsc -v // 检查是否安装成功
```
### 帮助信息
```
tsc -h // 基本的可用选项
tsh --all // 完整的帮助信息
```
### 编译脚本
**安装 tsc 之后，就可以编译 TypeScript 脚本了**。

tsc命令后面，加上 TypeScript 脚本文件，就可以将其编译成 JavaScript 脚本。
```
tsc app.ts
```
上面命令会在当前目录下，生成一个app.js脚本文件，这个脚本就完全是编译后生成的 JavaScript 代码。

**tsc 有很多参数，可以调整编译行为**。
- **如果想将多个 TypeScript 脚本编译成一个 JavaScript 文件，使用--outFile参数**。
```
tsc file1.ts file2.ts --outFile app.js // 一次编译多个 TypeScript 脚本，结果文件为app.js
```
- **编译结果默认都保存在当前目录，--outDir参数可以指定保存到其他目录**。
```
tsc app.ts --outDir dist // 在dist子目录下生成app.js
```
- **可以使用--target参数，指定编译后的 JavaScript 版本**。建议使用es2015，或者更新版本。
```
tsc --target es2015 app.ts
```
- tsc 还有一个--noEmit参数，只检查类型是否正确，不生成 JavaScript 文件。
```
tsc --noEmit app.ts // 只检查是否有编译错误，不会生成app.js
```
### 编译错误的处理
**如果编译报错**，tsc命令就会显示报错信息，但是这种情况下，**依然会编译生成 JavaScript 脚本**。TypeScript 团队认为，编译器的作用只是给出编译错误，至于怎么处理这些错误，那就是开发者自己的判断了。

**如果希望一旦报错就停止编译，不生成编译产物，可以使用--noEmitOnError参数**。
```
tsc --noEmitOnError app.ts // 若发生报错，就不会生成app.js。
```
### tsconfig.json
TypeScript 允许将tsc的编译参数，写在配置文件tsconfig.json。只要当前目录有这个文件，tsc就会自动读取，所以运行时可以不写参数。
```
tsc file1.ts file2.ts --outFile dist/app.js
```
上面这个命令写成tsconfig.json，就是下面这样。
```
{
  "files": ["file1.ts", "file2.ts"],
  "compilerOptions": {
    "outFile": "dist/app.js"
  }
}
```
有了这个配置文件，编译时直接调用tsc命令就可以了。
```
tsc
```
## ts-node 模块
[ts-node](https://github.com/TypeStrong/ts-node)是一个非官方的 npm 模块，可以直接运行 TypeScript 代码。
```
npm install -g ts-node // 安装
ts-node script.ts // 直接运行ts脚本
```
如果不安装 ts-node，也可以通过 npx 调用它来运行 TypeScript 脚本。
```
npx ts-node script.ts // npx会在线调用 ts-node，从而在不安装的情况下，运行script.ts。
```
如果执行 ts-node 命令不带有任何参数，它会提供一个 TypeScript 的命令行 REPL 运行环境，你可以在这个环境中输入 TypeScript 代码，逐行执行。
```
$ ts-node
> const twice = (x:string) => x + x;
> twice('abc')
'abcabc'
> 
```
要退出这个 REPL 环境，可以按下 Ctrl + d，或者输入.exit。
# TypeScript 的三种特殊类型
## any 类型
### 基本含义
any 类型表示没有任何限制，该类型的变量可以赋予任意类型的值。

**变量类型一旦设为any，TypeScript 实际上会关闭这个变量的类型检查**。即使有明显的类型错误，只要句法正确，都不会报错。由于这个原因，**应该尽量避免使用any类型，否则就失去了使用 TypeScript 的意义**。
```
let x:any = 'hello';

x(1) // 不报错
x.foo = 100; // 不报错
```
实际开发中，any类型主要适用以下两个场合。
1. 出于特殊原因，需要关闭某些变量的类型检查，就可以把该变量的类型设为any。
2. 为了适配以前老的 JavaScript 项目，让代码快速迁移到 TypeScript，可以把变量类型设为any。有些年代很久的大型 JavaScript 项目，尤其是别人的代码，很难为每一行适配正确的类型，这时你为那些类型复杂的变量加上any，TypeScript 编译时就不会报错。
### 类型推断问题
对于开发者没有指定类型、TypeScript 必须自己推断类型的那些变量，如果无法推断出类型，TypeScript 就会认为该变量的类型是any。这显然是很糟糕的情况，所以**对于那些类型不明显的变量，一定要显式声明类型，防止被推断为any**。
```
function add(x, y) {
  return x + y;
}

add(1, [1, 2, 3]) // 不报错
```
**TypeScript 提供了一个编译选项noImplicitAny，打开该选项，只要推断出any类型就会报错**。
```
tsc --noImplicitAny app.ts
```
上面命令使用了noImplicitAny编译选项进行编译，这时上面的函数add()就会报错。

有一个特殊情况，即使打开了noImplicitAny，使用let和var命令声明变量，但不赋值也不指定类型，是不会报错的。因此，**建议使用let和var声明变量时，如果不赋值，就一定要显式声明类型，否则可能存在安全隐患**。
```
var x; // 不报错
let y; // 不报错
```
const命令没有这个问题，因为 JavaScript 语言规定const声明变量时，必须同时进行初始化（赋值）。
```
const x; // 报错
```
const命令声明的x是不能改变值的，声明时必须同时赋值，否则报错，所以它不存在类型推断为any的问题。
### 污染问题
**any类型除了关闭类型检查，还有一个很大的问题，就是它会“污染”其他变量**。它可以赋值给其他任何类型的变量（因为没有类型检查），导致其他变量出错。
```
let x:any = 'hello';
let y:number;

y = x; // 不报错

y * 123 // 不报错
y.toFixed() // 不报错
```
上面示例中，变量x的类型是any，实际的值是一个字符串。变量y的类型是number，表示这是一个数值变量，但是它被赋值为x，这时并不会报错。然后，变量y继续进行各种数值运算，TypeScript 也检查不出错误，问题就这样留到运行时才会暴露。

**污染其他具有正确类型的变量，把错误留到运行时，这就是不宜使用any类型的另一个主要原因**。
## unknown 类型
为了解决any类型“污染”其他变量的问题，TypeScript 3.0 引入了unknown类型。它与any含义相同，表示类型不确定，可能是任意类型，但是它的使用有一些限制，不像any那样自由，可以视为严格版的any。

**unknown跟any的相似之处，在于所有类型的值都可以分配给unknown类型**。
```
let x:unknown;

x = true; // 正确
x = 42; // 正确
x = 'Hello World'; // 正确
```
unknown类型跟any类型的不同之处在于，它不能直接使用。主要有以下几个限制。
- unknown类型的变量，不能直接赋值给其他类型的变量（除了any类型和unknown类型）。
```
let v:unknown = 123;

let v1:boolean = v; // 报错
let v2:number = v; // 报错
```
- **不能直接调用unknown类型变量的方法和属性**。
```
let v1:unknown = { foo: 123 };
v1.foo  // 报错

let v2:unknown = 'hello';
v2.trim() // 报错

let v3:unknown = (n = 0) => n + 1;
v3() // 报错
```
- **unknown类型变量能够进行的运算是有限的**，只能进行比较运算（==、===、!=、!==、||、&&、?）、取反运算（!）、typeof运算符和instanceof运算符这几种，其他运算都会报错。
```
let a:unknown = 1;

a + 1 // 报错
a === 1 // 正确
```

怎么才能使用unknown类型变量呢？-> **只有经过“类型缩小”，unknown类型变量才可以使用**。所谓“类型缩小”，就是缩小unknown变量的类型范围，确保不会出错。这样设计的目的是，**只有明确unknown变量的实际类型，才允许使用它，防止像any那样可以随意乱用，“污染”其他变量。类型缩小以后再使用，就不会报错**。
```
let a:unknown = 1;

if (typeof a === 'number') {
  let r = a + 10; // 正确
}
```
上面示例中，unknown类型的变量a经过typeof运算以后，能够确定实际类型是number，就能用于加法运算了。这就是“类型缩小”，即将一个不确定的类型缩小为更明确的类型。

总之，**unknown可以看作是更安全的any**。一般来说，凡是需要设为any类型的地方，通常都应该优先考虑设为unknown类型。
## never 类型
为了保持与集合论的对应关系，以及类型运算的完整性，**TypeScript 还引入了“空类型”的概念，即该类型为空，不包含任何值**。 由于不存在任何属于“空类型”的值，所以**该类型被称为never**，即不可能有这样的值。
```
let x:never;
```
上面示例中，变量x的类型是never，就不可能赋给它任何值，否则都会报错。

never类型的使用场景，主要是在一些类型运算之中，保证类型运算的完整性。另外，不可能返回值的函数，返回值的类型就可以写成never。

如果一个变量可能有多种类型（即联合类型），通常需要使用分支处理每一种类型。这时，处理所有可能的类型之后，剩余的情况就属于never类型。
```
function fn(x:string|number) {
  if (typeof x === 'string') {
    // ...
  } else if (typeof x === 'number') {
    // ...
  } else {
    x; // never 类型
  }
}
```
**never类型的一个重要特点是，可以赋值给任意其他类型。**
```
function f():never {
  throw new Error('Error');
}

let v1:number = f(); // 不报错
let v2:string = f(); // 不报错
let v3:boolean = f(); // 不报错
```
上面示例中，函数f()会抛错，所以返回值类型可以写成never，即不可能返回任何值。各种其他类型的变量都可以赋值为f()的运行结果（never类型）。

为什么never类型可以赋值给任意其他类型呢？ -> 这也跟集合论有关，空集是任何集合的子集。**TypeScript 就相应规定，任何类型都包含了never类型**。因此，never类型是任何其他类型所共有的，TypeScript 把这种情况称为“底层类型”（bottom type）。

总之，**TypeScript 有两个“顶层类型”（any和unknown），但是“底层类型”只有never唯一一个**。
# TypeScript 的类型系统
## 基本类型
JavaScript 语言（注意，不是 TypeScript）将值分成8种类型。
- boolean
- string
- number
- bigint
- symbol
- object
- undefined
- null

TypeScript 继承了 JavaScript 的类型设计，以上8种类型可以看作 TypeScript 的基本类型。

上面所有类型的名称都是小写字母，首字母大写的Number、String、Boolean等在 JavaScript 语言中都是内置对象，而不是类型名称。

undefined 和 null 既可以作为值，也可以作为类型，取决于在哪里使用它们。

这8种基本类型是 TypeScript 类型系统的基础，复杂类型由它们组合而成。
### boolean 类型
boolean类型只包含true和false两个布尔值。
```
const x:boolean = true;
const y:boolean = false;
```
### string 类型
string类型包含所有字符串。
```
const x:string = 'hello';
const y:string = `${x} world`;
```
### number 类型
number类型包含所有整数和浮点数。
```
const x:number = 123;
const y:number = 3.14;
const z:number = 0xffff;
```
### bigint 类型
bigint 类型包含所有的大整数。
```
const x:bigint = 123n;
const y:bigint = 0xffffn;
```
bigint 与 number 类型不兼容。
```
const x:bigint = 123; // 报错
const y:bigint = 3.14; // 报错
```
### symbol 类型
symbol 类型包含所有的 Symbol 值。
```
const x:symbol = Symbol();
```
### object 类型
object 类型包含了所有对象、数组和函数。
```
const x:object = { foo: 123 };
const y:object = [1, 2, 3];
const z:object = (n:number) => n + 1;
```
### undefined 类型，null 类型
undefined 和 null 是两种独立类型，它们各自都只有一个值。

undefined 类型只包含一个值undefined，表示未定义（即还未给出定义，以后可能会有定义）。
```
let x:undefined = undefined;
```
null 类型也只包含一个值null，表示为空（即此处没有值）。
```
const x:null = null;
```
如果没有声明类型的变量，被赋值为undefined或null，它们的类型会被推断为any。
```
let a = undefined;   // any
const b = undefined; // any

let c = null;        // any
const d = null;      // any
```
如果希望避免这种情况，则需要打开编译选项strictNullChecks。
```
// 打开编译设置 strictNullChecks
let a = undefined;   // undefined
const b = undefined; // undefined

let c = null;        // null
const d = null;      // null
```
## 包装对象类型
### 包装对象的概念
JavaScript 的8种类型之中，undefined和null其实是两个特殊值，object属于复合类型，剩下的五种属于**原始类型（primitive value），代表最基本的、不可再分的值**。
- boolean
- string
- number
- bigint
- symbol

**上面这五种原始类型的值，都有对应的包装对象（wrapper object）**。 **所谓“包装对象”，指的是这些值在需要时，会自动产生的对象**。
```
'hello'.charAt(1) // 'e'
```
上面示例中，字符串hello执行了charAt()方法。但是，在 JavaScript 语言中，只有对象才有方法，原始类型的值本身没有方法。这行代码之所以可以运行，就是因为**在调用方法时，字符串会自动转为包装对象，charAt()方法其实是定义在包装对象上**。

这样的设计大大方便了字符串处理，省去了将原始类型的值手动转成对象实例的麻烦。

五种包装对象之中，symbol 类型和 bigint 类型无法直接获取它们的包装对象（即Symbol()和BigInt()不能作为构造函数使用），剩下三种可以。
- Boolean()
- String()
- Number()

以上三个构造函数，执行后可以直接获取某个原始类型值的包装对象。
```
const s = new String('hello');
typeof s // 'object'
s.charAt(1) // 'e'
```
上面示例中，s就是字符串hello的包装对象，typeof运算符返回object，不是string，但是本质上它还是字符串，可以使用所有的字符串方法。

注意，String()**只有当作构造函数使用时（即带有new命令调用），才会返回包装对象**。如果当作普通函数使用（不带有new命令），返回就是一个普通字符串。其他两个构造函数Number()和Boolean()也是如此。
### 包装对象类型与字面量类型
**由于包装对象的存在，导致每一个原始类型的值都有包装对象和字面量两种情况**。
```
'hello' // 字面量
new String('hello') // 包装对象
```
为了区分这两种情况，TypeScript 对五种原始类型分别提供了大写和小写两种类型。
- Boolean 和 boolean
- String 和 string
- Number 和 number
- BigInt 和 bigint
- Symbol 和 symbol

其中，**大写类型同时包含包装对象和字面量两种情况，小写类型只包含字面量，不包含包装对象**。
```
const s1:String = 'hello'; // 正确
const s2:String = new String('hello'); // 正确

const s3:string = 'hello'; // 正确
const s4:string = new String('hello'); // 报错
```
**建议只使用小写类型，不使用大写类型。因为绝大部分使用原始类型的场合，都是使用字面量，不使用包装对象**。而且，**TypeScript 把很多内置方法的参数，定义成小写类型，使用大写类型会报错**。
```
const n1:number = 1;
const n2:Number = 1;

Math.abs(n1) // 1
Math.abs(n2) // 报错
```
Symbol()和BigInt()这两个函数不能当作构造函数使用，所以没有办法直接获得 symbol 类型和 bigint 类型的包装对象，除非使用下面的写法。但是，它们没有使用场景，因此**Symbol和BigInt这两个类型虽然存在，但是完全没有使用的理由**。
```
let a = Object(Symbol());
let b = Object(BigInt());
```
注意，目前在 TypeScript 里面，symbol和Symbol两种写法没有差异，bigint和BigInt也是如此，不知道是否属于官方的疏忽。建议始终使用小写的symbol和bigint，不使用大写的Symbol和BigInt。
## Object 类型与 object 类型
TypeScript 的对象类型也有大写Object和小写object两种。
### Object 类型
**大写的Object类型代表 JavaScript 语言里面的广义对象。所有可以转成对象的值，都是Object类型，这囊括了几乎所有的值**。 另外，**空对象{}是Object类型的简写形式，所以使用Object时常常用空对象代替**。
```
// let obj:Object;
let obj:{};
 
obj = true;
obj = 'hi';
obj = 1;
obj = { foo: 123 };
obj = [1, 2];
obj = (a:number) => a + 1;
```
上面示例中，原始类型值、对象、数组、函数都是合法的Object类型。

**除了undefined和null这两个值不能转为对象，其他任何值都可以赋值给Object类型**。
```
let obj:Object;

obj = undefined; // 报错
obj = null; // 报错
```
**无所不包的Object类型既不符合直觉，也不方便使用**。
### object 类型
**小写的object类型代表 JavaScript 里面的狭义对象，即可以用字面量表示的对象，只包含对象、数组和函数，不包括原始类型的值**。
```
let obj:object;
 
obj = { foo: 123 };
obj = [1, 2];
obj = (a:number) => a + 1;
obj = true; // 报错
obj = 'hi'; // 报错
obj = 1; // 报错
```
大多数时候，我们使用对象类型，只希望包含真正的对象，不希望包含原始类型。所以，**建议总是使用小写类型object，不使用大写类型Object**。

**无论是大写的Object类型，还是小写的object类型，都只包含 JavaScript 内置对象原生的属性和方法**，用户自定义的属性和方法都不存在于这两个类型之中。
```
const o1:Object = { foo: 0 };
const o2:object = { foo: 0 };

o1.toString() // 正确
o1.foo // 报错

o2.toString() // 正确
o2.foo // 报错
```
## undefined 和 null 的特殊性 
undefined和null既是值，又是类型。

**作为值，它们有一个特殊的地方：任何其他类型的变量都可以赋值为undefined或null**。
```
let age:number = 24;

age = null;      // 正确
age = undefined; // 正确
```
但是有时候，这并不是开发者想要的行为，也不利于发挥类型系统的优势。
```
const obj:object = undefined;
obj.toString() // 编译不报错，运行就报错
```
为了避免这种情况，及早发现错误，TypeScript 提供了一个编译选项strictNullChecks。只要打开这个选项，undefined和null就不能赋值给其他类型的变量，只能赋值给自身，或者any类型和unknown类型的变量。
## 值类型
TypeScript 规定，单个值也是一种类型，称为“值类型”。
```
let x:'hello';

x = 'hello'; // 正确
x = 'world'; // 报错
```
TypeScript 推断类型时，**遇到const命令声明的变量，如果代码里面没有注明类型，就会推断该变量是值类型**。
```
// x 的类型是 "https"
const x = 'https';

// y 的类型是 string
const y:string = 'https';
```
这样推断是合理的，因为const命令声明的变量，一旦声明就不能改变，相当于常量。值类型就意味着不能赋为其他值。

**const命令声明的变量，如果赋值为对象，并不会推断为值类型。**
```
// x 的类型是 { foo: number }
const x = { foo: 1 };
```
变量x没有被推断为值类型，而是推断属性foo的类型是number。这是因为 JavaScript 里面，const变量赋值为对象时，属性值是可以改变的。

值类型可能会出现一些很奇怪的报错。
```
const x:5 = 4 + 1; // 报错
```
等号左侧的类型是数值5，等号右侧4 + 1的类型，TypeScript 推测为number。由于5是number的子类型，number是5的父类型，父类型不能赋值给子类型，所以报错了（详见本章后文）。

## 联合类型
## 交叉类型
## type 命令
## typeof 运算符
## 块级类型声明
## 类型的兼容