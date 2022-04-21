### Angular 路由策略 ( RouteReuseStrategy )

路由策略 ( RouteReuseStrategy ) 提供了一个管理组件状态 （ 生命周期 ） 和控制路由的方式。作为SPA （ 单页面应用 ）Angular 每一次的路由跳转，（ 默认情况下 ）都是一次新的组件的生成和旧的组件销毁的过程，RouteReuseStrategy 让开发者深入这一过程并进行行为控制。



#### RouteReuseStrategy 使用

> https://angular.cn/api/router/RouteReuseStrategy

```typescript
abstract class RouteReuseStrategy {
  abstract shouldDetach(route: ActivatedRouteSnapshot): boolean
  abstract store(route: ActivatedRouteSnapshot, handle: DetachedRouteHandle): void
  abstract shouldAttach(route: ActivatedRouteSnapshot): boolean
  abstract retrieve(route: ActivatedRouteSnapshot): DetachedRouteHandle | null
  abstract shouldReuseRoute(future: ActivatedRouteSnapshot, curr: ActivatedRouteSnapshot): boolean
}
```



+ ` shouldReuseRoute( future, curr )` 为整个策略的入口属性，每次路由变化时该方法首先被调用，该方法有两个参数 `future 和 curr`

  ```typescript
  abstract shouldReuseRoute(
      future: ActivatedRouteSnapshot, // 即将前往的路由
      curr: ActivatedRouteSnapshot    // 当前离开的路由
  ): boolean // 返回 boolean 
  ```

  返回值为 `true ` 时意味着路由未发生变化，后续的所有路由策略的方法均不会执行；

  返回值为 `false` 时，表示路由发生变化进行后续的处理；

  

  + 一个基本示例如下

  ```typescript
  shouldReuseRoute(future: ActivatedRouteSnapshot, curr: ActivatedRouteSnapshot): boolean {
      return future.routeConfig === curr.routeConfig;
  }
  ```

  **值的注意的是，为了实现类似刷新的功能，我们不仅要保证当前后路由一致时，在入口处返回 false ；同时另外一个额外的配置 ( ExtraOptions ) 为 `onSameUrlNavigation: 'reload'`, 该属性为 Router 的配置属性，如果没有该属性，当导航前后路由一致时，浏览器会直接忽略，shouldReuseRoute 也不会执行 **

  > onSameUrlNavigation?: 'reload' | 'ignore'
  >
  > 规定当路由器收到一个导航到当前 URL 的请求时该如何处理。 默认情况下，路由器会忽略本次导航。不过，这会阻止实现类似于"刷新"按钮的功能。 使用该选项可以控制导航到当前 URL 时的行为。默认为 'ignore'。
  >
  > https://angular.cn/api/router/ExtraOptions



当 `shouldReuseRoute` 返回 `false` 后，后续路由策略开始执行。为了便于理解和记忆，对于剩下的四个方法将分两组说明：

① 离开时 `shouldDetach` 和 `store` 

② 进入时 `shouldAttach` 和 `retrieve`



+ 离开当前路由

  从一个旧的路由离开时，路由策略执行了 `shouldDetach (判断)` - > `store (保存)` ，用于离开时判断是否要对旧的状态进行保存以及如何保存组件状态。

  + `shouldDetach`  离开路由时触发，用于判断当前离开的路由是否要进行组件状态 ( 快照 ) 的保存。

    ```typescript
    abstract shouldDetach(
        route: ActivatedRouteSnapshot // 离开的路由
    ): boolean // 返回 boolean
    ```

    返回值为 `true`  时执行 `store` 

    返回值为 `false`  时不执行

  + `store` 进行快照保存的主要方法。

    ```typescript
    abstract store(
        route: ActivatedRouteSnapshot, // 离开的路由
        handle: DetachedRouteHandle // 路由树 （快照）
    ): void
    ```

    开发者可以在 `store` 中对“路由树”进行处理，并且选择合适的方式进行存储，`store` 方法中的第二个参数 `handle` 即为缓存，存储的缓存将在 `rertieve` 中使用。

+ 进入新的路由

  跳转至一个未来的 ( future ) 的路由时，路由策略依次执行 `shouldAttach (判断)` - > `retrieve (还原)`。

  + `shouldAttach` 进入路由时触发，用于判断进入的路由 (组件) 是否应该从快照中还原还是构建一个新的组件。

    ```typescript
    abstract shouldAttach(
        route: ActivatedRouteSnapshot // 进入的路由
    ): boolean // true / false
    ```

    返回 `true` 时触发 `retrieve` 进行还原

    返回 `false` 时这个组件将会被重新创建

  + `retrieve` 获取存储的缓存

    ```typescript
    abstract retrieve(route: ActivatedRouteSnapshot): DetachedRouteHandle | null
    ```

    `retrieve` 方法将会返回一个 已经分离的路由树 ( DetachedRouteHandle ) 或 null ，返回 null 时路由将跳转失败。

