---
title: ChatGPT Team 支付长链接生成教程
data: 2026/7/19 16:50
tags: [AI]
---


# ChatGPT Team 支付长链接生成教程

> 本教程通过浏览器控制台调用 ChatGPT 官方支付接口，生成带优惠码的 Stripe 结账长链接。

---

## 前置条件

- 已安装 Chrome 浏览器
- 拥有有效的 ChatGPT 账号（已登录）
- 持有可用的优惠码（Promo Code）

---

## 步骤一：登录 ChatGPT

用 Chrome 打开 [https://chatgpt.com](https://chatgpt.com)，确保已正常登录账号。

---

## 步骤二：打开浏览器开发者工具

按 `F12`（或 `Ctrl + Shift + I`）打开开发者工具，切换到 **Console（控制台）** 标签页。

![控制台示意图](../img/gpt_console.png)

---

## 步骤三：填写配置并运行脚本

将下方代码中的两处配置替换为你的实际信息：

| 配置项                             | 说明                              |
|---------------------------------|---------------------------------|
| `WORKSPACE_NAME`                | 你想设置的团队工作区名称（可自定义，如 `"MyTeam"`） |
| `COUPON`                        | 你拿到的优惠码（如 `"ramsacuk"`）         |
| `cancel_url` 中的 `promoCode=xxx` | 将 `xxx` 替换为你的优惠码                |

替换完成后，将代码**完整粘贴**到控制台，按回车执行。

```js
(async function generateChatGPTTeamLink() {
  // ================= 配置项（修改这里）=================
  const WORKSPACE_NAME = "MyTeam";   // 团队工作区名称（自定义）
  const COUPON        = "ramsacuk";  // 优惠码
  const SEAT_QUANTITY = 2;           // 席位数量（Team 计划最少 2 个）
  // =====================================================

  console.log("⏳ 正在获取 ChatGPT Session Token...");

  // 1. 自动获取登录凭证
  let accessToken;
  try {
    const session = await fetch("/api/auth/session").then(r => r.json());
    accessToken = session?.accessToken;
    if (!accessToken) throw new Error("accessToken 为空，请确认已登录 ChatGPT 账号");
  } catch (e) {
    console.error("❌ 获取 Token 失败：", e.message);
    return;
  }
  console.log("✅ Token 获取成功");

  // 2. 构建请求 Payload
  const payload = {
    plan_name: "chatgptteamplan",
    team_plan_data: {
      workspace_name: WORKSPACE_NAME,
      price_interval: "month",       // 按月计费，如需按年改为 "year"
      seat_quantity:  SEAT_QUANTITY
    },
    billing_details: {
      country:  "GB",
      currency: "GBP"
    },
    cancel_url:       `https://chatgpt.com/?promoCode=${COUPON}`,
    promo_code:        COUPON,
    checkout_ui_mode: "hosted"
  };

  // 3. 请求 Stripe 结账长链接
  console.log("⏳ 正在请求 Stripe 支付长链接...");
  try {
    const resp = await fetch("https://chatgpt.com/backend-api/payments/checkout", {
      method: "POST",
      headers: {
        Authorization:  `Bearer ${accessToken}`,
        "Content-Type": "application/json"
      },
      body: JSON.stringify(payload)
    });

    const data = await resp.json();

    if (!resp.ok) {
      console.error(`❌ 请求失败 HTTP ${resp.status}`);
      console.log("📋 响应详情：", data);
      return;
    }

    const hostedUrl = data?.url || data?.stripe_hosted_url || data?.checkout_url;
    if (!hostedUrl) {
      console.warn("⚠️ 未找到长链接，原始响应：", data);
      return;
    }

    // 4. 输出结果
    console.log("─".repeat(60));
    console.log("✅ ChatGPT Team 支付链接生成成功！");
    console.log(`📌 工作区名称 : ${WORKSPACE_NAME}`);
    console.log(`💺 席位数量   : ${SEAT_QUANTITY}`);
    console.log(`🎟️  优惠码     : ${COUPON}`);
    console.log(`🌍 地区 / 货币 : ${payload.billing_details.country} (${payload.billing_details.currency})`);
    if (data.checkout_session_id) {
      console.log(`🆔 Session ID : ${data.checkout_session_id}`);
    }
    console.log("─".repeat(60));
    console.log("🔗 请复制以下链接到浏览器新标签页打开：");
    console.log(hostedUrl);
    console.log("─".repeat(60));} catch (e) {
    console.error("❌ 网络异常或请求失败：", e.message);
  }
})();
```

---

## 步骤四：打开链接完成支付

控制台输出成功后，复制 `🔗` 后面的长链接，在**浏览器新标签页**中打开，按页面提示填写银行卡信息完成付款。

![付款成功](../img/gpt_pay_success.png)

---

## 常见问题

**Q：执行后提示 `accessToken 为空`？**
重新刷新 chatgpt.com 页面，确认已登录，再重试。

**Q：HTTP 状态码 `401` 或 `403`？**
登录态已过期，退出后重新登录，再执行脚本。

**Q：返回 `200` 但找不到链接字段？**
接口返回结构可能有变化，展开控制台中的原始响应对象，手动查找包含 `https://checkout.stripe.com` 的字段。

**Q：`cancel_url` 需要改吗？**
不是必须的，它仅用于用户取消支付后的跳转地址，不影响链接生成。

---

> ⚠️ **注意**：本脚本仅在你本人已登录的 ChatGPT 账号会话中运行，不会将任何凭证发送到第三方服务器。优惠码具有使用限制，请勿滥用。

















