n Core CPU 限制CPU, 比如单核限制25%
```bash
systemctl set-property user.slice CPUQuota=n*25%
```
可以用stress来测试:
```bash
stress --cpu $(nproc)
```
若stress未安装,执行以下语句来安装:
```bash
apt install stress
```
若要解除限制,执行即可:
```bash
systemctl set-property user.slice CPUQuota=
```
验证是否生效,执行:
```bash
systemctl show user.slice | grep CPUQuota
```
若输出为CPUQuota=infinity 或未显示 CPUQuota，则表示限制已取消。
