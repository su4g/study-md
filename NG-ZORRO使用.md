在 Angular 前端项目中，为了节省开发成本和统一样式，我们全面使用 [`NG-ZOORO`](https://ng.ant.design/docs/introduce/zh) 作为项目第三方组件库，并且结合本地个性化修改构成我们项目的基础组件内容。熟悉 `NG-ZORRO` 和 本地样式库的使用，将建立开发人员对本项目 `UI` 逻辑的认知。

#### `NG-ZORRO` 概览

在学习如何使用 `NG-ZOORO` 库前，我们先通过[官方文档](https://ng.ant.design/docs/introduce/zh)和项目预览了解 `NG-ZOORO` 实现了什么。

访问 [NG-ZOORO 组件总览](https://ng.ant.design/components/overview/zh)

![image-20210825152613227](.\typora-user-images\image-20210825152613227.png)

`NG-ZOORO` 提供了布局、图标、数据展示和数据录入等功能集。帮助开发人员简化工作复杂度，更好的专注业务处理。开发人员应了解 `NG-ZOORO` 所提供的组件功能，在开发过程中，优先考虑使用已有的组件实现 `UI` 设计。

#### 如何使用？

##### 1. 使用 `select` 选择器使用组件。

第三方组件的使用本质上与我们自己开发的组件是一致的，在 `HTML` 中按照子组件的使用方式即可引入所需的组件。

具体的使用步骤如下：

① 在 `NZ-ZOROO` 中查找的所需组件；

例如引入 [`Button 按钮`](https://ng.ant.design/components/button/zh) 

![image-20210826114446798](.\typora-user-images\image-20210826114446798.png)

② 添加至使用页面中

![image-20210826114552926](.\typora-user-images\image-20210826114552926.png)

组件效果

![image-20210826114623989](.\typora-user-images\image-20210826114623989.png)

③ 自定义修改

组件引入后我们可以按照需要的场景、功能进行修改，`NG-ZORRO` 中的每个组件都提供了诸多便利的`API` ，在实际开发中我们需要根据功能需求使用 `API` 来满足不同业务需要。

![image-20210826114935583](.\typora-user-images\image-20210826114935583.png)

`API` 的本质上是组件提供的 `@Input` 和 `@Output` 。

 例如为按钮添加`载入状态`：

```html
<button nz-button nzType="primary" [nzLoading]="true">Primary Button</button>
```

![image-20210826115410254](.\typora-user-images\image-20210826115410254.png)

##### 2. 使用组件的服务方法

`NG-ZORRO` 为一些组件设计了开放的服务可供开发者使用，组件的服务的方法使用与项目中服务的使用方法一致。

① 引入 `service`

![image-20210826115730464](.\typora-user-images\image-20210826115730464.png)

```typescript
import { Component, OnInit } from '@angular/core';
// 在组件中引入 NzMessageService
import { NzMessageService } from 'ng-zorro-antd/message';

@Component({
  selector: 'app-hero',
  templateUrl: './hero.component.html',
  styleUrls: ['./hero.component.scss']
})
export class HeroComponent implements OnInit {

  constructor(
  // 注册
    private $nzMsg: NzMessageService,
  ) { }

  ngOnInit(): void {
      this.$nzMsg.success('内容');
  }
}
```

② 使用 `service`

每个服务都有导出的方法可供开发者使用，在 `API` 中可查看可使用的方法。

![image-20210826120126546](.\typora-user-images\image-20210826120126546.png)

使用时按照方法说明传入参数。

![image-20210826120259478](.\typora-user-images\image-20210826120259478.png)

ex : `  this.$nzMsg.success('内容');`

 #### 本地个性化(第一阶段)

由于组件的限制性（ 组件样式受制于组件库的设计模式 ），在项目中我们对`NG-ZOROO` 组件的样式进行了个性化配置，用于满足本项目的 `UI` 规范，其中涉及主题色、间距、布局等规范内容。

`UI` 规范见 ：企业微信 - 微盘 - UI组件

![image-20210826135222243](.\typora-user-images\image-20210826135222243.png)

目前可使用的本地样式库涉及字体、颜色、布局等，样式库见项目`src/style`。

![image-20210826135552804](.\typora-user-images\image-20210826135552804.png)

##### 1. 如何使用

在需要使用公共样式变量的组件 `scss` 文件中添加 `@import "~src/style/main.scss";`

- 变量使用

  在 `src\style\base\_variable.scss` 中包含了项目字体、颜色变量等。

  ![image-20210826140320198](.\typora-user-images\image-20210826140320198.png)

  在开发或修改组件时直接使用相关变量，这将为后续*换肤功能*提供帮助。

  ![image-20210826140457821](.\typora-user-images\image-20210826140457821.png)

- 类名使用

  除变量外，对于表单、布局、按钮等，按照规范设置了样式类名

  例如为**按钮**设置不同的 `padding` ，使用类名即可设置对应样式。

  ![image-20210826141015629](.\typora-user-images\image-20210826141015629.png)

  ![image-20210826141037872](.\typora-user-images\image-20210826141037872.png)

  **更多样式内容和本地样式库说明 **





