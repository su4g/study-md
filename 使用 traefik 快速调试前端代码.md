### 使用 traefik 快速调试前端代码

+ 当测试系统出现问题无法在本地开发环境中复现时；

+ 当测试系统数据复杂但本地没有数据环境时；

+ 当修改的问题涉及其他服务模块本地没有时；

+ 当修改完成需要本地测试时；

  使用 traefik 工具，快速调试前端代码，解决前端 “盲改”、前端人员本地后端难维护、后台服务过多等问题，让前端开发人员更专注前端代码。

##### 如何配置

![image-20210719153900258](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719153900258.png)

treafik  配置文件为 `config/dynamic_conf.toml`

```
# http routing section 配置示例
[http]
  [http.routers]
    [http.routers.backend]
    
    // 配置转发规则默认otrs 和 otrs-web ，其他服务同理，例如 es 添加 PathPrefix(`/es/`)
      rule = "PathPrefix(`/otrs/`) || PathPrefix(`/otrs-web/`) || PathPrefix(`/es/`)"

      middlewares = ["fixHeader"]
      service = "backend"
    [http.routers.frontend]
      rule = "PathPrefix(`/`)"
      service = "frontend"

  [http.services]
    [http.services.backend.loadBalancer]
      [[http.services.backend.loadBalancer.servers]]
      
      // 配置后端地址
        url = "https://fixes-mywork-bugs.otrs365.cn/"
        
    [http.services.frontend.loadBalancer]
      [[http.services.frontend.loadBalancer.servers]]
      
      // 配置前端地址
        url = "http://192.168.123.43:4300/"

  [http.middlewares.fixHeader.headers]
    [http.middlewares.fixHeader.headers.customRequestHeaders]
    
    // 主机
      Host = "fixes-mywork-bugs.otrs365.cn"
```

> 更多配置和参数说明详见官方文档 https://doc.traefik.io/traefik/v2.4/

##### 如何启用

在 traefik 解压路径下打开 windows 终端

![image-20210719155635829](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719155635829.png)

执行命令 `.\traefik.exe --configfile .\traefik.toml`

![image-20210719160939117](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719160939117.png)

查看规则

浏览器中输入 `http://127.0.0.1:8080/dashboard/#/http/routers`

![image-20210719161029083](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719161029083.png)



##### 访问程序

前端修改

取消 app.service.ts 中配置的服务地址

![image-20210719161611768](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719161611768.png)

访问地址使用 `http://127.0.0.1/user/login` 或 `http://127.0.0.1/customer/login`

![image-20210719161758114](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719161758114.png)





#### 常见错误记录

1. 端口占用

   启动报错

   ```
   time="2021-07-19T11:19:50+08:00" level=info msg="Configuration loaded from file: C:\\traefik_v2.4.8_windows_amd64_diantong\\traefik.toml"
   2021/07/19 11:19:50 traefik.go:76: command traefik.exe error: error while building entryPoint web: error preparing server: error opening listener: listen tcp :80: bind: Only one usage of each socket address (protocol/network address/port) is normally permitted.
   ```

   TCP：80 端口被占用

   ① 查看占用情况

   使用 ` netstat -ano` 或 ` netstat -aon|findstr "80"`查看端口使用

![image-20210719151131236](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719151131236.png)

​     ② 查看 PID 并终止该进程 

- 使用命令 `taskkill /T /F /PID 20372`

- 使用任务管理器

  右键勾选 PID 、查找对应 PID 并终止程序

![image-20210719153305965](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719153305965.png)

​	**无法终止**

 ![image-20210719160443725](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719160443725.png)



1. 以管理员身份运行

   ![image-20210719160639101](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719160639101.png)

2. 终止PID

   ![image-20210719160708733](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719160708733.png)

3. 重新启动

   ![image-20210719160740835](https://github.com/su4g/study-md/blob/main/typora-user-images/image-20210719160740835.png)