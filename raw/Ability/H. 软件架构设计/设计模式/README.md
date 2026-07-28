# 设计模式（Design Patterns）

> 状态：🟡 草稿 ｜ 最近更新：2026-07-27
>
> 本页整合了概念、真实应用与代码实现：GoF 全部 **23 个模式**，每个依次给出「意图 → 要点 → 真实软件中的应用 → Go / Java / C++ 实现」。代码以"一眼看懂"为目标，省略了 import、错误处理与内存回收细节。

## 目录

- 创建型：[单例](#单例-singleton) ｜ [工厂方法](#工厂方法-factory-method) ｜ [抽象工厂](#抽象工厂-abstract-factory) ｜ [建造者](#建造者-builder) ｜ [原型](#原型-prototype)
- 结构型：[适配器](#适配器-adapter) ｜ [装饰器](#装饰器-decorator) ｜ [代理](#代理-proxy) ｜ [外观](#外观-facade) ｜ [组合](#组合-composite) ｜ [桥接](#桥接-bridge) ｜ [享元](#享元-flyweight)
- 行为型：[策略](#策略-strategy) ｜ [观察者](#观察者-observer) ｜ [模板方法](#模板方法-template-method) ｜ [责任链](#责任链-chain-of-responsibility) ｜ [状态](#状态-state) ｜ [命令](#命令-command) ｜ [迭代器](#迭代器-iterator) ｜ [中介者](#中介者-mediator) ｜ [备忘录](#备忘录-memento) ｜ [访问者](#访问者-visitor) ｜ [解释器](#解释器-interpreter)
- 收尾：[关键细节 / 易错点](#关键细节--易错点) ｜ [实战与权衡](#实战与权衡) ｜ [一句话讲清楚](#一句话讲清楚)

---

## 是什么 / 解决什么问题

设计模式是**面向对象设计中反复出现的问题的、经过验证的可复用解决方案模板**。它不是能直接抄进项目的代码，而是一套"在什么场景下、用什么结构、能得到什么好处、要付出什么代价"的经验总结。最经典的来源是 GoF（Gang of Four）1994 年的《设计模式：可复用面向对象软件的基础》，归纳了 23 种模式。

它解决的核心问题是：**如何组织类与对象之间的关系，使系统在面对变化时改动最小、复用最大、耦合最低。** 几乎所有模式的本质都在做同一件事——**把"会变的部分"和"不变的部分"分开，让变化被隔离在一个可控的边界里**。

模式的价值有两层：

- **工程价值**：提供久经考验的结构，避免重复踩坑，让设计更可靠、可扩展。
- **沟通价值**：模式名是团队的通用词汇。说"这里用策略模式"，比画半天类图更快让人理解意图。

GoF 把 23 种模式按"意图"分成三类：

| 分类 | 关注点 | 一句话 |
|------|--------|--------|
| 创建型（Creational） | 对象怎么被创建 | 把"创建"和"使用"解耦 |
| 结构型（Structural） | 类与对象怎么组合 | 把小结构拼成大结构，同时保持灵活 |
| 行为型（Behavioral） | 对象之间怎么协作、分配职责 | 把"算法/职责/交互"解耦 |

## 23 个模式速查

**创建型** — 把对象的创建过程封装起来，让调用方不必关心"怎么 new 出来的"。

| 模式 | 意图 | 典型场景 |
|------|------|----------|
| 单例 Singleton | 保证一个类只有一个实例，并提供全局访问点 | 配置中心、连接池、日志器 |
| 工厂方法 Factory Method | 定义创建接口，由子类决定实例化哪个类 | 按类型创建不同产品，屏蔽 new |
| 抽象工厂 Abstract Factory | 创建一组相关/相互依赖的对象，而不指定具体类 | 跨平台 UI 组件族、多套主题 |
| 建造者 Builder | 分步构造复杂对象，同一构造过程可产出不同表示 | 参数很多的对象、链式构造 |
| 原型 Prototype | 通过复制已有实例来创建新对象 | 创建成本高、需大量相似对象 |

**结构型** — 处理类和对象的组合，用组合而非继承来构建更大、更灵活的结构。

| 模式 | 意图 | 典型场景 |
|------|------|----------|
| 适配器 Adapter | 把一个接口转换成客户端期望的另一个接口 | 对接第三方/遗留库、接口不兼容 |
| 装饰器 Decorator | 动态地给对象叠加职责，不改原类 | Java IO 流、给功能加缓存/日志/限流 |
| 代理 Proxy | 为对象提供一个替身以控制访问 | 远程代理、懒加载、权限校验、AOP |
| 外观 Facade | 为一组复杂子系统提供一个统一简化入口 | SDK 门面、封装复杂调用链 |
| 组合 Composite | 把对象组成树形结构，统一处理"单个"和"组合" | 文件系统、UI 组件树、组织架构 |
| 桥接 Bridge | 把抽象与实现分离，让两者独立变化 | 跨平台绘制、抽象维度 × 实现维度 |
| 享元 Flyweight | 共享细粒度对象以节省内存 | 大量重复对象（字符、图元、连接） |

**行为型** — 处理对象之间的职责分配和协作方式，把"算法、交互、状态流转"解耦出来。

| 模式 | 意图 | 典型场景 |
|------|------|----------|
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

# 创建型模式（Creational）

## 单例 Singleton

**意图**：保证一个类只有一个实例，并提供全局访问点。

**要点**：最常用也最常被滥用。关键是保证唯一实例 + 线程安全（懒汉式需加锁，或用双重检查 / 静态内部类 / 枚举实现）。易错点：单例本质是全局状态，会隐藏依赖、破坏可测试性，过度使用会让系统各处偷偷耦合到它。现代实践中常用依赖注入容器管理"单实例"，而不是硬编码单例。

**真实应用**

- **Java 标准库**：`Runtime.getRuntime()`、`java.awt.Desktop.getDesktop()` 全程序唯一。
- **Spring**：Bean 默认就是 `singleton` 作用域——容器为每个 Bean 维护单一实例（注意这是"每容器一个"，不是 GoF 的"每 JVM 一个"）。
- **Go 标准库**：`http.DefaultClient`、`http.DefaultServeMux` 是包级单实例；`database/sql.DB` 连接池通常全局一份。
- **日志库**：logrus / zap 的全局 logger、Log4j 的 `LogManager`。

**代码** — 场景：全局配置管理器，整个程序只允许存在一个实例。

**Go**（`sync.Once` 保证懒加载 + 线程安全）

```go
package config

import "sync"

type Config struct{ settings map[string]string }

var (
	instance *Config
	once     sync.Once
)

func GetInstance() *Config {
	once.Do(func() {
		instance = &Config{settings: map[string]string{"env": "prod"}}
	})
	return instance
}
// 使用：config.GetInstance().settings["env"]
```

**Java**（静态内部类 Holder 惯用法：懒加载、线程安全、无锁开销）

```java
public class Config {
    private final Map<String, String> settings = new HashMap<>();
    private Config() { settings.put("env", "prod"); }

    private static class Holder {
        private static final Config INSTANCE = new Config();
    }
    public static Config getInstance() { return Holder.INSTANCE; }
    public String get(String key) { return settings.get(key); }
}
// 更简单且能防反射/序列化破坏的写法是用 enum：enum Config { INSTANCE; ... }
```

**C++**（Meyers Singleton：C++11 起静态局部变量初始化线程安全）

```cpp
class Config {
public:
    static Config& getInstance() {
        static Config instance;   // 首次调用时构造，线程安全
        return instance;
    }
    std::string get(const std::string& k) { return settings_[k]; }

    Config(const Config&) = delete;            // 禁止拷贝
    Config& operator=(const Config&) = delete;
private:
    Config() { settings_["env"] = "prod"; }
    std::unordered_map<std::string, std::string> settings_;
};
```

---

## 工厂方法 Factory Method

**意图**：定义一个创建对象的接口，由子类决定实例化哪个类。

**要点**：核心是把"选择哪个具体类"这个决策从业务代码里抽走，针对"一个产品"做多态创建。使用方只依赖抽象产品接口，新增产品类型时业务代码零改动。

**真实应用**

- **Java 标准库**：`Calendar.getInstance()`、`NumberFormat.getInstance()`、`Collection.iterator()`（返回具体迭代器）。
- **JDBC**：`DriverManager.getConnection(url)` 根据 URL 返回对应数据库的 `Connection` 实现。
- **日志门面**：SLF4J 的 `LoggerFactory.getLogger()`。
- **Go**：大量 `NewXxx()` 构造函数、`http.NewRequest()`；`image` 包按格式解码返回不同实现。

**代码** — 场景：通知服务，调用方只说"发通知"，具体是邮件还是短信由子类/工厂决定。

**Go**（用函数类型充当工厂方法，是 Go 的地道写法）

```go
package notify

type Notification interface{ Send(msg string) }

type Email struct{}
func (Email) Send(msg string) { fmt.Println("Email:", msg) }

type SMS struct{}
func (SMS) Send(msg string) { fmt.Println("SMS:", msg) }

type Factory func() Notification

func NewEmail() Notification { return Email{} }
func NewSMS() Notification   { return SMS{} }

func Notify(create Factory, msg string) { create().Send(msg) }
// 使用：Notify(NewEmail, "hello")
```

**Java**（抽象创建者定义骨架，子类决定造哪种产品）

```java
interface Notification { void send(String msg); }
class Email implements Notification { public void send(String m){ System.out.println("Email: "+m);} }
class SMS   implements Notification { public void send(String m){ System.out.println("SMS: "+m);} }

abstract class NotifyService {
    protected abstract Notification create();     // 工厂方法
    public void notify(String msg) { create().send(msg); }
}
class EmailService extends NotifyService {
    protected Notification create() { return new Email(); }
}
class SMSService extends NotifyService {
    protected Notification create() { return new SMS(); }
}
```

**C++**

```cpp
struct Notification { virtual void send(const std::string&) = 0; virtual ~Notification() = default; };
struct Email : Notification { void send(const std::string& m) override { std::cout<<"Email: "<<m<<"\n"; } };
struct SMS   : Notification { void send(const std::string& m) override { std::cout<<"SMS: "<<m<<"\n"; } };

struct NotifyService {
    virtual std::unique_ptr<Notification> create() = 0;   // 工厂方法
    void notify(const std::string& msg) { create()->send(msg); }
    virtual ~NotifyService() = default;
};
struct EmailService : NotifyService {
    std::unique_ptr<Notification> create() override { return std::make_unique<Email>(); }
};
```

---

## 抽象工厂 Abstract Factory

**意图**：创建一组相关/相互依赖的对象，而不指定它们的具体类。

**要点**：与工厂方法的区别在于粒度——工厂方法针对"一个产品"的多态创建，抽象工厂针对"一族产品"的搭配创建，保证同族对象兼容（比如同一套主题下的按钮和输入框）。代价是增加新产品维度时要改所有工厂实现。

**真实应用**

- **Java XML 处理**：`DocumentBuilderFactory`、`SAXParserFactory`、`TransformerFactory` 创建一整族相关解析对象。
- **GUI 皮肤**：Swing 的 `LookAndFeel` 为同一套主题生产按钮、滚动条等配套控件。
- **C++ STL**：分配器（allocator）体系可视为为容器提供一族内存管理对象。
- **数据库驱动**：一个驱动为同一数据库生产 `Connection`/`Statement`/`ResultSet` 一整族对象。

**代码** — 场景：跨平台 UI，同一个工厂生产**配套的一族**控件（Windows 风格的按钮 + 复选框，或 Mac 风格的），保证同族控件风格一致。

**Go**

```go
package abstractfactory

// 产品族：按钮 + 复选框
type Button interface{ Render() }
type Checkbox interface{ Check() }

// 抽象工厂：一次生产一整族配套产品
type GUIFactory interface {
	CreateButton() Button
	CreateCheckbox() Checkbox
}

type WinButton struct{}
func (WinButton) Render() { fmt.Println("Windows 按钮") }
type WinCheckbox struct{}
func (WinCheckbox) Check() { fmt.Println("Windows 复选框") }

type WinFactory struct{}
func (WinFactory) CreateButton() Button     { return WinButton{} }
func (WinFactory) CreateCheckbox() Checkbox { return WinCheckbox{} }

type MacButton struct{}
func (MacButton) Render() { fmt.Println("Mac 按钮") }
type MacCheckbox struct{}
func (MacCheckbox) Check() { fmt.Println("Mac 复选框") }

type MacFactory struct{}
func (MacFactory) CreateButton() Button     { return MacButton{} }
func (MacFactory) CreateCheckbox() Checkbox { return MacCheckbox{} }
// 使用：var f GUIFactory = MacFactory{}; f.CreateButton().Render()
```

**Java**

```java
interface Button { void render(); }
interface Checkbox { void check(); }

interface GUIFactory {          // 抽象工厂
    Button createButton();
    Checkbox createCheckbox();
}

class WinButton implements Button { public void render(){ System.out.println("Windows 按钮"); } }
class WinCheckbox implements Checkbox { public void check(){ System.out.println("Windows 复选框"); } }
class WinFactory implements GUIFactory {
    public Button createButton(){ return new WinButton(); }
    public Checkbox createCheckbox(){ return new WinCheckbox(); }
}

class MacButton implements Button { public void render(){ System.out.println("Mac 按钮"); } }
class MacCheckbox implements Checkbox { public void check(){ System.out.println("Mac 复选框"); } }
class MacFactory implements GUIFactory {
    public Button createButton(){ return new MacButton(); }
    public Checkbox createCheckbox(){ return new MacCheckbox(); }
}
```

**C++**

```cpp
struct Button { virtual void render() = 0; virtual ~Button() = default; };
struct Checkbox { virtual void check() = 0; virtual ~Checkbox() = default; };

struct GUIFactory {             // 抽象工厂
    virtual std::unique_ptr<Button> createButton() = 0;
    virtual std::unique_ptr<Checkbox> createCheckbox() = 0;
    virtual ~GUIFactory() = default;
};

struct WinButton : Button { void render() override { std::cout << "Windows 按钮\n"; } };
struct WinCheckbox : Checkbox { void check() override { std::cout << "Windows 复选框\n"; } };
struct WinFactory : GUIFactory {
    std::unique_ptr<Button> createButton() override { return std::make_unique<WinButton>(); }
    std::unique_ptr<Checkbox> createCheckbox() override { return std::make_unique<WinCheckbox>(); }
};
```

---

## 建造者 Builder

**意图**：把复杂对象的构造过程分步进行，同一构造过程可产出不同表示。

**要点**：解决"构造函数参数爆炸"和"可选参数太多"的问题。链式 `builder.setA().setB().build()` 可读性好，还能在 `build()` 里做一次性校验、构造不可变对象。和抽象工厂的区别：建造者关注"一步步装配一个复杂对象"，工厂关注"一次性返回一个对象"。

**真实应用**

- **Java 标准库**：`StringBuilder`/`StringBuffer`、`Stream.Builder`、Java 11 `HttpRequest.newBuilder()`、`Calendar.Builder`。
- **常用库**：OkHttp 的 `Request.Builder`、Protobuf 生成代码的 `Builder`、Lombok 的 `@Builder`。
- **Go**：`strings.Builder`；以及广泛使用的 **函数式选项（functional options）** 模式（`NewServer(WithPort(8080), WithTLS(...))`），是 Go 里 Builder 思想的地道变体。

**代码** — 场景：组装一台电脑，配置项多且大多可选。链式调用可读性好，还能在 build 时统一校验。

**Go**

```go
package build

type Computer struct {
	CPU string
	RAM int
	GPU string
}

type Builder struct{ c Computer }

func New() *Builder                        { return &Builder{} }
func (b *Builder) CPU(v string) *Builder   { b.c.CPU = v; return b }
func (b *Builder) RAM(v int) *Builder      { b.c.RAM = v; return b }
func (b *Builder) GPU(v string) *Builder   { b.c.GPU = v; return b }
func (b *Builder) Build() Computer         { return b.c }
// 使用：pc := New().CPU("i9").RAM(32).GPU("RTX4090").Build()
```

**Java**（静态内部 Builder，构造不可变对象）

```java
public class Computer {
    private final String cpu; private final int ram; private final String gpu;
    private Computer(Builder b) { cpu = b.cpu; ram = b.ram; gpu = b.gpu; }

    public static class Builder {
        private String cpu; private int ram; private String gpu;
        public Builder cpu(String v){ this.cpu = v; return this; }
        public Builder ram(int v)   { this.ram = v; return this; }
        public Builder gpu(String v){ this.gpu = v; return this; }
        public Computer build()     { return new Computer(this); }
    }
}
// 使用：new Computer.Builder().cpu("i9").ram(32).gpu("RTX4090").build();
```

**C++**

```cpp
struct Computer { std::string cpu; int ram = 0; std::string gpu; };

class ComputerBuilder {
    Computer c_;
public:
    ComputerBuilder& cpu(std::string v){ c_.cpu = std::move(v); return *this; }
    ComputerBuilder& ram(int v)        { c_.ram = v; return *this; }
    ComputerBuilder& gpu(std::string v){ c_.gpu = std::move(v); return *this; }
    Computer build() { return c_; }
};
// 使用：auto pc = ComputerBuilder().cpu("i9").ram(32).gpu("RTX4090").build();
```

---

## 原型 Prototype

**意图**：通过复制已有实例来创建新对象，而不是重新构造。

**要点**：用克隆代替 new，适合初始化开销大、或运行时才知道具体类型的场景。易错点是**深拷贝 vs 浅拷贝**：浅拷贝共享引用成员，容易埋下互相污染的坑。

**真实应用**

- **Java**：`Object.clone()` + `Cloneable` 接口；`ArrayList.clone()`、`HashMap.clone()`。
- **游戏开发**：从"预制体（prefab）"克隆出大量敌人/子弹，避免重复初始化。
- **JavaScript**：语言级的原型链（prototype）就是这一思想的体现。

**代码** — 场景：游戏里以一个怪物模板为原型，克隆出大量相似怪物再微调属性，省去重复初始化。注意**深拷贝**可变字段。

**Go**

```go
package prototype

type Monster struct {
	Name   string
	HP     int
	Skills []string
}

// Clone 返回深拷贝（切片需单独复制，否则会共享底层数组）
func (m *Monster) Clone() *Monster {
	skills := make([]string, len(m.Skills))
	copy(skills, m.Skills)
	return &Monster{Name: m.Name, HP: m.HP, Skills: skills}
}
// 使用：elite := goblin.Clone(); elite.Name = "精英哥布林"; elite.HP *= 2
```

**Java**

```java
class Monster implements Cloneable {
    String name;
    int hp;
    List<String> skills;

    @Override
    public Monster clone() {
        try {
            Monster copy = (Monster) super.clone();     // 浅拷贝
            copy.skills = new ArrayList<>(this.skills);  // 手动深拷贝可变字段
            return copy;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(e);
        }
    }
}
```

**C++**（用虚 `clone()` 支持多态克隆）

```cpp
struct Monster {
    std::string name;
    int hp = 0;
    std::vector<std::string> skills;

    virtual std::unique_ptr<Monster> clone() const {
        return std::make_unique<Monster>(*this); // 拷贝构造，vector 自动深拷贝
    }
    virtual ~Monster() = default;
};
```

---

# 结构型模式（Structural）

## 适配器 Adapter

**意图**：把一个接口转换成客户端期望的另一个接口。

**要点**：最直观的"转接头"。当已有类的接口和你要用的接口对不上，又不能改它（第三方、遗留代码），就包一层做转换。分对象适配器（组合，推荐）和类适配器（多继承）。

**真实应用**

- **Java IO**：`InputStreamReader` 把字节流适配成字符流，`OutputStreamWriter` 反之。
- **集合**：`Arrays.asList()` 把数组适配成 `List` 视图。
- **Spring MVC**：`HandlerAdapter` 把形态各异的 Controller 适配成统一的调用接口。
- **日志桥接**：`log4j-over-slf4j`、`jul-to-slf4j` 把旧日志 API 适配到 SLF4J。
- **Go**：`http.HandlerFunc` 把普通函数适配成 `http.Handler` 接口。

**代码** — 场景：系统期望 `Logger.Log(msg)`，但手头只有第三方库的 `WriteLine(level, text)`，且不能改它。包一层做转换。

**Go**

```go
package adapter

type Logger interface{ Log(msg string) }

// 第三方库（不可修改）
type ThirdPartyLog struct{}
func (ThirdPartyLog) WriteLine(level, text string) {
	fmt.Printf("[%s] %s\n", level, text)
}

// 适配器：把第三方接口转成 Logger
type LogAdapter struct{ tp ThirdPartyLog }
func (a LogAdapter) Log(msg string) { a.tp.WriteLine("INFO", msg) }
```

**Java**

```java
interface Logger { void log(String msg); }

class ThirdPartyLog {  // 第三方库，不可改
    public void writeLine(String level, String text) {
        System.out.println("[" + level + "] " + text);
    }
}

class LogAdapter implements Logger {
    private final ThirdPartyLog tp = new ThirdPartyLog();
    public void log(String msg) { tp.writeLine("INFO", msg); }
}
```

**C++**

```cpp
struct Logger { virtual void log(const std::string&) = 0; virtual ~Logger() = default; };

struct ThirdPartyLog {  // 第三方库
    void writeLine(const std::string& level, const std::string& text) {
        std::cout << "[" << level << "] " << text << "\n";
    }
};

class LogAdapter : public Logger {
    ThirdPartyLog tp_;
public:
    void log(const std::string& msg) override { tp_.writeLine("INFO", msg); }
};
```

---

## 装饰器 Decorator

**意图**：动态地给对象叠加职责，而不修改原类。

**要点**：用组合在运行时叠加功能，避免"为每种功能组合都建一个子类"导致的类爆炸。和代理的区别：装饰器强调"增强功能"，代理强调"控制访问"，结构相似但意图不同。

**真实应用**

- **Java IO**（教科书级例子）：`new BufferedReader(new InputStreamReader(new FileInputStream(f)))`，一层层叠加缓冲、编码、数据类型等能力。
- **集合包装**：`Collections.synchronizedList()`、`unmodifiableList()` 给集合动态加上同步/只读能力。
- **Servlet**：`HttpServletRequestWrapper` / `HttpServletResponseWrapper` 用于包装增强请求响应。
- **Go**：`io` 包的各种包装器（`bufio.NewReader`、`gzip.NewReader`）；HTTP 中间件层层包裹 `http.Handler`。

**代码** — 场景：点咖啡，基础是浓缩，可动态叠加奶、糖等配料，价格和描述随之累加。

**Go**

```go
package decorator

type Beverage interface {
	Cost() float64
	Desc() string
}

type Espresso struct{}
func (Espresso) Cost() float64 { return 10 }
func (Espresso) Desc() string  { return "浓缩" }

type Milk struct{ b Beverage }   // 装饰器持有被装饰对象
func (m Milk) Cost() float64 { return m.b.Cost() + 2 }
func (m Milk) Desc() string  { return m.b.Desc() + "+奶" }

type Sugar struct{ b Beverage }
func (s Sugar) Cost() float64 { return s.b.Cost() + 1 }
func (s Sugar) Desc() string  { return s.b.Desc() + "+糖" }

// 使用：var b Beverage = Sugar{Milk{Espresso{}}} // 浓缩+奶+糖，13 元
```

**Java**

```java
interface Beverage { double cost(); String desc(); }
class Espresso implements Beverage {
    public double cost(){ return 10; } public String desc(){ return "浓缩"; }
}
abstract class Decorator implements Beverage {
    protected final Beverage b;
    protected Decorator(Beverage b){ this.b = b; }
}
class Milk extends Decorator {
    public Milk(Beverage b){ super(b); }
    public double cost(){ return b.cost() + 2; }
    public String desc(){ return b.desc() + "+奶"; }
}
// 使用：new Milk(new Espresso())
```

**C++**

```cpp
struct Beverage {
    virtual double cost() = 0;
    virtual std::string desc() = 0;
    virtual ~Beverage() = default;
};
struct Espresso : Beverage {
    double cost() override { return 10; }
    std::string desc() override { return "浓缩"; }
};
struct Milk : Beverage {
    std::unique_ptr<Beverage> b;
    explicit Milk(std::unique_ptr<Beverage> inner) : b(std::move(inner)) {}
    double cost() override { return b->cost() + 2; }
    std::string desc() override { return b->desc() + "+奶"; }
};
// 使用：auto d = std::make_unique<Milk>(std::make_unique<Espresso>());
```

---

## 代理 Proxy

**意图**：为对象提供一个替身，以控制对它的访问。

**要点**：在真实对象前放一个同接口的替身，用于延迟加载、访问控制、远程调用、缓存等。Spring AOP、RPC stub 都是代理的实际应用。

**真实应用**

- **Java 动态代理**：`java.lang.reflect.Proxy`——**Spring AOP、MyBatis Mapper、RMI stub、Hibernate 懒加载** 的底层机制（CGLIB 则是子类代理）。
- **RPC**：gRPC / Thrift 客户端生成的 stub 就是远程代理。
- **Go**：`httputil.ReverseProxy` 是反向代理的直接实现。

**代码** — 场景：图片懒加载。真正要显示时才从磁盘读取，避免一开始就付出昂贵的加载成本。

**Go**

```go
package proxy

type Image interface{ Display() }

type RealImage struct{ filename string }
func NewRealImage(f string) *RealImage {
	fmt.Println("从磁盘加载:", f) // 昂贵操作
	return &RealImage{f}
}
func (r *RealImage) Display() { fmt.Println("显示:", r.filename) }

type ProxyImage struct {
	filename string
	real     *RealImage
}
func (p *ProxyImage) Display() {
	if p.real == nil {
		p.real = NewRealImage(p.filename) // 第一次用到才加载
	}
	p.real.Display()
}
```

**Java**

```java
interface Image { void display(); }

class RealImage implements Image {
    private final String filename;
    RealImage(String f){ filename = f; System.out.println("从磁盘加载: " + f); }
    public void display(){ System.out.println("显示: " + filename); }
}

class ProxyImage implements Image {
    private final String filename;
    private RealImage real;
    ProxyImage(String f){ filename = f; }
    public void display() {
        if (real == null) real = new RealImage(filename); // 懒加载
        real.display();
    }
}
```

**C++**

```cpp
struct Image { virtual void display() = 0; virtual ~Image() = default; };

class RealImage : public Image {
    std::string filename_;
public:
    explicit RealImage(std::string f) : filename_(std::move(f)) {
        std::cout << "从磁盘加载: " << filename_ << "\n";
    }
    void display() override { std::cout << "显示: " << filename_ << "\n"; }
};

class ProxyImage : public Image {
    std::string filename_;
    std::unique_ptr<RealImage> real_;
public:
    explicit ProxyImage(std::string f) : filename_(std::move(f)) {}
    void display() override {
        if (!real_) real_ = std::make_unique<RealImage>(filename_);
        real_->display();
    }
};
```

---

## 外观 Facade

**意图**：为一组复杂子系统提供一个统一的简化入口。

**要点**：给复杂子系统一个"简单的前台"，降低使用方的认知成本和耦合。它不隐藏子系统（仍可直接用），只是提供更方便的默认路径。

**真实应用**

- **SLF4J**：名字里的 F 就是 Facade——为 Log4j/Logback 等多种实现提供统一的日志门面。
- **Spring**：`JdbcTemplate` 把"获取连接→建 Statement→执行→处理结果→释放资源"这一长串 JDBC 操作封装成一两个方法。
- **各类 SDK**：云厂商 Client SDK 把复杂的签名、重试、序列化封装成简单的方法调用。

**代码** — 场景：家庭影院，一个 `WatchMovie()` 就搞定调暗灯光、开投影、开音响这一串操作。

**Go**

```go
package facade

type Projector struct{}
func (Projector) On() { fmt.Println("投影仪开") }
type Sound struct{}
func (Sound) On() { fmt.Println("音响开") }
type Light struct{}
func (Light) Dim() { fmt.Println("灯光调暗") }

// 外观：把复杂子系统封装成一个简单入口
type HomeTheater struct {
	p Projector
	s Sound
	l Light
}
func (h HomeTheater) WatchMovie() {
	h.l.Dim()
	h.p.On()
	h.s.On()
	fmt.Println("开始观影")
}
```

**Java**

```java
class Projector { void on(){ System.out.println("投影仪开"); } }
class Sound     { void on(){ System.out.println("音响开"); } }
class Light     { void dim(){ System.out.println("灯光调暗"); } }

class HomeTheater {
    private final Projector p = new Projector();
    private final Sound s = new Sound();
    private final Light l = new Light();
    public void watchMovie() {
        l.dim(); p.on(); s.on();
        System.out.println("开始观影");
    }
}
```

**C++**

```cpp
struct Projector { void on(){ std::cout << "投影仪开\n"; } };
struct Sound     { void on(){ std::cout << "音响开\n"; } };
struct Light     { void dim(){ std::cout << "灯光调暗\n"; } };

class HomeTheater {
    Projector p_; Sound s_; Light l_;
public:
    void watchMovie() {
        l_.dim(); p_.on(); s_.on();
        std::cout << "开始观影\n";
    }
};
```

---

## 组合 Composite

**意图**：把对象组成树形结构，让客户端统一处理"单个对象"和"对象组合"。

**要点**：让"叶子节点"和"容器节点"实现同一接口，客户端可以用统一方式递归处理整棵树，是处理树形结构的标准解法。

**真实应用**

- **GUI**：AWT/Swing 的 `Container`/`Component`、Android 的 `ViewGroup`/`View`、React 组件树——容器和叶子实现同一接口，可递归处理。
- **DOM**：HTML/XML 的文档树。
- **文件系统**：目录与文件的树形结构。

**代码** — 场景：文件系统，文件（叶子）和文件夹（容器）实现同一接口，统一递归计算总大小。

**Go**

```go
package composite

type Node interface{ Size() int }

type File struct{ size int }
func (f File) Size() int { return f.size }

type Folder struct{ children []Node }
func (f *Folder) Add(n Node) { f.children = append(f.children, n) }
func (f *Folder) Size() int {
	total := 0
	for _, c := range f.children {
		total += c.Size() // 无需区分文件还是文件夹，递归即可
	}
	return total
}
```

**Java**

```java
interface Node { int size(); }

class File implements Node {
    private final int size;
    File(int size){ this.size = size; }
    public int size(){ return size; }
}

class Folder implements Node {
    private final List<Node> children = new ArrayList<>();
    public void add(Node n){ children.add(n); }
    public int size(){
        int total = 0;
        for (Node c : children) total += c.size(); // 统一处理
        return total;
    }
}
```

**C++**

```cpp
struct Node { virtual int size() = 0; virtual ~Node() = default; };

class File : public Node {
    int size_;
public:
    explicit File(int s) : size_(s) {}
    int size() override { return size_; }
};

class Folder : public Node {
    std::vector<std::unique_ptr<Node>> children_;
public:
    void add(std::unique_ptr<Node> n){ children_.push_back(std::move(n)); }
    int size() override {
        int total = 0;
        for (auto& c : children_) total += c->size();
        return total;
    }
};
```

---

## 桥接 Bridge

**意图**：把抽象与实现分离，让两者可以独立变化。

**要点**：当一个东西有两个独立变化的维度时（比如"遥控器种类"× "设备种类"），继承会导致 M×N 的类爆炸。桥接用组合把两个维度接起来，各自独立扩展。和适配器的区别：适配器是"事后补救"已有的不兼容接口，桥接是"事前设计"两个维度的分离。

**真实应用**

- **JDBC**：`java.sql.Driver` 抽象接口与各数据库厂商实现分离，让"数据库访问"这个抽象与"具体数据库"这个实现各自独立演化。
- **AWT**：组件（抽象）与平台 peer（实现）分离，实现跨平台。
- **图形/渲染库**：抽象的绘图 API × 可替换的渲染后端（OpenGL/Vulkan/软件渲染）。

**代码** — 场景：遥控器（抽象）× 设备（实现）。遥控器可以出高级版，设备可以加新型号，两个维度各自独立扩展，不必两两组合建类。

**Go**

```go
package bridge

// 实现维度：设备
type Device interface {
	On()
	Off()
}

type TV struct{}
func (TV) On()  { fmt.Println("电视开") }
func (TV) Off() { fmt.Println("电视关") }

type Radio struct{}
func (Radio) On()  { fmt.Println("收音机开") }
func (Radio) Off() { fmt.Println("收音机关") }

// 抽象维度：遥控器，通过组合"桥接"到设备
type Remote struct{ device Device }
func (r Remote) TogglePower() { r.device.On() }
```

**Java**

```java
interface Device { void on(); void off(); }
class TV implements Device {
    public void on(){ System.out.println("电视开"); }
    public void off(){ System.out.println("电视关"); }
}
class Radio implements Device {
    public void on(){ System.out.println("收音机开"); }
    public void off(){ System.out.println("收音机关"); }
}

class Remote {                       // 抽象持有实现引用（桥）
    protected final Device device;
    Remote(Device device){ this.device = device; }
    public void togglePower(){ device.on(); }
}
class AdvancedRemote extends Remote {  // 抽象维度可独立扩展
    AdvancedRemote(Device d){ super(d); }
    public void mute(){ device.off(); }
}
```

**C++**

```cpp
struct Device {
    virtual void on() = 0;
    virtual void off() = 0;
    virtual ~Device() = default;
};
struct TV : Device {
    void on() override { std::cout << "电视开\n"; }
    void off() override { std::cout << "电视关\n"; }
};

class Remote {
protected:
    std::shared_ptr<Device> device_;
public:
    explicit Remote(std::shared_ptr<Device> d) : device_(std::move(d)) {}
    void togglePower(){ device_->on(); }
    virtual ~Remote() = default;
};
```

---

## 享元 Flyweight

**意图**：通过共享细粒度对象来节省内存。

**要点**：把对象状态拆成**内部状态**（可共享、不变，存在享元里）和**外部状态**（随上下文变化，调用时传入）。通常配一个享元工厂做缓存池。前提是内部状态必须不可变，否则共享会互相污染。

**真实应用**

- **Java 缓存**（经典例子）：`Integer.valueOf()` 缓存 -128~127、`String` 常量池、`Boolean.TRUE/FALSE`、`Character` 缓存——共享不可变小对象省内存。
- **字体渲染**：同一字形（glyph）在多处复用，只存一份轮廓数据。
- **游戏**：大量相同的树、粒子、贴图共享同一份内部状态。

**代码** — 场景：文本编辑器里成千上万个字符，共享同一份"字形"对象（内部状态），坐标等外部状态由外部传入，大幅省内存。

**Go**

```go
package flyweight

// 享元：只存可共享的内部状态
type Glyph struct{ char rune }
func (g *Glyph) Draw(x, y int) { // 外部状态由参数传入
	fmt.Printf("在(%d,%d)画字符 %c\n", x, y, g.char)
}

// 享元工厂：同一字符只创建一个 Glyph
type GlyphFactory struct{ pool map[rune]*Glyph }
func NewGlyphFactory() *GlyphFactory { return &GlyphFactory{pool: map[rune]*Glyph{}} }
func (f *GlyphFactory) Get(c rune) *Glyph {
	if g, ok := f.pool[c]; ok {
		return g
	}
	g := &Glyph{char: c}
	f.pool[c] = g
	return g
}
```

**Java**

```java
class Glyph {                    // 享元：只存内部状态
    private final char c;
    Glyph(char c){ this.c = c; }
    public void draw(int x, int y){  // 外部状态作参数
        System.out.printf("在(%d,%d)画字符 %c%n", x, y, c);
    }
}

class GlyphFactory {
    private final Map<Character, Glyph> pool = new HashMap<>();
    public Glyph get(char c){
        return pool.computeIfAbsent(c, Glyph::new); // 同字符复用同一实例
    }
}
```

**C++**

```cpp
class Glyph {
    char c_;
public:
    explicit Glyph(char c) : c_(c) {}
    void draw(int x, int y){
        std::cout << "在(" << x << "," << y << ")画字符 " << c_ << "\n";
    }
};

class GlyphFactory {
    std::unordered_map<char, std::shared_ptr<Glyph>> pool_;
public:
    std::shared_ptr<Glyph> get(char c){
        auto it = pool_.find(c);
        if (it != pool_.end()) return it->second;
        return pool_[c] = std::make_shared<Glyph>(c);
    }
};
```

---

# 行为型模式（Behavioral）

## 策略 Strategy

**意图**：封装一族可互换的算法，让它们能在运行时切换。

**要点**：最常用的行为型模式之一。把"做同一件事的不同做法"抽成可替换的策略对象，用组合注入，替代一长串 `if/else` 或 `switch`。新增算法只需加一个策略类，符合开闭原则。它和状态模式结构几乎一样，区别在意图：策略由外部主动选择、策略之间互不知晓；状态由内部驱动流转、状态之间会互相切换。

**真实应用**

- **Java 排序**（教科书级例子）：把 `Comparator` 传给 `Collections.sort()` / `Arrays.sort()`，运行时切换比较策略。
- **线程池**：`ThreadPoolExecutor` 的 `RejectedExecutionHandler`（AbortPolicy / CallerRunsPolicy 等）是可替换的拒绝策略。
- **Go**：`sort.Slice(s, less)` 的 `less` 函数、`http.Client` 的 `RoundTripper`（可替换传输策略）。
- **加密/压缩**：可插拔的算法实现。

**代码** — 场景：购物车结算，运行时切换不同折扣算法（无折扣 / 打折 / 满减）。

**Go**

```go
package strategy

type Discount interface{ Apply(price float64) float64 }

type NoDiscount struct{}
func (NoDiscount) Apply(p float64) float64 { return p }

type PercentOff struct{ Rate float64 }
func (d PercentOff) Apply(p float64) float64 { return p * (1 - d.Rate) }

type Cart struct{ discount Discount }
func (c *Cart) SetDiscount(d Discount)       { c.discount = d }
func (c *Cart) Checkout(price float64) float64 { return c.discount.Apply(price) }
```

**Java**

```java
interface Discount { double apply(double price); }
class NoDiscount implements Discount { public double apply(double p){ return p; } }
class PercentOff implements Discount {
    private final double rate;
    PercentOff(double rate){ this.rate = rate; }
    public double apply(double p){ return p * (1 - rate); }
}
class Cart {
    private Discount discount = new NoDiscount();
    public void setDiscount(Discount d){ this.discount = d; }
    public double checkout(double price){ return discount.apply(price); }
}
```

**C++**（现代 C++ 里策略常直接用 `std::function`，省去一堆类）

```cpp
class Cart {
    std::function<double(double)> discount_ = [](double p){ return p; };
public:
    void setDiscount(std::function<double(double)> d){ discount_ = std::move(d); }
    double checkout(double price){ return discount_(price); }
};
// 使用：cart.setDiscount([](double p){ return p * 0.8; }); // 打八折
```

---

## 观察者 Observer

**意图**：定义一对多依赖，当一个对象状态变化时自动通知所有订阅者。

**要点**：发布-订阅的基础，实现松耦合的一对多通知。事件系统、响应式编程、消息总线都建立在这个思想上。易错点：同步通知可能拖慢主流程、观察者异常会影响发布方、忘记退订会内存泄漏。

**真实应用**

- **GUI 事件**：Swing 的 `ActionListener`、`PropertyChangeListener`；浏览器 `addEventListener`。
- **Spring**：`ApplicationEvent` + `ApplicationListener` 的事件机制。
- **响应式编程**：RxJava、Project Reactor、Node.js `EventEmitter` 都建立在观察者思想上。
- **消息**：Kafka / Redis Pub/Sub 是分布式版的发布-订阅。
- 注：`java.util.Observer` / `Observable` 自 Java 9 起已废弃，别再用。

**代码** — 场景：天气站温度更新时，自动通知所有已订阅的显示屏。

**Go**

```go
package observer

type Observer interface{ Update(temp float64) }

type Display struct{ name string }
func (d Display) Update(temp float64) {
	fmt.Printf("%s 显示温度: %.1f\n", d.name, temp)
}

type WeatherStation struct{ observers []Observer }
func (w *WeatherStation) Subscribe(o Observer) { w.observers = append(w.observers, o) }
func (w *WeatherStation) SetTemp(temp float64) {
	for _, o := range w.observers {
		o.Update(temp)
	}
}
```

**Java**

```java
interface Observer { void update(double temp); }
class Display implements Observer {
    private final String name;
    Display(String name){ this.name = name; }
    public void update(double temp){
        System.out.printf("%s 显示温度: %.1f%n", name, temp);
    }
}
class WeatherStation {
    private final List<Observer> observers = new ArrayList<>();
    public void subscribe(Observer o){ observers.add(o); }
    public void setTemp(double temp){
        for (Observer o : observers) o.update(temp);
    }
}
```

**C++**

```cpp
struct Observer { virtual void update(double temp) = 0; virtual ~Observer() = default; };

class Display : public Observer {
    std::string name_;
public:
    explicit Display(std::string n) : name_(std::move(n)) {}
    void update(double temp) override {
        std::cout << name_ << " 显示温度: " << temp << "\n";
    }
};

class WeatherStation {
    std::vector<Observer*> observers_;
public:
    void subscribe(Observer* o){ observers_.push_back(o); }
    void setTemp(double temp){ for (auto* o : observers_) o->update(temp); }
};
```

---

## 模板方法 Template Method

**意图**：父类定义算法骨架，把可变步骤延迟到子类实现。

**要点**：用继承固定"算法的骨架"，把可变步骤留成抽象方法给子类实现（钩子方法）。框架里"你只需实现这几个方法"的设计大多是它。和策略的区别：模板方法靠继承在编译期定制，策略靠组合在运行期切换。

**真实应用**

- **Servlet**：`HttpServlet.service()` 固定分发骨架，把 `doGet()`/`doPost()` 留给子类实现。
- **Java 集合骨架**：`AbstractList`、`AbstractMap`、`AbstractSet` 实现通用骨架，只留少数抽象方法给子类。
- **Spring**：`JdbcTemplate`、`RestTemplate` 固定资源管理流程，回调只填充可变部分。
- **测试框架**：JUnit 的 `setUp()` / 测试体 / `tearDown()` 生命周期。

**代码** — 场景：冲泡饮料，流程骨架固定（烧水→冲泡→倒杯→加料），其中"冲泡"和"加料"由具体饮料决定。

**Go**（无继承，用接口承接可变步骤 + 一个函数固定骨架）

```go
package template

type Brewer interface {
	Brew()          // 可变步骤
	AddCondiments() // 可变步骤
}

// 模板方法：固定骨架
func PrepareRecipe(b Brewer) {
	boilWater()
	b.Brew()
	pourInCup()
	b.AddCondiments()
}
func boilWater() { fmt.Println("烧水") }
func pourInCup() { fmt.Println("倒入杯中") }

type Coffee struct{}
func (Coffee) Brew()          { fmt.Println("冲泡咖啡粉") }
func (Coffee) AddCondiments() { fmt.Println("加糖和奶") }
```

**Java**（`final` 模板方法固定骨架，抽象方法交给子类）

```java
abstract class Beverage {
    public final void prepare() {   // 模板方法
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }
    private void boilWater(){ System.out.println("烧水"); }
    private void pourInCup(){ System.out.println("倒入杯中"); }
    protected abstract void brew();
    protected abstract void addCondiments();
}
class Coffee extends Beverage {
    protected void brew(){ System.out.println("冲泡咖啡粉"); }
    protected void addCondiments(){ System.out.println("加糖和奶"); }
}
```

**C++**

```cpp
class Beverage {
public:
    void prepare() {          // 模板方法（非虚，骨架固定）
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }
    virtual ~Beverage() = default;
protected:
    virtual void brew() = 0;
    virtual void addCondiments() = 0;
private:
    void boilWater(){ std::cout << "烧水\n"; }
    void pourInCup(){ std::cout << "倒入杯中\n"; }
};
class Coffee : public Beverage {
protected:
    void brew() override { std::cout << "冲泡咖啡粉\n"; }
    void addCondiments() override { std::cout << "加糖和奶\n"; }
};
```

---

## 责任链 Chain of Responsibility

**意图**：让请求沿着处理者链传递，直到有一个处理它。

**要点**：把多个处理者串成链，请求依次流过。好处是解耦发送者与处理者、可动态增删处理环节。Web 框架的中间件 / 过滤器链、审批流都是它的应用。

**真实应用**

- **Servlet / Spring Security**：`Filter` 链、Security 的过滤器链，请求依次流过每个过滤器。
- **Netty**：`ChannelPipeline` 里的 `ChannelHandler` 链。
- **Go Web**：gin / echo / 原生 `net/http` 的中间件链层层包裹。
- **Node.js**：Express 中间件 `app.use(...)`。

**代码** — 场景：请假审批，按天数逐级上报——组长批 1 天内，经理批 3 天内，更多则继续往上传。

**Go**

```go
package chain

type Approver interface {
	SetNext(Approver)
	Approve(days int)
}

type base struct{ next Approver }
func (b *base) SetNext(a Approver) { b.next = a }
func (b *base) forward(days int) {
	if b.next != nil {
		b.next.Approve(days)
	}
}

type TeamLead struct{ base }
func (t *TeamLead) Approve(days int) {
	if days <= 1 {
		fmt.Println("组长批准")
	} else {
		t.forward(days)
	}
}

type Manager struct{ base }
func (m *Manager) Approve(days int) {
	if days <= 3 {
		fmt.Println("经理批准")
	} else {
		m.forward(days)
	}
}
// 使用：lead.SetNext(mgr); lead.Approve(2) → 经理批准
```

**Java**

```java
abstract class Approver {
    protected Approver next;
    public Approver setNext(Approver n){ this.next = n; return n; }
    public abstract void approve(int days);
    protected void forward(int days){ if (next != null) next.approve(days); }
}
class TeamLead extends Approver {
    public void approve(int days){
        if (days <= 1) System.out.println("组长批准"); else forward(days);
    }
}
class Manager extends Approver {
    public void approve(int days){
        if (days <= 3) System.out.println("经理批准"); else forward(days);
    }
}
// 使用：lead.setNext(manager); lead.approve(2);
```

**C++**

```cpp
class Approver {
protected:
    std::shared_ptr<Approver> next_;
    void forward(int days){ if (next_) next_->approve(days); }
public:
    void setNext(std::shared_ptr<Approver> n){ next_ = std::move(n); }
    virtual void approve(int days) = 0;
    virtual ~Approver() = default;
};
class TeamLead : public Approver {
public:
    void approve(int days) override {
        if (days <= 1) std::cout << "组长批准\n"; else forward(days);
    }
};
class Manager : public Approver {
public:
    void approve(int days) override {
        if (days <= 3) std::cout << "经理批准\n"; else forward(days);
    }
};
```

---

## 状态 State

**意图**：把与状态相关的行为封装进独立的状态对象，让对象行为随状态改变。

**要点**：把庞大的状态判断（对象在不同状态下行为不同）拆成一组状态类，每个状态类只管自己那套行为和向下一个状态的迁移，替代散落各处的状态标志位判断。是实现状态机的干净方式。

**真实应用**

- **网络协议**：TCP 连接状态机（LISTEN→SYN_RCVD→ESTABLISHED…）。
- **工作流**：Spring StateMachine、订单/工单/审批状态流转引擎。
- **游戏**：角色的 待机/移动/攻击/死亡 状态切换。

**代码** — 场景：订单状态流转，待支付 → 已支付 → 已发货，每个状态自己知道下一步该怎么走。

**Go**

```go
package state

type State interface {
	Next(o *Order)
	Name() string
}

type Order struct{ state State }
func NewOrder() *Order          { return &Order{state: Unpaid{}} }
func (o *Order) Next()          { o.state.Next(o) }
func (o *Order) Status() string { return o.state.Name() }

type Unpaid struct{}
func (Unpaid) Name() string  { return "待支付" }
func (Unpaid) Next(o *Order) { o.state = Paid{}; fmt.Println("→ 已支付") }

type Paid struct{}
func (Paid) Name() string  { return "已支付" }
func (Paid) Next(o *Order) { o.state = Shipped{}; fmt.Println("→ 已发货") }

type Shipped struct{}
func (Shipped) Name() string  { return "已发货" }
func (Shipped) Next(o *Order) { fmt.Println("已是终态") }
```

**Java**

```java
interface State { void next(Order o); String name(); }

class Order {
    private State state = new Unpaid();
    void setState(State s){ this.state = s; }
    public void next(){ state.next(this); }
    public String status(){ return state.name(); }
}
class Unpaid implements State {
    public void next(Order o){ o.setState(new Paid()); System.out.println("→ 已支付"); }
    public String name(){ return "待支付"; }
}
class Paid implements State {
    public void next(Order o){ o.setState(new Shipped()); System.out.println("→ 已发货"); }
    public String name(){ return "已支付"; }
}
class Shipped implements State {
    public void next(Order o){ System.out.println("已是终态"); }
    public String name(){ return "已发货"; }
}
```

**C++**

```cpp
class Order;
struct State {
    virtual void next(Order& o) = 0;
    virtual std::string name() = 0;
    virtual ~State() = default;
};

class Order {
    std::unique_ptr<State> state_;
public:
    Order();
    void setState(std::unique_ptr<State> s){ state_ = std::move(s); }
    void next(){ state_->next(*this); }
    std::string status(){ return state_->name(); }
};

struct Shipped : State {
    void next(Order&) override { std::cout << "已是终态\n"; }
    std::string name() override { return "已发货"; }
};
struct Paid : State {
    void next(Order& o) override;
    std::string name() override { return "已支付"; }
};
struct Unpaid : State {
    void next(Order& o) override {
        o.setState(std::make_unique<Paid>());
        std::cout << "→ 已支付\n";
    }
    std::string name() override { return "待支付"; }
};
void Paid::next(Order& o) {
    o.setState(std::make_unique<Shipped>());
    std::cout << "→ 已发货\n";
}
Order::Order() : state_(std::make_unique<Unpaid>()) {}
```

---

## 命令 Command

**意图**：把请求封装成对象，从而支持排队、记录日志、撤销等操作。

**要点**：把"一个操作"封装成对象（含接收者、参数、执行逻辑），从而可以排队、记录日志、支持撤销/重做、组合成宏命令。调用者只依赖命令接口，完全不知道具体做了什么。

**真实应用**

- **并发任务**：`Runnable` / `Callable` 提交给 `ExecutorService`——把"要做的事"封装成对象排队执行。
- **撤销重做**：Swing 的 `javax.swing.undo`、编辑器的 undo/redo 栈。
- **GUI 动作**：Swing 的 `Action` 绑定到菜单/按钮。
- **任务队列**：消息队列里的命令消息、Sidekiq/Celery 的 Job。

**代码** — 场景：遥控器把"开灯""关灯"封装成命令对象，天然支持记录历史与撤销。

**Go**

```go
package command

type Command interface {
	Execute()
	Undo()
}

type Light struct{}
func (Light) On()  { fmt.Println("灯开") }
func (Light) Off() { fmt.Println("灯关") }

type LightOnCommand struct{ light Light }
func (c LightOnCommand) Execute() { c.light.On() }
func (c LightOnCommand) Undo()    { c.light.Off() }

// 调用者：只依赖 Command 接口，不关心具体做什么
type Remote struct{ history []Command }
func (r *Remote) Press(c Command) {
	c.Execute()
	r.history = append(r.history, c)
}
func (r *Remote) UndoLast() {
	if n := len(r.history); n > 0 {
		r.history[n-1].Undo()
		r.history = r.history[:n-1]
	}
}
```

**Java**

```java
interface Command { void execute(); void undo(); }

class Light {
    void on(){ System.out.println("灯开"); }
    void off(){ System.out.println("灯关"); }
}

class LightOnCommand implements Command {
    private final Light light;
    LightOnCommand(Light light){ this.light = light; }
    public void execute(){ light.on(); }
    public void undo(){ light.off(); }
}

class Remote {
    private final Deque<Command> history = new ArrayDeque<>();
    public void press(Command c){ c.execute(); history.push(c); }
    public void undoLast(){ if (!history.isEmpty()) history.pop().undo(); }
}
```

**C++**

```cpp
struct Command {
    virtual void execute() = 0;
    virtual void undo() = 0;
    virtual ~Command() = default;
};

struct Light {
    void on(){ std::cout << "灯开\n"; }
    void off(){ std::cout << "灯关\n"; }
};

class LightOnCommand : public Command {
    Light& light_;
public:
    explicit LightOnCommand(Light& l) : light_(l) {}
    void execute() override { light_.on(); }
    void undo() override { light_.off(); }
};

class Remote {
    std::vector<std::shared_ptr<Command>> history_;
public:
    void press(std::shared_ptr<Command> c){ c->execute(); history_.push_back(c); }
    void undoLast(){
        if (!history_.empty()){ history_.back()->undo(); history_.pop_back(); }
    }
};
```

---

## 迭代器 Iterator

**意图**：顺序访问集合元素，而不暴露其内部表示。

**要点**：把"遍历"的职责从集合中分离出来，调用方不必知道内部是数组、链表还是树。现代语言大多把它做进了语法（for-each / range-based for），实现约定好的协议即可。

**真实应用**

- **Java**：`Iterator` / `Iterable`，`for-each` 语法糖背后就是它。
- **C++ STL**：`begin()`/`end()` 迭代器是整个 STL 的基石，range-based for 依赖它。
- **Go**：早期靠 `for range` 遍历切片/map/channel；Go 1.23 起正式引入 range-over-func 迭代器（`iter.Seq`）。
- **Python**：`__iter__` / `__next__` 协议。

**代码** — 场景：自定义栈，提供统一遍历方式，调用方用 for 循环遍历而不必知道内部是切片还是链表。

**Go**（Go 1.23+ 的 range-over-func 迭代器）

```go
package iterator

import "iter"

type Stack struct{ items []int }
func (s *Stack) Push(v int) { s.items = append(s.items, v) }

// All 返回一个迭代器，可直接被 for range 消费
func (s *Stack) All() iter.Seq[int] {
	return func(yield func(int) bool) {
		for i := len(s.items) - 1; i >= 0; i-- { // 从栈顶开始
			if !yield(s.items[i]) {
				return // 消费方 break 时提前退出
			}
		}
	}
}
// 使用：for v := range stack.All() { ... }
```

**Java**（实现 `Iterable` 即可用 for-each）

```java
class Stack<T> implements Iterable<T> {
    private final List<T> items = new ArrayList<>();
    public void push(T v){ items.add(v); }

    public Iterator<T> iterator() {      // 返回迭代器，不暴露内部 List
        return new Iterator<>() {
            private int i = items.size() - 1;
            public boolean hasNext(){ return i >= 0; }
            public T next(){ return items.get(i--); } // 从栈顶开始
        };
    }
}
// 使用：for (T v : stack) { ... }
```

**C++**（暴露 `begin()/end()` 即可用 range-based for）

```cpp
class Stack {
    std::vector<int> items_;
public:
    void push(int v){ items_.push_back(v); }
    // 复用标准库反向迭代器，实现"从栈顶开始"的遍历
    auto begin() { return items_.rbegin(); }
    auto end()   { return items_.rend(); }
};
// 使用：for (int v : stack) { ... }
```

---

## 中介者 Mediator

**意图**：用一个中介对象封装多个对象之间的交互。

**要点**：当一组对象两两直接引用时，耦合是 N×N 的；引入中介后变成 N×1，每个对象只跟中介打交道。代价是中介本身可能膨胀成"上帝对象"，需要控制它的职责范围。

**真实应用**

- **GUI**：一个对话框/控制器协调其中众多控件的联动，避免控件之间两两直接引用。
- **聊天室**：服务器作为中介转发消息，客户端之间不直接通信。
- **经典类比**：塔台协调飞机起降。

**代码** — 场景：聊天室。用户之间不直接互相引用，所有消息都经聊天室（中介）转发。

**Go**

```go
package mediator

type ChatRoom struct{ users []*User }
func (c *ChatRoom) Register(u *User) {
	u.room = c
	c.users = append(c.users, u)
}
func (c *ChatRoom) Broadcast(from *User, msg string) {
	for _, u := range c.users {
		if u != from {
			u.Receive(msg)
		}
	}
}

type User struct {
	name string
	room *ChatRoom
}
func (u *User) Send(msg string)    { u.room.Broadcast(u, msg) } // 只跟中介打交道
func (u *User) Receive(msg string) { fmt.Printf("%s 收到: %s\n", u.name, msg) }
```

**Java**

```java
class ChatRoom {
    private final List<User> users = new ArrayList<>();
    public void register(User u){ u.setRoom(this); users.add(u); }
    public void broadcast(User from, String msg){
        for (User u : users) if (u != from) u.receive(msg);
    }
}

class User {
    private final String name;
    private ChatRoom room;
    User(String name){ this.name = name; }
    void setRoom(ChatRoom r){ this.room = r; }
    public void send(String msg){ room.broadcast(this, msg); }
    public void receive(String msg){ System.out.println(name + " 收到: " + msg); }
}
```

**C++**

```cpp
class User;  // 前向声明

class ChatRoom {
    std::vector<User*> users_;
public:
    void join(User* u);
    void broadcast(User* from, const std::string& msg);
};

class User {
    std::string name_;
    ChatRoom* room_ = nullptr;
public:
    explicit User(std::string n) : name_(std::move(n)) {}
    void setRoom(ChatRoom* r){ room_ = r; }
    void send(const std::string& msg){ room_->broadcast(this, msg); }
    void receive(const std::string& msg){ std::cout << name_ << " 收到: " << msg << "\n"; }
};

void ChatRoom::join(User* u){ u->setRoom(this); users_.push_back(u); }
void ChatRoom::broadcast(User* from, const std::string& msg){
    for (auto* u : users_) if (u != from) u->receive(msg);
}
```

---

## 备忘录 Memento

**意图**：在不破坏封装的前提下捕获并保存对象状态，以便之后恢复。

**要点**：三个角色——原发器（产生/恢复快照）、备忘录（不透明的状态载体）、管理者（只负责保管，不解读内容）。关键在于备忘录对外不透明，只有原发器能读它，这样才不泄漏内部结构。易错点是快照可能很大，需要考虑增量存储或限制历史长度。

**真实应用**

- **编辑器**：undo/redo 通过保存状态快照实现。
- **数据库**：事务的 savepoint / 回滚。
- **游戏存档**：把当前进度序列化保存，之后恢复。

**代码** — 场景：文本编辑器的撤销。把某一时刻的状态存成快照，之后可恢复，且不破坏对象封装。

**Go**

```go
package memento

// 备忘录：封装某一时刻的状态
type Memento struct{ content string }

// 原发器：产生 / 恢复备忘录
type Editor struct{ content string }
func (e *Editor) Type(s string)     { e.content += s }
func (e *Editor) Save() Memento     { return Memento{e.content} }
func (e *Editor) Restore(m Memento) { e.content = m.content }
func (e *Editor) Content() string   { return e.content }

// 管理者：只负责保管，不解读备忘录内部
type History struct{ snapshots []Memento }
func (h *History) Push(m Memento) { h.snapshots = append(h.snapshots, m) }
func (h *History) Pop() Memento {
	n := len(h.snapshots) - 1
	m := h.snapshots[n]
	h.snapshots = h.snapshots[:n]
	return m
}
```

**Java**（用内部类 + private 字段，只有 Editor 能读备忘录内容）

```java
class Editor {
    private String content = "";
    public void type(String s){ content += s; }
    public String getContent(){ return content; }

    public Memento save(){ return new Memento(content); }
    public void restore(Memento m){ content = m.content; }

    static class Memento {          // 对外不透明
        private final String content;
        private Memento(String c){ content = c; }
    }
}

class History {
    private final Deque<Editor.Memento> stack = new ArrayDeque<>();
    public void push(Editor.Memento m){ stack.push(m); }
    public Editor.Memento pop(){ return stack.pop(); }
}
```

**C++**

```cpp
class Memento {
    std::string content_;
public:
    explicit Memento(std::string c) : content_(std::move(c)) {}
    std::string content() const { return content_; }
};

class Editor {
    std::string content_;
public:
    void type(const std::string& s){ content_ += s; }
    std::string content() const { return content_; }
    Memento save() const { return Memento(content_); }
    void restore(const Memento& m){ content_ = m.content(); }
};

class History {
    std::vector<Memento> stack_;
public:
    void push(const Memento& m){ stack_.push_back(m); }
    Memento pop(){ auto m = stack_.back(); stack_.pop_back(); return m; }
};
```

---

## 访问者 Visitor

**意图**：在不修改元素类的前提下，为一组元素定义新的操作。

**要点**：把"操作"从"数据结构"里分离出来。新增操作只需加一个访问者（元素类零改动），但**新增元素类型要改所有访问者**——所以它适合"结构稳定、操作频繁增加"的场景，典型如 AST。实现依赖"双分派"（元素的 `accept` + 访问者的 `visit`）。

**真实应用**

- **编译器**：遍历抽象语法树（AST）时，对不同节点执行不同操作——javac、LLVM、Clang 大量使用。
- **Java**：`Files.walkFileTree()` 的 `FileVisitor`、注解处理器的 `ElementVisitor`、字节码库 ASM 的 `ClassVisitor`。
- **文档处理**：对 XML/JSON 树执行导出、校验等多种操作而不改节点类。

**代码** — 场景：对一组图形（圆、矩形）执行多种操作（求面积、导出等）。新增操作只需加一个访问者，元素类不用改。

**Go**

```go
package visitor

// 访问者：为每种元素定义一个访问方法
type Visitor interface {
	VisitCircle(c *Circle)
	VisitRect(r *Rect)
}

type Shape interface{ Accept(v Visitor) }

type Circle struct{ R float64 }
func (c *Circle) Accept(v Visitor) { v.VisitCircle(c) }

type Rect struct{ W, H float64 }
func (r *Rect) Accept(v Visitor) { v.VisitRect(r) }

// 新操作 = 新访问者，元素类零改动
type AreaVisitor struct{ Total float64 }
func (a *AreaVisitor) VisitCircle(c *Circle) { a.Total += 3.14159 * c.R * c.R }
func (a *AreaVisitor) VisitRect(r *Rect)     { a.Total += r.W * r.H }
```

**Java**（利用方法重载实现"双分派"）

```java
interface Visitor {
    void visit(Circle c);
    void visit(Rect r);
}
interface Shape { void accept(Visitor v); }

class Circle implements Shape {
    double r;
    Circle(double r){ this.r = r; }
    public void accept(Visitor v){ v.visit(this); }
}
class Rect implements Shape {
    double w, h;
    Rect(double w, double h){ this.w = w; this.h = h; }
    public void accept(Visitor v){ v.visit(this); }
}

class AreaVisitor implements Visitor {   // 新增操作只需加一个 Visitor
    double total = 0;
    public void visit(Circle c){ total += Math.PI * c.r * c.r; }
    public void visit(Rect r){ total += r.w * r.h; }
}
```

**C++**

```cpp
struct Circle;
struct Rect;

struct Visitor {
    virtual void visit(Circle& c) = 0;
    virtual void visit(Rect& r) = 0;
    virtual ~Visitor() = default;
};

struct Shape { virtual void accept(Visitor& v) = 0; virtual ~Shape() = default; };

struct Circle : Shape {
    double r;
    explicit Circle(double r) : r(r) {}
    void accept(Visitor& v) override { v.visit(*this); }
};
struct Rect : Shape {
    double w, h;
    Rect(double w, double h) : w(w), h(h) {}
    void accept(Visitor& v) override { v.visit(*this); }
};

struct AreaVisitor : Visitor {
    double total = 0;
    void visit(Circle& c) override { total += 3.14159 * c.r * c.r; }
    void visit(Rect& r) override { total += r.w * r.h; }
};
```

---

## 解释器 Interpreter

**意图**：为一种语言定义文法表示，并给出解释执行它的解释器。

**要点**：把文法的每条规则做成一个类，组成语法树后递归求值。适合文法简单稳定的小型 DSL；文法一复杂，类的数量会失控，此时应该换用专门的解析器生成器（ANTLR、yacc 等）。

**真实应用**

- **正则引擎**：`java.util.regex.Pattern` 把正则文法解析后解释执行。
- **表达式/规则引擎**：Spring EL（SpEL）、Drools、各种业务规则 DSL。
- **SQL**：数据库把 SQL 解析成语法树后解释/编译执行。

**代码** — 场景：解释简单的加减表达式，如 `5 + 3 - 2`。

**Go**

```go
package interpreter

type Expr interface{ Interpret() int }

type Num struct{ val int }
func (n Num) Interpret() int { return n.val }

type Add struct{ left, right Expr }
func (a Add) Interpret() int { return a.left.Interpret() + a.right.Interpret() }

type Sub struct{ left, right Expr }
func (s Sub) Interpret() int { return s.left.Interpret() - s.right.Interpret() }

// (5 + 3) - 2 → 构造成语法树后求值
// expr := Sub{Add{Num{5}, Num{3}}, Num{2}}; expr.Interpret() // 6
```

**Java**

```java
interface Expr { int interpret(); }

class Num implements Expr {
    private final int val;
    Num(int val){ this.val = val; }
    public int interpret(){ return val; }
}
class Add implements Expr {
    private final Expr left, right;
    Add(Expr l, Expr r){ left = l; right = r; }
    public int interpret(){ return left.interpret() + right.interpret(); }
}
class Sub implements Expr {
    private final Expr left, right;
    Sub(Expr l, Expr r){ left = l; right = r; }
    public int interpret(){ return left.interpret() - right.interpret(); }
}
// new Sub(new Add(new Num(5), new Num(3)), new Num(2)).interpret() // 6
```

**C++**

```cpp
struct Expr { virtual int interpret() = 0; virtual ~Expr() = default; };

struct Num : Expr {
    int val;
    explicit Num(int v) : val(v) {}
    int interpret() override { return val; }
};
struct Add : Expr {
    std::unique_ptr<Expr> l, r;
    Add(std::unique_ptr<Expr> a, std::unique_ptr<Expr> b) : l(std::move(a)), r(std::move(b)) {}
    int interpret() override { return l->interpret() + r->interpret(); }
};
struct Sub : Expr {
    std::unique_ptr<Expr> l, r;
    Sub(std::unique_ptr<Expr> a, std::unique_ptr<Expr> b) : l(std::move(a)), r(std::move(b)) {}
    int interpret() override { return l->interpret() - r->interpret(); }
};
```

---

# 收尾

## 关键细节 / 易错点

- **模式是手段不是目的**：为了用模式而用模式，是新手最常见的坑。会凭空增加间接层、抽象和类数量，让简单问题复杂化（over-engineering）。**先有真实的变化压力，再引入对应模式。**
- **优先组合而非继承**：GoF 的两大总原则之一。继承是编译期的强耦合，组合是运行期的灵活装配。装饰器、策略、桥接都是"用组合替代继承爆炸"的典范。
- **面向接口编程，而非面向实现**：另一大总原则。依赖抽象让实现可替换，是绝大多数模式起作用的前提。
- **易混淆的模式对**：
  - 策略 vs 状态：结构同，意图不同（外部选择 vs 内部流转）。
  - 装饰器 vs 代理：结构近，意图不同（增强功能 vs 控制访问）。
  - 工厂方法 vs 抽象工厂：单个产品的多态创建 vs 一族产品的搭配创建。
  - 适配器 vs 外观：转换单个接口 vs 简化一整个子系统。
  - 适配器 vs 桥接：事后补救不兼容接口 vs 事前设计两个维度分离。
- **单例的争议**：它是全局状态，破坏可测试性、隐藏依赖。多数场景更推荐用 DI 容器管理生命周期，而不是硬编码单例。
- **模式会随语言演化**：在支持一等函数的语言里，策略、命令、观察者常常直接用函数/闭包/委托实现，不必建一堆类。别把 Java 90 年代的写法照搬到现代语言。
- **真实框架里少有"教科书标准形态"**：模式常被裁剪、组合，或用语言特性（函数、闭包、泛型、反射）改写。**认出背后的意图（隔离了什么变化）比对照 UML 更重要。**

## 实战与权衡

1. **从"识别变化点"入手，而不是从"套模式"入手**：先问"这段代码最可能因为什么原因而改？"，把那个变化维度隔离出去，往往自然就落到某个模式上。
2. **重构中引入，而非一开始就设计满**：遵循"三次法则 / AHA（Avoid Hasty Abstractions）"，等重复和变化的模式看清楚了，再用模式重构，避免过早抽象抽错方向。
3. **权衡灵活性与复杂度**：每个模式都用"灵活性/解耦"换"更多的类和间接层"。只有当变化确实会发生、且收益大于理解成本时才划算。
4. **读源码时练习识别模式**：本页"真实应用"小节可以当索引——带着模式意识去读 JDK、Spring、Netty、Go 标准库，比单纯背定义有效得多。
5. **和架构模式区分粒度**：本页是类/对象级的设计模式；系统级的模式（微服务、CQRS、Event Sourcing、六边形架构等）见本知识库 H 模块其他条目——两者是同一"设计"谱系上的不同粒度。

## 一句话讲清楚

设计模式就是一批"把会变的部分从不变的部分里隔离出去"的成熟套路——它的价值在隔离变化、降低耦合和统一沟通语言，但一定要在真实的变化压力下按需引入，为用而用只会平添复杂度。

## 参考资料

- 《设计模式：可复用面向对象软件的基础》（GoF，Gamma / Helm / Johnson / Vlissides）
- 《Head First 设计模式》——图解入门，大量以 JDK 为例，强调"封装变化、组合优于继承、面向接口"
- 《重构：改善既有代码的设计》（Martin Fowler）——模式多在重构中自然浮现
- refactoring.guru——各模式的图解、代码、对比与"真实应用"小节
- 《A Philosophy of Software Design》（John Ousterhout）——从复杂度视角看抽象与模式
- 见本知识库 A2. 抽象建模、H 模块（DDD / Clean Architecture / 架构设计）
