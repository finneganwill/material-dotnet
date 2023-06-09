# DbContext

## 一、概述

### 01 介绍
DbContext 实例表示与数据库的会话，可用于查询和保存实体的实例。 DbContext 是工作单元和存储库模式的组合。

**Entity Framework Core 不支持在同一 DbContext 实例上运行多个并行操作**。 这包括异步查询的并行执行以及从多个线程进行的任何显式并发使用。 因此，请始终立即等待异步调用，或者对并行执行的操作使用单独的 DbContext 实例。

### 02 生存周期
`DbContext` 生存周期是指从创建实例开始，到实例释放结束的过程。`DbContext` 实例的生存周期通常较短，实例存在期间可以视为一个工作单元。

>“工作单元将持续跟踪在可能影响数据库的业务事务中执行的所有操作。 当你完成操作后，它将找出更改数据库作为工作结果时需要执行的所有操作。” ——Martin Fowler

EF Core 对该工作单元定义包含一下过程：

- 创建 `DbContext` 实例；
- 根据上下文跟踪实体实例；
- 根据需要对所跟踪的实例进行更改以实现业务规则；
- 调用 `SaveChanges` 或者 `SaveChangesAsync`。EF Core 检测所做的更改，并将这些更改写入数据库；
- 释放DbContext实例。


!!! Danger "重要"
	:one:  **使用后释放 `DbContext` 非常重要**。 这可确保释放所有非托管资源，并注销任何事件或其他挂钩，以防止在实例保持引用时出现内存泄漏。<br>
	:two: **`DbContext` 不是线程安全的**。不要在线程之间共享上下文， 请确保在继续使用上下文实例之前，等待所有异步调用。<br>
	:three: EF Core 代码引发的 `InvalidOperationException` 会使上下文进入不可恢复状态。此类异常表示程序出现错误，不能从中恢复。



## 二、上下文初始化
### 01 ASP.NET依赖注入

### 02 new

### 03 工厂模式

## 三、上下文配置
### 01 DbContextOptions

### 02 配置数据库提供程序

### 03 其他配置


## 四、上下文池