### 添加开机自启项目 chatgpt-web

### Chatgpt_servic内容：

创建开机shell脚本：

```
vim  /root/chatgpt-web/chatgpt_service.sh
```

```
#!/bin/bash
export PATH=$PATH:/root/.nvm/versions/node/v18.15.0/bin/
cd /root/chatgpt-web/service
pnpm start
```

#### Chatgpt_service自启：

可以按照以下步骤将`chatgpt_service.sh`脚本设置为系统启动时自动运行：

将`chatgpt_service.sh`脚本复制到`/usr/local/bin/`目录下：

```
sudo cp /root/chatgpt-web/chatgpt_service.sh /usr/local/bin/
```

创建一个名为`chatgpt_service.service`的`systemd`服务单元文件：

```
sudo vim /etc/systemd/system/chatgpt_service.service
```

将以下内容添加到`chatgpt_service.service`文件中：

```
[Unit]
Description=ChatGPT service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/chatgpt_service.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

保存并关闭文件，重新加载`systemd`并启用服务：

```
sudo systemctl daemon-reload
sudo systemctl enable chatgpt_service.service
```

验证服务是否已启用：

```
sudo systemctl is-enabled chatgpt_service.service
```

如果服务已经启用，则会返回`enabled`。

现在您可以重启系统，`chatgpt_service`服务将自动启动。您可以使用以下命令来管理服务的状态：

```
sudo systemctl start chatgpt_service.service    # 启动服务
sudo systemctl stop chatgpt_service.service     # 停止服务
sudo systemctl restart chatgpt_service.service  # 重启服务
sudo systemctl status chatgpt_service.service   # 查看服务状态
```

---

### Chatgpt_web_service内容：

创建开机shell脚本：

```
vim  /root/chatgpt-web/chatgpt_web.sh
```

```
#!/bin/bash
export PATH=$PATH:/root/.nvm/versions/node/v18.15.0/bin/
cd /root/chatgpt-web
pnpm dev
```

### Chatgpt_web_service自启：

复制脚本到/usr/local/bin/

```
sudo cp /root/chatgpt-web/chatgpt_web.sh /usr/local/bin/
```

创建一个名为`chatgpt_web.service`的`systemd`服务单元文件：

```
sudo vim /etc/systemd/system/chatgpt_web.service
```

将以下内容添加到`chatgpt_web.service`文件中：

```
[Unit]
Description=ChatGPT web
After=network.target
Requires=chatgpt_service.service
After=chatgpt_service.service

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/chatgpt_web.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

这里添加了 `Requires` 和 `After` 来确保在启动 `chatgpt_web.service` 之前， `chatgpt_service.service` 已经成功启动。

#### 保存并关闭文件，重新加载`systemd`并启用服务：

```
sudo systemctl daemon-reload
sudo systemctl enable chatgpt_web.service
```

验证服务是否已启用：

```
sudo systemctl is-enabled chatgpt_web.service
```

如果服务已经启用，则会返回`enabled`

启动管理：

```
sudo systemctl start chatgpt_web.service    # 启动服务
sudo systemctl stop chatgpt_web.service     # 停止服务
sudo systemctl restart chatgpt_web.service  # 重启服务
sudo systemctl status chatgpt_web.service   # 查看服务状态

```

