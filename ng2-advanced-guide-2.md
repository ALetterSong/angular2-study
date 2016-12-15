## [Angular2 高级文档 - 2]()

[Angular2 高级文档 - 1](ng2-advanced-guide.md)
[Angular2 高级文档 - 3](ng2-advanced-guide-3.md)

### HTTP 客户端

HTTP 是浏览器和服务器之间通讯的主要协议。
现代浏览器支持两种基于 HTTP 的 API ： XMLHttpRequest (XHR) 和 JSONP 。

##### 英雄指南范例的 HTTP 客户端

##### 用 http.get 获取数据

RxJS 库
启用 RxJS 操作符
##### 处理响应对象

##### 总是处理错误

##### 把数据发送到服务器

使用承诺 (Promise) 来取代可观察对象 (Observable)
##### 跨域请求： Wikipedia 例子

设置查询参数
限制搜索词输入频率
##### 防止跨站请求伪造

### 生命周期钩子

### Npm 包

### 管道

```
import { Component } from '@angular/core';

@Component({
  selector: 'hero-birthday',
  template: `<p>The hero's birthday is {{ birthday | date }}</p>`
})
export class HeroBirthdayComponent {
  birthday = new Date(1988, 3, 15); // April 15, 1988
}
```

在这个插值表达式中，我们让组件的 birthday 值通过管道操作符 ( | ) 流动到右侧的 Date 管道函数中。所有管道都会用这种方式工作。

### 路由与导航
##### 设置 页面的基地址（ base href ）

index.html
```
<base href="/">
```

##### 从 路由库 中导入

app.module.ts
```
import { RouterModule }   from '@angular/router';
```

##### 配置路由器
app.module.ts
```
@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    RouterModule.forRoot([
      { path: 'hero/:id', component: HeroDetailComponent },
      { path: 'crisis-center', component: CrisisListComponent },
      {
        path: 'heroes',
        component: HeroListComponent,
        data: {
          title: 'Heroes List'
        }
      },
      { path: '', component: HomeComponent },
      { path: '**', component: PageNotFoundComponent }
    ])
  ],
  declarations: [
    AppComponent,
    HeroListComponent,
    HeroDetailComponent,
    CrisisListComponent,
    PageNotFoundComponent
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule {
}
```
##### 路由插座
```
<!-- Routed views go here -->
<router-outlet></router-outlet>
```
##### 路由器链接

RouterLinkActive 指令，用于在相关的 RouterLink 被激活时为所在元素添加或移除 CSS 类。 该指令可以直接添加到该元素上，也可以添加到其父元素上。

```
template: `
  <h1>Angular Router</h1>
  <nav>
    <a routerLink="/crisis-center" routerLinkActive="active">Crisis Center</a>
    <a routerLink="/heroes" routerLinkActive="active">Heroes</a>
  </nav>
  <router-outlet></router-outlet>
`
```
##### 路由器状态

我们可以在应用中的任何地方用 Router 服务及其 routerState 属性来访问当前的 RouterState 值。

##### 重构路由到 路由模块

app-routing.module.ts
```
import { NgModule }     from '@angular/core';
import { RouterModule } from '@angular/router';

import { CrisisListComponent }  from './crisis-list.component';
import { HeroListComponent }    from './hero-list.component';

@NgModule({
  imports: [
    RouterModule.forRoot([
      { path: 'crisis-center', component: CrisisListComponent },
      { path: 'heroes', component: HeroListComponent }
    ])
  ],
  exports: [
    RouterModule
  ]
})
export class AppRoutingModule {}
```
##### 推动路由器导航的 链接参数数组 ，

##### 在 程序的控制下 进行导航

##### 从 当前路由获取信息

##### 为路由组件添加转场 动画

##### 相对当前 URL 进行导航

##### 利用 [router-link-active 指令 ] 切换 CSS 类 (#router-link-active)

##### 用 路由参数 把重要信息嵌入 URL

##### 在 可选路由参数 中提供非关键信息


##### 在“特性分区”下添加 子路由

##### 不借助组件 对子路由进行分组

##### 从一个路由 重定向 到另一个路由

##### 借助 守卫函数 确认或取消导航

用 CanActivate 阻止导航进某路由

用 CanActivateChild 阻止导航进某子路由

用 CanDeactivate 阻止离开当前路由的导航

用 Resolve 在路由激活之前预先获取数据

用 CanLoad 阻止异步路由

##### 用 查询参数 提供跨路由的可选信息

##### 使用 fragment 跳转到其它元素

##### 异步 加载特性分区

在导航时 预加载特性分区

使用 自定义策略 来只预加载指定分区

选择 "HTML5" 或 "hash"URL 风格

