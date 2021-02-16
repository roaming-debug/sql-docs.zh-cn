---
title: VDI 备份规范 - Linux 上的 SQL Server
description: 了解 Linux 上的 SQL Server 虚拟设备接口 (VDI) 客户端 SDK 所提供的接口。
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 0250ba2b-8cdd-450e-9109-bf74f70e1247
ms.openlocfilehash: dcdbd7ba3a00048b63836e52092835fe6f7a618f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343187"
---
# <a name="sql-server-on-linux-vdi-client-sdk-specification"></a>Linux 上的 SQL Server VDI 客户端 SDK 规范

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文档介绍 Linux 上的 SQL Server 虚拟设备接口 (VDI) 客户端 SDK 所提供的接口。 独立软件供应商 (ISV) 可以使用虚拟备份设备应用程序编程接口 (API) 将 SQL Server 集成到其产品中。 通常，Linux 上的 VDI 的行为与 Windows 上的 VDI 行为类似，但存在以下不同：

- Windows 共享内存变成了 POSIX 共享内存。
- Windows 信号灯变成了 POSIX 信号灯。
- HRESULT 和 DWORD 等 Windows 类型改为了整数等效项。
- 删除了 COM 接口，将其替换成了一对 C++ 类。
- Linux 上的 SQL Server 不支持命名实例，因此删除了对实例名称的引用。 
- 共享库在安装于 /opt/mssql/lib/libsqlvdi.so 的 libsqlvdi.so 中实现

本文档是 vbackup.chm 的附录，详细列明了 Windows 的 MS SQL Server VDI 规范  。 下载 [Windows 的 SQL VDI 规范](https://www.microsoft.com/download/details.aspx?id=17282)。

此外，请在 [SQL Server 示例 GitHub 存储库](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sqlvdi-linux)上查看示例 VDI 备份解决方案。

## <a name="user-permissions-setup"></a>用户权限设置

在 Linux 上，POSIX 基元由创建它们的用户及它们的默认组所有。 对于 SQL Server 创建的对象，这些对象将默认由 mssql 用户和 mssql 组所有。 若要允许在 SQL Server 与 VDI 客户端之间进行共享，建议使用以下两种方法之一：

1. 以 mssql 用户身份运行 VDI 客户端
   
   执行以下命令，切换为 mssql 用户：
   
   ```bash
   sudo su mssql
   ```

2. 将 mssql 用户添加到 vdiuser 组，然后将 vdiuser 添加到 mssql 组。
   
   执行以下命令：

   ```bash
   sudo useradd vdiuser
   sudo usermod -a -G mssql vdiuser
   sudo usermod -a -G vdiuser mssql
   ```

   重启服务器，为 SQL Server 和 vdiuser 选取新的组

## <a name="client-functions"></a>客户端函数

本章节包含对每个客户端函数的说明。 说明包含以下信息：

- 函数用途
- 函数语法
- 参数列表
- 返回值
- 备注

## <a name="clientvirtualdevicesetcreate"></a>ClientVirtualDeviceSet::Create

**用途** 此函数用于创建虚拟设备集。

**语法**
   ```
   int ClientVirtualDeviceSet::Create (
   char *   name,       // name for the set
   VDConfig   * cfg     // configuration for the set
   );
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| | name  | 用于标识虚拟设备集。 必须遵循 CreateFileMapping() 所用的名称规则。 可以使用除反斜杠 (\) 以外的任何字符。 这是一个字符串。 建议将用户的产品或公司名称和数据库名称作为字符串前缀。 |
| |**cfg** | 这是虚拟设备集的配置。 有关详细信息，请参阅本文档后面的“配置”。

| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** | 函数成功。 |
| |**VD_E_NOTSUPPORTED** |配置中的一个或多个字段无效或不受支持。 |
| |**VD_E_PROTOCOL** | 虚拟设备集已存在。

**备注** 每个 BACKUP 或 RESTORE 操作仅应调用一次 Create 方法。 调用 Close 方法后，客户端可以重复使用接口创建另一个虚拟设备集。

## <a name="clientvirtualdevicesetgetconfiguration"></a>ClientVirtualDeviceSet::GetConfiguration

**用途** 此函数用于等待服务器配置虚拟设备集。
**语法**
   ```
   int ClientVirtualDeviceSet::GetConfiguration (
   time_t       timeout,    // in milliseconds
   VDConfig *       cfg // selected configuration
   );
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| | **timeout** | 此为超时时间（毫秒）。 使用 INFINITE 或任何负整数来防止超时。
| | **cfg** | 执行成功后，此项将包含服务器所选的配置。 有关详细信息，请参阅本文档后面的“配置”。

| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** | 已返回配置。
| |**VD_E_ABORT** |已调用 SignalAbort。
| |**VD_E_TIMEOUT** |函数超时。

**备注** 在可发出警报状态下此函数会阻塞。 调用成功后，虚拟设备集中的设备可能会打开。


## <a name="clientvirtualdevicesetopendevice"></a>ClientVirtualDeviceSet::OpenDevice
**用途** 此函数将打开虚拟设备集中的某个设备。
**语法**
   ```
   int ClientVirtualDeviceSet::OpenDevice (
   char *           name,       // name for the set
   ClientVirtualDevice **       ppVirtualDevice // returns interface to device
   );
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| | name  |用于标识虚拟设备集。
| | **ppVirtualDevice** |如果此函数成功，会返回指向虚拟设备的指针。 此设备用于 GetCommand 和 CompleteCommand。

| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** |函数成功。
| |**VD_E_ABORT** | 已请求中止。
| |**VD_E_OPEN** |  所有设备均处于打开状态。
| |**VD_E_PROTOCOL** |  设备集未处于正在初始化状态或此特定设备已打开。
| |**VD_E_INVALID** |设备名称无效。 它不是构成设备集的已知名称之一。

**备注** 可能返回 VD_E_OPEN 且无任何问题。 在返回此代码前，客户端可能通过循环方式调用 OpenDevice。
如果配置了多台设备（例如，n 台设备），虚拟设备集将返回 n 个唯一的设备接口   。

此 `GetConfiguration` 函数可用于在可以打开设备前一直等待。
如果此函数不成功，将通过 ppVirtualDevice 返回一个 null 值。
 
## <a name="clientvirtualdevicegetcommand"></a>ClientVirtualDevice::GetCommand

**用途** 此函数用于获取下一个（排队等待应用到设备的）命令。 请求函数后，此函数将等待下一个命令。

**语法**
   ```
   int ClientVirtualDevice::GetCommand (
   time_t       timeout,    // time-out in milliseconds
   VDC_Command**    ppCmd   // returns the next command
   );
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| |**timeout** |此为等待时间（毫秒）。 使用 INFINTE 可无限期等待。 使用 0 可轮询命令。 如果当前无可用的命令，则返回 VD_E_TIMEOUT。 如果发生超时，客户端将决定后续操作。
| |**超时** |此为等待时间（毫秒）。 使用 INFINTE 或负值可无限期等待。 使用 0 可轮询命令。 如果在超时时间过去之前无可用命令，则返回 VD_E_TIMEOUT。 如果发生超时，客户端将决定后续操作。
| |**ppCmd** |成功返回命令后，参数将返回要执行的命令的地址。 返回的内存是只读的。 命令完成后，会将此指针传递到 CompleteCommand 例程。 有关每个命令的详细信息，请参阅本文档后面的“命令”。
        
| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** |已获取命令。
| |**VD_E_CLOSE** |服务器已关闭该设备。
| |**VD_E_TIMEOUT** |无可用命令且超时时间已过。
| |**VD_E_ABORT** |客户端或服务器已使用 SignalAbort 执行强制关闭。

**备注** 如果返回 VD_E_CLOSE，表示 SQL Server 已关闭设备。 这是正常关闭操作的一部分。 在所有设备均已关闭后，客户端将调用 ClientVirtualDeviceSet::Close 关闭虚拟设备集。
如果必须阻塞此例程以等待命令，则该线程将停留在可发出警报的状态。

## <a name="clientvirtualdevicecompletecommand"></a>ClientVirtualDevice::CompleteCommand

**用途** 此函数用于通知 SQL Server 命令已完成。 应返回相应的命令完成信息。 有关详细信息，请参阅本文档后面的“命令”。

**语法** 

   ```
   int ClientVirtualDevice::CompleteCommand (
   VDC_Command pCmd,        // the command
   int  completionCode,     // completion code
   unsigned long    bytesTransferred,   // bytes transferred
   int64_t  position        // current position
   );
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| |**pCmd** |这是之前从 ClientVirtualDevice::GetCommand 返回的命令的地址。
| |**completionCode** |这是指示完成状态的状态代码。 必须为所有命令返回此参数。 返回的代码应适合于正在执行的命令。 在任何情况下，ERROR_SUCCESS 均用于表示已成功执行的命令。 有关可能的代码的完整列表，请参阅文件 vdierror.h。 有关每个命令的典型状态代码列表，请参阅本文档后面的“命令”。
| |**bytesTransferred** |这是已成功传输的字节数。 仅为数据传输命令 Read 和 Write 返回。
| |**position** |这是仅针对 GetPosition 命令的响应。
        
| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** |已正确标注完成情况。
| |**VD_E_INVALID** |pCmd 不是一条活动命令。
| |**VD_E_ABORT** |已发出中止的信号。
| |**VD_E_PROTOCOL** |设备未打开。

**备注** 无

## <a name="clientvirtualdevicesetsignalabort"></a>ClientVirtualDeviceSet::SignalAbort

**用途** 此函数用于发出信号：应发生异常终止。

**语法** 

   ```
   int ClientVirtualDeviceSet::SignalAbort ();
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| |无 | 不适用
        
| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR**|已成功发布“中止”通知。

**Remarks** 任何时候，客户端都可以选择中止 BACKUP 或 RESTORE 操作。 此例程发出信号：应停止所有操作。 整个虚拟设备集的状态进入异常终止状态。 任何设备上都不会返回进一步的命令。 所有未完成的命令将自动完成，并返回 ERROR_OPERATION_ABORTED 作为完成代码。 在安全终止了所有提供给客户端的、尚未使用的缓冲区的使用以后，客户端应调用 ClientVirtualDeviceSet::Close。 有关详细信息，请参阅本文档前面的“异常终止”部分。

## <a name="clientvirtualdevicesetclose"></a>ClientVirtualDeviceSet::Close

**Purpose** 此函数用于关闭 ClientVirtualDeviceSet::Create 创建的虚拟设备集。 其结果是释放与虚拟设备集关联的所有资源。

**语法** 

   ```
   int ClientVirtualDeviceSet::Close ();
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| |无 |不适用
        
| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** |将在虚拟设备集成功关闭后返回。
| |**VD_E_PROTOCOL** |未采取任何操作，因为虚拟设备集未打开。
| |**VD_E_OPEN** |设备仍处于打开状态。

**Remarks** 对 Close 的调用是一条客户端声明，表示应释放虚拟设备集所用的所有资源。 调用 Close 前，客户端须确保已终止涉及数据缓冲区和虚拟设备的所有活动。 Close 会使由 OpenDevice 返回的所有虚拟设备接口失效。
在返回 Close 调用后，允许客户端在虚拟设备集接口上发出一个 Create 调用。 此类调用将为后续的 BACKUP 或 RESTORE 操作创建新的虚拟设备集。
如果在一台或多台虚拟设备处于打开状态时调用 Close，则会返回 VD_E_OPEN。 在这种情况下，如果可能，将在内部触发 SignalAbort 以确保正确关闭。 将释放 VDI 资源。 调用 ClientVirtualDeviceSet::Close 前，客户端应在每台设备上等待 VD_E_CLOSE 指示。 如果客户端知道虚拟设备集已处于异常终止状态，则它应不会收到 GetCommand 发出的 VD_E_CLOSE 指示，并且当共享缓冲区上的活动终止后，它可以调用 ClientVirtualDeviceSet::Close。
有关详细信息，请参阅本文档前面的“异常终止”部分。

## <a name="clientvirtualdevicesetopeninsecondary"></a>ClientVirtualDeviceSet::OpenInSecondary

**Purpose** 此函数打开次要客户端中的虚拟设备集。 主客户端必须已使用 Create 和 GetConfiguration 设置了虚拟设备集。

**语法** 
   
   ```
   int ClientVirtualDeviceSet::OpenInSecondary (
   char *   setName         // name of the set
   );
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| |**setName** |用于标识设备集。 此名称区分大小写，且必须与主客户端调用 ClientVirtualDeviceSet::Create 时所用的名称相匹配。

| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** |函数成功。
| |**VD_E_PROTOCOL** |尚未创建虚拟设备集，虚拟设备集已在此客户端上打开或虚拟设备集尚未准备好接受来自次要客户端的打开请求。
| |**VD_E_ABORT** |正在中止操作。

**Remarks** 使用多个进程模型时，主客户端负责检测次要客户端的正常或异常终止。

## <a name="clientvirtualdevicesetgetbufferhandle"></a>ClientVirtualDeviceSet::GetBufferHandle

**Purpose** 某些应用程序可能需要多个进程，以对由 ClientVirtualDevice::GetCommand 返回的缓冲区进行操作。 在这种情况下，接收命令的进程可使用 GetBufferHandle 获取用于标识缓冲区的、与进程无关的句柄。 随后可将此句柄传达给打开同一虚拟设备集的其他任何进程。 然后，此进程将使用 ClientVirtualDeviceSet::MapBufferHandle 获取缓冲区的地址。 因为每个进程可能映射不同地址中的缓冲区，因此该地址可能与其伙伴进程中的地址不同。

**语法** 

   ```
   int ClientVirtualDeviceSet::GetBufferHandle (
   uint8_t*     pBuffer,        // in: buffer address
   unsigned int*        pBufferHandle   // out: buffer handle
   );
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| |**pBuffer** |这是通过 Read 或 Write 命令获取的缓冲区的地址。
| |**BufferHandle** |返回缓冲区的唯一标识符。

| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** |函数成功。
| |**VD_E_PROTOCOL** |当前未打开虚拟设备集。
| |**VD_E_INVALID** |pBuffer 不是有效地址。

备注：调用 GetBufferHandle 函数的进程负责在数据传输完成后调用 ClientVirtualDevice::CompleteCommand。

## <a name="clientvirtualdevicesetmapbufferhandle"></a>ClientVirtualDeviceSet::MapBufferHandle

**Purpose** 此函数用于从通过其他某个进程获取的缓冲区句柄中获取有效的缓冲区地址。 

**语法** 

   ```
   int ClientVirtualDeviceSet::MapBufferHandle (
   i        nt  dwBuffer,   // in: buffer handle
   uint8_t**    ppBuffer        // out: buffer address
   );
   ```

| parameters | 参数 | 说明
| ----- | ----- | ------ |
| |**dwBuffer** |这是由 ClientVirtualDeviceSet::GetBufferHandle 返回的句柄。
| |**ppBuffer** |这是当前进程中有效缓冲区的地址。

| 返回值 | 参数 | 说明
| ----- | ----- | ------ |
| |**NOERROR** |函数成功。
| |**VD_E_PROTOCOL** |当前未打开虚拟设备集。
| |**VD_E_INVALID** |ppBuffer 是无效的句柄。

**Remarks** 为了与句柄正确通信，必须格外谨慎。 对于单个虚拟设备集而言，句柄是本地的。 共享句柄的合作伙伴进程必须确保仅在最初获取缓冲区的虚拟设备集范围内使用缓冲区句柄。


