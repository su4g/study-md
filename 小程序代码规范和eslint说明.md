依赖说明：

```json
    "vue-eslint-parser": "^7.11.0", // 语法转化，兼容vue template
    "babel-eslint": "^10.1.0",      // 语法转化，兼容ES6语法
    "eslint": "^7.32.0",            
    "eslint-plugin-vue": "^7.17.0", // eslint vue 版本
```

`eslint` 规则：

```json
"rules": {   
        "vue/no-side-effects-in-computed-properties": "off", // 允许在计算属性中调用
        "vue/valid-v-model": "warn", // v-model 使用规范
        "no-case-declarations": "off", // switch 声明在条件内部
        "no-console": "warn", // 尽可能去掉 console
        "default-case": "warn", // switch 请写 default
        "no-var": "warn", // 减少使用 var
        "vue/no-mutating-props": "off", // 允许更改 props
        "vue/script-indent": ["error", 4, { "baseIndent": 1 }], // 脚本缩进 4 空格
        "vue/no-custom-modifiers-on-v-model": "error", //
        "vue/no-multiple-template-root": "error", // 禁止多个根节点
        "vue/no-v-for-template-key": "error", // template 不使用 :key
        "vue/no-v-model-argument": "error",
        "vue/valid-v-bind-sync": "error", // v-bind:sync 使用规范
        "vue/html-end-tags": "error", // html标签包含结束标记
        "vue/html-indent": ["error", 4, { "baseIndent": 1 }], // html 缩进使用 4 空格
        "vue/multiline-html-element-content-newline": "warn", // 多个 html 请换行
        "vue/require-prop-types": "error", // 在 props 中声明类型
        "prefer-const": "warn", // 尽可能使用 const 、 let
        "vue/require-valid-default-prop":"warn", // props 中采用 function 声明默认值
        "vue/html-quotes": "warn",
        "vue/no-spaces-around-equal-signs-in-attribute": "warn",
        "no-unused-vars": "warn",
        "no-prototype-builtins":"warn"
    },
    "globals": { // 补充全局
        wx: true,
        App: true,
        uni: true,
        i18n:true,
        getApp: true,
        Page: true,
        getCurrentPages: true,
        Component: true,
        Behavior:true
    }
```



- 注释的规范性

```
// 单行注释

/**

* 块注释，通常用在方法或者组件顶部

*/
```

- 方法注释的标准可参考 [`JSDOC`](https://www.shouce.ren/api/view/a/13232)

```
/**

* function description 方法描述

* @param {type} name param discription

* @returns {type} name return discription

*/
```

