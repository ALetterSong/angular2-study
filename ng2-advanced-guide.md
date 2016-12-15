## [Angular2 高级文档 - 1](https://angular.cn/docs/ts/latest/guide/ngmodule.html)

[Angular2 高级文档 - 2](ng2-advanced-guide-2.md)
[Angular2 高级文档 - 3](ng2-advanced-guide-3.md)

### Angular模块(NgModule)[Not Completed]

- Angular 模块化

Angular 模块是一个由 @NgModule 装饰器提供元数据的类

- 应用的根模块
```
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import
       { AppComponent }  from './app.component';

@NgModule({
  imports: [ BrowserModule ],
  declarations: [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```
- 引导根模块

通过即时（JiT）编译器动态引导

```
// The browser platform with a compiler
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

// The app module
import { AppModule } from './app.module';

// Compile and launch the module
platformBrowserDynamic().bootstrapModule(AppModule);
```

使用预编译器（ AoT - Ahead-Of-Time ）进行静态引导

```
// The browser platform without a compiler
import { platformBrowser } from '@angular/platform-browser';

// The app module factory produced by the static offline compiler
import { AppModuleNgFactory } from './app.module.ngfactory';

// Launch with the app module factory.
platformBrowser().bootstrapModuleFactory(AppModuleNgFactory);
```

- 声明指令和组件
```
  declarations: [
    AppComponent,
    HighlightDirective,
    TitleComponent,
  ],
```
- 服务提供商
```
providers: [ UserService ],
```
- 导入支持模块

- 解决冲突

我们可以通过创建特性模块来消除组件与指令的冲突。
- 特性模块
- 通过路由器 惰性加载模块
- 共享模块
- 核心模块
- 用 forRoot 配置核心服务
- 禁止重复导入 CoreModule
- NgModule 元数据的属性

### 动画 

进场与离场动画示例:

html:

```
 <ul>
      <li *ngFor="let hero of heroes"
          [@flyInOut]="'in'">
        {{hero.name}}
      </li>
    </ul>
    
```

component:

```
animations: [
  trigger('flyInOut', [
    state('in', style({transform: 'translateX(0)'})),
    transition('void => *', [
      style({transform: 'translateX(-100%)'}),
      animate(100)
    ]),
    transition('* => void', [
      animate(100, style({transform: 'translateX(100%)'}))
    ])
  ])
]
```


### 属性型指令 

- 指令概览

在 Angular 中有三种类型的指令：

**组件** - 拥有模板的指令
**结构型指令** - 通过添加和移除 DOM 元素改变 DOM 格局的指令(NgFor,NgIf)
**属性型指令** - 改变元素显示和行为的指令。(NgStyle)

- 创建简单的属性型指令
```
<p [style.background]="'lime'">I am green with envy!</p>
```
参见[模板语法](https://angular.cn/docs/ts/latest/guide/template-syntax.html#!#style-binding)

- 把这个属性型指令应用到模板中的元素

app/highlight.directive.ts
```
import { Directive, ElementRef, Input, Renderer } from '@angular/core';
@Directive({ selector: '[myHighlight]' })
export class HighlightDirective {
    constructor(el: ElementRef, renderer: Renderer) {
       renderer.setElementStyle(el.nativeElement, 'backgroundColor', 'yellow');
    }
}
```

使用:
p元素就是这个属性型指令的宿主(host)

```
<h1>My First Attribute Directive</h1>
<p myHighlight>Highlight me!</p>
```

Angular 在 p 元素上发现了一个 myHighlight 属性。 然后它创建了一个 HighlightDirective 类的实例，并把所在元素的引用注入到了指令的构造函数中。 
在构造函数中，我们把 p 元素的背景设置为了黄色。

- 响应用户引发的事件

@HostListener 装饰器引用的是我们这个属性型指令的宿主元素，在这个例子中就是 p
```
import { Directive, ElementRef, HostListener, Input, Renderer } from '@angular/core';
@Directive({
  selector: '[myHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef, private renderer: Renderer) { }
  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }
  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }
  private highlight(color: string) {
    this.renderer.setElementStyle(this.el.nativeElement, 'backgroundColor', color);
  }
}
```

我们也可以直接给 DOM 元素挂载事件监听器,但必须销毁监听器,否则会导致内存泄漏,同时,我们也应该避免与 DOM API 打交道。

- 使用数据绑定把值传到指令中

```
export class HighlightDirective {
  private _defaultColor = 'red';

  constructor(private el: ElementRef, private renderer: Renderer) { }

  @Input('myHighlight') highlightColor: string;

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || this._defaultColor);
  }
  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.renderer.setElementStyle(this.el.nativeElement, 'backgroundColor', color);
  }
}
```

- 绑定第二个属性

```
@Input() set defaultColor(colorName: string){
  this._defaultColor = colorName || this._defaultColor;
}
```

使用:

```
<p [myHighlight]="color" [defaultColor]="'violet'">
  Highlight me too!
</p>

```

### 浏览器支持

Chrome, Firefox, IE 9+, Safari 7+, iOS 7+, Android 4.1+

[详细支持列表](https://angular.cn/docs/ts/latest/guide/browser-support.html)



### 组件样式 

- 使用组件样式

在组件的元数据中设置 styles 属性,它们只会应用在组件自身的模板中。

```
@Component({
  selector: 'hero-app',
  template: `
    <h1>Tour of Heroes</h1>
    <hero-app-main [hero]=hero></hero-app-main>`,
  styles: ['h1 { font-weight: normal; }']
})
export class HeroAppComponent {
/* . . . */
}
```
- 特殊的选择器

组件样式有一些从[Shadow DOM](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components/%E5%BD%B1%E5%AD%90_DOM)引入的特殊选择器
*而 Shadow DOM 仅支持 Chrome 53+*

例如 **:host** 伪类选择器，用来选择组件宿主元素中的元素 ( 相对于组件模板内部的元素 ) 
```
:host {
  display: block;
  border: 1px solid black;
}
```

- 把样式加载进组件

1.内联在模板的 HTML 中
2.设置 styles 或 styleUrls 元数据
3.通过 CSS 文件导入

[Webpack 参考](https://angular.cn/docs/ts/latest/guide/component-styles.html#!#loading-styles)

- 控制视图的包装模式：仿真 (Emulated) 、原生 (Native) 或无 (None)

**Emulated**: 只进不出，全局样式能进来，组件样式出不去
**Native**: 不进不出，没有样式能进来，组件样式出不去([不建议使用](http://caniuse.com/#feat=shadowdom))
**None**: Angular 会把 CSS 添加到全局样式中,而不会应用隔离和保护等规则。

```
// warning: few browsers support shadow DOM encapsulation at this time
encapsulation: ViewEncapsulation.Native
```

### 多级注入器 [Not Completed]
