# EntityFramworkCore

## EntityFramworkCore介绍

Entity Framework (EF) Core 是轻量化、可扩展、开源和跨平台版的常用 Entity Framework 数据访问技术。

EF Core 可用作对象关系映射程序 (O/RM)，以便于 .NET 开发人员能够使用 .NET 对象来处理数据库，这样就不必经常编写大部分数据访问代码了。

## EntityFramworkCore使用

### 数据迁移常用命令：

创建迁移：

> Add-Migration InitialCreate

> dotnet ef migrations add InitialCreate

更新数据库：

> Update-Database

> dotnet ef database update

删除迁移：

> Remove-Migration

> dotnet ef migrations remove

还原迁移：

> Update-Database LastGoodMigration

> dotnet ef database update LastGoodMigration

生成SQL脚本：

> Script-Migration

> dotnet ef migrations script

在运行时应用迁移：

> myDbContext.Database.Migrate();