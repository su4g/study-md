### ESLint 如何约束代码规范

[ESLint](https://eslint.org/docs/user-guide/getting-started) 可以帮助开发团队统一代码规范、风格等 ，开发过程中，不符合规则表的书写将会收到 `warn` 或 `error` 的提示引导开发者正确书写。

`ERROR` :

![image-20210830140925376](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210830140925376.png)

`WARN `:

![image-20210830141023931](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210830141023931.png)

### 如何设置 ESLint 规则

ESLint 规则设置在项目根目录 `.eslintrc.json` 中添加。

````json
{
  "root": true,
  "ignorePatterns": [
    "projects/**/*"
  ],
  "overrides": [
    {
       // 约束 ts 文件规范
      "files": [
        "*.ts"
      ],
      "parserOptions": {
        "project": [
          "tsconfig.json",
          "e2e/tsconfig.json"
        ],
        "createDefaultProgram": true
      },
      "extends": [
        "plugin:@angular-eslint/ng-cli-compat",
        "plugin:@angular-eslint/ng-cli-compat--formatting-add-on",
        "plugin:@angular-eslint/template/process-inline-templates"
      ],
      "rules": {
        // 规则集
      }
    },
    {
      // 约束 html 文件规范
      "files": [
        "*.html"
      ],
      "extends": [
        "plugin:@angular-eslint/template/recommended"
      ],
      "rules": {
        // 规则集
      }
    }
  ]
}

````

在`.eslintrc.json` 中设置 `rules`。

每条规则可设置为关闭(`off`) 、`error` 和 `warn` 三个状态，将会影响规则对语句的检验状态。

![image-20210830142423409](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210830142423409.png)

[`ESLint` 规则](![image-20210830142543138](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210830142543138.png)) 列表 https://eslint.bootcss.com/docs/rules/

`codelyzer` 和 `TSLint` 已经转换为接近的 `ESLint` 规则，`codelyzer` 和 `@angular-eslint` 转换状态可在 [Status of Codelyzer Rules Conversion](https://github.com/angular-eslint/angular-eslint) 查看。

### 检查和自动修复

使用 `npx ng lint otrsnew` 进行规则检查，设置为 `error` 和 `warn` 规则的语法错误将会抛出错误信息。

![image-20210830145055659](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210830145055659.png)

对于可以自动修复的规则使用 `--fix` 可以进行自动修复。

支持自动修复的规则可在 `ESLint` [规则列表](https://eslint.bootcss.com/docs/rules/)中查看，

![image-20210830145502700](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210830145502700.png)

![image-20210830145512860](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210830145512860.png)

