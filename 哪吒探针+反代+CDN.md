### 来自一点科技 https://1keji.net/t/topic/62
开启哪吒探针面板域名的CDN功能，以及解决在CDN和Agent 使用 TLS 连接的情况下，不能添加监控端的问题。

以下是完整安装步骤：

第一步：
1.解析一个域名到安装哪吒面板的服务器。哪吒v1新版只需要一个域名就可以了。
例如：
https://vps.1keji.net
2.生成ssl证书

第二步
安装Docker 和 Docker Compose

一键脚本：
```bash
wget -O 1keji_docker.sh "https://pan.1keji.net/f/rRi2/1keji_docker.sh" && chmod +x 1keji_docker.sh && ./1keji_docker.sh
```
上传ssl文件

修改root文件夹权限

chmod 777 /root

第三步：（如果服务器上已经有docker和哪吒面板，可以直接看这一步）
运行nginx反代和tls的一键脚本（已添加自动跳转https）
```bash
wget -O 1keji_nginxfdnz06.sh "https://pan.1keji.net/f/P9u3/1keji_nginxfdnz06.sh" && chmod +x 1keji_nginxfdnz06.sh && ./1keji_nginxfdnz06.sh
```
下面这个脚本，添加了宝塔安装nginx的使用支持：
```bash
wget -O 1keji_nznginx.sh "https://pan.1keji.net/f/YJTA/1keji_nznginx.sh" && chmod +x 1keji_nznginx.sh && ./1keji_nznginx.sh
```
第四步：（如果已经安装过可以忽略）
安装哪吒v1新版的一键脚本：
```bash
curl -L https://raw.githubusercontent.com/nezhahq/scripts/refs/heads/main/install.sh -o nezha.sh && chmod +x nezha.sh && sudo ./nezha.sh
```
如果提示需要安装unzip，执行以下代码安装就可以了
```bash
apt install unzip -y
```

哪吒探针官方安装文档：https://nezha.wiki

卸载 Agent
卸载 Agent 包括停止服务、卸载服务，以及删除相关文件。以下是 Ubuntu 系统的卸载步骤：

停止并卸载服务：
```bash
cd /opt/nezha/agent/
./nezha-agent service uninstall
```
删除 Agent 文件夹：
```bash
rm -rf /opt/nezha/agent/
```
CDN回源IP：

set_real_ip_from 173.245.48.0/20;

set_real_ip_from 103.21.244.0/22;

set_real_ip_from 103.22.200.0/22;

set_real_ip_from 103.31.4.0/22;

set_real_ip_from 141.101.64.0/18;

set_real_ip_from 108.162.192.0/18;

set_real_ip_from 190.93.240.0/20;

set_real_ip_from 188.114.96.0/20;

set_real_ip_from 197.234.240.0/22;

set_real_ip_from 198.41.128.0/17;

set_real_ip_from 162.158.0.0/15;

set_real_ip_from 104.16.0.0/12;

set_real_ip_from 172.64.0.0/13;

set_real_ip_from 131.0.72.0/22;

real_ip_header CF-Connecting-IP;

旧脚本存档
nginx脚本
```bash
wget -O 1keji_nginxfdnz.sh "https://pan.1keji.net/f/JWcK/1keji_nginxfdnz.sh" && chmod +x 1keji_nginxfdnz.sh && ./1keji_nginxfdnz.sh
```
下面这个脚本，添加了宝塔安装nginx的使用支持：
```bash
wget -O 1keji_nznginx.sh "https://pan.1keji.net/f/YJTA/1keji_nznginx.sh" && chmod +x 1keji_nznginx.sh && ./1keji_nznginx.sh
```
