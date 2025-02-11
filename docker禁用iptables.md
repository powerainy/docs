

要通过 UFW 禁用 Docker 端口而不依赖 Docker 自动生成的 iptables 规则，需修改 Docker 的默认网络配置并手动管理防火墙规则。以下是分步解决方案：

---

### **步骤 1：禁止 Docker 操作 iptables**
1. **修改 Docker 配置文件**  
   编辑 `/etc/docker/daemon.json`，添加以下内容（若文件不存在则新建）：
   ```json
   {
     "iptables": false
   }
   ```
   这会禁止 Docker 自动配置 iptables，避免规则冲突。

2. **重启 Docker 服务**  
   ```bash
   sudo systemctl restart docker
   ```

---

### **步骤 2：配置 UFW 默认策略**
1. **设置默认拒绝所有入站/转发流量**  
   ```bash
   sudo ufw default deny incoming
   sudo ufw default deny forward
   ```

2. **允许必要的基础通信（如 SSH）**  
   ```bash
   sudo ufw allow 22/tcp  # 根据实际 SSH 端口调整
   ```

---

### **步骤 3：手动允许容器网络（可选）**
如果容器需要访问外部网络，需手动添加 NAT 规则。编辑 UFW 的 **`/etc/ufw/after.rules`** 文件，在末尾追加以下内容：
```bash
*nat
:POSTROUTING ACCEPT [0:0]
-A POSTROUTING -s 172.17.0.0/16 ! -o docker0 -j MASQUERADE
COMMIT
```
此规则允许 Docker 默认网段 `172.17.0.0/16` 的容器通过 NAT 访问外部网络。

---

### **步骤 4：禁用 Docker 暴露的端口**
1. **阻止 Docker 服务的默认端口**  
   ```bash
   sudo ufw deny 2375/tcp  # Docker 未加密通信端口
   sudo ufw deny 2376/tcp  # Docker TLS 加密端口
   sudo ufw deny 2377/tcp  # Swarm 模式端口
   ```

2. **阻止应用程序使用的端口**  
   例如，若容器映射了 `80` 和 `443` 端口：
   ```bash
   sudo ufw deny 80/tcp
   sudo ufw deny 443/tcp
   ```

---

### **步骤 5：启用 UFW 并验证**
1. **激活 UFW 防火墙**  
   ```bash
   sudo ufw enable
   ```

2. **检查规则是否生效**  
   ```bash
   sudo ufw status verbose
   ```

3. **测试端口访问**  
   使用 `nmap` 或 `telnet` 验证端口是否已关闭：
   ```bash
   nmap -p 80,2375 your_server_ip
   ```

---

### **注意事项**
- **容器间通信**：上述配置可能影响容器间通信，如需保留内部通信，可在 `after.rules` 中添加允许规则。
- **Docker 重启影响**：每次 Docker 服务重启后，需确保手动 iptables 规则（如 NAT）仍有效。
- **兼容性**：此方法适用于 Docker 默认的 `bridge` 网络模式，自定义网络可能需要额外配置。

通过以上步骤，Docker 将不再绕过 UFW，所有端口暴露均由 UFW 直接控制。
