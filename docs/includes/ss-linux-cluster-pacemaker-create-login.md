1. **在所有 SQL Server 上，为 Pacemaker 创建 Server 登录名**。 以下 Transact-SQL 创建登录名：

   ```Transact-SQL
   USE [master]
   GO
   CREATE LOGIN [pacemakerLogin] with PASSWORD= N'ComplexP@$$w0rd!'
    
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [pacemakerLogin]
   ```

  创建可用性组时，在创建该可用性组之后到向其中添加任何节点之前，pacemaker 用户需要对可用性组的 ALTER、CONTROL 和 VIEW DEFINITION 权限。

1. **在所有 SQL Server 上，保存 SQL Server 登录名的凭据**。

   ```bash
   echo 'pacemakerLogin' >> ~/pacemaker-passwd
   echo 'ComplexP@$$w0rd!' >> ~/pacemaker-passwd
   sudo mv ~/pacemaker-passwd /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/secrets/passwd
   sudo chmod 400 /var/opt/mssql/secrets/passwd # Only readable by root
   ```
