小程序开发借助 [`uni-app`](https://uniapp.dcloud.io/) 框架使用 [`Vue2`](https://vuejs.org/) 开发，官方推荐所用开发`IDE`为 [`HBuliderX`](https://www.dcloud.io/hbuilderx.html?hmsr=vue-en&hmpl=&hmcu=&hmkw=&hmci=)，同时我们也可以借助[`VSCode`](https://code.visualstudio.com/)进行本地开发，两种方式各有优劣，开发者可根据自身情况和需要进行选择。

### 使用 HBuliderX 构建开发环境

#### 1、下载 [`HBilderX`](https://www.dcloud.io/hbuilderx.html?hmsr=vue-en&hmpl=&hmcu=&hmkw=&hmci=) 以及 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

![image-20210908114001509](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908114001509.png)

![image-20210908114012086](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908114012086.png)

在`Windos`下下载和安装上述工具，并进行配置：

打开 `HBuliderX` 通过 `工具 -> 设置 -> 运行配置 -> 小程序运行配置 ` 添加 微信开发者工具 路径

**路径指向微信开发者工具根目录即可，无需指向.exe程序**

![image-20210908114331939](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908114331939.png)

#### 2、`clone` 小程序项目

*有关 [`git`](https://git-scm.com/) 的使用操作此处不详细阐述，可通过其他[文档](https://workshops.otrs365.cn/web/#/109?page_id=1229)学习 `git` 使用。*

使用容器或者`Windows GIT ` 克隆  [`sc-miniprogram-vue`](https://git.otrs365.cn/service-cool-uniapp/uniapp-cool) 项目：

![image-20210908115128975](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908115128975.png)

#### 3、导入项目

*由于 `HBuliderX` 没有连接 `Docker` 的插件，因此在容器中克隆的项目需要使用 `ftp/sftp` 导出至`Windows`中。（若使用`Windows GIT` 建议也从克隆的库中导出一份，不要直接在本地`git`库中修改代码）*

在`HBuliderX` 中选择 `导入 -> 从本地目录导入` 将本地项目导入 `HBuilderX`。

![image-20210908115857437](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908115857437.png)

#### 4、安装依赖并运行项目

##### ① 在 `Windows` 中安装 `node` 并配置环境变量，可参考网上教程

> https://www.jianshu.com/p/13f45e24b1de
>
> https://cloud.tencent.com/developer/article/1572591

##### ② 安装终端插件并执行安装

点击 `HBulderX` 终端，会自动安装终端插件

![image-20210908150635421](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908150635421.png)

打开终端后执行 `npm i(install)` 安装依赖

##### ③ 使用运行或发行模式启动

![image-20210908151006520](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908151006520.png)

![image-20210908151021435](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908151021435.png)

**注意：请确保微信开发者工具使用的账号具有该小程序的开发权限**

![image-20210908171111549](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908171111549.png)

**`HBuilderX` 会自动启动 微信开发者工具 **



### 使用 `VSCode` 开发小程序

#### 1、安装 [`VSCode`](https://code.visualstudio.com/) 和 `Node` 

此过程不再赘述，`node` 安装和环境变量配置参考上文 `4-1`

#### 2、克隆项目

参考上文 `2、clone 小程序`

#### 3、导入项目安装依赖

*Docker  中的项目需要复制至 `windows` 下*

参考上文 `3、导入项目` ，在 `VSCode` 终端中执行 `npm i`

![image-20210908164013469](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908164013469.png)

#### 4、使用 `npm run` 启动并配置 微信开发者工具

由于`VSCode` 没有 `HBuliderX` 的"运行"和"发行"按钮，因此需要通过 `npm run` 进行启动

打开 `package.json` 文件，查看 `scripts` 中 `dev` 和 `build`  （最新版本）

![image-20210908161337800](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908161337800.png)

或者是（旧版本）

![image-20210908161546980](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908161546980.png)

**启动项目模式使用命令`npm run dev / npm run dev:mp-weixin`**

**发行项目模式使用命令`npm run build / npm run build:mp-wein`**

![image-20210908162019858](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908162019858.png)

![image-20210908164904578](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908164904578.png)

**启动和发行模式会在项目目录 `dist` 文件下生成 `dev` 和 `build` 包** 

![image-20210908164234097](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908164234097.png)

**`VSCode` 不会帮我们自动打开 微信开发者工具并连接相关文件包，因此需要手动导入**

在`微信开发者工具 - 小程序` 中新建项目

![image-20210908162420641](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908162420641.png)

导入编译文件

![image-20210908164435646](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908164435646.png)

![image-20210908164651142](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908164651142.png)

**需要发行版本时请使用发行模式，以更好压缩体积**

需要发行至其他小程序，在微信开发者工具中修改`AppID` 即可

![image-20210908171441529](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908171441529.png)

![image-20210908171557235](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210908171557235.png)



