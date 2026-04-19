---
title: Git多用户配置
date: 2026-04-19 13:30
tags: [Git]
---


## Git 多用户配置

1. 生成key

   ``` bash
   ssh-keygen -t rsa -C "youremail@example.com"
   // 回车后按提示输入密钥路径与名称
   // Enter file in which to save the key (/Users/duanrui/.ssh/id_rsa): ~/.ssh/id_rsa_github
   ```

2. 进入`ssh-agent`

   ```bash
   eval `ssh-agent -s`
   ```

3. 添加ssh密钥

   ``` bash
   ssh-add ~/.ssh/id_rsa_github
   ```

4. 按上述步骤，添加多个账户信息，每个ssh公钥添加到对应平台的ssh设置内

5. 编写config文件，放入~/.ssh路径，内容如下

   ``` properties
   # gitlab
   Host gitlab.com
   HostName gitlab.txzing.com
   User myuser
   IdentityFile ~/.ssh/id_rsa.gitlab
   
   # github
   Host github.com
   HostName github.com
   User myuser
   IdentityFile ~/.ssh/id_rsa.github
   
   # gitee
   Host gitee.com
   HostName gitee.com
   User myuser
   IdentityFile ~/.ssh/id_rsa.gitee
   ```

6. 测试连接

   ``` base
   ssh -T git@github.com
   ```