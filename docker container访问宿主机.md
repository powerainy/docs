

在Linux环境下，Docker容器访问宿主机端口的服务可以通过以下方法实现：

---

### 1. **使用宿主机的Docker网桥网关IP**
默认的Docker桥接网络（`bridge`）中，宿主机对容器的IP通常是网关地址（如 `172.17.0.1`）。  
**操作步骤：**
- **查找网关IP**：
  ```bash
  docker network inspect bridge | grep Gateway
  # 或查看docker0网卡IP
  ip addr show docker0
  ```
- **在容器内通过该IP访问服务**：  
  假设网关IP为 `172.17.0.1`，服务端口为 `8080`，则在容器中使用：
  ```bash
  curl http://172.17.0.1:8080
  ```

---

### 2. **配置 `host.docker.internal` 解析**
在容器中使用 `host.docker.internal` 作为宿主机别名（类似Docker Desktop的功能）。  
**操作步骤：**
- **启动容器时添加主机映射**（Docker 20.10+支持`host-gateway`）：
  ```bash
  docker run --add-host=host.docker.internal:host-gateway your_image
  ```
- **在容器内通过别名访问**：
  ```bash
  curl http://host.docker.internal:8080
  ```

---

### 3. **使用 `host` 网络模式**
直接共享宿主机的网络命名空间，容器内 `localhost` 指向宿主机。  
**操作步骤：**
- **启动容器时指定网络模式**：
  ```bash
  docker run --network=host your_image
  ```
- **在容器内直接访问本地服务**：
  ```bash
  curl http://localhost:8080
  ```
**注意**：此模式会降低隔离性，可能导致端口冲突。

---

### 4. **确保服务监听地址正确**
宿主机服务需绑定到 `0.0.0.0`（而非 `127.0.0.1`），允许外部访问：  
- **检查服务监听地址**：
  ```bash
  ss -tuln | grep :8080
  # 输出应为 0.0.0.0:8080
  ```
- **修改服务配置**：  
  将服务的绑定地址设置为 `0.0.0.0`。

---

### 5. **检查防火墙规则**
确保宿主机防火墙（如 `iptables`、`ufw`）允许来自Docker网络的流量：  
- **开放Docker网段**（如 `172.17.0.0/16`）：
  ```bash
  sudo ufw allow from 172.17.0.0/16 to any port 8080
  ```

---

### **总结**
- **推荐方法**：使用 `host.docker.internal`（需Docker 20.10+）或网关IP `172.17.0.1`。  
- **快速验证**：先确保服务监听 `0.0.0.0`，再通过网关IP测试访问。  
- **注意事项**：避免使用 `host` 网络模式除非必要，优先保证网络安全配置。
