
> [原文地址 Angular 2.x 从0到1](https://github.com/wpcfan/awesome-tutorials/tree/master/angular2/ng2-tut#angular-2x-%E4%BB%8E0%E5%88%B01)


# 基础概念

## 什么是模块

简单来说模块就是提供相对独立的功能块，每块模块聚焦于一个特定业务领域。Angular内建的很多库都是以模块形式提供的。比如，`FormsModule`封装了表单处理，`HttpModule`封装了http处理等。
  
每个angular应用至少有一个模块类——**根模块**,我们将引导**根模块**来启动应用。按照约定，**根模块**的类名叫做`AppModule`被放在`app.module.ts`文件中。此例子中的**根模块**位于`src\app\app.module.ts`
```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';
import { LoginComponent } from './login/login.component';

@NgModule({
  declarations: [
    AppComponent,
    LoginComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```
  
`@NgModule`是装饰器，用来为模块定义元数据。`declarations`列出应用中的顶层组件，包括引导性的`AppComponent`及用命令行`ng g c login`新建的`LoginComponent`。在module中声明的组件在module范围内都可以直接使用，也就是说在同一module中任何Component都可以在其模板文件中直接使用声明的组件。例如：在`AppCompont`模板中加入的`<app-login></app-login>`
  
imports 引入了3个辅助模块：
* `BrowserModule` —— 提供了运行在浏览器中应用所需要的关键服务(Service)和指令(Directive)，这个模块所有需要在浏览器中跑的应用都是必须引用的
* `FormsModule` —— 提供了表单处理和双向绑定等服务的指令
* `HttpModule` —— 提供Http请求和响应的服务
  
`providers`会列出在此模块中“注入”的服务(Service)
  
`bootstrap`则指明哪个组件为引导性组件（本实例中的AppComponent）
  
当Angular引导应用时，它会在DOM中渲染这个引导性的组件，并把结果放进`index.html`中该组件的元素标签中（即`<app-root></app-root>`）
  
## 引导过程
  
Angular2通过在`main.ts`中引导`AppModule`来启动应用。针对不同的平台，Angular提供了很多引导选项。下面的代码是通过即时(JiT)编译器动态引导，一般在进行开发调试时，默认采用这种方式：
```typescript
//main.ts
import './polyfills.ts';

// 连同Angular编译器一起发布到浏览器
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { enableProdMode } from '@angular/core';
import { environment } from './environments/environment';
import { AppModule } from './app/';

if (environment.production) {
  enableProdMode();
}
//Angular编译器在浏览器中编译并引导该应用
platformBrowserDynamic().bootstrapModule(AppModule);
```
   
另一种方式是使用预编译器（AoT - Ahead - Of - Time）进行静态引导，静态方案可以生成更小、启动更快的应用，**建议优先使用**。特别是在移动端或高延迟网络下。
使用static选项，Angular编译器作为构建流程的一部分提前运行，生成一组类工厂。它们的核心就是AppModuleNgFactory.引导预编译的AppModuleNgFactory语法与动态引导AppModule类的方式非常相似
```typescript
// 不把编译器发布到浏览器
import { platformBrowser } from '@angular/platform-browser';

// 静态编译器会生成一个AppModule的工厂AppModuleNgFactory
import { AppModuleNgFactory } from './app.module.ngfactory';

// 引导AppModuleNgFactory
platformBrowser().bootstrapModuleFactory(AppModuleNgFactory);
```








