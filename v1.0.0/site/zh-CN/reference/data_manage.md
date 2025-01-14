---
id: data_manage.md
---

# 使用 MySQL 管理元数据

Milvus 默认使用 SQLite 管理元数据，SQLite 具有易于使用、高鲁棒的特性，且无需启动额外服务。但是在生产环境中，基于可靠性的考虑，我们强烈建议你使用 MySQL。

<div class="alert warning">
Milvus 在 CentOS 系统中不支持 MySQL 8.0 或更高版本。
</div>

请参考以下步骤使用 MySQL 作为元数据管理服务：

1. 拉取 MySQL 最新镜像。

    ```shell
    $ docker pull mysql:5.7
    ```

2. 启动 MySQL 服务（密码和端口可自行设置）。

    ```shell
    $ docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
    ```

3. 使用 root 账号和 MySQL 服务所在的主机 IP （`<MySQL_server_host IP>`）登录 MySQL，回车后系统提示输入密码。输入上一步设置的密码。

    ```shell
    $ mysql -h<MySQL_server_host IP> -uroot -p
    ```

4. 进入 MySQL 客户端命令行，创建一个 database，名称可自行设定，这里使用 `milvus`。

    ```sql
    mysql> create database milvus;
    ```

5. 退出 MySQL 客户端, 修改 **server_config.yaml** 文件的 `meta_uri` 参数。使用 MySQL 服务所在的主机 IP 作为 IP 地址（`<MySQL_server_host IP>`）。注意密码、IP 地址、端口以及 database 名称要和以上几步的设置一致。

    ```yaml
    meta_uri: mysql://root:123456@<MySQL_server_host IP>:3306/milvus
    ```

6. 使用修改过的 **server_config.yaml** 启动 Milvus 服务。



## 常见问题

<details>
<summary><font color="#4fc4f9">出现 <code>database is locked</code> 的报错怎么解决？</font></summary>
如果元数据管理用的是 SQLite，在有数据频繁写入的情况下会出现该错误。建议将 SQLite 更换为 MySQL。如何更换请参考文档 <a href="data_manage.md">使用 MySQL 管理元数据</a>。
</details>
<details>
<summary><font color="#4fc4f9">为什么我在 SQLite / MySQL 找不到向量数据？</font></summary>
SQLite / MySQL 只是存放原始向量数据的元数据。向量和索引直接以文件的形式存在磁盘上，不存放在 SQLite 或 MySQL里。详见 <a href="storage_concept.md">存储相关概念</a>。
</details>
<details>
<summary><font color="#4fc4f9">Milvus 的元数据存储可以使用 SQL Server 或者 PostgreSQL 吗？</font></summary>
不可以，目前仅支持 SQLite 和 MySQL。
</details>




## 数据管理相关博客

从数据导入，数据存储到数据查询和调度，请参阅我们的博客深入了解 Milvus 数据管理方案。

- [数据管理策略](https://www.milvus.io/cn/blogs/2019-11-08-data-management.md)
- [数据文件清理机制的改进](https://www.milvus.io/cn/blogs/2019-12-18-datafile-cleanup.md)
- [查看元数据](https://www.milvus.io/cn/blogs/2019-12-24-view-metadata.md)
- [元数据表的字段](https://www.milvus.io/cn/blogs/2019-12-27-meta-table.md)
- [如何通过元数据管理数据文件](https://www.milvus.io/cn/blogs/2020-01-09-milvus-meta.md)
