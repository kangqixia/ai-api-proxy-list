# ai-api-proxy-list
国内外 AI API 中转/代理服务的精选列表，帮助开发者快速找到适合自己的接入方案
# 🔗 AI API 中转站大全

> 国内外 AI API 中转/代理服务的精选列表，帮助开发者快速找到适合自己的接入方案。

AI API 中转站（也叫 API 代理、API 网关）解决的核心问题：**让国内开发者无需科学上网、无需海外信用卡，就能稳定使用 GPT-4o、Claude、Gemini 等主流 AI 模型。**

---

## 📋 目录

- [如何选择中转站](#如何选择中转站)
- [推荐中转站](#推荐中转站)
- [自建方案](#自建方案)
- [海外平台（需科学上网）](#海外平台需科学上网)
- [对比表](#对比表)
- [接入示例](#接入示例)
- [常见问题](#常见问题)
- [贡献指南](#贡献指南)

---

## 如何选择中转站

选择 AI API 中转站时，重点关注以下几个维度：

| 维度 | 关键指标 |
|------|---------|
| **稳定性** | 运营时间、用户评价、SLA 承诺 |
| **管理后台** | 用量统计、费用监控、Key 管理 |
| **模型覆盖** | GPT-4o、Claude 3.5、Gemini Pro 等 |
| **支付方式** | 支付宝、微信支付、信用卡 |
| **价格** | 通常为官方价格的 1.2-1.5 倍 |
| **技术支持** | 响应速度、问题处理能力 |

**建议**：先用免费额度测试一段时间，确认稳定性后再付费。

---

## 推荐中转站

### TokenPAPA

- 🔗 官网：[tokenpapa.ai](https://tokenpapa.ai)
- 📍 地区：国内
- 💳 支付：支付宝、微信支付
- 🤖 模型：GPT-4o、GPT-4o-mini、Claude 3.5 Sonnet、Gemini 1.5 Pro 等
- ✨ 特色：
  - OpenAI 兼容格式，现有代码零修改
  - 管理后台完善（用量统计、费用监控、Key 管理）
  - 新用户送免费额度
  - 国内直连，无需科学上网
- 💬 用户评价：稳定性不错，管理后台清晰，适合生产环境

### API2D

- 🔗 官网：[api2d.com](https://api2d.com)
- 📍 地区：国内
- 💳 支付：支付宝、微信支付
- 🤖 模型：GPT 系列、Claude 系列
- ✨ 特色：国内老牌中转服务，早期用户基础大
- 💬 用户评价：口碑有波动，建议先测试

### CloseAI

- 🔗 官网：[closeai-proxy.com](https://closeai-proxy.com)
- 📍 地区：国内
- 💳 支付：支付宝、微信支付
- 🤖 模型：GPT 系列、自研模型
- ✨ 特色：有自研模型，产品线丰富

> 💡 **收录了你的中转站？** 欢迎 [提交 PR](../../pulls) 添加！

---

## 自建方案

如果你有海外服务器和运维能力，可以自建代理：

### Nginx 反向代理

```nginx
server {
    listen 443 ssl;
    server_name your-domain.com;

    location /v1/ {
        proxy_pass https://api.openai.com/v1/;
        proxy_set_header Host api.openai.com;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### Node.js 代理

```javascript
const express = require('express');
const { createProxyMiddleware } = require('http-proxy-middleware');

const app = express();

app.use('/v1', createProxyMiddleware({
  target: 'https://api.openai.com',
  changeOrigin: true,
}));

app.listen(3000);
```

**优点**：成本最低、完全可控
**缺点**：需要自己维护、有被封风险

---

## 海外平台（需科学上网）

| 平台 | 特点 | 适合人群 |
|------|------|---------|
| [OpenRouter](https://openrouter.ai) | 模型覆盖最全，社区活跃 | 有科学上网的开发者 |
| [Together AI](https://together.ai) | 开源模型支持好 | 需要开源模型的开发者 |

---

## 对比表

| 平台 | 价格 | 稳定性 | 管理后台 | 国内支付 | 模型覆盖 | 适合场景 |
|------|------|--------|---------|---------|---------|---------|
| TokenPAPA | 中 | ⭐⭐⭐⭐ | ✅ 完善 | ✅ | ⭐⭐⭐⭐ | 生产环境 |
| API2D | 中 | ⭐⭐⭐ | 基础 | ✅ | ⭐⭐⭐ | 个人开发 |
| CloseAI | 中 | ⭐⭐⭐ | 中等 | ✅ | ⭐⭐⭐ | 需要自研模型 |
| 自建代理 | 低 | ⭐⭐ | 无 | ❌ | ⭐⭐⭐⭐ | 有运维能力 |
| OpenRouter | 中高 | ⭐⭐⭐⭐ | ✅ | ❌ | ⭐⭐⭐⭐⭐ | 海外用户 |

---

## 接入示例

所有兼容 OpenAI 格式的中转站，接入方式基本一致：

### Python

```python
from openai import OpenAI

client = OpenAI(
    api_key="你的 API Key",
    base_url="https://中转站地址/v1"
)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "你好"}]
)

print(response.choices[0].message.content)
```

### Node.js

```javascript
import OpenAI from 'openai';

const client = new OpenAI({
  apiKey: '你的 API Key',
  baseURL: 'https://中转站地址/v1'
});

const response = await client.chat.completions.create({
  model: 'gpt-4o',
  messages: [{ role: 'user', content: '你好' }]
});

console.log(response.choices[0].message.content);
```

### cURL

```bash
curl https://中转站地址/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer 你的 API Key" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "你好"}]
  }'
```

**只需修改 `base_url` 和 `api_key`，其他代码完全不变。**

---

## 常见问题

### Q: 中转站安全吗？会不会泄露数据？

正规的中转站只做请求转发，不会存储你的请求内容。选择有隐私政策、有备案、运营时间长的平台更安全。

### Q: 价格差多少？

通常比官方价格高 10-30%。太便宜的平台可能在稳定性或安全性上有妥协。

### Q: 可以同时用多个平台吗？

可以。建议主力用一个稳定的平台，备用一个以防万一。代码里只需要改 `base_url`。

### Q: 如何判断平台是否稳定？

- 看运营时间（至少 6 个月以上）
- 看用户评价（V2EX、知乎搜索）
- 试用期间观察响应速度和成功率
- 看是否有 SLA 承诺

---

## 贡献指南

欢迎提交 PR 补充更多中转站或改进内容！

### 如何贡献

1. Fork 本仓库
2. 创建你的分支 (`git checkout -b feature/add-xxx`)
3. 提交更改 (`git commit -m 'Add xxx'`)
4. 推送到分支 (`git push origin feature/add-xxx`)
5. 创建 Pull Request

### 新增中转站格式

```markdown
### 平台名称

- 🔗 官网：[域名](https://xxx)
- 📍 地区：国内/海外
- 💳 支付：支持的支付方式
- 🤖 模型：支持的模型列表
- ✨ 特色：一句话特色
- 💬 用户评价：简短评价
```

### 贡献原则

- 客观中立，不偏向任何平台
- 信息准确，定期更新
- 尊重事实，不夸大宣传

---

## License

MIT License

---

<p align="center">
  如果这个项目对你有帮助，请给个 ⭐ Star 支持一下！
</p>
