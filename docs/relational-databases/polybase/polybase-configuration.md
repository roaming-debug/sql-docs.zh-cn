---
title: Hadoop 的 PolyBase 配置和安全 | Microsoft Docs
description: 使用这些设置将 PolyBase 连接到 Hadoop，包括 Hadoop.RPC.Protection、CDH 5.X 群集的示例 XML 文件和 Kerberos 配置。
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016 '
ms.openlocfilehash: 140c5d20c18fc7dae3710ae91c37f5ee1bb8d242
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103874"
---
# <a name="polybase-configuration-and-security-for-hadoop"></a>Hadoop 的 PolyBase 配置和安全

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

本文为影响 PolyBase 与 Hadoop 的连接的各种配置设置提供参考。 有关如何将 PolyBase 与 Hadoop 配合使用的演练，请参阅[配置 PolyBase 以访问 Hadoop 中的外部数据](polybase-configure-hadoop.md)。

## <a name="hadooprpcprotection-setting"></a><a id="rpcprotection"></a> Hadoop.RPC.Protection 设置

在 Hadoop 群集中保护通信的常用方法是将 hadoop.rpc.protection 配置更改为“隐私”或“完整性”。 默认情况下，PolyBase 假定配置设置为“身份验证”。 要替代此默认设置，请将以下属性添加到 core-site.xml 文件。 通过更改此配置，可以实现 hadoop 节点之间的安全数据传输，以及 TLS 与 SQL Server 的连接。

```xml
<!-- RPC Encryption information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG -->
   <property>
     <name>hadoop.rpc.protection</name>
     <value></value>
   </property> 
```

若要对 hadoop.rpc.protection 使用“隐私”或“完整性”，SQL Server 必须至少为 SQL Server 2016 SP1 CU7、SQL Server 2016 SP2 或 SQL Server 2017 CU3。

## <a name="example-xml-files-for-cdh-5x-cluster"></a>CDH 5.X 群集的示例 XML 文件

包含 yarn.application.classpath 和 mapreduce.application.classpath 配置的 Yarn-site.xml。

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/,$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

如果你选择将两个配置设置拆分为 mapred-site.xml 和 yarn-site.xml，则这两个文件将如下所示：

**yarn-site.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

**mapred-site.xml**

注意，我们添加了属性 mapreduce.application.classpath。 在 CDH 5.x 中，你会发现遵守 Ambari 中的相同命名约定的配置值。

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration xmlns:xi="http://www.w3.org/2001/XInclude">
   <property>
     <name>mapred.min.split.size</name>
       <value>1073741824</value>
   </property>
   <property>
     <name>mapreduce.app-submission.cross-platform</name>
     <value>true</value>
   </property>
<property>
     <name>mapreduce.application.classpath</name>
     <value>$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH</value>
   </property>


<!--kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
     <name>mapreduce.jobhistory.principal</name>
     <value></value>
   </property>
   <property>
     <name>mapreduce.jobhistory.address</name>
     <value></value>
   </property>
-->
</configuration>
```

## <a name="kerberos-configuration"></a>Kerberos 配置  

请注意，当 PolyBase 向 Kerberos 保护的群集证明身份时，默认情况下需要将 hadoop.rpc.protection 设置设为“身份验证”。 这会使 Hadoop 节点间的数据通信保持非加密状态。 要为 hadoop.rpc.protection 使用“隐私”或“完整性”设置，请在 PolyBase 服务器上更新 core-site.xml 文件。 有关详细信息，请参阅上一节的[使用 Hadoop.rpc.protection 连接到 Hadoop 群集](#rpcprotection)。

使用 MIT KDC 连接到 Kerberos 保护的 Hadoop 群集：

1. 在 SQL Server 的安装路径中查找 Hadoop 配置目录。 通常情况下，该路径为：  

   ```  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf  
   ```  

2. 查找表中列出的配置密钥 Hadoop 端配置值。 （对于 Hadoop 计算机，在 Hadoop 配置目录中查找文件。）  
   
3. 将配置值复制到 SQL Server 计算机上对应文件的值属性中。  
   
   |**#**|**配置文件**|**配置密钥**|**Action**|  
   |------------|----------------|---------------------|----------|   
   |1|core-site.xml|polybase.kerberos.kdchost|指定 KDC 主机名。 例如：kerberos.your-realm.com。|  
   |2|core-site.xml|polybase.kerberos.realm|指定 Kerberos 领域。 例如：YOUR-REALM.COM <br><br>**配置说明**：领域名必须采用大写形式。|  
   |3|core-site.xml|hadoop.security.authentication|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如：KERBEROS<br></br>**安全说明：** KERBEROS 必须采用大写形式。 如果采用小写，则可能不会打开。|   
   |4|hdfs-site.xml|dfs.namenode.kerberos.principal|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如： hdfs/_HOST@YOUR-REALM.COM|  
   |5|mapred-site.xml|mapreduce.jobhistory.principal|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如： mapred/_HOST@YOUR-REALM.COM|  
   |6|mapred-site.xml|mapreduce.jobhistory.address|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如：10.193.26.174:10020|  
   |7|yarn-site.xml yarn。|yarn.resourcemanager.principal|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如： yarn/_HOST@YOUR-REALM.COM|  

4. 创建数据库范围内的凭据对象，以指定每个 Hadoop 用户的身份验证信息。 请参阅 [PolyBase T-SQL 对象](../../relational-databases/polybase/polybase-t-sql-objects.md)。  

## <a name="next-steps"></a>后续步骤  

有关详细信息，请参阅以下文章：

[配置 PolyBase 以访问 Hadoop 中的外部数据](polybase-configure-hadoop.md)
[PolyBase 概述](../../relational-databases/polybase/polybase-guide.md)
