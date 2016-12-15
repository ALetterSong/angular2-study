## [Angular2 开发指南](https://angular.cn/docs/ts/latest/guide/)

[Angular2 高级文档](docs/ng2-advanced-guide.md)

[Angular2 烹饪宝典](docs/ng2-cookbook.md)

### 架构:

- 模块 (Modules)
Angular 应用是模块化的，并且 Angular 有自己的模块系统，它被称为 Angular 模块 或 NgModules 

- 组件 (Components)
组件 负责控制屏幕上的一小块地方，我们称之为 视图 。

- 模板 (Templates)
我们通过组件的自带的 模板 来定义视图。模板以 HTML 形式存在，用来告诉 Angular 如何渲染组件 ( 视图 ) 。

- 元数据 (Metadata)
元数据告诉 Angular 如何处理一个类。在 TypeScript 中，我们用 装饰器 (decorator)(如:`@Component`,`@Injectable`,`@Input`,`@Output`) 来附加元数据。

```
@Component({
  moduleId: module.id,
  selector:    'hero-list',
  templateUrl: 'hero-list.component.html',
  providers:  [ HeroService ]
})
export class HeroListComponent implements OnInit {
/* . . . */
}
```
`moduleId`为与模块相关的 URL（如templateUrl）提供基地址

[相对于组件的路径](https://angular.cn/docs/ts/latest/cookbook/component-relative-paths.html)
[CommonJS规范](http://javascript.ruanyifeng.com/nodejs/module.html)
- 数据绑定 (Data Binding)
Angular 支持 数据绑定 ，一种让模板的各部分与组件的各部分相互合作的机制。 我们往模板 HTML 中添加绑定标记，来告诉 Angular 如何连接两者。

- 指令 (Directives)
Angular 模板是 动态的 。当 Angular 渲染它们时，它会根据 指令 提供的操作指南对 DOM 进行修改。

- 服务 (Services)
服务 分为很多种，包括：值、函数，以及应用所需的特性。

- 依赖注入 (Dependency Injection)
“依赖注入”是提供类的新实例的一种方式，还负责处理好类所需的全部依赖。大多数依赖都是服务。 Angular 也使用依赖注入提供我们需要的组件以及这些组件所需的服务。

### 显示数据

- 通过插值表达式显示组件的属性
```
{{myHero}}
```

- 通过 NgFor 显示数组型属性
[TypeScript-for..of](http://www.tslang.cn/docs/handbook/iterators-and-generators.html)
[ES6-for..of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)
```
<li *ngFor="let hero of heroes">
   {{ hero }}
</li>
```
- 通过 NgIf 实现按条件显示
```
<p *ngIf="heroes.length > 3">There are many heroes!</p>
```

### 用户输入

- 绑定到用户输入事件
我们只要把 DOM 事件的名字包裹在圆括号中，然后用一个放在引号中的“模板语句”对它赋值就可以了。
```
<button (click)="onClickMe()">Click me!</button>
```

- 通过 $event 对象取得用户输入
```
template: `
  <input (keyup)="onKey($event)">
  <p>{{values}}</p>
`
```
```
export class KeyUpComponent_v1 {
  values = '';

  // with strong typing
  onKey(event: KeyboardEvent) {
    this.values += (<HTMLInputElement>event.target).value + ' | ';
  }
}
```

- 从一个模板引用变量中获得用户输入

Angular 有一个叫做 模板引用变量 的语法特性。 这些变量给了我们直接访问元素的能力。 通过在标识符前加上井号 (#) ，我们就能定义一个模板引用变量。

```
@Component({
  selector: 'key-up2',
  template: `
    <input #box (keyup)="onKey(box.value)">
    <p>{{values}}</p>
  `
})
export class KeyUpComponent_v2 {
  values = '';
  onKey(value: string) {
    this.values += value + ' | ';
  }
}
```
- 按键事件过滤 ( 通过 key.enter)

在这个例子中，更新代码是写在事件绑定语句中的。但在实践中更好的方式是把更新代码放到组件中。

```
@Component({
  selector: 'key-up3',
  template: `
    <input #box (keyup.enter)="values=box.value">
    <p>{{values}}</p>
  `
})
export class KeyUpComponent_v3 {
  values = '';
}
```
- blur( 失去焦点 ) 事件
```
@Component({
  selector: 'key-up4',
  template: `
    <input #box
      (keyup.enter)="values=box.value"
      (blur)="values=box.value">

    <p>{{values}}</p>
  `
})
export class KeyUpComponent_v4 {
  values = '';
}
```

### 表单

- 使用组件和模板构建一个 Angular 表单

- 使用 [(ngModel)] 语法实现双向数据绑定，以便于读取和写入输入控件的值

```
<input type="text"  class="form-control" id="name"
       required
       [(ngModel)]="model.name" name="name">
```
- 结合表单来使用 ngModel ，能让我们跟踪状态的变化并对表单控件做验证

- 使用特殊的 CSS 类来跟踪控件状态，并提供强烈的视觉反馈

- 向用户显示有效性验证的错误提示，以及禁用 / 启用表单控件

- 通过模板引用变量，在控件之间共享信息

### 依赖注入
让一个类从外部源中获得它的依赖，而不必亲自创建它们。

- 为什么依赖注入(Dependency injection)？

without DI

```
public engine: Engine;
public tires: Tires;
public description = 'No DI';

constructor() {
  this.engine = new Engine();
  this.tires = new Tires();
}
```

with DI
```
public description = 'DI';

constructor(public engine: Engine, public tires: Tires) { }
```

- Angular 依赖注入

1.在 Angular 2 中，服务只是一个类。 除非我们把它注册进一个 Angular 注入器，否则它没有任何特别之处。

2.在 NgModule 或者组件中注册提供商

3.构造函数类型、 @Component 装饰器、父级的 providers 信息这三个合起来，一起告诉 Angular 的注入器，在任何时候新建一个新的 ExampleComponent 的时候，注入一个 ExampleService 的实例

[我该把“全应用级”提供商加到根模块 AppModule 还是根组件 AppComponent ？](https://angular.cn/docs/ts/latest/cookbook/ngmodule-faq.html#!#q-root-component-or-module)

4.服务需要别的服务
```
import { Injectable } from '@angular/core';
import { HEROES }     from './mock-heroes';
import { Logger }     from '../logger.service';
@Injectable()
export class HeroService {
  constructor(private logger: Logger) {  }
  getHeroes() {
    this.logger.log('Getting heroes ...');
    return HEROES;
  }
}
```
5.@Injectable() 标志着一个类可以被一个注入器实例化。通常来讲，在试图实例化一个没有被标识为 @Injectable() 的类时候，注入器将会报告错误。

- 注入器提供商

提供商(provider)提供所需依赖值的一个具体的运行期版本。 注入器(injector)依靠提供商们来创建服务的实例，它会被注入器注入(inject)到组件或其它服务中(components and other services)。

有时我们需要动态创建这个依赖值，因为它所需要的信息我们直到最后一刻才能确定, 这种情况下, 使用工厂提供商。 

hero.service.provider.ts
```
let heroServiceFactory = (logger: Logger, userService: UserService) => {
  return new HeroService(logger, userService.user.isAuthorized);
};

export let heroServiceProvider =
  { provide: HeroService,
    // useFactory 字段告诉 Angular ：这个提供商是一个工厂方法，它的实现是 heroServiceFactory
    useFactory: heroServiceFactory,
    // deps 属性是一个 提供商令牌 数组。 Logger 和 UserService 类作为它们自身提供商的令牌。 注入器解析这些令牌，并且把相应的服务注入到工厂函数中相应的参数中去。
    deps: [Logger, UserService]
  };
```

heroes.component.ts

```
import { Component }          from '@angular/core';
import { heroServiceProvider } from './hero.service.provider';
@Component({
  selector: 'my-heroes',
  template: `
  <h2>Heroes</h2>
  <hero-list></hero-list>
  `,
  providers: [heroServiceProvider]
})
export class HeroesComponent { }
```

- 依赖注入令牌[Not Completed]

### 模板语法[Not Completed]

- HTML
```
<h1>My First Angular App</h1>
```

- 插值表达式
```
<p>My current hero is {{currentHero.firstName}}</p>

<!-- "The sum of 1 + 1 is 2" -->
<p>The sum of 1 + 1 is {{1 + 1}}</p>
```

- 模板表达式

禁止可能引发副作用的表达式: =,+=,-=,new,++,--

不支持位运算 | 和 &

模板表达式不能引用全局命名空间中的任何东西,它们被局限于只能访问来自表达式上下文中的成员

模板表达式请遵循下列指南:
1.没有可见的副作用: 模板表达式除了目标属性的值以外，不应该改变应用的任何状态。这条规则是 Angular “单向数据流”策略的基础。
2.执行迅速: 表达式应该快速结束
3.简单
4.幂等性: 不管调用几次, 一个幂等的表达式应该总是返回完全相同的东西

- 模板语句


- 绑定语法
- 属性绑定
- HTML 属性、 class 和 style 绑定
- 事件绑定
- 使用 NgModel 进行双向数据绑定

- 内置指令
NgClass
NgStyle
NgIf
NgSwitch
NgFor
- * 与 <template>
- 模板引用变量
- 输入输出属性
- 模板表达式操作符
管道
“安全导航操作符” (?.)

### Angular 小抄
[cheatsheet](https://angular.cn/docs/ts/latest/guide/cheatsheet.html)

### 风格指南
[style-guide](https://angular.cn/docs/ts/latest/guide/style-guide.html)

### 词汇表
[glossary](https://angular.cn/docs/ts/latest/guide/glossary.html)