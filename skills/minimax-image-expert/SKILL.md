---
name: minimax-image-expert
description: MiniMax 图像生成专家 Agent，精通 image-01 模型的文生图和图生图能力。
metadata: {"clawdbot":{"emoji":"🎨","requires":{"bins":["curl","jq","base64"]}}}
---

# MiniMax 图像生成专家 Agent

## 正确获取 API Key 的方法（必须遵守！否则图片生成会失败！）

**步骤 1: 加载环境变量**
```bash
source /root/.openclaw/workspace/scripts/load-env.sh
```

**步骤 2: 验证环境变量**
```bash
echo "API Key: $MINIMAX_API_KEY"
```
如果输出不是以 `sk-` 开头的长字符串，说明加载失败！

**步骤 3: 调用 API**
```bash
curl -s "https://api.minimaxi.com/v1/image_generation" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${MINIMAX_API_KEY}" \
  -d '{"model":"image-01","prompt":"YOUR_PROMPT","aspect_ratio":"1:1","response_format":"base64"}'
```

---

## 完整生成流程

```bash
# 1. 加载环境变量（必须！）
source /root/.openclaw/workspace/scripts/load-env.sh

# 2. 验证
if [ -z "$MINIMAX_API_KEY" ]; then
    echo "错误：环境变量未加载！"
    exit 1
fi

# 3. 生成图片
FILENAME="image_$(date +%s).jpg"

RESPONSE=$(curl -s "https://api.minimaxi.com/v1/image_generation" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${MINIMAX_API_KEY}" \
  -d "{\"model\":\"image-01\",\"prompt\":\"YOUR_PROMPT\",\"aspect_ratio\":\"1:1\",\"response_format\":\"base64\"}")

# 4. 检查结果
STATUS=$(echo "$RESPONSE" | jq -r '.base_resp.status_msg')
if [ "$STATUS" != "success" ]; then
    echo "API Error: $STATUS"
    exit 1
fi

# 5. 保存
echo "$RESPONSE" | jq -r '.data.image_base64[0]' | base64 -d > /root/.openclaw/workspace/data/${FILENAME}

# 6. 验证文件
if [ ! -s /root/.openclaw/workspace/data/${FILENAME} ]; then
    echo "生成失败！"
    exit 1
fi

# 7. 发送
<qqimg>/root/.openclaw/workspace/data/${FILENAME}</qqimg>
```

## API 信息

- 端点: `https://api.minimaxi.com/v1/image_generation`
- 模型: `image-01`
- 认证: 使用 `${MINIMAX_API_KEY}` 环境变量
