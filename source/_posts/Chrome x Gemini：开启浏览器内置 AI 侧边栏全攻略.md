---
title: 🚀 Chrome x Gemini：开启浏览器内置 AI 侧边栏全攻略
data: 2026/4/21 0:51
tags: [Chrome]
---

Google 近期在 Chrome 浏览器中深度集成了 Gemini AI。通过侧边栏模式，你可以在不离开当前网页的情况下，直接调用 AI 进行总结、分析和创作。

## 一、 核心功能优势
零插件依赖：原生集成，响应速度快，无需安装第三方扩展。

上下文感知：支持读取“当前网页”或“全部网页”，省去复制粘贴的繁琐步骤。

多文档联动：可同时分析多个已打开的标签页内容，适合做资料汇总。

常驻便捷：侧边栏设计，边看网页边对话，互不干扰。


## 二、 手把手开启步骤
1. 环境准备
   + 更新浏览器：确保 Google Chrome 已更新至最新版本（建议 V131 及以上）
   + 自备魔法，最好是美国节点
   + 登录账号：确保已在浏览器中登录你的 Google 账号

2. Chrome浏览器页面语言设置为英文
   + 打开Chrome设置页，找到`语言/Languages`选项
   + 首选语言设置为英语, 注意: 一定要选中右边三个点内的`以此语言显示 Chrome 界面内容`选项
   + 重启Chrome

3. Google账号地址设置为美国
   + 打开设置页，找到`You and Google`选项
   + 点击账户，找到`Manage you Google Account`选项进入账户管理页面
   + 在`Personal info`设置内找到`Home address`，地址选到美国

4. 关闭Chrome浏览器, 用命令行执行以下命令
   + Windows(PowerShell): 
     ``` powershell
     irm https://raw.githubusercontent.com/appsail/Gemini-in-Chrome/main/install.ps1 | iex
     ```
   + Mac: 
     ``` shell
     open -n -a "Google Chrome" --args --variations-override-countrys=us
     ```

5. 重新打开Chrome浏览器, 可以看到地址栏右侧有个Ask Gemini, 便可以直接开始使用了

## 三、 实战提效场景
场景 1：长文一键摘要
操作：打开一篇长篇资讯或论文，在 Gemini 侧边栏输入：“总结这篇文章的核心观点”。

效果：AI 会直接读取当前页面内容，瞬间生成精简的要点说明。

场景 2：跨页面综合分析
操作：同时打开 3-5 个竞品或资料页面。在 Gemini 侧边栏的范围选项中选择 “全部网页”。

指令：例如“对比这几个网页提到的价格和功能差异，并整理成表格”。

场景 3：在线文档助手
操作：在使用飞书、Notion 或 Google Docs 撰写文案时，侧边栏 Gemini 可以随时作为翻译官或扩写助手，辅助你完成创作。

