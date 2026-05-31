# Sing-Box Rulesets

适用于 [Sing-Box](https://github.com/SagerNet/sing-box) 的自定义规则集，用于精细化流量分流。

## 📁 文件说明

| 文件 | 类型 | 说明 |
|------|------|------|
| `geosite_google-gemini.json` | domain | Google Gemini / AI Studio 相关域名 |
| `geosite_twitter.json` | domain | Twitter / X 相关域名 |
| `geosite_openai.srs` | binary | OpenAI 规则集（编译后） |
| `google.json` | ip | Google 服务 IP CIDR 段 |
| `ID_wdeok4pe.srs` | binary | 规则集（编译后） |

## 🚀 使用方式

### 本地规则集

```json
{
  "rule_set": [
    {
      "type": "local",
      "tag": "geosite-twitter",
      "path": "/etc/sing-box/rule-set/geosite_twitter.json"
    }
  ]
}
```

### 远程规则集（推荐公开仓库）

```json
{
  "rule_set": [
    {
      "type": "remote",
      "tag": "geosite-gemini",
      "format": "binary",
      "url": "https://raw.githubusercontent.com/dainch/SingBox-rulesets/main/geosite_google-gemini.json"
    },
    {
      "type": "remote",
      "tag": "geosite-twitter",
      "format": "binary",
      "url": "https://raw.githubusercontent.com/dainch/SingBox-rulesets/main/geosite_twitter.json"
    }
  ]
}
```

### 在 DNS / 路由中使用

```json
{
  "route": {
    "rules": [
      {
        "rule_set": "geosite-gemini",
        "outbound": "proxy"
      },
      {
        "rule_set": "geosite-twitter",
        "outbound": "proxy"
      }
    ]
  }
}
```

## 📝 规则格式

规则集使用 Sing-Box 的 [rule-set 格式](https://sing-box.sagernet.org/configuration/rule-set/)，支持：

- `domain` — 完整域名匹配
- `domain_suffix` — 域名后缀匹配
- `domain_keyword` — 域名关键词匹配
- `domain_regex` — 域名正则匹配
- `ip_cidr` — IP CIDR 匹配

### 示例

```json
{
  "version": 2,
  "rules": [
    {
      "domain": ["ai.google.dev"],
      "domain_suffix": ["gemini.google.com", "bard.google.com"]
    }
  ]
}
```

## 🔧 自行编译为 .srs

如果需要编译为二进制 `.srs` 格式以减小体积、加快加载：

```bash
sing-box rule-set compile geosite_twitter.json
```

> 需要安装 sing-box 命令行工具。

## 📄 许可

MIT License
