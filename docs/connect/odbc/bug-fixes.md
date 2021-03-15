---
title: 已修复的 bug 列表
description: 此页列出了自 Microsoft ODBC Driver 17 for SQL Server 起每个发行版中修复的缺陷。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-chojas
ms.author: v-jizho2
manager: kenvh
ms.openlocfilehash: 1816f1a8255041d6211931ffc41963acc460e15d
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770465"
---
# <a name="list-of-bugs-fixed"></a>已修复的 bug 列表

此页列出了自 [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 起每个发行版中修复的缺陷。

### <a name="bug-fixes-in-the-msconame-odbc-driver-1772-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.7.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复使用托管服务标识身份验证时出现“404 未找到”错误的问题
- 修复高多线程负载下的间断性“不支持加密”错误
- 修复高线程负载下的间歇性故障

### <a name="bug-fixes-in-the-msconame-odbc-driver-177-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.7 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复 BCP NATIVE 模式下 VARIANT 列的字符编码
- 修复特定条件下的 SQL_ATTR_PARAMS_PROCESSED_PTR 设置
- 修复包含注释的语句的 FMTONLY 模式下的 SQLDescribeParam
- 解决使用 Okta 时的联合身份验证问题
- 修复多处理器系统上过多的内存使用量
- 修复 Azure SQL DB 的某些变体的 Azure AD 身份验证

### <a name="bug-fixes-in-the-msconame-odbc-driver-176-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.6 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复了在使用联合帐户 (Windows) 进行身份验证时的 ADAL 错误
- 修复了在异步通知操作期间出现超时的情况下驱动程序无响应的问题
- 修复了 Alpine Linux 升级时驱动程序引用计数的问题
- 修复了 Ubuntu 的 libc6 依赖项版本问题
- 将缺少的定义添加到 Linux/Mac msodbcsql.h

### <a name="bug-fixes-in-the-msconame-odbc-driver-17522-for-ssnoversion-alpine-linux-only"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.5.2.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复（仅限 Alpine Linux）

- 修复了在 Alpine Linux 上使用具有安全 Enclave 的 Always Encrypted 时出现的崩溃问题

### <a name="bug-fixes-in-the-msconame-odbc-driver-1752-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.5.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 已将 msodbcsql.h 添加到 Alpine Linux 包中

### <a name="bug-fixes-in-the-msconame-odbc-driver-175-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.5 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复了 Linux/macOS 上的 AKV CMK 元数据哈希计算
- 修复了加载 OpenSSL 1.0.0 时的错误
- 修复了使用 ISO-8859-1 和 ISO-8859-2 代码页时的转换问题
- 修复了 macOS 上的内部库名称，以包含版本号
- 修复了在使用单独的长度和指示器绑定时，null 指示器的设置

### <a name="bug-fixes-in-the-msconame-odbc-driver-1742-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.4.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

 - 修复了进程 ID 和应用程序名称无法正确发送到 SQL Server（用于 sys.dm_exec_sessions 分析）(Linux) 的问题
 - 删除了 libuuid (Linux) 上的冗余依赖项
 - 修复了将 UTF8 数据发送到 SQL Server 2019 的一个 bug
 - 修复了未正确检测以“@euro”结尾的区域设置的 bug
 - 修复了以下 bug：在使用 Always Encrypted 并作为输出参数获取 XML 数据时，不正确地返回 XML 数据

### <a name="bug-fixes-in-the-msconame-odbc-driver-174-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.4 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复了在驱动程序停止响应的情况下启用多重活动结果集 (MARS) 的间歇性问题
- 修复了在驱动程序停止响应的情况下启用异步通知的连接复原能力问题
- 修复了在检索多线程连接尝试的诊断记录时崩溃的问题
- 修复了在使用 SQL_USER_NAME 和 SQL_DATA_SOURCE_READ_ONLY 调用 SQLGetInfo() 后，重新连接出现“不支持加密”的问题
- 修复了 Azure Active Directory 交互式身份验证期间的 COM 初始化错误
- 修复了多字节 UTF8 数据的 SQLGetData()
- 修复了使用 SQLGetData() 检索 sql_variant 列长度的问题
- 修复了使用 BCP 导入包含超过 7992 个字节的 sql_variant 列的问题
- 修复了将正确的编码发送到服务器以获取窄字符数据的问题

### <a name="bug-fixes-in-the-msconame-odbc-driver-173-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.3 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复了 TCP 发送通知事件句柄内存泄漏问题
- 修复了重新定义 msodbcsql.h 头文件中的枚举 _SQL_FILESTREAM_DESIRED_ACCESS 的问题
- 修复了用于 Linux 的 msodbcsql.h 头文件中缺少 ACCESS_TOKEN 和 AUTHENTICATION 相关定义

### <a name="bug-fixes-in-the-msconame-odbc-driver-172-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复了 Azure Active Directory 身份验证相关的错误消息
- 修复了设置了不同区域设置环境变量时的编码检测
- 修复了在断开正在恢复的连接时出现的崩溃问题
- 修复了对连接运行情况的检测
- 修复了对已关闭套接字的不正确检测
- 修复了尝试在失败的恢复过程中释放语句句柄时无限等待的问题
- 修复了 Windows 上同时安装了版本 13 和 17 时的错误卸载行为
- 修复了旧版 Windows 平台（Windows 7、Windows 8 和 Windows Server 2012）上的解密行为
- 修复了在 Windows 上使用 ADAL 身份验证时的缓存问题
- 修复了在 Windows 上锁定并覆盖跟踪日志的问题

### <a name="bug-fixes-in-the-msconame-odbc-driver-171-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.1 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复了在已启用 MARS 并且连接属性为“Encrypt=yes”的情况下调用 SQLFreeHandle 时的 1 秒延迟问题
- 修复了以下问题：当传入的缓冲区大小小于正在检索的数据时，SQLGetData 中出现错误 22003 崩溃 (Windows)
- 修复了截断的 ADAL 错误消息
- 修复了在将浮点数转换为整数时，32 位 Windows 上的罕见错误
- 修复了以下问题：如果在启用 Always Encrypted 时将 double 插入小数字段，将返回数据截断错误
- 修复了有关 macOS 安装程序的警告
- 修复了以下问题：在同时启用连接复原和连接池的情况下，在会话恢复尝试期间将错误状态发送到 SQL Server，从而导致服务器删除会话

### <a name="bug-fixes-in-the-msconame-odbc-driver-17-for-ssnoversion"></a>[!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 bug 修复

- 修复了以下 bug：在使用 Kerberos 身份验证时，大容量插入可能会失败并出现“拒绝访问”错误
- 删除了 2.3.1 以下版本中存在的 unixODBC bug 的解决方法（驱动程序将传递给 unixODBC 的某些缓冲区大小增加了一倍）
- 修复了使用 ColumnEncryption=enabled 时的连接复原（重新连接）停止响应的问题
- 修复了 DSN 创建 bug：在使用“Active Directory 交互式身份验证”选项时，Azure 身份验证窗口可能会变得无响应 (Windows)
- 修复了在启用异步执行时 ODBC 关闭（在清除连接句柄时发生）的罕见故障
- 修复了 SQL 驱动程序在执行较长的存储过程时导致 CPU 使用率高的问题
- 修复了不进行转换就无法检索已加密 varbinary(max) 列中的数据的问题
- 修复了以下问题：在静态游标上使用 SQLGetData() 提取 null varchar(max) 加密列后，即使该列包含数据，后面一列也同样为 null
- 修复了在启用 Always Encrypted 的情况下提取 varbinary(max) 字段的问题
- 修复在 setlocale() 无法与 Always Encrypted 一起使用的问题
- 修复了以下问题：在启用 Always Encrypted 的情况下，在针对 XML 类型的存储过程参数调用 SQLDescribeParam() 时返回错误
- 修复了经过转义的下划线在 SQLTables 中不起作用的问题
- 修复了以下 bug：在 Linux 上以宽字符形式返回希伯来语数据 (varchar) 时，该数据被截断
- 修复了从 UTF-8 应用程序查询 Shift-JIS 编码的 char/varchar 的问题
- 修复了以下缺陷：使用 SQL_DRIVER_NAME 参数调用 SQLGetInfo 在 macOS 上返回了 Linux 样式文件名
- 修复了以下问题：通过 BCP 实用工具使用大小大于 32k 字节的输入文件将 Windows-1252 字符数据加载到 VARCHAR 列中会导致失败
