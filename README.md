# 使用方法

## 添加脚本到 1Panel 计划任务

1. 在 1Panel 控制面板中，进入"计划任务"页面。
2. 点击"新增任务"，选择任务类型为"Shell 脚本"。
3. 在脚本框中粘贴以下代码：

```bash
#!/bin/bash

# 清理旧的临时目录
rm -rf /tmp/appstore

git clone --depth=1 https://github.com/miludeshiji/1Panel-Apps /tmp/appstore/1Panel-Apps

# 复制 数据（完整复制）
cp -rf /tmp/appstore/1Panel-Apps/apps/* /opt/1panel/resource/apps/local/

# 清理临时目录
rm -rf /tmp/appstore
echo "应用商店数据已更新"
```