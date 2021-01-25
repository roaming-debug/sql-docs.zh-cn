---
description: 将具有安全 Enclave 的 Always Encrypted 与 JDBC 驱动程序结合使用
title: 将具有安全 Enclave 的 Always Encrypted 与 JDBC 驱动程序结合使用 | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: reneye
ms.author: v-reye
ms.openlocfilehash: 4016f3eb5d725673b1e4149d43dc21d20cdc627f
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534636"
---
# <a name="using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver"></a>将具有安全 Enclave 的 Always Encrypted 与 JDBC 驱动程序结合使用
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

本页介绍如何使用 [具有安全 Enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md) 和 Microsoft JDBC Driver for SQL Server 8.2（或更高版本）来开发 Java 应用程序。

安全 Enclave 是现有 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) 功能的补充。 安全 Enclave 旨在解决使用 Always Encrypted 数据时的限制。 以前，用户只能对 Always Encrypted 数据执行相等性比较，并且必须检索和解密数据才能执行其他操作。 而使用安全 Enclave，可以在服务器端对安全 Enclave 内的纯文本数据进行计算，从而解决了这一限制。 安全 enclave 是 SQL Server 进程内受保护的内存区域，并充当用于处理 SQL Server 引擎中的敏感数据的受信任执行环境。 安全 enclave 显示为托管计算机上的其余 SQL Server 和其他进程的黑盒。 无法从外部查看 enclave 内的任何数据或代码，即使采用调试程序也是如此。

## <a name="prerequisites"></a>先决条件
- 确保在开发计算机上安装 Microsoft JDBC Driver for SQL Server 8.2（或更高版本）。
- 验证环境依赖项（如 Dll、密钥库等）是否位于正确的路径中。 具有安全 Enclave 的 Always Encrypted 是现有 [Always Encrypted](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md) 功能的附加产品，与其有类似的先决条件。

> [!Note]
> 如果使用的是旧版本的 JDK 8，则必须下载并安装 Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files。 请确保阅读包含在 zip 文件中的自述文件，以获取安装说明和导出/导入问题的相关详细信息。  
>
> 可以从 [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8 下载](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)下载策略文件

## <a name="setting-up-secure-enclaves"></a>设置安全 Enclave
按照[教程：在 SQL Server 中开始使用具有安全 enclave 的 Always Encrypted](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) 或[教程：在 Azure SQL 数据库中开始使用具有安全 enclave 的 Always Encrypted](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)，开始使用安全 enclave。 有关更深入的详细信息，请参阅[具有安全 Enclave 的 Always Encrypted](../../relational-databases/security/encryption/always-encrypted-enclaves.md)。

## <a name="connection-string-properties"></a>连接字符串属性

若要为数据库连接启用 enclave 计算，除了启用 Always Encrypted 之外，还需要设置以下连接字符串关键字。

- enclaveAttestationProtocol - 指定证明协议。 
  - 如果使用的是 [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] 和主机保护者服务 (HGS)，则此关键字的值应为 `HGS`。
  - 如果使用的是 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 Microsoft Azure 证明，则此关键字的值应为 `AAS`。

- enclaveAttestationUrl - 指定证明 URL（证明服务终结点）。 你需要从证明服务管理员处获取环境的证明 URL。
  - 如果使用的是 [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] 和主机保护者服务 (HGS)，请参阅[确定并共享 HGS 证明 URL](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url)。
  - 如果使用的是 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 Microsoft Azure 证明，请参阅[确定证明策略的证明 URL](/azure-sql/database/always-encrypted-enclaves-configure-attestation#determine-the-attestation-url-for-your-attestation-policy)。

用户必须启用 columnEncryptionSetting 并正确设置上述两个连接字符串属性，才能从 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 中启用具有安全 Enclave 的 Always Encrypted。

## <a name="working-with-secure-enclaves"></a>使用安全 Enclave
如果正确设置了 Enclave 连接属性，此功能将以透明方式工作。 驱动程序将确定查询是否需要自动使用安全 Enclave。 下面是触发 enclave 计算的查询示例。 可以在[教程：在 SQL Server 中开始使用具有安全 enclave 的 Always Encrypted](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) 或[教程：在 Azure SQL 数据库中开始使用具有安全 enclave 的 Always Encrypted](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) 中找到数据库和表设置。


大量查询将触发 Enclave 计算：
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL)) {
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SSN LIKE ?")) {
        p.setString(1, "%6818");
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
    
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SALARY > ?")) {
        ((SQLServerPreparedStatement) p).setMoney(1, new BigDecimal(0));
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
}
```

在列上切换加密也会触发 Enclave 计算：
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL);Statement s = c.createStatement()) {
    s.executeUpdate("ALTER TABLE Employees ALTER COLUMN SSN CHAR(11) NULL WITH (ONLINE = ON)");
}
```

## <a name="java-8-users"></a>Java 8 用户
此功能需要 RSASSA-PSA 签名算法。 此算法是在 JDK 11 中添加的，但不会反向移植到 JDK 8。 如果用户想要将此功能与 JDK 8 版本的 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 一起使用，则必须加载自己的提供程序，该提供程序需支持 RSASSA-PSA 签名算法，或包含 BouncyCastleProvider 可选的依赖项。 如果 JDK 8 向后移植签名算法，或者 JDK 8 的支持生命周期结束，则将在以后删除依赖项。

## <a name="see-also"></a>另请参阅
[通过 JDBC 驱动程序使用 Always Encrypted](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)  
