# 开始使用 Mariadb


大多数的数据库管理系统都是商业的，但作为一个重度的Linux使用者来说，开源的DBMS还是更有吸引力。之前一直使用的是 `MySQL`，但自从被`Oracle` 收购，就很让人揪心。好在子分支的 `MariaDB` 能够兼容 `MySQL` 的命令和操作，加上开源社区的支持力度，让我决定加入`MariaDB` 的阵营。

&lt;!--more--&gt;

### 1 两者异同

迁移前总得先了解一下两者的异同，从网上搜集了一下资料，主要以下几点

#### 1.1 使用情况

自 1995 年以来，`MySQL` 一直被视为最广泛使用的开源数据库，而 `MariaDB` 在最近几年才渐渐得到几个主要巨头比如 `Google` 和 `Linux` 社区的赏识，我也是被 `ArchLinux wiki` 推介从而了解到这个 `DBMS`。

#### 1.2 索引结构

`MariaDB` 完全兼容 `MySQL` 的数据、表格定义、结构和 `API`，也就是说完全可以平滑的从 `MySQL` 过渡到 `MariaDB`

#### 1.3 二进制实现

`MariaDB` 除 `C`、和 `C&#43;&#43;` 外，还使用 `Bash` 和 `Perl`

#### 1.4 复制与集群

`MariaDB` 和 `MySQL` 为主终端用户提供与主从主复制和主从复制相同的复制和集群功能。但 `MariaDB` 还使用 10.1 版以后的 `Galera Cluster`。

#### 1.5 支持

`MySQL` 由 `Oracle` 的开发人员和工程师支持，`MariaDB` 由开源社区提供技术支持。

#### 1.6 安全性

就安全性而言，`MySQL` 为表空间数据提供了强大的加密机制。它提供了强大的安全参数，包括选择好的密码，不给用户不必要的特权，并通过防止`SQL` 注入和数据损坏来确保应用程序安全。`MariaDB` 在内部安全和密码检查，验证模块（`PAM`）和轻量级目录访问协议（`LDAP`）认证，`Kerberos`，用户角色以及对表空间，表格和日志的强大加密等安全功能方面取得了重大进展。

#### 1.7 可扩展性

`MySQL` 不支持，`MariaDB` 建立在现代架构的基础之上，可以在每一层 -- 客户端，集群，内核和存储上进行扩展。这种可扩展性提供了两个主要优势。它允许通过插件实现持续的社区创新，这意味着可以通过 `MariaDB` 的可扩展架构集成各种存储引擎，如 `MariaDB ColumnStore` 或`Facebook` 的 `MyRocks`。此外，它使客户能够轻松配置 `MariaDB` 以支持从联机事务处理（`OLTP`）到联机分析处理（`OLAP`）的各种用例。

#### 1.8 `JSON`支持

`MySQL` 支持本地 `JSON` 数据类型，可以在 `JSON（JavaScript Object Notation）` 文档中高效地访问数据。`MariaDB Server 10.2` 引入了一整套用于读写 `JSON` 文档的 24 个函数。另外，`JSON_VALID` 函数可以与校验约束一起使用，而像 `JSON_VALUE` 这样的函数可以与动态列一起使用来索引特定的字段。

#### 1.9 授权许可

`MySQL` 在 `GPL` 下以开放源代码提供代码，并以 `MySQL Enterprise` 形式提供非 `GPL` 商业分发选项。`MariaDB` 只能使用 `GPL`，因为它的工作源于该许可条款下的 `MySQL` 源代码。

#### 1.10 性能

 `MariaDB` 通过 `MySQL` 的许多创新实现了同类最佳性能。其中包括线程池管理以最大限度地提高处理效率，以及 `InnoDB` 数据存储区内的碎片整理等广泛的优化功能。因此，当从 `InnoDB` 表中删除行时，可用空间立即可供操作系统使用。不需要将旧表中的数据复制到新表中，并且表空间中没有空闲。`MariaDB` 还提供与引擎无关的表统计信息，以改善优化程序的性能，加快对表的大小和结构进行查询处理和数据分析。

### 2 优缺点

#### 2.1 优点

- `MariaDB` 针对性能进行了优化，对于大型数据集，它比 `MySQL` 强大得多。从其他数据库系统可以优雅的迁移到 `MariaDB` 是另一个好处。
- 从 `MySQL` 切换到 `MariaDB` 相对容易，这对于系统管理员来说好像是一块蛋糕。
- `MariaDB` 通过引入微秒级精度和扩展用户统计数据提供更好的监控。
- `MariaDB` 增强了 `KILL` 命令，使您可以杀死用户的所有查询（`KILL USER` 用户名）或杀死查询ID（`KILL QUERY ID query_id`）。`MariaDB` 也转而使用 `Perl` 兼容的正则表达式（`PCRE`），它提供比标准 `MySQL` 正则表达式支持更强大和更精确的查询。
- `MariaDB` 为与磁盘访问，连接操作，子查询，派生表和视图，执行控制甚至解释语句相关的查询应用了许多查询优化。
- `MariaDB` 纯粹是开源的，而不是 `MySQL` 使用的双重授权模式。一些仅适用于 `MySQL Enterprise` 客户的插件在 `MariaDB` 中具有等效的开源实现。
- 与 `MySQL` 相比，`MariaDB` 支持更多的引擎（`SphinxSE，Aria，FederatedX，TokuDB，Spider，ScaleDB`等）。
- `MariaDB` 提供了一个用于商业用途的集群数据库，它也支持多主复制。任何人都可以自由使用它，并且不需要依赖 `MySQL Enterprise` 系统。

#### 2.2 缺点

- 从版本 5.5.36 开始，`MariaDB` 无法迁移回 `MySQL`。
- 对于 `MariaDB` 的新版本，相应的库（用于 `Debian` ）不会及时部署，由于依赖关系，这将导致必需升级到较新的版本。
- `MariaDB` 的群集版本不是很稳定。

### 3 开始使用

```bash
$ sudo pacman -S mariadb
$ sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql # init dir
$ sudo systemctl start mariadb # or mysqld
$ mysql-secure_installation # install instruction
```

### 参考

- [MariaDB与MySQL的区别](https://www.infoq.cn/article/mariadb-vs-mysql)



---

> 作者: [千玄子](https://zjuyk.site)  
> URL: http://localhost:1313/posts/mariadb/  

