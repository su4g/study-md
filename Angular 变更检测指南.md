### Angular 变更检测指南

+ **什么是变更检测？**

  变更检测作用于视图和模型绑定的数据的更新，当检测到模型中绑定的值发生改变时，就把数据同步到视图上。

  通俗的说就是页面和逻辑处理的同步，例如在 `hero.component.html` 中使用[插值](https://angular.cn/guide/interpolation)绑定属性 `name`。

  在线 `Demo` 地址 https://stackblitz.com/edit/angular-ivy-jndrop?file=src/app/heroPush.component.ts

  ```typescript
  import { Component } from '@angular/core';
  
  @Component({
    selector: 'hero',
    template: `
      <h1>hero works {{ name }}!</h1>        // View 视图 {{name}}
    `,
    styles: [`h1 {font-family: Lato;}`]
  })
  export class HeroComponent {
    name: string = '';                      // module 模型 name = ''
  
    ngOnInit(): void {
      this.name = 'Super man';              // module data change => name = 'Super man'
    }
  }
  ```

    `name` 属性设置的默认值为空，但在组件初始化时进行了修改，因此当模型中数据 ( `name` ) 修改为 `Super man` 时，Angular 执行了变更检测（ DOM被新数据更新 ）。

  *Angular 变更检测原理可了解  [NgZone](https://angular.cn/guide/zone) 和 [zone.js](https://github.com/angular/zone.js)*

  ![image-20210823173715763](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210823173715763.png)

+ [变更检测策略](https://angular.cn/api/core/ChangeDetectionStrategy)

  Angular 提供两种 ( `OnPush` 和 `Default` ) 变更检测策略，他们所对应的基本规则为：

  `OnPush`      -   禁用自动执行变更检测

  `Default`    -   自动执行变更检测

  `Defalut` 为每个组件的默认状态，开发者无需特别设定，在上述 `hero` 组件的示例中，英雄组件运行在此状态下，当模型数据变化之后，自动执行了变更检测。

  `OnPush` 模式：

  设置 `OnPush` 需要在组件装饰器 `@Component` 中添加 

  `changeDetection: ChangeDetectionStrategy.OnPush`

  添加组件 `heroPush` 并设置变更检测策略为 `OnPush`

  ```typescript
  import {Component,ChangeDetectionStrategy,ChangeDetectorRef} from '@angular/core';
  
  @Component({
    selector: 'hero-push',
    template: `<h1>hero-push works {{ name }}!</h1>`,
    styles: [`h1 {font-family: Lato;}`],
    changeDetection: ChangeDetectionStrategy.OnPush   // 设置为 OnPush 模式
  })
  export class HeroPushComponent {
    name: string = '';
    constructor(private $cdr: ChangeDetectorRef) {}
    ngOnInit(): void {
      setTimeout(() => {
        this.name = 'Super man';  // 修改数据
        console.log('name change ,but do not detect');
        // this.$cdr.detectChanges(); 手动执行变更检测，暂时注释不执行
      }, 1000);
    }
  }
  ```

  当数据被异步 ( `setTimeout` ) 修改时并未刷新视图。

  ![image-20210823173944959](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210823173944959.png)

  **值得注意的是，在演示中，数据的更新被放置在异步 `setTimeout` 中执行，因为能够触发变更检测的情况有以下四种 :**

  ① 事件触发 `MouseEvent` 、`Keyup` 等；

  ② `@Input` 数据发生变化时（ `OnPush` 模式引用类型数据修改堆内存不触发 ）；

  ③ 定时器 `setTimeout` 、`setInterval` ；

  ④ `http` 请求 ;

  当组件在 `defalut` 模式下时，以上所有情况 Angular 都会自动执行变更检测，而组件在 `OnPush` 模式下只有事件触发和`@Input` 数据改变会进行变更检测。

  因此在 `heroPush` 组件中，在定时器中修改 `name` 视图并未刷新。

  在这种情况下，我们便需要手动执行变更检测。Angular 为开发者提供了控制程序执行变更检测的功能类——[`ChangeDetectorRef`](https://angular.cn/api/core/ChangeDetectorRef) ，`ChangeDetectorRef.detectChanges()` 可以主动进行当前组件和子组件的视图检查，更新绑定数据。

  ![image-20210823181433987](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210823181433987.png)

  使用 `detectChanges()` 手动检查当前组件及子组件的视图：

  ![image-20210823181736358](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210823181736358.png)

+ *`ChangeDetectorRef` 扩展*

  Angular 程序在运行时，加载的组件以树的形式呈现，并且变更检测遵循单向原则（ 即视图刷新按照从树的根节点向下刷新的原则 ）。

  ![image-20210824101434659](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824101434659.png)

  节点的刷新按照每个组件设置的变更检测策略执行视图的刷新，当变更检测刷新至设置为 `OnPush` 模式的组件时，会被阻断（ 如下图所示 有颜色标记的是在一次视图刷新周期内被更新的组件）

  *`Angualr` 变更检测在线演示 https://danielwiehl.github.io/edu-angular-change-detection/*

  ![image-20210824103159423](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824103159423.png)

  无论子组件设置的模式是否是 `Default` ，只要父组件设置了 `OnPush` 均会被阻止刷新，也就是所谓的**单向原则**。

  使用 `ChangeDetectorRef.markForCheck()` 标记 `OnPush` 组件。

  ![image-20210824104032401](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824104032401.png)

  `markForCheck()` 可标记当前组件为**未刷新的**，以便在下一次 Angular 变更检测周期进行刷新，值的注意的是 `markForCheck()` 并非是直接的执行刷新行为，而是仅标记 `OnPush` 组件，其本身并不会引起刷新。

  首先标记 `OnPush` 的组件 `comp-1-x-2` 。

  ![image-20210824104526890](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824104526890.png)

  再次执行变更检测，可以观察到被标记的`comp-1-x-2` 在本次变更检测中进行了视图刷新。

  ![image-20210824104619357](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824104619357.png)

  `detech()`和`reattach()`

  ![image-20210824105108546](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824105108546.png)

  ![image-20210824105122749](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824105122749.png)

  `ChangeDetectorRef.detech()` 可以将当前组件及其子组件从整个视图树中剥离，被剥离的组件无论是`Default`还是`OnPush` 模式都不会进行刷新。

  ![image-20210824105522934](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824105522934.png)

  `ChangeDetectorRef.reattach()` 作用于将被剥离的视图树重新挂载至整个视图树中，被重新挂载的组件遵循原有设置的变更检测策略 ( `Default` 和 `OnPush` )。

  `ChangeDetectorRef.detectChanges()` 

  ![image-20210824105849551](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824105849551.png)

  `detectChanges()` 被用于手动刷新当前组件及子组件，与整个树刷新不同，`detectChanges()` 只会从被调用的组件节点开始进行视图刷新，从而更加节省性能。

  ![image-20210824110118233](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210824110118233.png)

  *子组件依旧遵循变更检测策略来进行视图刷新*

  
