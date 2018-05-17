
# Gago TypeScript Style Guide() {

<a name="table-of-contents"></a>
## 目录
  1. [tsconfig配置规范](#tsconfig)
  1. [文件夹及文件](#directory-file)
  1. [总体命名原则](#naming-rule)
  1. [空白和缩进](#whitespace-indent)
  1. [符号](#punctuation)
  1. [变量](#variables)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [数字和字符串](#numbers-strings)
  1. [枚举](#enums)
  1. [接口和类型](#interface-type)
  1. [解构](#destructuring)
  1. [函数](#functions)
  1. [类](#classes)
  1. [比较运算](#comparison)
  1. [模块](#modules)
  1. [注释](#comments)
  1. [react](#react)
  1. [参考](#references)

<a name="tsconfig"></a>
## tsconfig 配置规范

  > 此推荐 config 适用于 typescript v2.7.x 以上。建议使用 `tsc --init`自动生成配置文件，会有相应的配置信息提示。

  ```json
  {
    "compilerOptions": {
      "target": "es5",
      "module": "commonjs",
      "checkJs": true,
      "jsx": "react",
      "sourceMap": true,
      "importHelpers": true,
      "strict": true,
      "alwaysStrict": true,
      "noUnusedLocals": true,
      "noUnusedParameters": true,
      "noImplicitReturns": true,
      "noFallthroughCasesInSwitch": true,
      "moduleResolution": "node",
      "esModuleInterop": true
    }
  }
  ```

  其中需要注意以下几点：

  - **target** 若无特殊情况默认 `ES5`，其余情况下自行查询 node 或 browser 的兼容性后可配置为 `ES2015+`。

  - **module** 默认为 `commonjs`。

    * node 环境下永远为 `commonjs`。

    * 若要配合 *antd* 使用，需设置为 `es2015`。

    * 使用 *dynamic import* (前端代码切割) 时需配置为 `esnext`。

    * 前端打包一个库时编译为 `umd`。

  - **jsx** 使用 *react* 时配置为 `react`。

  - 默认开启严格模式及几个严格的检查。其中可酌情关闭的有:

    * `strictNullChecks`: 适用于将 undefined 和 null 赋值给其他类型。

    * `strictPropertyInitialization`: 检查类成员是否初始化。

    * `noUnusedParameters`: 是否有未使用的参数。

  - 需要使用**装饰器**时，开启如下两个选项。

    ```json
    {
      "experimentalDecorators": true,
      "emitDecoratorMetadata": true,
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="directory-file"></a>
## 文件夹及文件

  - [2.1](#2.1) <a name="2.1"></a> **文件夹**一律使用`小写`命名。使用 `"-"` 来分隔描述性单词。例如 `react-dom`。

  - [2.2](#2.2) <a name="2.2"></a> 推荐根据业务逻辑来划分文件夹。在保证目录结构清晰的情况下应尽量减少嵌套层次。

    ```
    // bad
    ├── controllers
    │   ├── user.controller.ts
    │   └── xxx.controller.ts
    ├─── models
    │   ├── user.controller.ts
    │   └── xxx.controller.ts
    └─── test
        ├── user.spec.ts
        └── xxx.spec.ts

    // good
    ├── user
    │   ├── user.controller.ts
    │   ├── user.model.ts
    │   └── user.spec.ts
    └─── test
        ├── xxx.controller.ts
        ├── xxx.model.ts
        └── xxx.spec.ts

    ```

  - [2.3](#2.3) <a name="2.3"></a> *文件名*使用`小写`命名。使用 `"-"` 来分隔描述性单词。使用 `"."` 来分隔类型。遵循 `feature.type.ts` 的模式。例如 `user.controller.ts`。

  - [2.4](#2.4) <a name="2.4"></a> **单元测试文件**添加 `.spec` 后缀，**端到端测试文件**添加 `.e2e-spec` 后缀。

  - [2.5](#2.5) <a name="2.5"></a> `export name` 应尽量与 `filename` 保持一致。使用命名导出使而不是默认导出。(使用 *react* 的同学注意)。

  - [2.6](#2.6) <a name="2.6"></a> 其中可在模块文件夹（即上文中的 *user* 文件夹）下使用 `index.ts` **重新导出**其他模块需要 *import* 的内容。

    > Why？重新导出可以简化其他模块导入时的路径意为。

    > Why？重新导出意为 `public api`，即不在该文件中导出的内容理论上作为模块的 `private api` 不建议导入。

  - [2.7](#2.7) <a name="2.7"></a> 所有文件应遵循单一功能原则，一个文件一个类，考虑每个文件的代码限制在 `400` 行以内。

    > Why？这有助于使代码更简洁，更容易阅读、维护和测试。

**[⬆ 返回目录](#table-of-contents)**

<a name="naming-rule"></a>
## 总体命名原则

  - [3.1](#3.1) <a name="3.1"></a> 命名使用英文单词，不允许使用拼音，要见名知意，不常见的单词需添加中文注释。

  - [3.2](#3.2) <a name="3.2"></a> 当表示数组列表等可数名词要加 `s` 后缀，js 里由于没有类型，不知道是否是数组，推荐不可数名词也加上 `s` 以示区别，ts 在有类型声明的情况下可以不用加。

  - [3.3](#3.3) <a name="3.3"></a> 函数方法名中动词使用一般现在时，`getXXX()，fetchData()`。

  - [3.4](#3.4) <a name="3.4"></a> 表生命周期的方法可以加时态，例如 *react* 式的 `didMount`, *vue* 式的 `mounted`。

  - [3.5](#3.5) <a name="3.5"></a> 布尔值加上 `is` 前缀，`isOK`, 表状态时加上时态，`isLoading，isLoaded`。

  - [3.6](#3.6) <a name="3.6"></a> 原则上任何命名不允许缩写。

  - [3.7](#3.7) <a name="3.7"></a> 单个单词原则上坚决不允许缩写，尤其是用在变量声明时。例如 `password => pwd`，但允许像 `obj` 这种缩写，在函数参数名中，一些常见的可以使用缩写，例如 `information => info, parameters => params`。

  - [3.8](#3.8) <a name="3.8"></a> 拼接词长度超过 20 个可以使用缩写

    * 缩写时采用常见的缩写方式（以能猜出单词意思或音译出为佳）例如: `button => btn`。

    * 其余缩写方式以缩写单词前面字母为主，例如 `parameters => params`。

    * 如果单词实在过长或单词过多，采用缩写首字母方式， 例如 `Hypertext Markup Language => HTML`，表明变量或方法的主要单词尽量不要缩写。

    * 缩写词必须添加完整的单词注释，不常见的词要添加中文注释。

  - [3.9](#3.9) <a name="3.9"></a> 若无特殊说明，默认使用 `camelCase`，保留前置下划线，但不是用于表达私有变量，多用于不可变数据重新赋值，考虑添加 `$` 来表示一类特殊的值，例如 *RxJS* 中的 *Observable*。

**[⬆ 返回目录](#table-of-contents)**

<a name="whitespace-indent"></a>
## 空白和缩进

  - [4.1](#4.1) <a name="4.1"></a> 在文件末尾保留一个空行。VSCODE 可配置 `"files.insertFinalNewline": true`。

  - [4.2](#4.2) <a name="4.2"></a> 不允许有连续多行空行。

  - [4.3](#4.3) <a name="4.3"></a> 块语句不以空行结束。

  - [4.4](#4.4) <a name="4.4"></a> 块语句之后添加一个空行。

  - [4.5](#4.5) <a name="4.5"></a> 考虑 `return` 语句前添加一个空行。

  - [4.6](#4.6) <a name="4.6"></a> 禁止空的代码块。允许 `function noop() {}` 工具函数。

  - [4.7](#4.7) <a name="4.7"></a> 禁止行尾的空格。VSCODE 可配置 `·"files.trimTrailingWhitespace": true`。

  - [4.8](#4.8) <a name="4.8"></a> 禁止水平对齐。

  - [4.9](#4.9) <a name="4.9"></a> 使用`两个空格`缩进。不要混用空格和制表符缩进。

  - [4.10](#4.10) <a name="4.10"></a> 以下情况前后各保留一个空格。

    * 算术运算符( `+, -, *, /, %, **` )

    * 关系运算符( `in, instanceof, <, >, <=, >=` )

    * 相等操作符( `==, !=, ===, !==` )

    * 逻辑运算符( `&&, ||` )

    * 三元运算符( `?, :` )

    * 赋值运算符( `=, +=, -=, *=, /=, %=, **=` )


  - [4.11](#4.11) <a name="4.11"></a> 关键字之后保留一个空格。(` function, class, const, if, for ...`)

  - [4.12](#4.12) <a name="4.12"></a> 冒号，逗号，分号之后保留一个空格，他们之前禁止保留空格。

  - [4.13](#4.13) <a name="4.13"></a> 圆括号，方括号之间不需要空格，花括号之间需要空格。

  - [4.14](#4.14) <a name="4.14"></a> 对象的计算属性不需要空格。

  - [4.15](#4.15) <a name="4.15"></a> 函数名和圆括号之间不需要空格。

  - [4.16](#4.16) <a name="4.16"></a> 左花括号之前需要一个空格。

**[⬆ 返回目录](#table-of-contents)**

<a name="punctuation"></a>
## 符号

  - [5.1](#5.1) <a name="5.1"></a> 需要分号，禁止多余的分号。

  - [5.2](#5.2) <a name="5.2"></a> 前端使用单引号，node 使用双引号，jsx 中使用双引号。

  - [5.3](#5.3) <a name="5.3"></a> 禁止使用逗号操作符。[Read More](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comma_Operator)

  - [5.4](#5.4) <a name="5.4"></a> 不要使用行首逗号。

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - [5.5](#5.5) <a name="5.5"></a> 考虑增加结尾的逗号。

    > Why? 这会让 git diffs 更干净。

    ```typescript
    // bad - git diff without trailing comma
    const hero = {
      firstName: 'Florence',
    - lastName: 'Nightingale'
    + lastName: 'Nightingale',
    + inventorOf: ['coxcomb graph', 'modern nursing']
    }

    // good - git diff with trailing comma
    const hero = {
      firstName: 'Florence',
      lastName: 'Nightingale',
    + inventorOf: ['coxcomb chart', 'modern nursing'],
    }

    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="variables"></a>
## 变量

  - [6.1](#6.1) <a name="6.1"></a> 使用 `let` 声明变量，`const` 声明常量。不要使用 `var` 声明任何变量或常量。

  - [6.2](#6.2) <a name="6.2"></a> 变量使用 `camelCase` 命名，常量可使用 `camelCase, UPPER_CASE, PascalCase` 命名。

    ```typescript
    let a = 1;
    // 全局的常量使用 UPPER_CASE
    const API_URL = 'http://www.gagogroup.cn/api';
    function fn() {
      // 块内部的常量使用 camelCase
      const defaultValue = 1;
    }
    // 当常量表示一个 react 组件或全局的单例时，使用 PascalCase
    const Home = () => <h1>Hello, World!</h1>;
    export const Config = {};
    ```

  - [6.3](#6.3) <a name="6.3"></a> 一个变量一个声明语句。

    ```typescript
    // bad
    const num = 1,
        str = 'hello world',
        flag = true;

    // good
    const num = 1;
    const str = 'hello world';
    const flag = true;
    ```

  - [6.4](#6.4) <a name="6.4"></a> 对 `const` 和 `let` 声明进行分组，常量声明放置在最前，已赋值的变量其次，未赋值的变量最后。

    ```typescript
    // bad
    let a: number;
    const x = 1;
    let b = '1';
    const y = 2;

    // good
    const x = 1;
    const y = 2;
    let b = '1';
    let a: number;
    ```

  - [6.5](#6.5) <a name="6.5"></a> 声明之前禁止使用。

  - [6.6](#6.6) <a name="6.6"></a> 禁止覆盖外层作用域变量。

    ```typescript
    // bad
    const a = 1;
    function fn() {
      const a = true;
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="objects"></a>
## 对象

  - [7.1](#7.1) <a name="7.1"></a> 使用字面量创建对象。键值使用 `camelCase`。

    ```typescript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  - [7.2](#7.2) <a name="7.3"></a> 不要使用保留字作为键值，使用同义词进行替换。

    ```typescript
    // bad
    const superman = {
      class: 'alien',
      private: true,
    };

    // bad
    const superman = {
      klass: 'alien',
      private: true,
    };

    // good
    const superman = {
      type: 'alien',
      hidden: true,
    };

  - [7.3](#7.3) <a name="7.3"></a> 使用对象方法的简写。

    ```typescript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  - [7.4](#7.4) <a name="7.4"></a> 考虑使用对象属性值的简写。

    ```typescript
    const name = 'Luke Skywalker';

    // bad
    const obj = {
      name: name,
    };

    // good
    const obj = {
      name,
    };
    ```

  - [7.5](#7.5) <a name="7.5"></a> 如果使用了简写属性，将简写属性放置在最前。

    ```typescript
    const id = 1;
    const name = 'Luke Skywalker';

    // bad
    const obj = {
      age: 12,
      id,
      name,
    };

    // good
    const obj = {
      id,
      name,
      age: 12,
    };
    ```

  - [7.6](#7.6) <a name="7.6"></a> 键值需要加引号时再加引号。

    ```typescript
    // bad
    const user = {
      'id': 1,
      'first name': 'tom',
    };

    // good
    const user = {
      id: 1,
      'first name': 'tom',
    };

    // best
    const user = {
      id: 1,
      firstName: 'tom',
    };
    ```

  - [7.7](#7.7) <a name="7.7"></a> 在对象有明确的类型时，使用 `.` 语法，对象类型是 `any` 或索引类型时，使用 `['key']` 语法。

    ```typescript
    interface User {
      id: number
      name: string;
    }

    const user: User = {
      id: 1,
      name: 'tom',
    };

    // bad
    console.log(user['name']);

    // good
    console.log(user.name);

    const user: any = {
      id: 1,
      name: 'tom',
    }
    console.log(user['name']);

    interface AnyObject {
      [key: string]: any;
    }

    const obj: AnyObject = {};
    console.log(obj['data']);
    ```

  - [7.8](#7.8) <a name="7.8"></a> 考虑使用对象 `...` 扩展运算符代替 `Object.assign()`。

    ```typescript
    const original = { a: 1, b: 2 };

    // bad
    const result = Object.assign({}, original, { c: 3 });

    // good
    const result = { ...original, c: 3 };
    const result = { ...original, ...{ c: 3 } };
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="arrays"></a>
## 数组

  - [8.1](#8.1) <a name="8.1"></a> 使用字面量创建数组。

    ```typescript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - [8.2](#8.2) <a name="8.2"></a> 不要创建稀疏数组。

    ```typescript
    // bad
    const items: number[] = [1, , 2];
    ```
  

  - [8.3](#8.3) <a name="8.3"></a> 向数组添加元素时使用 Arrary#push 替代直接赋值。

    ```typescript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - [8.4](#8.4) <a name="8.4"></a> 使用扩展运算符 `...` 复制数组。

    ```typescript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```
  - [8.5](#8.5) <a name="8.5"></a> 使用 Array#from 把一个类数组对象转换成数组。

    ```typescript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

  - [8.6](#8.6) <a name="8.6"></a> 使用 map，forEach，filter，reduce 等方法代替 for 循环。

    ```typescript
    const items: number[] = [1, 2, 3];

    // bad
    const newItems: number[] = [];
    for (let i = 0; i < items.length; i ++) {
      const item = items[i] * 2;
      newItems.push(item);
    }

    // good
    const newItems = items.map(item => item * 2);

    // bad
    for (let i = 0; i < items.length; i ++) {
      console.log(items[i]);
    }

    // good
    items.forEach(item => console.log(item));

    // bad
    let sum: number;
    for (let i = 0; i < items.length; i ++) {
      sun += items[i];
    }

    // good
    const sum = items.reduce((prevSum, item) => prevSum + item);
    ```
  - [8.7](#8.7) <a name="8.7"></a> 在使用 pop，push，reverse 等会对数组产生突变的方法时，考虑用扩展运算符，slice() 等产生新的引用再操作，以使数据保持不可变。

    ```typescript
    const items: number[] = [1, 2, 3];

    // bad
    items.push(4);

    // good
    const newItems = items.concat([4]);
    // or
    const newItems = [...items, 4];

    // bad reverse 会改变数组后返回该引用，即 newItems 和 items 指向同一地址。
    const newItems = items.reverse();

    // good
    const newItems = items.slice().reverse();
    // or
    const newItems = [...items].reverse();
    ```
  - [8.8](#8.8) <a name="8.8"></a> 当数据类型不一致但顺序和数组长度固定时考虑使用元组类型。

    ```typescript
    // bad
    const a: Array<string | number> = [1, 'a'];

    // good
    interface NumStrTuple extends Array<number | string> {
      0: number;
      1: string;
      length: 2;
    }
    const a: NumStrTuple = [1, 'a'];
    const a: NumStrTuple = ['a', 1]; // error
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="numbers-strings"></a>
## Numbers & Strings

  - [9.1](#9.1) <a name="9.1"></a> 使用字面量创建 `number` 和 `string` 。

  - [9.2](#9.2) <a name="9.2"></a> 保留小数点之前的 0，不要保留小数末尾的 0。
    ```typescript
    // bad
    const a = .1;
    const b = 0.10;

    // good
    const c = 0.1;
    ```
  - [9.3](#9.3) <a name="9.3"></a> 不要过多的使用 `+` 号拼接字符串，考虑使用模板字符串。

  - [9.4](#9.4) <a name="9.4"></a> 尽量不要出现魔术字符串和魔术数字，考虑用有意义的变量代理。（数字 -1，0，1，100 等除外）。

  - [9.5](#9.5) <a name="9.5"></a> 使用 `parseInt` 取整时指定进制。

  - [9.6](#9.6) <a name="9.6"></a> 加号两边只能同为字符串或数字。

**[⬆ 返回目录](#table-of-contents)**

<a name="enums"></a>
## 枚举

  - [10.1](#10.1) <a name="10.1"></a> 枚举使用 `PascalCase`，枚举值考虑使用 `PascalCase` 或 `UPPER_CASE` 来与普通对象区分。

  - [10.2](#10.2) <a name="10.2"></a> 考虑使用常量枚举，这样编译后生成的代码更小，同时避免一些运行时性能损耗。

**[⬆ 返回目录](#table-of-contents)**

<a name="interface-type"></a>
## 接口和类型

  - [11.1](#11.1) <a name="11.1"></a> 接口和类型定义使用 `PascalCase`。

  - [11.2](#11.2) <a name="11.2"></a> 接口定义之前不要加 `I` 前缀。

  - [11.3](#11.3) <a name="11.3"></a> 考虑简单数据类型（number, string, boolean）不需要声明类型。

  - [11.4](#11.4) <a name="11.4"></a> 数组类型声明使用 T[]。当 T 为复杂结构时，使用 Array<T>。

    ```typescript
    interface Data {
      code: number;
      data: any;
    }
    type NumberOrString = number | string;
    const numbers: number[] = [1, 2, 3];
    const result: Data[] = [1, 2, 3];

    // bad
    const items: (string | number)[] = [];
    const items {id: number; name: string}[] = [];

    // good
    const items: Array<string | number> = [];
    const items: Array<{id: number, name: string}> = [];
    ```

  - [11.5](#11.5) <a name="11.5"></a> 定义函数接口时考虑使用 `type` 而不是 `interface`。

    ```typescript
    // bad
    interface Foo {
      (a: number, b: number): number;
    }

    // good 参数较多，类型比较复杂可以这么写
    type Foo = (a: number, b: number) => number;
    const foo: Foo = function(a, b) {
      return a + b;
    }

    // 但通常情况下更建议直接写在函数里
    function foo(a: number, b: number): number {
      return a + b;
    }
    ```

  - [11.6](#11.6) <a name="11.6"></a> 定义方法接口考虑使用对象的方法简写。

    ```typescript
    // bad
    interface User {
      getName: () => string;
    }

    // good
    interface User {
      getName(): string;
    }
    ```

  - [11.7](#11.7) <a name="11.7"></a> 函数重载签名放在一起。

  - [11.8](#11.8) <a name="11.8"></a> 合并可以合并的类型声明。

    ```typescript
    // bad
    function foo(a: number): void;
    function foo(a: number, b: number): void;

    function bar(a: number): void;
    function bar(a: string): void;

    // good
    function foo(a: number, b?: number): void;

    function bar(a: number | string): void;
    ```

  - [11.9](#11.9) <a name="11.9"></a> 泛型中包含默认类型时，不用添加默认类型。

    ```typescript
    function foo<T = number>(a: T): T;
    // bad
    foo<number>(1);

    // good
    foo(1);
    foo<string>('text');
    ```
  - [11.10](#11.10) <a name="11.10"></a> 当类型是固定的几个数字或字符串时，使用字面量类型代替 `string` 和 `number`。

    ```typescript
    type Position = 'left' | 'right' | 'top' | 'bottom';

    // bad
    function foo(position: string): void;

    // good
    function foo(position: Position): void;
    ```


**[⬆ 返回目录](#table-of-contents)**

<a name="destructuring"></a>
## 解构

  - [12.1](#12.1) <a name="12.1"></a> 使用解构存取和使用多属性对象。

    > Why？因为解构能减少临时引用属性。

    ```typescript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(obj) {
      const { firstName, lastName } = obj;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  - [12.2](#12.2) <a name="12.2"></a> 对数组使用解构赋值。

    ```typescript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  - [12.3](#12.3) <a name="12.3"></a> 需要回传多个值时，使用对象解构，而不是数组解构。

    > Why？增加属性或者改变排序不会改变调用时的位置。

    ```typescript
    // bad
    function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
    }

    // 调用时需要考虑回调数据的顺序。
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // then a miracle occurs
      return { left, right, top, bottom };
    }

    // 调用时只选择需要的数据
    const { left, right } = processInput(input);
    ```
**[⬆ 返回目录](#table-of-contents)**

<a name="functions"></a>
## 函数

  - [13.1](#13.1) <a name="13.1"></a> 使用函数声明代替函数表达式。函数表达式只适用于箭头函数。

    > Why？因为函数声明是可命名的，所以他们在调用栈中更容易被识别。此外，函数声明会把整个函数提升（hoisted），而函数表达式只会把函数的引用变量名提升。这条规则使得[箭头函数](#arrow-functions)可以取代函数表达式。

    ```typescript
    // bad
    const foo = function () {
    };

    // good
    function foo() {
    }
    const foo = （）=> ({});
    ```

  - [13.2](#13.2) <a name="13.2"></a> 不要在 `if`、`while` 等代码块中声明函数。

    ```typescript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  - [13.3](#13.3) <a name="13.3"></a> 使用 rest 语法 `...` 代替 `arguments`。

    > Why？使用 `...` 能明确你要传入的参数。另外 rest 参数是一个真正的数组，而 `arguments` 是一个类数组。

    ```typescript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  - [13.4](#13.4) <a name="13.4"></a> 直接给函数的参数指定默认值，不要使用一个变化的函数参数。

    ```typescript
    // really bad
    function handleThings(opts) {
      // 不！我们不应该改变函数参数。
      // 更加糟糕: 如果参数 opts 是 false 的话，它就会被设定为一个对象。
      // 但这样的写法会造成一些 Bugs。
      //（译注：例如当 opts 被赋值为空字符串，opts 仍然会被下一行代码设定为一个空对象。）
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - [13.5](#13.5) <a name="13.5"></a> 直接给函数参数赋值时需要避免副作用。

    > Why？因为这样的写法让人感到很困惑。

    ```typescript
    let b = 1;
    // bad
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```
  - [13.6](#13.6) <a name="13.6"></a> 禁止对函数参数重新赋值。

  - [13.7](#13.7) <a name="13.7"></a> 返回 `Promise` 的函数明确用 `async` 标记。

  - [13.8](#13.8) <a name="13.8"></a> 不要使用函数来实现类，总是使用 `class` 来声明一个类和使用 `extends` 实现继承。

  - [13.9](#13.9) <a name="13.9"></a> 当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。

    > Why？因为箭头函数创造了新的一个 `this` 执行环境（译注：参考 [Arrow functions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 和 [ES6 arrow functions, syntax and lexical scoping](http://toddmotto.com/es6-arrow-functions-syntaxes-and-lexical-scoping/)），通常情况下都能满足你的需求，而且这样的写法更为简洁。

    > Why不？如果你有一个相当复杂的函数，你或许可以把逻辑部分转移到一个函数声明上。

    ```typescript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  - [13.10](#13.10) <a name="13.10"></a> 如果一个函数适合用一行写出并且只有一个参数，那就把花括号、圆括号和 `return` 都省略掉。如果不是，那就不要省略。

    > Why？语法糖。在链式调用中可读性很高。

    > Why不？当你打算回传一个对象的时候。

    ```typescript
    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].reduce((total, n) => {
      return total + n;
    }, 0);
    ```

  - [13.11](#13.11) <a name="13.11"></a> 函数（方法）及其参数总是使用 `camelCase`，当表示一个 *react* 组件时，使用 `PascalCase`。

  - [13.12](#13.12) <a name="13.12"></a> 函数的参数个数最多不要超过5个，建议保留在3个之内，当参数过多时，建议使用对象或数组传递参数。

**[⬆ 返回目录](#table-of-contents)**

<a name="classes"></a>
## 类

  - [14.1](#14.1) <a name="14.1"></a> 所有类使用 `PascalCase`，考虑一个文件一个类。

  - [14.2](#14.2) <a name="14.2"></a> 使用参数属性简化类的写法。

    ```typescript
    // bad
    class User {
      public id: number;
      public name: string;

      constructor(id: number, name: string) {
        this.id = id;
        this.name = name;
      }
    }

    // good
    class User {
      constructor(public id: number, public name: string) {}
    }
    ```
  - [14.3](#14.3) <a name="14.3"></a> 考虑对类成员进行排序。

    ```typescript
    class User {
      static a;
      static b() {}

      public c;
      protected d;
      private e;

      constructor() {}

      public f() {}
      protected g() {}
      private h() {}
    }
    ```


**[⬆ 返回目录](#table-of-contents)**

<a name="comparison"></a>
## 比较运算符和等号

  - [15.1](#15.1) <a name="15.1"></a> 永远使用严格等 `===` 和 `!==` 而不是 `==` 和 `!=`。

  - [15.2](#15.2) <a name="15.2"></a> 在条件语句中不要使用常量表达式。

    ```typescript
    // error
    if (true) {}
    ```

  - [15.3](#15.3) <a name="15.3"></a> 禁止不必要的 `else`。

    ```typescript
    // error
    function(flag: boolean) {
      if (flag) {
        return 'hello';
      } else {
        return 'world';
      }
    }

    // good
    function(flag: boolean) {
      if (flag) {
        return 'hello';
      }

      return 'world';
    }

    // this is also ok
    function(flag: boolean) {
      if (flag) {
        // do something
        // no return, no throw
      } else {
        // do other thing
      }
    }
    ```

  - [15.4](#15.4) <a name="15.4"></a> 先判断错误条件，提前退出。

    ```typescript
    // error
    function fn() {
      if (true) {
        // aaa...
        // bbb...
        // ccc...
      }
    }

    // good

    function fn() {
      if (false) {
        return;
      }

      // aaa...
      // bbb...
      // ccc...
    }
    ```
  - [15.5](#15.5) <a name="15.5"></a> 不要在条件语句中赋值。

  - [15.6](#15.6) <a name="15.6"></a> 不要与字面量布尔值比较。

  - [15.7](#15.7) <a name="15.7"></a> 使用严格的条件表达式。

    ```typescript
    let a: string;
    let b: number;

    // error
    if (!a) {}
    if (!b) {}

    // good
    if (a !== '') {}
    if (b !== 0) {}

    // null or undefined is ok
    const c = null;
    const d = undefined;
    if (!c) {}
    if (!d) {}
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="modules"></a>
## 模块

  - [14.1](#14.1) <a name="14.1"></a> 使用 `ES2015` 的模块系统而不是其他。

  - [14.2](#14.2) <a name="14.2"></a> 考虑使用命名导出代替默认导出。

  - [14.3](#14.3) <a name="14.3"></a> 导入顺序遵循如下规则。

    ```typescript
    import 'hammerjs';
    import * as Foo from 'foo';
    import React from 'react';
    import { a, b, c } from 'bar';

    import '../xxx';
    import * as Bar from '../bar';
    import Utils from '../utils';
    import {
      A,
      B,
      C,
    } from '../yyy';

    import './iife';
    import * as Baz from './baz';
    import xyz from './xyz';
    import z, { x, y } from './balabala';
    ```

**[⬆ 返回目录](#table-of-contents)**

<a name="comments"></a>
## 注释

  - [17.1](#17.1) <a name='17.1'></a> 使用 `/** ... */` 作为多行注释。中间的 `*` 号之后保留一个空格。

  - [17.2](#17.2) <a name='17.2'></a> 使用 `//` 作为单行注释。`//` 之后需要保留一个空格，如果注释和代码在同一行内，之前也需要保留一个空格。

  - [17.5](#17.5) <a name='17.5'></a> 对外输出的内容尽量添加注释。

**[⬆ 返回目录](#table-of-contents)**

<a name="references"></a>
## 参考

1. [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
2. [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
3. [JavaScript Standard Style](https://standardjs.com/index.html)
4. [Microsoft TypeScript Coding Guidelines](https://github.com/Microsoft/TypeScript/wiki/Coding-guidelines)
5. [Airbnb React/JSX Style Guide](https://github.com/airbnb/javascript/tree/master/react)
6. [Angular Style Guide](https://angular.io/docs/ts/latest/guide/style-guide.html)
7. [TSLint Rules](https://palantir.github.io/tslint/rules/)
8. [ESLint Rules](http://eslint.org/docs/rules/)
9. [JSDoc](http://usejsdoc.org/)

**[⬆ 返回目录](#table-of-contents)**
