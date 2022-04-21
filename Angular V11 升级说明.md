### Angular V11 升级说明

+ 现已移除 @angular/material，目前项目暂无使用；
+ 新增 eslint ，代码校验规则增加，详见 `.eslintrc.json`；
+ V10 版本之后 RouteReuseStrategy 进行调整，对应的方法处理目前已经修改；
+ NG-ZORRO 同步更新至 V11   nz-tab 样式发生变化，已移除 `@tabs-horizontal-margin: 0 32px 0 0`；
+ node_module_changes / ng-zorro-ant 已删除，源文件改动较大，无法使用；
+ hmrWarning 已被弃用，目前已经无效
+ 新项目使用文件名 `.browserslistrc` 而不是 `browserslist` 。



#### V9 -> V10 更新变动

+ **在版本10中，不再支持使用Angular功能且没有Angular装饰器的类。[了解更多](https://v10.angular.io/guide/migration-undecorated-classes)。**

+ 从 Angular 9 开始，对 DI 的 @Injectable 装饰器的实施更加严格，不完整的提供程序定义的行为也有所不同。[了解更多](https://v9.angular.io/guide/migration-injectable)。

+ 在您的 Angular 原理图(schematics)中停止使用 `styleext` 或 `spec`。

+ Angular 的 NPM 软件包不再包含 jsdoc 注释，这是与闭包编译器一起使用所必需的(极为罕见)。这种支持是实验性的，仅在某些用例中有效。不久将宣布一条替代的推荐路线。

+ **如果您使用 Angular 表单，则 `number` 类型的输入将不再监听[更改事件](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/changeevent)(此事件不必为每个更改触发该值)，而监听[输入事件](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/inputevent)。**

+ **对于 Angular 表单验证，`minLength` 和 `maxLength` 验证器将验证表单控件的值的 length 属性。**

+ [Angular 包格式](https://g.co/ng/apf)已更新，已删除 `esm5` 和 `fesm5` 格式。这些不再分发到 npm 软件包中。如果您不使用 CLI，则可能需要自己将 Angular 代码降级为 ES5。

+ 有关未知元素的警告现在记录为错误。这不会破坏您的应用程序，但可能会使一些工具无法通过 `console.error` 记录任何东西。

+ 所有返回 `EMPTY` 的解析器(resolver)都将取消导航(navigation)。如果要允许导航继续进行，则需要更新解析器以发出一些值(例如，`defaultIfEmpty(...)`，`of(...)`等)。

+ 如果您使用 Angular Service Worker 并通过 [Vary Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary) 依赖资源，则现在将忽略这些 Headers 以避免跨浏览器的行为出现不可预测的情况。为了避免这种情况，请[配置 service worker](https://angular.io/guide/service-worker-config) 以避免缓存这些资源。

+ 您可能会看到在使用 `async` 管道之前未检测到的 `ExpressionChangedAfterItHaHasBeenChecked` 错误。该错误以前可能没有被检测到，因为两个 `WrappedValues` 在所有情况下都被视为“相等”，即使检查时未将其各自的未包装值。在版本 10 中，`WrappedValue` 已被删除。

+ **如果您具有属性绑定，例如 `[val]=(observable | async).someProperty`，则如果 `someProperty` 的值与先前的发射(emit)相同，则它将不再触发更改检测。如果您依赖于此，请根据需要手动订阅并调用 `markForCheck` 或更新绑定以确保引用更改。**

+ **如果您使用 `formatDate()` 或 `DatePipe` 以及 `b` 或 `B` 格式的代码，则逻辑已更新，以便与一天中跨越午夜的时间段相匹配，因此它现在将呈现正确的输出，例如在英语中为“night”。**

+ 如果您使用 `UrlMatcher`，现在类型反映出它始终可以返回 `null`。

+ 对于 Angular Universal 用户，如果使用 `useAbsoluteUrl` 设置 `platform-server`，则现在还需要指定 `baseUrl`。

+ 有关弃用，自动迁移和更改的更多详细信息，请访问 [Angular 更新到版本 10](https://angular.io/guide/updating-to-version-10)

+ **不再支持 IE9，IE10 和 IE mobile。这是在 [Angular 版本 10 更新](http://blog.angular.io/version-10-of-angular-now-available-78960babd41#c357) 中宣布的**

  

  

#### V10 -> V11 更新变动

+ 现在可以通过使用 Yarn 且在 `package.json` 中添加 `"resolutions": {"webpack": "^5.0.0"}` 来选择使用 webpack 5。
+ **Angular 现在需要 [TypeScript 4.0](https://devblogs.microsoft.com/typescript/announcing-typescript-4-0/)。**
+ 生成新项目时，将询问您是否要启用严格模式。这将配置 TypeScript 和 Angular 编译器以进行更严格的类型检查，并默认应用更小的包预算(bundle budgets)。可以使用 `--strict=true` 或 `--strict=false` 跳过提示。
+ 如果您使用路由(Router)，则 `relativeLinkResolution` 的默认值已从 `legacy` 改为 `corrected`。如果您的应用程序以前未在 ExtraOptions 中指定值而使用了默认值，并且在从空路径路由的子级导航时使用相对链接(relative links)，则需要更新 `RouterModule` 的配置，以专门为 `relativeLinkResolution` 指定 `legacy`。有关更多详细信息，请参阅[文档](https://v11.angular.io/api/router/ExtraOptions#relativeLinkResolution)。
+ 在 Angular 路由(Router)中，v4 中为弃用的选项 `initialNavigation` 已被删除。如果您以前使用过 `enabled` 或 `true`，则现在选择 `enabledNonBlocking` 或 `enabledBlocking`。如果您以前使用过 `false` 或 `legacy_disabled`，现在使用 `disabled`。
+ **在 Angular 路由(Router)的 `routerLink` 中，`preserveQueryParams` 已被删除，请改用 `queryParamsHandling="preserve"`。**
+ **如果访问的是 `queryParams`、`fragment`或 `queryParamsHandling` 的 `routerLink` 值，则可能需要放宽类型以同时接受 `undefined` 和 `null`。**
+ 组件视图封装选项 `ViewEncapsulation.Native` 已被删除。使用 `ViewEncapsulation.ShadowDom` 代替。
+ 如果使用 i18n，现在将再次检查 Unicode 国际组件(ICU)表达式中的表达式。如果在 ICU 中出现的表达式中发现错误，这可能会导致编译失败。
+ **`@angular/forms` 包中的指令过去将 `any[]` 作为构造函数中预期的 `validators` 和 `asyncValidators` 参数的类型。现在这些参数类型正确，因此，如果代码依赖于 `@angular/forms` 的指令构造函数类型，则可能需要进行一些更新以提高类型安全性。**
+ **如果您使用 Angular Forms，则 `AbstractFormControl.parent` 的类型现在包含 null。ng update 将自动迁移此项。但是在一个不太可能的情况下，您的代码是针对 undefined 和 strict equality 来测试父对象的，因此需要将其改为 `=== null`，因为父对象现在显式地用 `null` 初始化，而不是保持 undefined。**
+ **在 v8 中已弃用了很少使用的 `@angular/platform-webworker` 和 `@angular/platform-webworker-dynamic`，并已将其删除。在 Web worker 中运行 Angular 的部分内容是一项实验，在常见的用例中效果不佳。Angular 仍然对 [Web Workers](https://angular.io/guide/web-worker) 有很好的支持。**
+ `slice` 管道现在为未定义的输入值返回 null，这与大多数管道的行为一致。
+ **`keyvalue` 管道已修复，可以报告具有数字 key 的输入对象，结果类型将包含键的字符串表示形式。已经是这种情况，并且仅对代码进行了更新以反映这一点。如果他们依赖不正确的类型，请更新管道输出的使用者。请注意，这不会影响输入值为 `Map` 的用例，因此，如果需要保留 `number`，这是一种有效的方法。**
+ 现在，数字管道(`decimal`， `percent`， `currency`等)明确说明了接受哪些类型。
+ **现在，`date` 管道明确说明了接受哪些类型。**
+ **当将日期时间格式的字符串以包含毫秒部分的格式传递给 `DatePipe` 时，现在毫秒将取整，而不是取最接近的毫秒。大多数应用程序不会受到此更改的影响。如果这不是期望的行为，则在将字符串传递给 `DatePipe` 之前，考虑对其进行预处理以舍入毫秒部分。**
+ 异步管道不再声明为未定义类型的输入返回未定义。注意，对于未定义的输入，代码实际上返回null。

+ `uppercase` 和 `lowercase` 管道不再让虚假的值通过。现在，它们将 `null` 和 `undefined` 都映射到 `null`，并在无效输入(`0`， `false`， `NaN`)上引发异常。这与其他 Angular 管道匹配。
+ **如果您将路由(Router)与 `NavigationExtras` 一起使用，则新的类型允许传入类型为 `NavigationExtras` 的变量，但它们将不允许对象文字(object literals)，因为它们可能仅指定已知属性。他们也将不接受那些与 `Pick` 中的属性没有共同属性的类型。如果您受到此更改的影响，请仅从 NavigationExtras 中指定实际在各个函数调用中使用的属性，或在对象或变量上使用类型声明：`as NavigationExtras`。**
+ 在测试中，如果在 TestBed 初始化之后调用 `TestBed.overrideProvider`，overrideProvider 已移除。此行为与其他替代方法(例如 `TestBed.overrideDirective` 等)一致，但它们会抛出错误来表明这一点。该检查以前在 TestBed.overrideProvider 函数中丢失。如果看到此错误，则应在完成 TestBed 初始化之前移除 `TestBed.overrideProvider` 调用。
+ **如果使用路由(Router)的 RouteReuseStrategy，参数顺序已更改。先前在评估子路由时调用 `RouteReuseStrategy#shouldReuseRoute` 时，它们将使用被交换的 `future` 和 `current` 参数来调用。如果您的 `RouteReuseStrategy` 仅依赖于将来或当前的快照状态，您可能需要更新 `future` 和 `current`、`ActivateRouteSnapshots` 的 `shouldReuseRoute` 实现。**
+ 如果您使用区域设置数据数组(locale data arrays)，则此 API 现在将返回只读数组。如果您要对它们进行突变(例如，调用 `sort()`，`push()`，`splice()`等)，则您的代码将不再编译。如果您需要更改数组，现在应该获取副本(例如，通过调用 `slice()`)并更改副本。
+ 在更改检测中，`CollectionChangeRecord` 已移除，应改用 `IterableChangeRecord`。
+ **如果在初始化时对 `FormControl`、`FormGroup` 或 `FormArray` 类实例上定义的异步验证器使用 Angular Forms，则在异步验证器完成后，以前不会发出状态更改事件。这已被更改，因此 status 事件被发送到 `statusChanges` observable 中。如果代码依赖于旧的行为，则可以忽略此附加的状态更改事件。**

