Go 的并发编程建立在一句哲学之上：**不要通过共享内存来通信，要通过通信来共享内存**。这一篇把 channel、select、以及各种同步原语（Mutex、WaitGroup、Once、Pool、atomic）讲透，并落到"如何优雅控制 goroutine 生命周期、避免泄漏"这个专家级实战问题上。

### 📬 channel：有缓冲 vs 无缓冲

*   **无缓冲 channel**：发送和接收必须**同时到场**才能完成（同步握手）。
*   **有缓冲 channel**：缓冲没满发送不阻塞，没空接收不阻塞。

关闭规则要背牢：
*   向已关闭 channel **发送** → panic
*   **关闭**已关闭 channel → panic
*   从已关闭 channel **读** → 立即返回零值 + `ok=false`

🧠 **类比**：无缓冲 channel 像"当面交接文件"，双方都得在场；有缓冲像"文件柜（容量 N）"，投件方只要柜子没满就能塞、取件方只要柜子没空就能拿。关闭 channel 像"贴上'已停用'封条"，再往里塞（发送）会出事，重复封（关闭）也会出事。

**关闭原则**：谁发送谁关闭，且只关一次；多发送者场景用 `sync.Once` 或额外的信号 channel 协调关闭。

### 🔀 select：多路复用与超时/非阻塞

*   多个 case 同时就绪时**随机**选一个（防止饥饿、防止你依赖顺序）。
*   都没就绪且有 `default` 就走 default（**非阻塞**收发）。
*   **超时**：`select { case v := <-ch: ...; case <-time.After(t): /* 超时 */ }`。

🧠 **类比**：select 像"多个窗口同时办业务，哪个先叫到号就去哪个"，都叫到了就随机挑一个；`default` 像"都没空就先干别的别干等着"。

**高频坑**：`time.After` 在 `for-select` 循环里每轮都新建 timer 且到期前不回收，高频场景会内存泄漏；应改用 `time.NewTimer` + `Reset` 复用。

### 🎯 优雅控制 goroutine 生命周期（专家核心题）

这是并发编程最重要的一题。**启动一个 goroutine 前，先想清楚它怎么退出**。

*   用 context 传取消信号；worker 用 `select { case <-ctx.Done(): return; case job := <-ch: ... }` 监听退出。
*   确保每个 channel 都有出口，不会永久阻塞。

**常见泄漏场景**：向没有接收者的 channel 发送、`range` 一个永不关闭的 channel、忘记调用 cancel、WaitGroup 计数不匹配。

**排查手段**：`pprof goroutine` 看堆栈聚集处、`runtime.NumGoroutine()` 监控趋势。

🧠 **类比**：goroutine 泄漏像"派出去的外卖员一直在门口等一个永远不开门的客户"，越积越多。给每个外卖员配"召回哨（ctx）"和"最长等待时间（超时）"就不会无限等。

### 🔒 Mutex 与 RWMutex：正常模式与饥饿模式

*   **读多写少** → RWMutex（多个读可并行）。
*   **写频繁或临界区极短** → 普通 Mutex（RWMutex 的读写记账反而更慢）。

Mutex 有两种模式：
*   **正常模式**：新来的 goroutine 可能"插队"抢锁，吞吐高。
*   **饥饿模式**：等待超过 1ms 就切换，锁直接按 FIFO 交给队首，防止尾部长期饿死。

🧠 **类比**：RWMutex 像"图书馆阅览室"——多人可同时看书（读），但有人要重新排版（写）时得清场独占。饥饿模式像"排队太久的人被安排优先办理"，防止总被插队的人饿死。

**实践**：锁粒度要小（只锁必要临界区）；避免锁内做 IO / RPC；用 `defer unlock` 防漏解锁，但热点路径注意其性能。

### 🧮 WaitGroup：为什么 Add 要在 goroutine 外

*   `Add` 必须在启动 goroutine **之前**调用；若在 goroutine 内部 Add，主协程可能先跑到 `Wait` 发现计数为 0 提前返回。
*   WaitGroup 不能被复制（内含 noCopy）；计数变负会 panic。

🧠 **类比**：WaitGroup 像"点名册"——出发前（启动 goroutine 前）先记下"共 N 人"（Add N），每人回来签退（Done），带队的等所有人签完（Wait）。如果边走边记名，可能还没记就以为人齐了。

常配合 **errgroup** 用于"并发子任务 + 收集错误"。

### 🔑 sync.Once 的实现原理

内部一个 `done` 原子标志 + 一把互斥锁。快路径先 atomic 读 done，已完成直接返回；慢路径加锁、双重检查、执行 f、置 done。

🧠 **类比**：像"景区检票口"——大多数人来了先瞄一眼"已开门"的牌子（atomic 读）直接进；第一个到的人负责开门（加锁执行），开完挂牌，后面的人就不用再开了。

**为什么不能只用 atomic？** 纯 atomic 只能标记"开始了"，无法保证"执行**完成**"后其他人才通过，必须靠锁串行化并建立 happens-before 关系。

### ♻️ sync.Pool：复用对象降 GC 压力

*   缓存可复用的临时对象，降低分配和 GC 压力；每个 P 有本地池，减少竞争。
*   **陷阱一**：每轮 GC 会清空 Pool（不能当持久缓存/连接池用）。
*   **陷阱二**：取出的对象状态是"上次用过的"，必须先 Reset。

🧠 **类比**：sync.Pool 像"公共雨伞架"——用完放回，下次别人拿现成的（省分配）；但清洁工（GC）来了会把伞架清空，所以别指望伞一直在。拿到的伞可能还带着上次的水（要先甩干 = Reset）。

典型用于 bytes.Buffer、序列化临时缓冲、[]byte 缓冲。

### ⚔️ 数据竞争（data race）

**定义**：≥2 个 goroutine **并发**访问同一内存，且**至少一个是写**，且**没有同步**。结果不确定，可能读到半更新的值。

*   **检测**：`go test -race` / `go run -race`（运行期插桩，性能降但能抓现行）。
*   **避免**：加锁 / channel 通信 / atomic / 只读共享（不可变）。

🧠 **类比**：data race 像"两个人同时在同一张白板上写字又互相看"——最后是谁的字、看到的是不是完整的字，全凭运气。加锁就是"约定一次只让一个人上手"。

**辨析**：data race ≠ race condition。前者是内存层并发访问，后者是逻辑时序依赖，范围更广。

### ⚛️ atomic 与 CAS

*   **适用**：计数器、标志位等**单个变量**的简单读改写，比锁轻量。
*   **CAS（CompareAndSwap）**："值还等于旧值才更新"，是无锁乐观并发的基础。

🧠 **类比**：CAS 像"改共享文档前先确认'还是我上次看到的版本吗？'，是才提交，否则重试"——乐观锁思想。

atomic 只保证**单个变量**原子，多个变量的复合操作仍需锁；`atomic.Value`/`atomic.Pointer` 可原子替换整个对象（配置热更新常用）。

### 🏊 实战题：限制并发数的任务池

*   **思路一（信号量）**：带缓冲 channel 当信号量——`sem := make(chan struct{}, N)`，进任务前 `sem <- struct{}{}`，完成后 `<-sem`。
*   **思路二（worker 池）**：固定 N 个 worker + 一个任务 channel，worker `range` 任务 channel 消费。
*   **考察点**：优雅关闭（关任务 channel + WaitGroup 等待）、错误收集、结果聚合、context 取消联动、panic 恢复。

🧠 **类比**：信号量像"停车场 N 个车位"，来车先取停车卡（占位），没卡就等，走了还卡（腾位）。worker 池像"N 个固定收银员轮流处理排队顾客"。

### 🧷 errgroup：WaitGroup 的增强版

errgroup = WaitGroup + 首个错误返回 + context 联动取消（一个子任务出错，`ctx` 取消，其余尽快停）；`SetLimit(n)` 可限并发。

🧠 **类比**：WaitGroup 只负责"等所有人回来"；errgroup 还多了"谁先出事就通知全队撤退，并把出事原因带回来"。适合"扇出多个依赖查询、任一失败即整体失败"的场景。

### ⏱️ 超时控制的几种方式

`context.WithTimeout`（推荐，可级联传播）、`select + time.After`、`time.AfterFunc`。RPC/DB/HTTP 客户端都应设超时。

🧠 **类比**：没有超时的调用像"打电话没人接还一直握着听筒"，占着线路（连接/goroutine）不放，最终线路耗尽 = 雪崩。

### 🤝 happens-before：并发可见性的基石

只有建立了 happens-before 关系，一个 goroutine 的写才**保证**对另一个可见。channel 收发、锁、Once、atomic、WaitGroup、go 语句启动都能建立这种关系。

🧠 **类比**：像"接力赛交棒"——只有棒（同步点）交到手上，你才确定上一棒跑完了、能看到他的成果；没交棒就各跑各的，看到什么全不保证。核心记忆：**共享数据的可见性不是"写了就能看到"，而是"同步过才能看到"**。

### 📌 记忆卡

*   **channel**：无缓冲要双方在场；关闭已关/向已关发送都 panic；谁发送谁关。
*   **select**：多就绪随机选；default 非阻塞；for-select 里的 `time.After` 会泄漏。
*   **goroutine 生命周期**：启动前先想好怎么退；ctx.Done() 兜底；pprof 查泄漏。
*   **Mutex**：读多写少用 RWMutex；等待超 1ms 进饥饿模式防饿死。
*   **WaitGroup**：Add 在 goroutine 外；不能复制。
*   **Once**：atomic + 锁双重检查；纯 atomic 不能保证"执行完成"。
*   **Pool**：每轮 GC 清空；取出先 Reset。
*   **data race**：并发 + 至少一写 + 无同步；-race 检测。

### 🔗 相关阅读

*   [[Go语言的并发模型]] —— GMP 调度底层
*   [[Go语言底层与运行时]] —— context、interface、defer 等
*   [[Go内存模型与GC及性能调优]] —— happens-before 与内存模型
