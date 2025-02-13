

要删除UFW中的特定规则，请按照以下步骤操作：

1. **列出带编号的规则**：
   ```bash
   sudo ufw status numbered
   ```

2. **找到要删除的规则**。例如，输出可能显示：
   ```
   Status: active

        To                         Action      From
        --                         ------      ----
   [3] 8080/tcp                   ALLOW       172.17.0.0/16
   ```

3. **根据编号删除规则**（假设编号为3）：
   ```bash
   sudo ufw delete 3
   ```

**注意**：确保选择正确的编号，删除后规则将立即生效。

**替代方法**（如果明确规则细节）：
```bash
sudo ufw delete allow proto tcp from 172.17.0.0/16 to any port 8080
```
（根据实际协议和端口调整命令）

**提示**：删除规则前请仔细核对，避免误删。
