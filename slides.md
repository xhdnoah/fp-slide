---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
title: 函数式编程入门
---

# 函数式编程入门

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Concepts

工程中的函数式编程大体上推崇以下原则

1. 📝 纯函数 pure function
2. 🎨 函数组合 function composition
3. 💣 避免副作用 avoid side effects
4. 🧑‍⚖️ 函数一等公民 first-class function
5. 📤 避免状态共享 immutability

<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>


---

# 纯函数
纯函数的输入输出数据流是显式的，函数与外界交换数据只有唯一渠道，即参数和返回值

纯函数的好处
- 无状态、线程安全，不需要线程同步
- 多个纯函数可以组合成纯函数
- 运行环境可以对纯函数的结果缓存

副作用：除了返回值外，对主调用函数产生附加的影响

- 修改全局变量
- 修改函数参数
- 向主调方终端、管道输出字符
- 改变外部存储信息


---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Array Operation

Use code snippets and get the highlighting directly![^1]

```js
const origin = [-1, 2, 3]
const squared2 = origin.map(n => n * n);
const positives = origin.filter(n => n > 0);
const sum = origin.reduce((acc, n) => acc + n, 0)
```

<arrow v-click="3" x1="400" y1="420" x2="230" y2="330" color="#564" width="3" arrowSize="1" />

[^1]: [Learn More](https://sli.dev/guide/syntax.html#line-highlighting)

<style>
.footnotes-sep {
  @apply mt-20 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>


---

# 控制反转

```java
public class TextEditor {

    private IocSpellChecker checker;

    public TextEditor(IocSpellChecker checker) {
        this.checker = checker;
    }
}

```

---

setTimeout 是有副作用的函数，可以用依赖注入来做动态替换
```js
function doSomething() {
	doFirstThing()
	setTimeout(doSecondThing, 60 * 60 * 1000)
}

function doSomething1(timeout) {
	doFirstThing()
	timeout(doSecondThing, 60 * 60 * 1000)
}

function test() {
	const myTimeout = (f, time) => f()
	doSomething1(myTimeout)
}

const halfTimeout = (f, time) => {
	setTimeout(f, time / 2)
}
doSomething1(halfTimeout)

```

---

# 组合

函数组合是将两个或更多的函数组合起来，产生一个新函数的过程

`compose` 将 `fn1(fn2(fn3(fn4(x))))` 这种嵌套调用改成 `compose(fn1, fn2, fn3, fn4)(x)` 通过 `reduce` 不断将右边的函数当成参数传入已组合好的函数中



```typescript
export default function compose(...funcs: Function[]) {
  if (funcs.length === 0) {
    return <T>(arg: T) => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce(
    (a, b) =>
      (...args: any) =>
        a(b(...args))
  )
}
```

---
class: px-20
---

### Continuation Passing Style

业务重点由返回值转移到了回调函数中

```js
function foo(x, bar) {
  return bar(x);
}
```

---
---

# Immutable
Persistent Data Structures

### Trees & Sharing

想象我们在诺亚方舟上有八个动物

![animal array](/animal-array.svg)

---
preload: false
---

我们将数组状的动物排列成树状态:

![animal tree](/animal-tree.svg)

---
---
### 路径复制

当我们想修改一个节点的时候，只需将即将被修改的节点路径上的所有节点克隆出新的
克隆而得的所有节点指向旧节点的指针必须被修改为指向新节点。这些修改会引起连锁更改，直达根节点。
新旧树的大部分结构得到复用，两棵树的结构被共享了

![new animal tree](/new-tree.svg)

---
layout: center
class: text-center
---

# Learn More

[CPS](https://matt.might.net/articles/by-example-continuation-passing-style/)
