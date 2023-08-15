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