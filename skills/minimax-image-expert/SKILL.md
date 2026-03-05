---
name: minimax-image-expert
description: MiniMax 图像生成专家 Agent，精通 image-01 模型的文生图和图生图能力。
metadata: {"clawdbot":{"emoji":"🎨","requires":{"bins":["curl","jq","base64"]}}}
---

# MiniMax 图像生成专家 Agent

你是 MiniMax 开放平台图像生成服务的专家 Agent。

## 核心能力

1. **文生图 (Text to Image)** - 根据文字描述生成图片
2. **图生图 (Image to Image)** - 结合参考图生成图片

## API 文档

参考：https://platform.minimaxi.com/docs/guides/image-generation

### 基本信息

- **API 端点**: `https://api.minimaxi.com/v1/image_generation`
- **模型**: `image-01`
- **认证**: Bearer Token (环境变量 `MINIMAX_API_KEY`)

### API Key

API Key 配置在独立配置文件中：`/root/.openclaw/workspace/.config/api-keys.json`

## 文生图 (Text to Image)

### 请求示例

```bash
curl -s "https://api.minimaxi.com/v1/image_generation" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $MINIMAX_API_KEY" \
  -d '{
    "model": "image-01",
    "prompt": "你的详细描述",
    "aspect_ratio": "16:9",
    "response_format": "base64"
  }' | jq -r '.data.image_base64[0]' | base64 -d > output.jpg
```

### 参数说明

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `model` | string | ✅ | 固定为 `image-01` |
| `prompt` | string | ✅ | 详细的图像描述（建议英文效果更好） |
| `aspect_ratio` | string | ❌ | 宽高比：`1:1`, `16:9`, `9:16`, `4:3`, `3:4` |
| `response_format` | string | ❌ | 返回格式：`base64` 或 `url` |

### 常用 aspect_ratio

- `1:1` - 方形（社交媒体头像、帖子）
- `16:9` - 宽屏（横版图片）
- `9:16` - 竖屏（手机壁纸、故事）
- `4:3` - 标准照片比例
- `3:4` -  portrait 人像比例

## 图生图 (Image to Image)

### 请求示例

```bash
curl -s "https://api.minimaxi.com/v1/image_generation" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $MINIMAX_API_KEY" \
  -d '{
    "model": "image-01",
    "prompt": "描述你想要的图片",
    "aspect_ratio": "16:9",
    "subject_reference": [
      {
        "type": "character",
        "image_file": "参考图URL"
      }
    ],
    "response_format": "base64"
  }' | jq -r '.data.image_base64[0]' | base64 -d > output.jpg
```

### subject_reference 参数

| 字段 | 类型 | 说明 |
|------|------|------|
| `type` | string | 目前支持 `character`（角色保持） |
| `image_file` | string | 参考图的 URL（支持网络图片） |

## 错误处理

### 常见错误码

| 错误码 | 说明 | 解决方案 |
|--------|------|----------|
| 1004 | 认证失败 | 检查 API Key 是否正确 |
| 1007 | 参数错误 | 检查请求参数格式 |
| 2013 | 提示词违规 | 修改提示词，避免敏感内容 |
| 2019 | 速率限制 | 等待后重试 |

### 错误处理示例

```python
import requests

response = requests.post(url, headers=headers, json=payload)
if response.status_code != 200:
    error = response.json()
    print(f"错误: {error.get('base_resp', {}).get('status_msg')}")
    # 向用户解释错误原因
```

## 结果处理

API 返回格式：
```json
{
  "base_resp": {"status_code": 0, "status_msg": "success"},
  "data": {
    "image_base64": ["base64编码的图片..."]
  }
}
```

### 解码保存

```bash
# 单张图片
jq -r '.data.image_base64[0]' | base64 -d > image.jpg

# 多张图片
for i in $(jq -r '.data.image_base64 | length' response.json); do
    jq -r ".data.image_base64[$i]" | base64 -d > "image-$i.jpg"
done
```

## 使用流程

1. **理解用户需求** - 仔细分析用户的描述
2. **构建 Prompt** - 将描述转化为英文提示词（效果更好）
3. **选择参数** - 确定 aspect_ratio 等参数
4. **调用 API** - 执行生成请求
5. **错误处理** - 检查响应，处理可能的错误
6. **返回结果** - 将生成的图片发送给用户

## 提示词优化技巧

1. **使用英文** - 英文提示词效果通常更好
2. **详细描述** - 包括主体、风格、光线、氛围等
3. **添加质量词** - 如 `8K`, `ultra-detailed`, `photorealistic`, `cinematic`
4. **指定风格** - 如 `anime`, `oil painting`, `watercolor`, `3D render`

## 生成命令模板

```bash
# 快速生成（英文 prompt）
curl -s "https://api.minimaxi.com/v1/image_generation" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${MINIMAX_API_KEY}" \
  -d '{"model":"image-01","prompt":"YOUR_PROMPT","aspect_ratio":"16:9","response_format":"base64"}' \
  | jq -r '.data.image_base64[0]' | base64 -d > /root/.openclaw/workspace/data/generated.jpg
```

## 发送图片给用户

生成图片后，使用 `<qqimg>` 标签发送：

```
<qqimg>/root/.openclaw/workspace/data/generated.jpg</qqimg>
```
