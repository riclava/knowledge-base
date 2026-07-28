---
title: 结构型模式
type: concept
created: 2026-07-28
updated: 2026-07-28
sources: [H. 软件架构设计/设计模式/README.md]
tags: [concept, software-architecture, design-patterns, gof, structural-patterns, oop, adapter, decorator, proxy, facade, composite, bridge, flyweight]
---

结构型模式是 GoF 23 个模式中关注"类与对象怎么组合"的 7 个模式，共同取向是用组合而非继承来构建更大、更灵活的结构，把小结构拼成大结构的同时保持可替换性。

## Pattern Summary

| 模式 | 意图 | 典型场景 |
|---|---|---|
| 适配器 Adapter | 把一个接口转换成客户端期望的另一个接口 | 对接第三方/遗留库、接口不兼容 |
| 装饰器 Decorator | 动态地给对象叠加职责，不改原类 | Java IO 流、给功能加缓存/日志/限流 |
| 代理 Proxy | 为对象提供一个替身以控制访问 | 远程代理、懒加载、权限校验、AOP |
| 外观 Facade | 为一组复杂子系统提供一个统一简化入口 | SDK 门面、封装复杂调用链 |
| 组合 Composite | 把对象组成树形结构，统一处理"单个"和"组合" | 文件系统、UI 组件树、组织架构 |
| 桥接 Bridge | 把抽象与实现分离，让两者独立变化 | 跨平台绘制、抽象维度 × 实现维度 |
| 享元 Flyweight | 共享细粒度对象以节省内存 | 大量重复对象（字符、图元、连接） |

---

## 适配器 Adapter

**意图**：把一个接口转换成客户端期望的另一个接口。

**要点**：最直观的"转接头"。当已有类的接口和你要用的接口对不上，又不能改它（第三方、遗留代码），就包一层做转换。分对象适配器（组合，推荐）和类适配器（多继承）。

**真实应用**

- **Java IO**：`InputStreamReader` 把字节流适配成字符流，`OutputStreamWriter` 反之。
- **集合**：`Arrays.asList()` 把数组适配成 `List` 视图。
- **Spring MVC**：`HandlerAdapter` 把形态各异的 Controller 适配成统一的调用接口。
- **日志桥接**：`log4j-over-slf4j`、`jul-to-slf4j` 把旧日志 API 适配到 SLF4J。
- **Go**：`http.HandlerFunc` 把普通函数适配成 `http.Handler` 接口。

---

## 装饰器 Decorator

**意图**：动态地给对象叠加职责，而不修改原类。

**要点**：用组合在运行时叠加功能，避免"为每种功能组合都建一个子类"导致的类爆炸。和代理的区别：装饰器强调**增强功能**，代理强调**控制访问**，结构相似但意图不同。

**真实应用**

- **Java IO**（教科书级例子）：`new BufferedReader(new InputStreamReader(new FileInputStream(f)))`，一层层叠加缓冲、编码、数据类型等能力。
- **集合包装**：`Collections.synchronizedList()`、`unmodifiableList()` 给集合动态加上同步/只读能力。
- **Servlet**：`HttpServletRequestWrapper` / `HttpServletResponseWrapper` 用于包装增强请求响应。
- **Go**：`io` 包的各种包装器（`bufio.NewReader`、`gzip.NewReader`）；HTTP 中间件层层包裹 `http.Handler`。

---

## 代理 Proxy

**意图**：为对象提供一个替身，以控制对它的访问。

**要点**：在真实对象前放一个同接口的替身，用于延迟加载、访问控制、远程调用、缓存等。

**真实应用**

- **Java 动态代理**：`java.lang.reflect.Proxy` 是 **Spring AOP、MyBatis Mapper、RMI stub、Hibernate 懒加载**的底层机制（CGLIB 则是子类代理）。
- **RPC**：gRPC / Thrift 客户端生成的 stub 就是远程代理。
- **Go**：`httputil.ReverseProxy` 是反向代理的直接实现。

---

## 外观 Facade

**意图**：为一组复杂子系统提供一个统一的简化入口。

**要点**：给复杂子系统一个"简单的前台"，降低使用方的认知成本和耦合。它**不隐藏**子系统（仍可直接用），只是提供更方便的默认路径——这是它与适配器、代理的关键差别。

**真实应用**

- **SLF4J**：名字里的 F 就是 Facade——为 Log4j / Logback 等多种实现提供统一的日志门面。
- **Spring**：`JdbcTemplate` 把"获取连接→建 Statement→执行→处理结果→释放资源"这一长串 JDBC 操作封装成一两个方法。
- **各类 SDK**：云厂商 Client SDK 把复杂的签名、重试、序列化封装成简单的方法调用。

---

## 组合 Composite

**意图**：把对象组成树形结构，让客户端统一处理"单个对象"和"对象组合"。

**要点**：让叶子节点和容器节点实现同一接口，客户端可以用统一方式递归处理整棵树，是处理树形结构的标准解法。

**真实应用**

- **GUI**：AWT/Swing 的 `Container` / `Component`、Android 的 `ViewGroup` / `View`、React 组件树。
- **DOM**：HTML/XML 的文档树。
- **文件系统**：目录与文件的树形结构。

---

## 桥接 Bridge

**意图**：把抽象与实现分离，让两者可以独立变化。

**要点**：当一个东西有两个独立变化的维度时（比如"遥控器种类" × "设备种类"），继承会导致 M×N 的类爆炸。桥接用组合把两个维度接起来，各自独立扩展。和适配器的区别：适配器是**事后补救**已有的不兼容接口，桥接是**事前设计**两个维度的分离。

**真实应用**

- **JDBC**：`java.sql.Driver` 抽象接口与各数据库厂商实现分离，让"数据库访问"这个抽象与"具体数据库"这个实现各自独立演化。
- **AWT**：组件（抽象）与平台 peer（实现）分离，实现跨平台。
- **图形/渲染库**：抽象的绘图 API × 可替换的渲染后端（OpenGL / Vulkan / 软件渲染）。

---

## 享元 Flyweight

**意图**：通过共享细粒度对象来节省内存。

**要点**：把对象状态拆成**内部状态**（可共享、不变，存在享元里）和**外部状态**（随上下文变化，调用时传入），通常配一个享元工厂做缓存池。**前提是内部状态必须不可变**，否则共享会互相污染。

**真实应用**

- **Java 缓存**（经典例子）：`Integer.valueOf()` 缓存 -128~127、`String` 常量池、`Boolean.TRUE/FALSE`、`Character` 缓存。
- **字体渲染**：同一字形（glyph）在多处复用，只存一份轮廓数据。
- **游戏**：大量相同的树、粒子、贴图共享同一份内部状态。

---

## Trade-offs

- **适配器 / 外观**：换来接口对齐与认知成本下降，代价是多一层转发，且转换层可能掩盖底层错误语义。
- **装饰器**：换来运行期可自由叠加的能力组合，代价是调用栈变深、调试时难以一眼看出实际生效的包装顺序。
- **代理**：换来访问控制与延迟加载，代价是行为不再"所见即所得"（AOP 织入、懒加载触发点都可能出人意料）。
- **组合**：换来叶子与容器的统一处理，代价是接口需要容纳两类节点，容器专属方法（如 `add`）会泄漏到叶子接口上。
- **桥接**：换来两个维度独立演化，代价是前期就要正确识别出"哪两个维度会独立变化"——识别错就是过度抽象。
- **享元**：换来内存节省，代价是内部/外部状态的强制拆分，以及共享对象必须保持不可变的约束。

## Related Pages

- [[design-patterns]]
- [[creational-patterns]]
- [[behavioral-patterns]]
- [[design-patterns-note]]
- [[abstraction-and-modeling]]
