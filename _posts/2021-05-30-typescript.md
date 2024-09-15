## 1. typescript 带来了什么优势？

1. 在开发过程中，发现潜在问题，会有类型提示，参数检测等。
2. 更加友好的编辑器自动提示？
3. 代码语义更加清晰易懂

安装 typescript ：`yarn add typescript -D`

初始化 typescript ，生成 tsconfig.json 文件：`yarn tsc --init`

添加直接运行 ts 文件的第三方包：`yarn add ts-node @types/node -D`

运行某个 ts 文件`yarn ts-node demo.ts`

### 2. TypeScript 的类型

1. 基础类型：boolean,string,number,undefined,void,null,symbol

```jsx
// 无法进行类型推断
let count;
count = 1;

// 可以进行类型推断
let count = 1;
```

2.  对象类型：{},function,[],class

```jsx
// 声明函数的方法：

const toNumber = (str: string): number => {
  return parseInt(str, 10);
};

const toNum: (str: string) => number = (str) => {
  return parseInt(str);
};
// 冒号（：）后面跟的是类型，等号（=）后面跟的是具体实现！

// 还有Date类型
const date = new Date();
```

3.  类型注解：我们手动告诉 TS ，声明的变量是什么类型

```jsx

```

4.  类型推断：TS 会自动尝试去分析变量的类型，如果 TS 无法自动推断类型，那么需要类型注解。

```jsx
const add = (a: number, b: number) => {
  return a + b;
};
const result = add(10, 20);

// 对于返回值的类型，虽然TS能够进行类型推断，但是最好也要写，因为可能出现如下情况！
const add = (a: number, b: number) => {
  // 可能代码写错了，这样就，类型推断出的返回值就是string类型的
  return a + b + "string";
};
```

5.  函数的定义

```jsx
function add() {}
const sub = function () {};
const mul = () => {};
```

5.1 函数返回值为 `void` ,`never`

```jsx
// 返回值为void
const voidFunc = (): void => {
  // console.log(`this is a void function`)
};
// 返回值是never
const errorEmitter = (): never => {
  throw new Error(`this is a error`);
};
// 解构赋值
const add = ({ first, second }: { first: number, second: number }): number => {
  return first + second;
};
add({ first: 1, second: 2 });
```

### 3. 数组和元组的类型注解

3.1 `数组`

```jsx
const arr: (number | string)[] = [1, "2", 3];

const objArray: { name: string, age: number }[] = [{ name: "ts", age: 5 }];

// type alias
type User = { name: string, age: number };

// 与 class 的集成
class Person {
  name: string;
  age: number;
  address: string;
}

const objArr: Person[] = [
  { name: "ts", age: 5, address: "China" }, // 1. 数据结构和Person保持一致就行
  new Person(),
];
```

3.2 `元组`

```jsx
const tuple: [string, number] = ["ts", 4];
```

### 4. `interface` 和 `type`

```jsx
interface IPerson {
  name: string;
  [propName: string]: any;
}

type TPerson = {
  // 1. readonly name: string;
  name: string,
};

// 2. type 能实现基础类型的定义，interface不行，比如说下面👇：
type TBase = string | boolean;

const getPersonName = (person: TPerson): string => {
  return person.name;
};

const setPersonName = (person: IPerson, name: string): void => {
  person.name = name;
};

const person = {
  name: "js",
  address: "China",
};

// 3. 如果以字面量形式，TS会进行强校验
getPersonName({
  name: "js",
  address: "China",
});

// 4. 如果是使用了一个变量进行缓存，那么TS就不会进行强校验！这个和对象字面量直接赋值有关
getPersonName(person);
// 如果需要解决这个问题在interface中添加这个属性 [propName: string]: any;
interface IPerson {
  name: string;
  [propName: string]: any;
  say(): string; // 也可以添加一个方法
}

setPersonName(person, "js");
```

**能用 interface 实现的，就用 interface 实现，实在不行的，就用 type**

4.1. 使用 interface 定义函数类型,还有索引类型。

```jsx
interface ISayHi {
  (word: string): string;
}

const sayHi: ISayHi = (word) => {
  return `hello ${word}`;
};
```

### 5. 类

```jsx
class Persons {
  name = "ts";
  getName() {
    return this.name;
  }
}

class Teacher extends Persons {
  getTeacherName() {
    return `teacher`;
  }
  getName() {
    // 重写父类的方法，那么如果我就是想返回父类的方法呢？
    // 使用super调用父类的方法
    // return super.getName();
    return `js`;
  }
}

const teacher = new Teacher();
// console.log(`getTeacherName`, teacher.getTeacherName());
// console.log(`getName`, teacher.getName());
console.log(`getName`, teacher.getName());
```

5.1 访问修饰符

- `public`:允许在自己类的内部和外部都能被访问(默认);
- `private`:只允许在自己类的内部被访问;
- `protected`:允许在自己类的内部以及继承的子类中被访问;

```jsx
class Man {
  public name = "ts";
  private age = 10;
  protected address = "China";
  getName() {
    return this.name;
  }
  getAge() {
    return this.age;
  }
}

class Me extends Man {
  getAddress() {
    this.address; // protected
  }
}

const man = new Man();
console.log(`man:`, man);
```

5.2 类的构造函数： `constructor`

```jsx
class Parent {
  public name: string = "ts";
  constructor(name: string = "ts") {
    this.name = name;
  }
}
```

在构造函数中传参，还可以有以下的写法：

```jsx
class Parent {
  constructor(public name: string = "ts") {
    // .....
  }
}
```

5.3 类的继承：`super`

```tsx
class Parent {
  constructor(public name: string = "ts") {}
}

class Child extends Parent {
  // 继承了 Parent类，如果在子类中调用了constructor
  constructor() {
    super(); // 就一定要在constructor中调用super
  }
}
```

5.4 类的静态属性：`getter` 和 `setter`

```jsx
class Human {
  constructor(private _name: string) {}
  get name() {
    return `get ${this._name}`;
  }
  set name(name: string) {
    this._name = `set ${name}`;
  }
}

const human = new Human(`ts`);
console.log(human.name);

human.name = "js";
console.log("set", human.name);
```

单例模式

```tsx
class Singleton {
  private constructor() {}
  private static instance: Singleton;

  static getInstance() {
    if (this.instance) return this.instance;
    this.instance = new Singleton();
    return this.instance;
  }
}

const single = Singleton.getInstance();
const singleTwo = Singleton.getInstance();
console.log(`isSame:`, singleTwo === single);
```

## 6. `tsconfig.json`文件的配置

运行`tsc`命令的时候，默认会读取项目目录下的`tsconfig.json`文件（如果这个文件存在），如果指定运行某个`.ts`文件，则不会读取配置文件。但是`ts-node`会读取这个配置文件。

## 7. 类型保护

```tsx
// 类型保护的方式
interface Bird {
  fly: boolean;
  sing: () => {};
}

interface Dog {
  fly: boolean;
  bark: () => {};
}

// 1. 采用类型断言的形式
function trainAnimal(animal: Bird | Dog) {
  //  animal.fly() 目前只能确定anima上只有fly属性
  (animal as Bird).sing();
  (animal as Dog).bark();
}

// 2. 采用 in 语法来进行类型保护
function trainAnimalAgain(animal: Bird | Dog) {
  if ("bark" in animal) {
    animal.bark();
  }
}
```

## 8. 枚举类型`enum`

```tsx
enum NETWORK_STATUS {
  offline,
  online = 3,
  delete,
}
// 可以反查NETWORK_STATUS[0] 就是 offline
console.log(`:>>>>: NETWORK_STATUS`, NETWORK_STATUS);
// const NETWORK_STATUS = {
//   offline: 0,
//   online: 1,
//   delete: 2,
// };

function getNetworkStatus(status: number) {
  switch (status) {
    case NETWORK_STATUS.offline:
      return "offline";
    case NETWORK_STATUS.online:
      return "online";
    default:
      return "delete";
  }
}

const network = getNetworkStatus(NETWORK_STATUS.online);
console.log(`:>>>>: network`, network);
```

## 9. 泛型 `generics`

### 9.1 函数中的泛型

```jsx
// 泛型 generics泛指的类型
function join(first: string | number, second: number | string) {
  return `${first} join ${second}`;
}
join("1", "3");
// 假设现在，我要求当join函数的第一个参数的类型是string的时候，
// 第二个也必须是string，那该怎么办？
```

我们需要将这个`join`函数进行改造一下：

```tsx
function join<T, K>(first: T, second: K) {
  return `${first} join ${second}`;
}
// 只有你用的时候，才知道类型
join<number, string>(1, "3");
```

或者接收一个数组

```tsx
function array<P>(params: Array<P>): P {
  return params[0];
}

array<string>(["map"]);
```

### 9.2 类中的泛型

```jsx
class DataManager {
  constructor(private data: string[]) {}
  getItem(index: number): string {
    return this.data[index];
  }
}

const data = new DataManager(["1"]);
data.getItem(0);
```

使用泛型改造如下：

```tsx
class DataManager<T> {
  constructor(private data: T[]) {}
  getItem(index: number): T {
    return this.data[index];
  }
}

const data = new DataManager<number>([1]);
data.getItem(0);
```

泛型的继承：

```tsx
interface Item {
  name: string;
}

class DataManager<T extends Item> {
  constructor(private data: T[]) {}
  getItem(index: number): string {
    // 假如我现在要求每个T类型的实例，必须还要求有name属性，那么该如何处理？
    // 继承一个Item，实例化的时候，必须要有个name属性
    return this.data[index].name;
  }
}

const data = new DataManager([
  {
    name: "ts",
  },
]);
```

### 9.3 泛型中`keyof`语法的使用

```tsx
interface ITeacher {
  name: string;
  age: number;
  gender: string;
  // [propName: string]: any;
}

class Teachers {
  constructor(private info: ITeacher) {}
  getInfo(key: string) {
    // 怎么保证传入的这个key一定是name，age，gender中的一种？
    // 假设我调用getInfo的时候，传入的是一个'address'的字符串,那么返回值是什么类型的呢？
    // 主要问题是这个key的值不确定
    return this.info[key];
  }
}

const teach = new Teachers({
  name: "ts",
  age: 19,
  gender: "male",
});
const result = teach.getInfo("name");
```

可以每个 key 判断一下:

```tsx
class Teachers {
  constructor(private info: ITeacher) {}
  getInfo(key: string) {
    if (key === "name" || key === "age" || key === "gender") {
      return this.info[key];
    }
  }
}
```

但是，在调用的时候，`teach.getInfo`函数的类型却是`string`| `number`| `undefined`，这个是

`typescript`给出的类型推断，但是这个并不准确，是否有其他更好的解决办法呢？使用`const result = teach.getInfo("name") as string;` ？但是这个并不是一个最优的解法！

```tsx
const teach = new Teachers({
  name: "ts",
  age: 19,
  gender: "male",
});
const result = teach.getInfo("name");
```

使用`keyof`解决这个问题，`keyof`的原理其实就是循环遍历`ITeacher`上的每个属性，并且泛型`T`继承了每个属性!

```tsx
class Teachers {
  constructor(private info: ITeacher) {}
  getInfo<T extends keyof ITeacher>(key: T) {
    return this.info[key];
  }
}

const teach = new Teachers({
  name: "ts",
  age: 19,
  gender: "male",
});
const result = teach.getInfo("age");
```

## 10. 命名空间 `namespace`

## 11. 自定义全局类型声明文件
