---
title: JDBC Driver 发行说明
description: 本文列出了 Microsoft JDBC Driver for SQL Server 的各个发行版。 对于每个发行版本，将对所做的更改进行命名和说明。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 074f211e-984a-4b76-bb15-ee36f5946f12
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0ffc7d8e4090673f3fc8feb96aa3650d35361281
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234860"
---
# <a name="release-notes-for-the-microsoft-jdbc-driver-for-sql-server"></a>Microsoft JDBC Driver for SQL Server 发行说明

本文列出了 Microsoft JDBC Driver for SQL Server  的版本。 对于每个发行版本，将对所做的更改进行命名和说明。

## <a name="92"></a><a id="92"></a> 9.2

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 9.2 for SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2153622)  
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 9.2 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2153521)  

版本号：9.2.0 发布时间：2021 年 1 月 29 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 zip 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40a)  

### <a name="compliance"></a>合规性

| 符合性更改 | 详细信息 |
| :---------------- | :------ |
| 下载 JDBC Driver 9.2 的最新更新。 | &bull; &nbsp; [GitHub，9.2.0](https://github.com/Microsoft/mssql-jdbc/releases/tag/v9.2.0)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| 完全符合 JDBC API 规范 4.2。 | 根据 Java 版本兼容性命名 9.2 包中的 jar。<br/><br/>例如，9.2 包中的 mssql-jdbc-9.2.0.jre15.jar 文件应与 Java 15 配合使用。 |
| 与 Java 开发工具包 (JDK) 版本 15.0、11.0 和 1.8 兼容。 | 除 JDK 11.0 和 1.8 外，Microsoft JDBC Driver 9.2 for SQL Server 现在还与 Java 开发工具包 (JDK) 版本 15.0 兼容。 |
| &nbsp; | &nbsp; |

### <a name="releases"></a>版本

版本号：9.2.0 发布时间：2021 年 1 月 29 日

### <a name="support-for-jdk-15"></a>支持 JDK 15

除 JDK 11.0 和 1.8 外，Microsoft JDBC Driver 9.2 for SQL Server 现在还与 Java 开发工具包 (JDK) 版本 15.0 兼容。

### <a name="added-support-for-azure-active-directory-interactive-authentication"></a>添加了对 Azure Active Directory 交互式身份验证的支持
| 添加身份验证类型 | 详细信息 |
| :---------- | :------ |
| Microsoft JDBC Driver 9.2 for SQL Server 现支持通过交互式身份验证向 Azure Key Vault 进行身份验证。 | 请参阅[使用 Azure Active Directory 身份验证进行连接](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md)。 |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-active-directory-service-principal-authentication"></a>添加了对 Azure Active Directory 服务主体身份验证的支持
| 添加身份验证类型 | 详细信息 |
| :---------- | :------ |
| Microsoft JDBC Driver 9.2 for SQL Server 现在支持使用服务主体标识的客户端 ID 和密码来向 Azure Key Vault 进行身份验证。 | 请参阅[使用 Azure Active Directory 身份验证进行连接](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md)。 |
| &nbsp; | &nbsp; |

### <a name="updated-azure-key-vault-library-to-use-modern-azure-key-vault-library"></a>更新 Azure Key Vault 库以使用新式 Azure Key Vault 库
| Azure Key Vault 的库更改 | 详细信息 |
| :------------------- | :------ |
| Microsoft JDBC Driver 9.2 从上一代的 Azure Key Vault 库和 ADAL 库迁移到了更新式的 Azure Key Vault 和 Azure Identity 等效项。 | DEPENDENCY CHANGE 如果利用 Azure Active Directory 身份验证或 Azure Key Vault，请确保更新应用程序依赖项。 [使用 Azure Key Vault 提供程序](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-the-azure-key-vault-provider)或 [Azure Active Directory 身份验证](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-azure-active-directory-authentication)时，请参阅依赖项要求 |
| &nbsp; | &nbsp; |

### <a name="enabled-usebulkcopyforbatchinsert-for-non-azure-synapse-analytics-servers"></a>为非 Azure Synapse Analytics 服务器启用了 useBulkCopyForBatchInsert
| 非 Azure Synapse Analytics 服务器的 useBulkCopyForBatchInsert 更改 | 详细信息 |
| :------------------- | :------ |
| Microsoft JDBC Driver 9.2 现在允许大容量复制 API 针对 Azure Synapse Analytics 服务器进行批量插入操作。 | 请参阅[将大容量复制 API 用于批量插入操作](../../connect/jdbc/use-bulk-copy-api-batch-insert-operation.md)。 |
| &nbsp; | &nbsp; |


## <a name="previous-releases"></a>以前的版本

## <a name="84"></a><a id="84"></a> 8.4

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 8.4 for SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2137600)  
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 8.4 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2137502)  

版本号：8.4.1  
发布日期：2020 年 8 月 27 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 zip 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40a)  

### <a name="compliance"></a>合规性

| 符合性更改 | 详细信息 |
| :---------------- | :------ |
| 下载 JDBC Driver 8.4 的最新更新。 | &bull; &nbsp; [GitHub, 8.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| 完全符合 JDBC API 规范 4.2。 | 根据 Java 版本兼容性命名 8.4 包中的 jar。<br/><br/>例如，8.4 包中的 mssql-jdbc-8.4.1.jre14.jar 文件应与 Java 14 配合使用。 |
| 与 Java 开发工具包 (JDK) 版本 14.0、11.0 和 1.8 兼容。 | 除 JDK 11.0 和 1.8 外，Microsoft JDBC Driver 8.4 for SQL Server 现在还与 Java 开发工具包 (JDK) 版本 14.0 兼容。 |
| &nbsp; | &nbsp; |

### <a name="releases"></a>版本

版本号：8.4.1  
发布日期：2020 年 8 月 27 日  
已解决的问题：  

- 修复了 `SQLServerConnectionPoolProxy` 与 `delayLoadingLobs` 不兼容的问题
- 修复了 `delayLoadingLobs` 的潜在 `NullPointerException` 问题
- 修复了使用 Windows 证书存储时解密列加密密钥的问题

版本号：8.4.0  
发布日期：2020 年 7 月 31 日  

### <a name="support-for-jdk-14"></a>支持 JDK 14

除 JDK 11.0 和 1.8 外，Microsoft JDBC Driver 8.4 for SQL Server 现在还与 Java 开发工具包 (JDK) 版本 14.0 兼容。

### <a name="added-support-for-authentication-to-azure-key-vault-using-managed-identity"></a>添加了对使用托管标识向 Azure Key Vault 进行身份验证的支持

| 添加身份验证类型 | 详细信息 |
| :---------- | :------ |
| Microsoft JDBC Driver 8.4 for SQL Server 现支持使用托管标识向 Azure Key Vault 进行身份验证。 | 请参阅[通过 JDBC 驱动程序使用 Always Encrypted](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)。 |
| &nbsp; | &nbsp; |

### <a name="extended-support-for-bulk-copy-for-azure-data-warehouse"></a>Azure 数据仓库的大容量复制扩展支持

| Azure 数据仓库的大容量复制更改 | 详细信息 |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 添加新的连接属性 `sendTemporalDataTypesAsStringForBulkCopy`。 默认情况下，此布尔属性为 TRUE。 | 请参阅[通过 JDBC 驱动程序使用大容量复制](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md)。 |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-sql-dns-caching"></a>添加了对 Azure SQL DNS 缓存的支持

| DNS 缓存 | 详细信息 |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 for SQL Server 现支持针对 Azure SQL Server 的 DNS 缓存。 | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="added-backwards-compatibility-for-streaming-lob-objects"></a>为流式处理 LOB 对象添加了向后兼容性

| LOB 流式处理 | 详细信息 |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 for SQL Server 添加了新的连接属性 `delayLoadingLobs`。 | 将 `delayLoadingLobs` 设置为 FALSE 将导致从 ResultSet 检索到的所有 LOB 对象不会进行流式处理。 这意味着，驱动程序会一次将整个 LOB 对象加载到内存中，这类似于版本 6.4 版本前驱动程序的运作方式。 |
| &nbsp; | &nbsp; |

### <a name="added-support-for-client-certificate-authentication-for-loopback-scenarios"></a>添加了对环回方案的客户端证书身份验证的支持

| 客户端证书身份验证 | 详细信息 |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 for SQL Server 为环回方案添加了称为“客户端证书身份验证”的新身份验证方法。 | 请参阅[环回方案的客户端证书身份验证](../../connect/jdbc/client-certification-authentication-for-loopback-scenarios.md)。 |

## <a name="82"></a><a id="82"></a> 8.2

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 8.2 for SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2122433)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 8.2 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122536)   

版本号：8.2.2 发布时间：2020 年 3 月 24 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 zip 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40a)  

### <a name="compliance"></a>合规性

| 符合性更改 | 详细信息 |
| :---------------- | :------ |
| 下载 JDBC Driver 8.2 的最新更新。 | &bull; &nbsp; [GitHub，8.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| 完全符合 JDBC API 规范 4.2。 | 根据 Java 版本兼容性命名 8.2 包中的 jar。<br/><br/>例如，8.2 包中的 mssql-jdbc-8.2.2.jre11.jar 文件应与 Java 11 配合使用。 |
| 与 Java 开发工具包 (JDK) 版本 13.0、11.0 和 1.8 兼容。 | 除 JDK 11.0 和 1.8 外，Microsoft JDBC Driver 8.2 for SQL Server 现在还与 JDK 开发工具包 (JDK) 版本 13.0 兼容。 |
| &nbsp; | &nbsp; |

### <a name="releases"></a>版本

版本号：8.2.2  
发布日期：2020 年 3 月 24 日  
已解决的问题：  

- 添加了用于配置受信任的 Azure Key Vault 终结点列表的选项

版本号：8.2.1  
发布日期：2020 年 2 月 26 日  
已解决的问题：  

- 修复了使用 `SQLServerResultSet.getObject()` 检索 `java.time.LocalTime` 或 `java.time.LocalDate` 类型的数据时可能出现的 `NullPointerException` 问题

版本号：8.2.0  
发布日期：2020 年 1 月 31 日  

### <a name="support-for-jdk-13"></a>支持 JDK 13

除 JDK 11.0 和 1.8 外，Microsoft JDBC Driver 8.2 for SQL Server 现在还与 JDK 开发工具包 (JDK) 版本 13.0 兼容。

### <a name="always-encrypted-with-secure-enclaves"></a>具有安全 Enclave 的 Always Encrypted

| Always Encrypted 更改 | 详细信息 |
| :--------- | :------ |
| Microsoft JDBC Driver 8.2 for SQL Server 现在支持具有安全 Enclave 的 Always Encrypted。 此处提供了更多详细信息：具有安全 Enclave 的 Always Encrypted。 |
| 更多详细信息和示例代码。 | 请参阅[包含安全 Enclave 的 Always Encrypted](../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md)。 |
| &nbsp; | &nbsp; |

### <a name="performance-improvement-when-retrieving-temporal-datatypes-from-sql-server-sup1sup"></a>提高了从 SQL Server 检索临时数据类型时的性能 <sup>1</sup>

| 临时数据类型更改 | 详细信息 |
| :---------- | :------ |
| Microsoft JDBC Driver 8.2 for SQL Server 提高了从 SQL Server 检索临时数据类型时的性能。 | 此更改通过尽可能避免使用 java.util.Calendar 来消除不必要的临时数据类型转换。 |
| 以下是已受此性能改进影响的临时数据类型的列表；格式为 SQL Server 数据类型，后跟相应的 Java 映射。 | date (java.sql.Date)、datetime (java.sql.Timestamp)、datetime2 (java.sql.Timestamp)、smalldatetime (java.sql.Timestamp) 和 time (java.sql.Time)。 |
| &nbsp; | &nbsp; |

<sup>1</sup> 由于 java.util.Calendar 和 java.time.LocalDateTime API 在处理时区方面存在差异，因此具有用户提供的与其关联的 java.util.Calendar 对象的时态数据类型或 microsoft.sql.DateTimeOffset 数据类型无法受益于这一改进。

### <a name="deployment-of-mssql-jdbc_auth-version-archdll-previously-sqljdbc_authdll-to-maven-repository"></a>将 mssql-jdbc_auth-\<version>-\<arch>.dll（之前是 sqljdbc_auth.dll）部署到 Maven 存储库

| sqljdbc_auth.dll 更改 | 详细信息 |
| :------------------- | :------ |
| 从 Microsoft JDBC Driver 8.2 for SQL Server 开始，驱动程序依赖于 mssql-jdbc_auth-\<version>-\<arch>.dll（而不是 sqljdbc_auth.dll）来使用 Azure Active Directory 身份验证功能。 | &nbsp; |
| DLL 已被上传到 Maven 存储库，以便更轻松地进行访问。 | 请参阅[本页](https://search.maven.org/artifact/com.microsoft.sqlserver/mssql-jdbc_auth)。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :----------- | :------ |
| 运用 Java 8 来使用具有安全 Enclave 的 Always Encrypted 时。 | 用户必须将 BouncyCastle 提供程序作为依赖项，或者映射/加载支持 RSASSA-PSS 签名算法的安全提供程序。 |
| &nbsp; | &nbsp; |

## <a name="a-id74-741"></a><a id="74"> 7.4.1

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 7.4.1 for SQL Server (self-extracting exe)](https://go.microsoft.com/fwlink/?linkid=2122712)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 7.4.1 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122613)   

版本号：7.4.1  
发布日期：2019 年 8 月 2 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 self-extracting exe 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40a)  

### <a name="compliance"></a>合规性

| 符合性更改 | 详细信息 |
| :---------------- | :------ |
| 下载 JDBC Driver 7.4 的最新更新。 | &bull; &nbsp; [GitHub, 7.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| 完全符合 JDBC API 规范 4.2。 | 根据 Java 版本兼容性命名 7.4 包中的 jar。<br/><br/>例如，7.4 包中的 mssql-jdbc-7.4.1.jre11.jar 文件应与 Java 11 配合使用。 |
| 与 Java 开发工具包 (JDK) 版本 12.0、11.0 和 1.8 兼容。 | 除 JDK 11.0 和 1.8 外，Microsoft JDBC Driver 7.4 for SQL Server 现在还与 JDK 开发工具包 (JDK) 版本 12.0 兼容。 |
| &nbsp; | &nbsp; |

### <a name="releases"></a>版本

版本号：7.4.1  
发布日期：2019 年 8 月 2 日  
已解决的问题：  

- 从 `SQLServerDataTable` 和 `SQLServerDataColumn` 恢复了新的 `hashCode()` 和 `equals()` API 实现，因为 API 更改中断了向后兼容性

版本号：7.4.0  
发布日期：2019 年 7 月 31 日  

### <a name="support-for-jdk-12"></a>支持 JDK 12

除 JDK 11.0 和 1.8 外，Microsoft JDBC Driver 7.4 for SQL Server 现在还与 JDK 开发工具包 (JDK) 版本 12.0 兼容。

### <a name="introduces-ntlm-authentication"></a>引入 NTLM 身份验证

| NTLM 更改 | 详细信息 |
| :--------- | :------ |
| 支持 NTLM 身份验证模式。 | 这种身份验证模式允许 Windows 和非 Windows 客户端使用 Windows 域用户对 SQL Server 进行身份验证。 |
| 使用此身份验证模式的更多详细信息以及示例应用程序。 | 请参阅[使用 NTLM 身份验证进行连接](using-ntlm-authentication-to-connect-to-sql-server.md)。 |
| &nbsp; | &nbsp; |

### <a name="introduces-querying-parametermetadata-via-_usefmtonly_"></a>通过 useFmtOnly  引入了对 ParameterMetaData 的查询

| useFmtOnly 更改 | 详细信息 |
| :---------- | :------ |
| 添加了 useFmtOnly  连接属性。 | 借助此功能，用户可以选择通过 `SET FMTONLY ON` 旧版 API 查询 ParameterMetaData。 这对于 `sp_describe_undeclared_parameters` 不按预期执行的情况很有用。 |
| 更多详细信息和限制。 | 请参阅[使用 useFmtOnly](using-usefmtonly.md) |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-121"></a>更新了 Microsoft Azure Key Vault SDK for Java  版本 1.2.1

| Key Vault SDK 更改 | 详细信息 |
| :------------------- | :------ |
| 已将 _Microsoft Azure Key Vault SDK for Java_ 的 Maven 依赖项更新为版本 1.2.1。 | &nbsp; |
| 删除作为 Maven 依赖项的 _Microsoft Azure SDK for Key Vault WebKey_。 | &nbsp; |
| 其他详细信息。 | 请参阅 [Microsoft JDBC Driver for SQL Server 的功能依赖项](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md)。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :----------- | :------ |
| 使用 NTLM 身份验证时。 | 目前不支持同时启用扩展保护和加密连接。 |
| 使用 useFmtOnly 时。 | 此功能存在一些问题，是由 SQL 分析逻辑中的缺陷导致的。 有关更多详细信息和解决方法建议，请参阅[使用 useFmtOnly](using-usefmtonly.md)。 |
| &nbsp; | &nbsp; |

## <a name="a-id72-722"></a><a id="72"> 7.2.2

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 7.2.2 for SQL Server (self-extracting exe)](https://go.microsoft.com/fwlink/?linkid=2122435)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 7.2.2 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122434)   

版本号：7.2.2  
发布日期：2019 年 4 月 16 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 self-extracting exe 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40a)  

### <a name="compliance"></a>合规性

| 符合性更改 | 详细信息 |
| :---------------- | :------ |
| 下载 JDBC Driver 7.2 的最新更新。 | &bull;&nbsp;[GitHub, 7.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| 完全符合 JDBC API 规范 4.2。 | 根据 Java 版本兼容性命名 7.2 包中的 jar。<br/><br/>例如，7.2 包中的 mssql-jdbc-7.2.2.jre11.jar 文件应与 Java 11 配合使用。 |
| 除 Java 开发工具包 (JDK) 版本 1.8 外，还与 JDK 11.0 兼容。 | 除 Java 开发工具包 (JDK) 版本 1.8 外，Microsoft JDBC Driver 7.2 for SQL Server 现在还与 JDK 11.0 兼容。 |
| &nbsp; | &nbsp; |

### <a name="releases"></a>版本

版本号：7.2.2  
发布日期：2019 年 4 月 16 日  
已解决的问题：  

- 修复了无法正确清除 ActivityID 的问题

版本号：7.2.1  
发布日期：2019 年 2 月 11 日  
已解决的问题：  

- 修复了某些参数化查询的分析问题

版本号：7.2.0  
发布日期：2019 年 1 月 31 日  

### <a name="active-directory-_managed-identity_-msi-authentication"></a>Azure Active Directory 托管标识 (MSI) 身份验证

| MSI 更改 | 详细信息 |
| :--------- | :------ |
| 支持 Active Directory 托管标识 (MSI) 身份验证模式。 | 此身份验证模式适用于支持“标识”功能的 Azure 资源。<br/><br/>两种类型的托管标识 (MSI) 均受驱动程序支持，可获取用于建立安全连接的 accessToken。 |
| 使用此身份验证模式的更多详细信息以及示例应用程序。 | 请参阅[使用 Azure Active Directory 身份验证进行连接](connecting-using-azure-active-directory-authentication.md)。 |
| &nbsp; | &nbsp; |

### <a name="introduces-_open-service-gateway-initiative_-osgi-support"></a>引入开放服务网关协议  (OSGi) 支持

| OSGi 更改 | 详细信息 |
| :---------- | :------ |
| 添加了  DataSourceFactory 实现。 | &bull; &nbsp; `org.osgi.service.jdbc.DataSourceFactory`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.SQLServerDataSourceFactory` |
| 添加了  Activator 实现。 | &bull; &nbsp; `org.osgi.framework.BundleActivator`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.Activator` |
| &nbsp; | &nbsp; |

### <a name="introduces-_sqlservererror_-apis"></a>引入  SQLServerError API

| 错误 API 更改 | 详细信息 |
| :--------------- | :------ |
| 引入了 SQLServerError API。 | 获取 API 以检索有关从服务器生成的错误的其他详细信息。<br/><br/>&bull; &nbsp; `SQLServerException.getSQLServerError()`<br/>&bull; &nbsp; `SQLServerError` |
| 其他详细信息。 | 请参阅[处理错误](handling-errors.md)。 |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-active-directory-authentication-library-adal4j-for-java_-version-163"></a>更新了用于 Java 的 Microsoft Azure Active Directory 身份验证库 (ADAL4J)  版本 1.6.3

| ADAL4J 更改 | 详细信息 |
| :------------ | :------ |
| 已将 ADAL4J 上的 Maven 依赖项更新为版本 1.6.3。 | &nbsp; |
| 引入了“适用于 AutoRest 的 Java 客户端运行时”  作为 Maven 依赖项版本 1.6.5。 | &nbsp; |
| 其他详细信息。 | 请参阅 [Microsoft JDBC Driver for SQL Server 的功能依赖项](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md)。 |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-120"></a>更新了 Microsoft Azure Key Vault SDK for Java  版本 1.2.0

| Key Vault SDK 更改 | 详细信息 |
| :------------------- | :------ |
| 已将 Microsoft Azure Key Vault SDK for Java  的 Maven 依赖项更新为版本 1.2.0。 | &nbsp; |
| 引入了 Microsoft Azure SDK for Key Vault WebKey  作为 Maven 依赖项（版本 1.2.0）。 | &nbsp; |
| 其他详细信息。 | 请参阅 [Microsoft JDBC Driver for SQL Server 的功能依赖项](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md)。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题

| 已知问题 | 详细信息 |
| :----------- | :------ |
| 在某些情况下，参数化查询。 | 已于 2019 年 2 月发布 7.2.0 版本和 v7.2.1 的更新来解决此问题。 |
| 清除 ActivityID。 | 已于 2019 年 4 月发布 7.2.1 版本和 v7.2.2 的更新来解决此问题。 |
| &nbsp; | &nbsp; |

## <a name="70"></a>7.0

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 7.0 for SQL Server (self-extracting exe)](https://go.microsoft.com/fwlink/?linkid=2122713)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 7.0 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122614)   

版本号：7.0.0  
发布日期：2018 年 7 月 31 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 self-extracting exe 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40a)  

Microsoft JDBC Driver 7.0 for SQL Server 完全符合 JDBC API 规范 4.2。 根据 Java 版本兼容性命名 7.0 包中的 jar。 例如，7.0 包中的 mssql-jdbc-7.0.0.jre10.jar 文件应与 Java 10 配合使用。

### <a name="support-for-jdk-10"></a>支持 JDK 10

除 Java 开发工具包 (JDK) 版本 1.8 外，Microsoft JDBC Driver 7.0 for SQL Server 现在还与 JDK 10.0 兼容。 此更新还通过其清单文件将驱动程序的 `Automatic-Module-Name` 公开为 `com.microsoft.sqlserver.jdbc`。

### <a name="support-for-spatial-datatypes"></a>支持空间数据类型

Microsoft JDBC Driver 7.0 for SQL Server 现在提供对 SQL Server 空间数据类型 Geography 和 Geometry 的支持。 有关空间数据类型 API 以及如何使用它们的详细信息，请参阅[使用空间数据类型](use-spatial-datatypes.md)。

### <a name="implementation-for-jdbc-43-introduced-javasqlconnection-apis-beginrequest-and-endrequest"></a>JDBC 4.3 实现引入了 java.sql.Connection API beginRequest() 和 endRequest()

Microsoft JDBC Driver 7.0 for SQL Server 现在实现了 `java.sql.Connection` 类中的 `beginRequest()` 和 `endRequest()` API。 这些 API 是使用 JDBC 4.3 规范和 JDK 9 引入的。 有关这些 API 的驱动程序实现的详细信息，请参阅 [JDBC 驱动程序的 JDBC 4.3 符合性](jdbc-4-3-compliance-for-the-jdbc-driver.md)。

### <a name="support-for-sql-data-discovery-and-classification"></a>支持 SQL 数据发现和分类

Microsoft JDBC Driver 7.0 for SQL Server 支持 SQL 数据发现和分类，以用于支持此功能的任何目标数据库。 驱动程序现在公开 `SQLServerResultSet.getSensitivityClassification()` API 以从提取的 `ResultSet` 中提取此信息。

若要详细了解如何将此功能用于 JDBC 驱动程序，请参阅 [SQL 数据发现和分类](data-discovery-classification-sample.md)中的示例。

### <a name="added-connection-property-usebulkcopyforbatchinsert"></a>添加了连接属性：useBulkCopyForBatchInsert

Microsoft JDBC Driver 7.0 for SQL Server 引入了新的连接属性 `useBulkCopyForBatchInsert`。 只有 Azure Synapse Analytics 才支持此属性。

默认情况下禁用此属性。 当将大量数据推送到 Azure Synapse Analytics 时，可以启用此属性以提高用户应用程序的性能。 启用此属性将更改批插入操作的行为，以切换为对用户提供的数据执行大容量复制操作。 有关此属性及其限制的详细信息，请参阅[将大容量复制 API 用于批插入操作](use-bulk-copy-api-batch-insert-operation.md)。

### <a name="added-connection-property-cancelquerytimeout"></a>添加了连接属性：cancelQueryTimeout

Microsoft JDBC Driver 7.0 for SQL Server 引入了新的连接属性 `cancelQueryTimeout` 以取消 `java.sql.Connection` 和 `java.sql.Statement` 对象上的 `queryTimeout`。

### <a name="added-azure-key-vault-provider-constructors"></a>添加了 Azure Key Vault 提供程序构造函数

Microsoft JDBC Driver 7.0 for SQL Server 为 `SQLServerColumnEncryptionAzureKeyVaultProvider` 重新引入了先前删除的构造函数。 它允许通过用 `SQLServerKeyVaultAuthenticationCallback` 实现的自定义方法进行身份验证来提取访问令牌。

新的构造函数具有以下定义：

```java
/* This constructor is added to provide backward compatibility with 6.0
* version of the driver. It is marked deprecated for removal in the next
* stable release.
*/
@Deprecated
public SQLServerColumnEncryptionAzureKeyVaultProvider(
        SQLServerKeyVaultAuthenticationCallback authenticationCallback,
        ExecutorService executorService) throws SQLServerException;

/*New constructor to replace the above constructor*/
public SQLServerColumnEncryptionAzureKeyVaultProvider(
            SQLServerKeyVaultAuthenticationCallback authenticationCallback) throws SQLServerException;
```

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-160"></a>更新了“用于 Java 的 Microsoft Azure Active Directory 身份验证库 (ADAL4J)”版本：1.6.0

Microsoft JDBC Driver 7.0 for SQL Server 已将“用于 Java 的 Microsoft Azure Active Directory 身份验证库 (ADAL4J)”上的 Maven 依赖项更新为版本 1.6.0。 有关依赖项的详细信息，请参阅 [Microsoft JDBC Driver for SQL Server 的功能依赖项](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md)。

## <a name="64"></a>6.4

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 6.4 for SQL Server (self-extracting exe)](https://go.microsoft.com/fwlink/?linkid=2122436)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 6.4 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122537)   

版本号：6.4.0  
发布日期：2018 年 2 月 27 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 self-extracting exe 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40a)  

Microsoft JDBC Driver 6.4 for SQL Server 完全符合 JDBC 规范 4.1 和 4.2。 根据 Java 版本兼容性命名 6.4 包中的 jar。 例如，6.4 包中的 mssql-jdbc-6.4.0.jre8.jar 文件必须与 Java 8 配合使用。

### <a name="support-for-jdk-9"></a>支持 JDK 9

除了支持 JDK 8.0 和 7.0 以外，驱动程序还支持 JDK 版本 9.0。

### <a name="jdbc-43-compliance"></a>JDBC 4.3 符合性

除了支持 4.1 和 4.2 以外，驱动程序还支持 Java Database Connectivity API 4.3 规范。 JDBC 4.3 API 方法已添加，但尚未实现。 有关详细信息，请参阅 [JDBC Driver 的 JDBC 4.3 合规性](jdbc-4-3-compliance-for-the-jdbc-driver.md)。

### <a name="added-connection-property-sslprotocol"></a>添加了连接属性：sslProtocol

新的连接属性允许用户指定 TLS 协议关键字。 可能的值包括：“TLS”、“TLSv1”、“TLSv1.1”和“TLSv1.2”。 有关详细信息，请参阅 [SSLProtocol](https://github.com/Microsoft/mssql-jdbc/wiki/SSLProtocol)。

### <a name="deprecated-connection-property-fipsprovider"></a>弃用了连接属性：fipsProvider

连接属性 `fipsProvider` 已从接受的连接属性的列表中删除。 有关详细信息，请参阅相关的 [GitHub 拉取请求](https://github.com/Microsoft/mssql-jdbc/pull/460)。

### <a name="added-connection-properties-for-specifying-a-custom-trustmanager"></a>添加了用于指定自定义 TrustManager 的连接属性

驱动程序现在支持指定添加了 `trustManagerClass` 和 `trustManagerConstructorArg` 连接属性的自定义 TrustManager。 可以动态指定基于每个连接信任的一组证书，而无需修改 Java 虚拟机 (JVM) 环境的全局设置。

### <a name="added-support-for-datetimesmalldatetime-in-table-valued-parameters"></a>添加了对表值参数中的 datetime/smallDatetime 的支持

当使用的是表值参数 (TVP) 时，驱动程序现在支持数据类型 `datetime` 和 `smallDatetime`。

### <a name="added-support-for-the-sql_variant-datatype"></a>添加了对 sql_variant 数据类型的支持

JDBC 驱动程序现在支持要用于 SQL Server 的 `sql_variant` 数据类型。 TVP 和大容量复制等功能也支持 `sql_variant` 数据类型，但具有以下限制：

* **对于日期值**：

  当使用 TVP 填充包含存储在 `sql_variant` 列中的 `datetime`、`smalldatetime` 或 `date` 值的表时，对结果集调用 `getDateTime()`、`getSmallDateTime()` 或 `getDate()` 方法不起作用并引发以下异常：

  `java java.lang.String cannot be cast to java.sql.Timestamp`

  作为一种解决办法，可改为使用 `getString()` 或 `getObject()` 方法。

* **对 null 值结合使用 TVP 和 sql_variant**：
  
  如果使用 TVP 填充一个表，并将 NULL 值发送给 `sql_variant` 列类型，则会引发异常。 当前不支持将 NULL 值插入 TVP 中的列类型 `sql_variant`。

### <a name="implemented-prepared-statement-metadata-caching"></a>实现了预处理语句元数据缓存

JDBC 驱动程序已实现预处理语句元数据缓存以改进性能。 现在驱动程序支持使用 `disableStatementPooling` 和 `statementPoolingCacheSize` 连接属性缓存驱动程序中的预处理语句元数据。 默认情况下，该功能被禁用。 有关详细信息，请参阅 [JDBC 驱动程序的预处理语句元数据缓存](prepared-statement-metadata-caching-for-the-jdbc-driver.md)。

### <a name="added-support-for-azure-ad-integrated-authentication-on-linuxmacos"></a>Linux/macOS 现已开始支持 Azure AD 集成身份验证

JDBC 驱动程序现在支持，在所有受支持的操作系统（Windows、Linux 和 macOS）上结合使用 Azure Active Directory (Azure AD) 集成身份验证和 Kerberos。 或者，在 Windows 操作系统上，用户可以使用 mssql-jdbc_auth-\<version>-\<arch>.dll 进行身份验证。

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-140"></a>更新了“用于 Java 的 Microsoft Azure Active Directory 身份验证库 (ADAL4J)”版本：1.4.0

JDBC 驱动程序已将“用于 Java 的 Microsoft Azure Active Directory 身份验证库 (ADAL4J)”上的 Maven 依赖项更新为版本 1.4.0。 有关依赖项的详细信息，请参阅 [Microsoft JDBC Driver for SQL Server 的功能依赖项](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md)。

## <a name="62"></a>6.2

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 6.2 for SQL Server (self-extracting exe)](https://go.microsoft.com/fwlink/?linkid=2122616)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 6.2 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122615)   

版本号：6.2.2  
发布日期：2017 年 9 月 29 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 self-extracting exe 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40a)  

Microsoft JDBC Driver 6.2 for SQL Server 完全符合 JDBC 规范 4.1 和 4.2。 根据 Java 版本兼容性命名 6.2 包中的 jar。 例如，建议将 6.2 包中的 mssql-jdbc-6.2.2.jre8.jar 文件与 Java 8 配合使用。

### <a name="releases"></a>版本

版本号：6.2.2  
发布日期：2017 年 10 月 3 日  
已解决的问题：  

- 已将 ADAL4J 依赖项更新到版本 1.2.0，并将 Azure Key Vault 依赖项更新到版本 1.0.0

版本号：6.2.1  
发布日期：2017 年 7 月 14 日  
已解决的问题：  

- 修正了使用 `preparedStatement` 运行没有参数的查询时的问题

版本号：6.2.0  
发布日期：2017 年 6 月 30 日  

> [!NOTE]  
> 在 2017 年 6 月 29 日发布的 JDBC 6.2 RTW 中发现了元数据缓存改进方面的问题。 改进已回滚，并且新的 jar（版本 6.2.1）已于 2017 年 7 月 17 日发布。
>
> 另一个改进已将 Azure Key Vault 依赖库版本升级到 1.0.0，并且新的 jar（版本 6.2.2）已于 2017 年 10 月 19 日发布。
>
> 通过上述链接、[GitHub](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2) 或 [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) 下载 JDBC Driver 6.2 的最新更新。 请更新项目以使用 6.2.2 版本 jar。 有关详细信息，请查看 [6.2.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.1) 和 [6.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2) 的发行说明。

### <a name="azure-ad-support-for-linux"></a>适用于 Linux 的 Azure AD 支持

通过用户名/密码和访问令牌方法使用 Azure AD 身份验证将 Linux 应用程序连接到 Azure SQL 数据库。

### <a name="fips-enabled-jvms"></a>已启用 FIPS 的 JVM

JDBC 驱动程序现可在以符合美国联邦信息处理标准 (FIPS) 140 的模式运行的 JVM 上使用，以符合联邦标准。

### <a name="kerberos-authentication-improvements"></a>Kerberos 身份验证改进

JDBC 驱动程序现在具有以下支持：

* Kerberos 配置无法修改或无法检索新令牌或 keytab 的应用程序的主体/密码方法。 此方法可用于对仅允许 Kerberos 身份验证的 SQL Server 实例进行身份验证。
* 跨领域身份验证，方法是使用 Kerberos 集成身份验证，而无需明确设置服务器 SPN。 驱动程序现在会自动计算领域，即使未提供领域也是如此。
* Kerberos 约束委派，方法是通过数据源接受被模拟用户凭据作为 GSS 凭据对象。 然后，此模拟凭据用于建立 Kerberos 连接。

### <a name="added-timeouts"></a>添加了超时

JDBC 驱动程序现在支持以下可配置超时。 可以根据应用程序的需要进行更改。

* 查询超时用于控制在运行查询时发生超时之前等待的秒数。
* 套接字超时用于指定在读取或接受套接字时发生超时之前等待的毫秒数。

## <a name="61"></a>6.1

版本号：6.1.0  
发布日期：2016 年 11 月 17 日  

Microsoft JDBC Driver 6.1 for SQL Server 完全符合 JDBC 规范 4.1 和 4.2。 这是 JDBC 驱动程序的初始开源版本。 可在 [GitHub v6.1.0 标记](https://github.com/microsoft/mssql-jdbc/releases/tag/v6.1.0) 中找到源代码。 其中生成了 mssql-jdbc-6.1.0.jre8.jar 和 mssql-jdbc-6.1.0.jre7.jar 文件，它们对应于 Java 版本兼容性。

## <a name="60"></a>6.0

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 6.0 for SQL Server (self-extracting exe)](https://go.microsoft.com/fwlink/?linkid=2122617)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 6.0 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122714)   

版本号：6.0.8112  
发布日期：2017 年 1 月 17 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 self-extracting exe 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40a)  

Microsoft JDBC Driver 6.0 for SQL Server 完全符合 JDBC 规范 4.1 和 4.2。 根据 JDBC API 版本兼容性命名 6.0 包中的 jar。 例如，6.0 包中的 sqljdbc42.jar 文件符合 JDBC API 4.2。 同样，sqljdbc41.jar 文件符合 JDBC API 4.1。

若要确保 sqljdbc42.jar 或 sqljdbc41.jar 文件正确，请运行以下代码行。 如果输出为“驱动程序版本：6.0.7507.100”，则具有 JDBC 驱动程序 6.0 包。

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="always-encrypted"></a>Always Encrypted

驱动程序支持 SQL Server 2016 中的 Always Encrypted 功能。 此功能可确保从不在 SQL Server 实例中以纯文本形式显示敏感数据。 Always Encrypted 工作方式是，以透明方式加密应用程序中的数据，这样 SQL Server 就只会处理已加密数据，而不会处理纯文本值。 即使 SQL Server 实例或主机计算机遭入侵，攻击者也只能获得敏感数据的已加密文本。 有关详细信息，请参阅[对 JDBC 驱动程序使用 Always Encrypted](using-always-encrypted-with-the-jdbc-driver.md)。

### <a name="internationalized-domain-names"></a>国际化域名

驱动程序支持服务器名称的国际化域名 (IDN)。 有关详细信息，请参阅 [JDBC 驱动程序的国际功能](international-features-of-the-jdbc-driver.md)一文中的“使用国际域名”。

### <a name="parameterized-queries"></a>参数化查询

驱动程序现在支持，使用已准备的语句为复杂查询（如子查询和/或联接）检索参数元数据。 请注意，此改进仅对 SQL Server 2012 及更高版本有效。

### <a name="azure-active-directory"></a>Azure Active Directory

Azure AD 身份验证是一种使用 Azure AD 标识连接到 Azure SQL 数据库 v12 的机制。 Azure AD 身份验证可用于集中管理数据库用户的标识，并替代 SQL Server 身份验证。

可使用 JDBC Driver 6.0 指定 JDBC 连接字符串中的 Azure AD 凭据以连接到 Azure SQL 数据库。 有关详细信息，请参阅[设置连接属性](setting-the-connection-properties.md)一文中的身份验证属性。

### <a name="table-valued-parameters"></a>表值参数

借助 TVP，可以将多行数据从客户端应用程序轻松封送到 SQL Server，而无需进行多次往返或使用特殊的服务器端逻辑来处理数据。 可使用 TVP 来封装客户端应用程序中的数据行，并以一个参数化命令的形式将数据发送到服务器。 传入数据行将存储在随后可使用 Transact-SQL 对其进行操作的表变量中。 有关详细信息，请参阅[使用表值参数](using-table-valued-parameters.md)。

### <a name="always-on-availability-groups"></a>AlwaysOn 可用性组

驱动程序现在支持透明连接到 AlwaysOn 可用性组。 驱动程序快速发现服务器基础结构的当前 AlwaysOn 拓扑，并以透明方式连接到当前的活动服务器。

## <a name="42"></a>4.2

[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 4.2 for SQL Server (self-extracting exe)](https://go.microsoft.com/fwlink/?linkid=2122538)   
[![下载](../../ssms/media/download-icon.png)下载 Microsoft JDBC Driver 4.2 for SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122437)   

版本号：4.2.8112  
发布日期：2015 年 8 月 24 日

如果你需要下载非所检测到的语言的驱动程序，则可使用以下直接链接。  
对于 self-extracting exe 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40a)  
对于 tar.gz 文件中的驱动程序：[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40a)  

Microsoft JDBC Driver 4.2 for SQL Server 完全符合 JDBC 规范 4.1 和 4.2。 根据 JDBC API 版本兼容性命名 4.2 包中的 jar。 例如，4.2 包中的 sqljdbc42.jar 文件符合 JDBC API 4.2。 同样，sqljdbc41.jar 文件符合 JDBC API 4.1。

若要确保 sqljdbc42.jar 或 sqljdbc41.jar 文件正确，请运行以下代码行。 如果输出为“驱动程序版本：4.2.6420.100”，则具有 JDBC 驱动程序 4.2 包。

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="support-for-jdk-8"></a>对 JDK 8 的支持

除了支持 JDK 7.0、6.0 和 5.0 以外，驱动程序还支持 JDK 版本 8.0。

### <a name="jdbc-41-and-42-compliance"></a>JDBC 4.1 和 4.2 的遵从性

除了支持 4.0 以外，驱动程序还支持 Java Database Connectivity API 4.1 和 4.2 规范。 有关详细信息，请参阅 [JDBC 驱动程序的 JDBC 4.1 符合性](jdbc-4-1-compliance-for-the-jdbc-driver.md)和 [JDBC 驱动程序的 JDBC 4.2 符合性](jdbc-4-2-compliance-for-the-jdbc-driver.md)。

### <a name="bulk-copy"></a>大容量复制

大容量复制功能可用于将大量数据快速复制到 SQL Server 数据库中的表或视图。 有关详细信息，请参阅[结合使用大容量复制和 JDBC Driver](using-bulk-copy-with-the-jdbc-driver.md)。

### <a name="xa-transaction-rollback-option"></a>XA 事务回滚选项

驱动程序新增了适用于未准备好事务的现有自动回滚的超时选项。 有关详细信息，请参阅[了解 XA 事务](understanding-xa-transactions.md)。

### <a name="new-kerberos-principal-connection-property"></a>新 Kerberos 主体连接属性

驱动程序使用新连接属性，提高 Kerberos 连接灵活性。 有关详细信息，请参阅[使用 Kerberos 集成身份验证连接到 SQL Server](using-kerberos-integrated-authentication-to-connect-to-sql-server.md)。

## <a name="41"></a>4.1

版本号：4.1.8112  
发布日期：2014 年 12 月 12 日

### <a name="support-for-jdk-7"></a>支持 JDK 7

除了支持 JDK 6.0 和 5.0 以外，驱动程序还支持 JDK 版本 7.0。

## <a name="itanium-not-supported-for-jdbc-driver-applications"></a>JDBC Driver 应用程序不支持 Itanium

不支持 Microsoft JDBC Driver for SQL Server 在 Itanium 计算机上运行。

## <a name="see-also"></a>另请参阅

[JDBC 驱动程序的概述](overview-of-the-jdbc-driver.md)
