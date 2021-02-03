---
description: MSSQLSERVER_17832
title: MSSQLSERVER_17832 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17828 (Database Engine error)
- maxtokensize
- 17832 (Database Engine error)
- login packet (bad)
ms.assetid: bd56ffe4-0855-4ada-8aca-251fbc6ff2ce
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e11485d4acc9638741f3ed48943d314a3efb0449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196634"
---
# <a name="mssqlserver_17832"></a>MSSQLSERVER_17832
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|17832|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SRV_BAD_LOGIN_PKT|  
|消息正文|用于打开该连接的登录数据包的结构无效；该连接已关闭。 请与客户端库的供应商联系。%.*ls|  
  
## <a name="explanation"></a>说明  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机无法处理客户端登录数据包。 这可能是由于未正确创建数据包或数据包在传输过程中受损造成的。 也可能是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机的配置引起的。 所列出的 IP 地址为客户端计算机的地址。  
  
### <a name="more-information"></a>更多信息  
当在 Kerberos 环境中使用 Windows 身份验证时，客户端会接收包含特权属性证书 (PAC) 的 Kerberos 票证。 PAC 包含各种类型的身份验证数据，包括用户所在的组、用户拥有的权限以及对用户应用的策略。 当客户端接收 Kerberos 票证时，包含在 PAC 中的信息将用于生成用户的访问标记。 客户端会将该标记作为登录数据包的组成部分提交给 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机。  
  
如果未正确创建该标记或该标记在传输过程中受损，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法提供有关此问题的其他信息。  
  
如果用户是多个组的成员或具有多个策略，则该标记的长度可能会比正常标记大一些以全部列出这些信息。 如果该标记的长度大于服务器计算机的 **MaxTokenSize** 值，则客户端将无法连接并显示常规网络错误 (GNE)，且会出现错误 17832。 此问题可能只会影响某些用户，即拥有多个组的成员身份或具有多个策略的用户。 当问题在于服务器计算机的 **MaxTokenSize** 值时，在出现 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中的错误 17832 的同时还会出现状态为 9 的错误。 有关 Kerberos 和 **MaxTokenSize** 的其他详细信息，请参阅 [KB327825](https://support.microsoft.com/kb/327825)。  
  
## <a name="user-action"></a>用户操作  
若要解决此问题，请将服务器计算机的 **MaxTokenSize** 值增大到足以包含单位中任一用户的最长标记的大小。 若要研究适合于您单位的正确标记长度，请考虑使用 **Tokensz** 应用程序。  
  
> [!CAUTION]  
> [!INCLUDE[ssNoteRegistry](../../includes/ssnoteregistry-md.md)]  
  
**更改服务器计算机上的 MaxTokenSize**  
  
1.  在 **“开始”** 菜单上，单击 **“运行”** 。  
  
2.  键入 **regedit**，然后单击“确定”。 （如果此时出现“用户帐户控制”对话框，请单击“继续”。）   
  
3.  导航到 **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa\Kerberos\Parameters**。  
  
4.  如果 **MaxTokenSize** 参数不存在，请右键单击“参数”，指向“新建”，然后单击“DWORD (32 位)”值。   将注册表项命名为 **MaxTokenSize**。  
  
5.  右键单击 **MaxTokenSize**，然后单击“修改”。  
  
6.  在“数值数据”框中键入所需的 **MaxTokenSize** 值。  
  
    > [!NOTE]  
    > 建议使用的最大标记长度为十六进制值 ffff（十进制值 65535）。 提供此值后很可能会解决问题，但可能会对计算机的性能产生负面影响。 建议您建立可包含单位中任一用户最长标记的最小 **MaxTokenSize** 值并输入该值。  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
8.  关闭 **注册表编辑器**。  
  
9. 重新启动计算机。  
  
