---
title: 创建型模式
type: concept
created: 2026-07-28
updated: 2026-07-28
sources: [H. 软件架构设计/设计模式/README.md]
tags: [concept, software-architecture, design-patterns, gof, creational-patterns, oop, singleton, factory, builder, prototype]
---

创建型模式是 GoF 23 个模式中关注"对象怎么被创建"的 5 个模式，共同目标是把对象的创建过程封装起来，让调用方不必关心"怎么 new 出来的"，从而把"创建"和"使用"解耦。

## Pattern Summary

| 模式 | 意图 | 典型场景 |
|---|---|---|
| 单例 Singleton | 保证一个类只有一个实例，并提供全局访问点 | 配置中心、连接池、日志器 |
| 工厂方法 Factory Method | 定义创建接口，由子类决定实例化哪个类 | 按类型创建不同产品，屏蔽 new |
| 抽象工厂 Abstract Factory | 创建一组相关/相互依赖的对象，而不指定具体类 | 跨平台 UI 组件族、多套主题 |
| 建造者 Builder | 分步构造复杂对象，同一构造过程可产出不同表示 | 参数很多的对象、链式构造 |
| 原型 Prototype | 通过复制已有实例来创建新对象 | 创建成本高、需大量相似对象 |

---

## 单例 Singleton

**意图**：保证一个类只有一个实例，并提供全局访问点。

**要点**：最常用也最常被滥用。关键是唯一实例 + 线程安全（懒汉式需加锁，或用双重检查 / 静态内部类 / 枚举实现）。**易错点**：单例本质是全局状态，会隐藏依赖、破坏可测试性，过度使用会让系统各处偷偷耦合到它。现代实践中常用依赖注入容器管理"单实例"，而不是硬编码单例。

**真实应用**

- **Java 标准库**：`Runtime.getRuntime()`、`java.awt.Desktop.getDesktop()` 全程序唯一。
- **Spring**：Bean 默认是 `singleton` 作用域——注意这是"每容器一个"，不是 GoF 的"每 JVM 一个"。
- **Go 标准库**：`http.DefaultClient`、`http.DefaultServeMux` 是包级单实例；`database/sql.DB` 连接池通常全局一份。
- **日志库**：logrus / zap 的全局 logger、Log4j 的 `LogManager`。

**语言惯用法**：Go 用 `sync.Once` 保证懒加载 + 线程安全；Java 用静态内部类 Holder（懒加载、线程安全、无锁开销），`enum` 写法还能防反射/序列化破坏；C++ 用 Meyers Singleton（C++11 起静态局部变量初始化线程安全）并显式删除拷贝构造与赋值。

---

## 工厂方法 Factory Method

**意图**：定义一个创建对象的接口，由子类决定实例化哪个类。

**要点**：把"选择哪个具体类"的决策从业务代码里抽走，针对**一个产品**做多态创建。使用方只依赖抽象产品接口，新增产品类型时业务代码零改动。

**真实应用**

- **Java 标准库**：`Calendar.getInstance()`、`NumberFormat.getInstance()`、`Collection.iterator()`。
- **JDBC**：`DriverManager.getConnection(url)` 根据 URL 返回对应数据库的 `Connection` 实现。
- **日志门面**：SLF4J 的 `LoggerFactory.getLogger()`。
- **Go**：大量 `NewXxx()` 构造函数、`http.NewRequest()`；`image` 包按格式解码返回不同实现。

**语言惯用法**：Java/C++ 用抽象创建者定义骨架、子类覆写工厂方法；Go 的地道写法是用**函数类型充当工厂**（`type Factory func() Notification`），把工厂作为参数传入而不建继承层次。

---

## 抽象工厂 Abstract Factory

**意图**：创建一组相关/相互依赖的对象，而不指定它们的具体类。

**要点**：与工厂方法的区别在粒度——工厂方法针对"一个产品"的多态创建，抽象工厂针对**一族产品**的搭配创建，保证同族对象兼容（比如同一套主题下的按钮和输入框）。代价是**增加新产品维度时要改所有工厂实现**。

**真实应用**

- **Java XML 处理**：`DocumentBuilderFactory`、`SAXParserFactory`、`TransformerFactory` 创建一整族相关解析对象。
- **GUI 皮肤**：Swing 的 `LookAndFeel` 为同一套主题生产按钮、滚动条等配套控件。
- **C++ STL**：分配器（allocator）体系可视为为容器提供一族内存管理对象。
- **数据库驱动**：一个驱动为同一数据库生产 `Connection` / `Statement` / `ResultSet` 一整族对象。

---

## 建造者 Builder

**意图**：把复杂对象的构造过程分步进行，同一构造过程可产出不同表示。

**要点**：解决"构造函数参数爆炸"和"可选参数太多"的问题。链式 `builder.setA().setB().build()` 可读性好，还能在 `build()` 里做一次性校验、构造不可变对象。和抽象工厂的区别：建造者关注"一步步装配一个复杂对象"，工厂关注"一次性返回一个对象"。

**真实应用**

- **Java 标准库**：`StringBuilder` / `StringBuffer`、`Stream.Builder`、Java 11 `HttpRequest.newBuilder()`、`Calendar.Builder`。
- **常用库**：OkHttp 的 `Request.Builder`、Protobuf 生成代码的 `Builder`、Lombok 的 `@Builder`。
- **Go**：`strings.Builder`；以及广泛使用的**函数式选项（functional options）**模式（`NewServer(WithPort(8080), WithTLS(...))`），是 Go 里 Builder 思想的地道变体。

---

## 原型 Prototype

**意图**：通过复制已有实例来创建新对象，而不是重新构造。

**要点**：用克隆代替 new，适合初始化开销大、或运行时才知道具体类型的场景。**易错点是深拷贝 vs 浅拷贝**：浅拷贝共享引用成员，容易埋下互相污染的坑。

**真实应用**

- **Java**：`Object.clone()` + `Cloneable` 接口；`ArrayList.clone()`、`HashMap.clone()`。
- **游戏开发**：从"预制体（prefab）"克隆出大量敌人/子弹，避免重复初始化。
- **JavaScript**：语言级的原型链（prototype）就是这一思想的体现。

**语言惯用法**：Go 需要手动复制切片/map 等引用类型才能得到真正的深拷贝；Java 的 `super.clone()` 是浅拷贝，必须手动深拷贝可变字段；C++ 通常用虚 `clone()` 返回 `unique_ptr` 以支持多态克隆，容器成员由拷贝构造自动深拷。

---

## Trade-offs

- **单例**：换来全局唯一与访问便利，代价是全局状态、隐藏依赖和可测试性下降；多数场景 DI 容器是更好的替代。
- **工厂方法 / 抽象工厂**：换来"新增产品不改调用方"，代价是类数量增长；抽象工厂在**新增产品维度**时改动面最大。
- **建造者**：换来可读性与不可变对象，代价是每个被构造类型都要维护一套 Builder 样板代码（Lombok、Go 函数式选项都是为削减这份样板出现的）。
- **原型**：换来创建成本的节省，代价是必须正确处理深/浅拷贝边界，出错时表现为跨实例的隐蔽污染。

## Related Pages

- [[design-patterns]]
- [[structural-patterns]]
- [[behavioral-patterns]]
- [[design-patterns-note]]
- [[abstraction-and-modeling]]
