### Compodoc 指南

- **什么是 [Compodoc](https://compodoc.app/) ?**

  Compodoc 是 Angular 应用程序的文档工具，它会生成应用程序的静态文档网站（ 可包含模块、组件、路由等内容 ），使团队的开发人员可以轻松了解应用程序的功能。

  ![image-20210809141445744](.\typora-user-images\image-20210809141445744.png)



- **如何使用**

  - 西安内网访问：http://192.168.123.96:8080/index.html

  - 本地私有化：

  1. 安装

     本地安装：

     执行  ` npm install --save-dev @compodoc/compodoc`  在本地开发环境中安装 compodoc 。

     >NPMERR 解决指导
     >
     >安装出错时可按以下步骤排查解决
     >
     >① 使用 `npm config get registry` 查看下载源，建议使用 Npm 官方源 ，使用 `npm config set registy https://registry.npmjs.org/ ` 更换为官方源 。
     >
     >② 清除缓存，执行 `npm cache verify`  、`npm cache clean --force`
     >
     >③ 重新安装

  2. 工具配置

     ① 在项目根目录下建立  `tsconfig.doc.json` 

     ```json
     // tsconfig.doc.json
     {
       "include": [  "src/**/*.ts"  ],
       "exclude": [  "src/test.ts", 
       				"src/**/*.spec.ts", 
       				"src/app/file-to-exclude.ts"
       			 ]
     }
     ```

     Compodoc 会从提供 config 的文件夹开始获取文档

     ```
     ├── src
     │ ├── app
     │ │ ├── app.component.ts
     │ │ └── app.module.ts
     │ ├── main.ts
     │ └── ...
     ├── tsconfig.app.json
     ├── tsconfig.doc.json
     └── tsconfig.json
     ```

     ② 在 package.json 中定义一个脚本任务：

     ```json
     "scripts": {
       "compodoc": "./node_modules/.bin/compodoc -p ./tsconfig.doc.json --language zh-CN --hideGenerator"
     }
     ```

     `-p` 用于指定配置文件的位置

     `--language` 用于指定语言

     `--hideGenerator` 隐藏网站的 Compodoc Logo

     > **更多配置访问 [https://compodoc.app/guides/options.html](https://compodoc.app/guides/options.html)**

  3. 文档生成

     ① 修改 `dynamic-form.service.ts` 和 `form.service.ts` , 删除方法注释中的 `@author` 和 `@memebers` （ 使用该注释字段会导致 compodoc 转换错误 ）。

     ![image-20210809152638219](.\typora-user-images\image-20210809152638219.png)

     ② 执行 `npm run compodoc` 

     完成后将生成 `documentation` 文件夹

     ![image-20210809153146629](.\typora-user-images\image-20210809153146629.png)

  4. 启动服务

     执行 `compodoc -s` 或 `npx compodoc -s` ，将使用默认端口 ( 8080 ) 启动文档服务。

     ![image-20210809155008826](.\typora-user-images\image-20210809155008826.png)



### JSDoc 和 Compodoc 说明

+ **什么是JSDoc？**

  JSDoc是一个根据javascript文件中注释信息，生成JavaScript应用程序或库、模块的API文档 的工具。

  JSDoc本质是代码注释，所以使用起来非常方便，但是他有一定的格式和规则。

  >[JSDoc介绍(中文) https://www.shouce.ren/api/view/a/13232](https://www.shouce.ren/api/view/a/13232)
  >
  >[JSDoc文档(英文) https://devdocs.io/jsdoc/](https://devdocs.io/jsdoc/)

  JSDoc注释一般应该放置在方法或函数声明之前，它必须以`/**`开始，以便由JSDoc解析器识别。

  ```javascript
  // 示例
  /**
   * Book类，代表一个书本.
   * @constructor
   * @param {string} title - 书本的标题.
   * @param {string} author - 书本的作者.
   */
  function Book(title, author) {
      this.title=title;
      this.author=author;
  }
  ```

+ **JSDoc In Compodoc**

  目前 Compodoc 只支持部分 JSDoc [标签](https://compodoc.app/guides/jsdoc-tags.html)，因此在本地化安装时，我们需要对文件中的相关注释进行修改 （ 不支持 @author、@memberof 等）。 

  **在我们避免使用这些不支持的标签时，为了更好地生成清晰的文档，建议遵循以下注释方式：**

  ```typescript
  /**
   * {@link http://www.google.com} // 使用@link可以引入链接
   *
   * L1 描述 
   *                   // 在 JSDoc 中需要分多行描述时需要空一行，否则将被视为一行
   * L2 参数列表
   * @param param1 参数一
   * @param param2 参数二
   * @param param3 参数三
   * @returns 返回值
   */
  customeFunction(
      param1:object,   // 在方法参数入口标明数据类型，将会反应在文档中
      param2: number[], 
      param3: boolean
      ): string {      // 返回类型
      return '';
  }
  ```

  文档展示：

  ![image-20210809162525964](.\typora-user-images\image-20210809162525964.png)

  

+ **Compodoc 使用 Markdown**

  Compodoc 允许通过 `<component name>.component.md` 为组件或服务等添加 [Markdown](https://www.markdownguide.org/) 文档。

  > Markdown 文档 (中文)  https://markdown.com.cn/basic-syntax/

  ![image-20210809163220389](.\typora-user-images\image-20210809163220389.png)

  添加`MD`文件后，在组件的文档选项卡中会多出 `README` 。

  ![image-20210809163351959](.\typora-user-images\image-20210809163351959.png)

  

