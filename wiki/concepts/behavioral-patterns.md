---
title: 行为型模式
type: concept
created: 2026-07-28
updated: 2026-07-28
sources: [H. 软件架构设计/设计模式/README.md]
tags: [concept, software-architecture, design-patterns, gof, behavioral-patterns, oop, strategy, observer, state-machine, chain-of-responsibility, command, visitor]
---

行为型模式是 GoF 23 个模式中关注"对象之间怎么协作、如何分配职责"的 11 个模式，共同取向是把算法、交互和状态流转从固定结构里解耦出来。

## Pattern Summary

| 模式 | 意图 | 典型场景 |
|---|---|---|
| 策略 Strategy | 封装一族可互换的算法，运行时切换 | 多种支付/排序/折扣算法 |
| 观察者 Observer | 一对多依赖，状态变化自动通知订阅者 | 事件监听、发布订阅、MVC |
| 模板方法 Template Method | 父类定义算法骨架，子类填充可变步骤 | 框架钩子、流程固定但步骤可变 |
| 责任链 Chain of Responsibility | 请求沿处理者链传递，直到被处理 | 中间件、过滤器、审批流 |
| 状态 State | 把状态相关行为封装进状态对象 | 状态机、订单/工单流转 |
| 命令 Command | 把请求封装成对象，支持排队/撤销/日志 | 撤销重做、任务队列、事务 |
| 迭代器 Iterator | 顺序访问集合元素而不暴露内部结构 | 各类容器遍历 |
| 中介者 Mediator | 用中介对象封装多对象交互 | 复杂 UI 组件协调、聊天室 |
| 备忘录 Memento | 在不破坏封装的前提下保存/恢复状态 | 快照、撤销、存档 |
| 访问者 Visitor | 在不改元素类的前提下新增操作 | AST 遍历、编译器、报表 |
| 解释器 Interpreter | 为语言定义文法并解释执行 | 规则引擎、简单 DSL |

---

## 策略 Strategy

**意图**：封装一族可互换的算法，让它们能在运行时切换。

**要点**：把"做同一件事的不同做法"抽成可替换的策略对象，用组合注入，替代一长串 `if/else` 或 `switch`。新增算法只需加一个策略类，符合开闭原则。它和状态模式结构几乎一样，区别在意图：**策略由外部主动选择、策略之间互不知晓；状态由内部驱动流转、状态之间会互相切换**。

**真实应用**

- **Java 排序**（教科书级例子）：把 `Comparator` 传给 `Collections.sort()` / `Arrays.sort()`，运行时切换比较策略。
- **线程池**：`ThreadPoolExecutor` 的 `RejectedExecutionHandler`（AbortPolicy / CallerRunsPolicy 等）是可替换的拒绝策略。
- **Go**：`sort.Slice(s, less)` 的 `less` 函数、`http.Client` 的 `RoundTripper`（可替换传输策略）。
- **加密/压缩**：可插拔的算法实现。

**语言惯用法**：现代 C++ 常直接用 `std::function` 承载策略，省去一整套类层次；Go 用函数类型；Java 8+ 可用函数式接口 + lambda。

---

## 观察者 Observer

**意图**：定义一对多依赖，当一个对象状态变化时自动通知所有订阅者。

**要点**：发布-订阅的基础，实现松耦合的一对多通知。**易错点**：同步通知可能拖慢主流程、观察者异常会影响发布方、忘记退订会内存泄漏。

**真实应用**

- **GUI 事件**：Swing 的 `ActionListener`、`PropertyChangeListener`；浏览器 `addEventListener`。
- **Spring**：`ApplicationEvent` + `ApplicationListener` 的事件机制。
- **响应式编程**：RxJava、Project Reactor、Node.js `EventEmitter` 都建立在观察者思想上。
- **消息**：Kafka / Redis Pub/Sub 是分布式版的发布-订阅。
- 注：`java.util.Observer` / `Observable` 自 **Java 9 起已废弃**，不应继续作为示例推荐。

---

## 模板方法 Template Method

**意图**：父类定义算法骨架，把可变步骤延迟到子类实现。

**要点**：用继承固定"算法的骨架"，把可变步骤留成抽象方法给子类实现（钩子方法）。框架里"你只需实现这几个方法"的设计大多是它。和策略的区别：**模板方法靠继承在编译期定制，策略靠组合在运行期切换**。

**真实应用**

- **Servlet**：`HttpServlet.service()` 固定分发骨架，把 `doGet()` / `doPost()` 留给子类实现。
- **Java 集合骨架**：`AbstractList`、`AbstractMap`、`AbstractSet` 实现通用骨架，只留少数抽象方法给子类。
- **Spring**：`JdbcTemplate`、`RestTemplate` 固定资源管理流程，回调只填充可变部分。
- **测试框架**：JUnit 的 `setUp()` / 测试体 / `tearDown()` 生命周期。

**语言惯用法**：Go 没有继承，地道写法是用接口承接可变步骤 + 一个包级函数固定骨架；Java 用 `final` 模板方法防止子类改写骨架。

---

## 责任链 Chain of Responsibility

**意图**：让请求沿着处理者链传递，直到有一个处理它。

**要点**：把多个处理者串成链，请求依次流过。好处是解耦发送者与处理者、可动态增删处理环节。

**真实应用**

- **Servlet / Spring Security**：`Filter` 链、Security 的过滤器链，请求依次流过每个过滤器。
- **Netty**：`ChannelPipeline` 里的 `ChannelHandler` 链。
- **Go Web**：gin / echo / 原生 `net/http` 的中间件链层层包裹。
- **Node.js**：Express 中间件 `app.use(...)`。

---

## 状态 State

**意图**：把与状态相关的行为封装进独立的状态对象，让对象行为随状态改变。

**要点**：把庞大的状态判断拆成一组状态类，每个状态类只管自己那套行为和向下一个状态的迁移，替代散落各处的状态标志位判断。是实现状态机的干净方式，也是 [[state-and-data-flow-modeling]] 中状态转移模型的标准实现落法。

**真实应用**

- **网络协议**：TCP 连接状态机（LISTEN → SYN_RCVD → ESTABLISHED…）。
- **工作流**：Spring StateMachine、订单/工单/审批状态流转引擎。
- **游戏**：角色的待机/移动/攻击/死亡状态切换。

---

## 命令 Command

**意图**：把请求封装成对象，从而支持排队、记录日志、撤销等操作。

**要点**：把"一个操作"封装成对象（含接收者、参数、执行逻辑），从而可以排队、记录日志、支持撤销/重做、组合成宏命令。调用者只依赖命令接口，完全不知道具体做了什么。

**真实应用**

- **并发任务**：`Runnable` / `Callable` 提交给 `ExecutorService`——把"要做的事"封装成对象排队执行。
- **撤销重做**：Swing 的 `javax.swing.undo`、编辑器的 undo/redo 栈。
- **GUI 动作**：Swing 的 `Action` 绑定到菜单/按钮。
- **任务队列**：消息队列里的命令消息、Sidekiq / Celery 的 Job。

---

## 迭代器 Iterator

**意图**：顺序访问集合元素，而不暴露其内部表示。

**要点**：把"遍历"的职责从集合中分离出来，调用方不必知道内部是数组、链表还是树。现代语言大多把它做进了语法（for-each / range-based for），实现约定好的协议即可。

**真实应用**

- **Java**：`Iterator` / `Iterable`，`for-each` 语法糖背后就是它。
- **C++ STL**：`begin()` / `end()` 迭代器是整个 STL 的基石，range-based for 依赖它。
- **Go**：早期靠 `for range` 遍历切片/map/channel；**Go 1.23 起**正式引入 range-over-func 迭代器（`iter.Seq`）。
- **Python**：`__iter__` / `__next__` 协议。

---

## 中介者 Mediator

**意图**：用一个中介对象封装多个对象之间的交互。

**要点**：当一组对象两两直接引用时，耦合是 N×N 的；引入中介后变成 N×1，每个对象只跟中介打交道。**代价是中介本身可能膨胀成"上帝对象"**，需要控制它的职责范围。

**真实应用**

- **GUI**：一个对话框/控制器协调其中众多控件的联动，避免控件之间两两直接引用。
- **聊天室**：服务器作为中介转发消息，客户端之间不直接通信。
- **经典类比**：塔台协调飞机起降。

---

## 备忘录 Memento

**意图**：在不破坏封装的前提下捕获并保存对象状态，以便之后恢复。

**要点**：三个角色——原发器（产生/恢复快照）、备忘录（不透明的状态载体）、管理者（只负责保管，不解读内容）。关键在于**备忘录对外不透明**，只有原发器能读它，这样才不泄漏内部结构。**易错点是快照可能很大**，需要考虑增量存储或限制历史长度。

**真实应用**

- **编辑器**：undo / redo 通过保存状态快照实现。
- **数据库**：事务的 savepoint / 回滚。
- **游戏存档**：把当前进度序列化保存，之后恢复。

---

## 访问者 Visitor

**意图**：在不修改元素类的前提下，为一组元素定义新的操作。

**要点**：把"操作"从"数据结构"里分离出来。新增操作只需加一个访问者（元素类零改动），但**新增元素类型要改所有访问者**——所以它适合"结构稳定、操作频繁增加"的场景，典型如 AST。实现依赖**双分派**（元素的 `accept` + 访问者的 `visit`）。

**真实应用**

- **编译器**：遍历抽象语法树（AST）时对不同节点执行不同操作——javac、LLVM、Clang 大量使用。
- **Java**：`Files.walkFileTree()` 的 `FileVisitor`、注解处理器的 `ElementVisitor`、字节码库 ASM 的 `ClassVisitor`。
- **文档处理**：对 XML/JSON 树执行导出、校验等多种操作而不改节点类。

---

## 解释器 Interpreter

**意图**：为一种语言定义文法表示，并给出解释执行它的解释器。

**要点**：把文法的每条规则做成一个类，组成语法树后递归求值。适合**文法简单稳定**的小型 DSL；文法一复杂，类的数量会失控，此时应换用专门的解析器生成器（ANTLR、yacc 等）。

**真实应用**

- **正则引擎**：`java.util.regex.Pattern` 把正则文法解析后解释执行。
- **表达式/规则引擎**：Spring EL（SpEL）、Drools、各种业务规则 DSL。
- **SQL**：数据库把 SQL 解析成语法树后解释/编译执行。

---

## Trade-offs

- **策略 / 命令 / 观察者**：这三个在支持一等函数的语言里常被闭包直接取代；坚持建类层次只在需要命名、组合或携带状态时才划算。
- **观察者**：换来发布方与订阅方解耦，代价是通知时序、异常隔离和退订生命周期都变成需要显式设计的问题。
- **模板方法**：换来骨架复用，代价是继承带来的编译期强耦合；子类数量增长后骨架难以演进。
- **责任链**：换来处理环节可插拔，代价是请求路径变隐式，排障时需要能列出实际链序。
- **状态**：换来状态逻辑内聚，代价是状态类数量与转移关系的维护成本。
- **中介者**：换来 N×N → N×1 的耦合下降，代价是中介容易演变成上帝对象。
- **备忘录**：换来非侵入式撤销能力，代价是快照体积与历史长度管理。
- **访问者**：换来"操作可无限扩展"，代价是元素类型固化——结构不稳定时它是错误选择。
- **解释器**：换来自定义文法的直接可执行，代价是文法复杂度上升后类数量爆炸。

## Related Pages

- [[design-patterns]]
- [[creational-patterns]]
- [[structural-patterns]]
- [[design-patterns-note]]
- [[state-and-data-flow-modeling]]
- [[abstraction-and-modeling]]
