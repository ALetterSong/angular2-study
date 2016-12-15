## [Angular2 高级文档 - 3]()

[Angular2 高级文档 - 1](ng2-advanced-guide.md)
[Angular2 高级文档 - 2](ng2-advanced-guide-2.md)

### 安全

- 最佳实践 。

- 防范跨站脚本 (XSS) 攻击 。

- 信任安全值 。

- HTTP 级别的漏洞 。

### 结构型指令

不同于传统应用每次浏览都从服务器取得整个页面。单页面应用中, Angular 将操纵 DOM 树。

- 学习什么是结构型 (structural) 指令

结构型指令通过添加和删除 DOM 元素来改变 DOM 的布局。

```
<div *ngIf="hero">{{hero}}</div>
<div *ngFor="let hero of heroes">{{hero}}</div>
<div [ngSwitch]="status">
  <template [ngSwitchCase]="'in-mission'">In Mission</template>
  <template [ngSwitchCase]="'ready'">Ready</template>
  <template ngSwitchDefault>Unknown</template>
</div>
```

- 研究 ngIf

ngIf 接受一个布尔值，并据此让一整块 DOM 树出现或消失。

```
<p *ngIf="condition">
  condition is true and ngIf is true.
</p>
<p *ngIf="!condition">
  condition is false and ngIf is false.
</p>
```

ngIf 指令并不会隐藏元素。元素将被移除,在它的位置上是空白的 `<script>` 标签。

- `<template>` 元素揭秘

[`<template>`标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/template)

Angular 把 `<template>` 标签及其内容替换成了一个空白的 `<script>` 标签。 这只是它的默认行为。

```
<div [ngSwitch]="status">
  <template [ngSwitchCase]="'in-mission'">In Mission</template>
  <template [ngSwitchCase]="'ready'">Ready</template>
  <template ngSwitchDefault>Unknown</template>
</div>
```

然而当使用 ngSwitch 时,这些 ngSwitch 的条件之一为真的时候， Angular 把模板的内容插入到了 DOM 中。

- 理解 `*ngFor` 中的星号 (*)

星号是一种“语法糖”。它简化了 ngIf 和 ngFor。

```
<!-- Examples (A) and (B) are the same -->
<!-- (A) *ngIf paragraph -->
<p *ngIf="condition">
  Our heroes are true!
</p>

<!-- (B) [ngIf] with template -->
<template [ngIf]="condition">
  <p>
    Our heroes are true!
  </p>
</template>
```

```
<!-- Examples (A) and (B) are the same -->

<!-- (A) *ngFor div -->
<div *ngFor="let hero of heroes">{{ hero }}</div>

<!-- (B) ngFor with template -->
<template ngFor let-hero [ngForOf]="heroes">
  <div>{{ hero }}</div>
</template>
```

- 写我们自己的结构型指令[Not Completed]


### 测试

### TypeScript 配置

### 从 1.x 升级

### Webpack 简介