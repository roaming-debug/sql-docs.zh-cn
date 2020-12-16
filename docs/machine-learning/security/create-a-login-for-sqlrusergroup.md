---
title: 为 SQLRUserGroup 创建登录名
description: 在 SQL Server 中为 SQLRUserGroup 创建登录名，使用默示身份验证登录到服务器，将身份转换回调用用户。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/25/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9734b1315aa782f83f8f02cfb9d0d78da52460d0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470728"
---
# <a name="create-a-login-for-sqlrusergroup"></a>为 SQLRUserGroup 创建登录名
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

当脚本中的[环回连接](../../relational-databases/security/authentication-access/create-a-login.md)指定了信任连接，且用于执行包含代码的对象的标识是 Windows 用户帐户时，在 [SQL Server](../concepts/security.md#sqlrusergroup) 中为 [SQLRUserGroup](../../machine-learning/concepts/security.md#implied-authentication) 创建登录名  。

信任连接是那些在连接字符串中具有 `Trusted_Connection=True` 的连接。 当 SQL Server 收到指定信任连接的请求时，它将检查当前 Windows 用户的标识是否具有登录名。 对于作为工作线程帐户执行的外部进程（例如 SQLRUserGroup 中的 MSSQLSERVER01），请求失败是因为默认情况下这些帐户没有登录名  。

可以通过为 SQLServerRUserGroup 创建登录名来解决连接错误问题  。 有关标识和外部进程的详细信息，请参阅[扩展性框架安全概述](../concepts/security.md)。

> [!Note]
> 请确保 SQLRUserGroup 具有“允许本地登录”权限  。 默认情况下，会向所有本地新用户授予此权限，但某些组织更严格的组策略可能会禁用此权限。

## <a name="create-a-login"></a>创建登录名

1. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的“对象资源管理器”中，展开“安全性”  ，右键单击“登录名”  ，然后选择“新建登录名”  。

2. 在“登录名 - 新建”对话框中，选择“搜索”   。 （请勿在该框中键入任何内容。）
    
     ![单击“搜索”为机器学习添加新的登录名](media/implied-auth-login1.png "单击“搜索”为机器学习添加新的登录名")

3. 在“选择用户或组”框中，单击“对象类型”按钮   。

     ![搜索对象类型为机器学习添加新的登录名](media/implied-auth-login2.png "搜索对象类型为机器学习添加新的登录名")

4. 在“对象类型”对话框中，选择“组”   。 清除所有其他复选框。

     ![在“对象类型”对话框中选择“组”](media/implied-auth-login3.png "在“对象类型”对话框中选择“组”")

4. 单击“高级”，验证要搜索的位置是否位于当前计算机，然后单击“立即查找”   。

     ![单击“立即查找”获取组列表](media/implied-auth-login4.png "单击“立即查找”获取组列表")

5. 滚动服务器上的组帐户列表，直到找到一个以 `SQLRUserGroup` 开头的组帐户。
    
    + 无论安装的是 R 还是 Python，或者两者都已安装，与默认实例的 Launchpad 服务相关联的组名称始终为 SQLRUserGroup   。 仅为默认实例选择此帐户。
    + 如果使用的是命名实例，则实例名称将附加到默认工作组名称  _的名称之后_`SQLRUserGroup`。 例如，如果实例名称为“MLTEST”，则此实例的默认用户组名称将为 SQLRUserGroupMLTest  。
 
    ![服务器上的组示例](media/implied-auth-login5.png "服务器上的组示例")
   
5. 单击“确定”关闭高级搜索对话框  。

    > [!IMPORTANT]
    > 请确保已为实例选择了正确的帐户。 每个实例只能使用自己的 Launchpad 服务以及为该服务创建的组。 实例不能共享 Launchpad 服务或工作线程帐户。

6. 再次单击“确定”关闭“选择用户或组”对话框   。

7. 在“登录名 - 新建”对话框中，单击“确定”   。 默认情况下，将登录名分配到 **公共** 角色，且有权连接到数据库引擎。

## <a name="next-steps"></a>后续步骤

+ [安全性概述](../concepts/security.md)
+ [扩展性框架](../concepts/extensibility-framework.md)