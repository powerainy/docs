创建一个SSH用户 `user001`，并且通过配置SSH服务器，将其限制在 `/home/user001` 目录中，无法访问其他路径。这个用户只能通过SFTP协议进行文件传输，无法通过SSH登录服务器并执行其他操作。

### **步骤解析：**

1. **创建用户并设置shell：**
   ```bash
   useradd -m -d /home/user001 -s /usr/bin/ssh-rsa user001
   ```
   - 这里，`-s /usr/bin/ssh-rsa` 将用户的shell设置为`ssh-rsa`，这意味着该用户只能通过SFTP进行文件传输，无法通过SSH登录并获得完整的shell访问。

2. **设置用户密码：**
   ```bash
   passwd user001
   ```
   为用户设置密码，以便通过SFTP登录。

3. **配置SSH服务器：**
   在 `/etc/ssh/sshd_config` 中添加：
   ```
   Match User user001
       ChrootDirectory /home/user001
       ForceCommand internal-sftp
       AllowTcpForwarding no
       X11Forwarding no
   ```
   - `ChrootDirectory` 将用户限制在指定的目录中。
   - `ForceCommand internal-sftp` 强制用户只能使用SFTP协议。
   - `AllowTcpForwarding no` 和 `X11Forwarding no` 禁止TCP转发和X11转发，进一步限制用户权限。

4. **确保目录权限：**
   ```bash
   chmod 755 /home/user001
   chown user001:user001 /home/user001
   ```
   - 确保 `/home/user001` 目录权限正确，用户 `user001` 拥有读、写和执行权限。

5. **重启SSH服务：**
   ```bash
   sudo systemctl restart sshd
   ```
   应用新的SSH配置。

6. **测试配置：**
   - 使用SFTP客户端连接：
     ```bash
     sftp user001@server-ip
     ```
     你应该只能访问 `/home/user001` 目录，无法访问其他路径。
   - 尝试通过SSH登录：
     ```bash
     ssh user001@server-ip
     ```
     根据配置，这应该是不允许的，用户无法获得shell访问权限。

### **总结：**

通过上述步骤，你已经成功创建了一个SSH用户 `user001`，该用户只能通过SFTP协议访问 `/home/user001` 目录，无法通过SSH登录服务器并执行其他操作。这确保了服务器的安全性，防止用户滥用权限。

如果你需要允许用户执行其他操作，比如安装软件，你可能需要调整配置，例如更改用户的shell，或者为用户分配更高的权限。但根据你的描述，限制用户只能在特定目录中进行文件传输是正确的做法，以确保服务器的安全。
