# PersonaloonRuleset

基于 [可莉（iKeLee）Loon 进阶配置](https://github.com/luestr/ProxyResource/blob/main/Tool/Loon/Lcf/zh-CN/Loon_Advanced_Config_By_iKeLee.lcf)，结合本人 [PersonalClashRuleSet](https://github.com/GGforceP/PersonalClashRuleSet) 的分流策略整理。

> **安全原则：公开仓库仅保存不含机密的配置模板和规则文件。真实机场订阅地址、订阅 Token、SSID 以及 MITM 证书和口令必须仅保存在本人设备或本地私人文件，严禁提交 GitHub。**

## 配置

| 文件 | 内容 |
| --- | --- |
| [Loon_Advanced_Generic.lcf](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Templates/Loon_Advanced_Generic.lcf) | **公开通用模板**：移除个人订阅、SSID 和 MitM 证书；使用可莉默认公开插件列表；仅供本地填入参数后使用 |

> 私人完整配置 **不在此仓库**。本机私人版保留当前 3 个节点订阅、68 个插件引用、原始 DNS/Host、MitM 证书和其他配置。公开模板与私人版插件数量不同是**有意的安全/通用性取舍**。

## 功能设计

- **家庭 Wi-Fi 直连**：本地配置 `ssid-trigger = "YOUR_HOME_WIFI":DIRECT,"cellular":RULE,"default":RULE`；Loon 保持运行，由软路由承接后续代理。SSID 策略组同时将服务策略指向 DIRECT。
- **外出自动代理**：按服务使用香港、美国、新加坡等地区自动节点，原有手动策略也保留。
- **自动测速**：7 个 `url-test` 组，默认 `interval=300` 秒、`tolerance=150` 毫秒；并保留原有 `fallback` 策略。
- **远程规则**：用 `[Remote Rule]` 引用 GitHub 的 `.list` 文件，分流策略在主配置的 `policy=` 中指定。
- **规则优先顺序**：自己维护的具体规则 → Clash 上游 YAML 转换集 → 额外服务类别 → **可莉原始远程规则** → `FINAL`。

**重要限制：** Loon 规则来源优先级是 `本地 > 插件 > 远程订阅`，因此 GitHub 远程规则的前后顺序**不能强制覆盖插件规则**；此外域名规则通常优先于 IP 规则匹配。全部 27 个 GitHub 规则文件不等于原 Mihomo 的全量 GEOSITE/GeoIP 数据库，仍可能存在未覆盖服务。

## 规则清单

### Rules/Personal · 来自本人 Clash 仓库

| RAW 文件 | 用途 |
| --- | --- |
| [00-Legacy-Direct.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/00-Legacy-Direct.list) | 原 Loon 直连 IP 例外 |
| [01-Edge-Copilot.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/01-Edge-Copilot.list) | Edge Copilot |
| [02-Microsoft-Designer.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/02-Microsoft-Designer.list) | Microsoft Designer |
| [03-Edge-NewTab.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/03-Edge-NewTab.list) | Edge / MSN 新标签页 |
| [04-AI-Special.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/04-AI-Special.list) | 小众 AI 服务 |
| [05-Linux-Proxy.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/05-Linux-Proxy.list) | Linux 海外仓库优先代理 |
| [06-Linux-Direct.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/06-Linux-Direct.list) | Linux 常用站点直连 |
| [07-AdGuard-VPN.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/07-AdGuard-VPN.list) | AdGuard VPN |
| [08-Yandex.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/08-Yandex.list) | Yandex 生态 |
| [09-Custom-Proxy.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Personal/09-Custom-Proxy.list) | 自定义指定域名代理 |

### Rules/Upstream · 来自 Aethersailor/Custom_OpenClash_Rules

使用公开的 YAML 文本源转换为 Loon 规则，**未将 `.mrs` 二进制改后缀使用**；保留上游归属标注。文件为本仓库副本，未来需要人工同步上游修改。

| RAW 文件 | 用途 |
| --- | --- |
| [01-Custom-Direct-Domain.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Upstream/01-Custom-Direct-Domain.list) | OpenClash 上游域名直连 |
| [02-Custom-Direct-IP.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Upstream/02-Custom-Direct-IP.list) | OpenClash 上游 IP 直连 |
| [03-Custom-Direct-Port.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Upstream/03-Custom-Direct-Port.list) | OpenClash 上游例外端口直连 |
| [04-Custom-Proxy-Domain.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Upstream/04-Custom-Proxy-Domain.list) | OpenClash 上游指定域名代理 |
| [05-Custom-Proxy-IP.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Upstream/05-Custom-Proxy-IP.list) | OpenClash 上游指定 IP 代理 |
| [06-Steam-CDN-Domain.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Upstream/06-Steam-CDN-Domain.list) | Steam CDN 域名直连 |
| [07-Steam-CDN-IP.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Upstream/07-Steam-CDN-IP.list) | Steam CDN IP 直连 |
| [08-Unusual-Ports.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Upstream/08-Unusual-Ports.list) | 非常规端口策略 |

### Rules/Services · 对缺失 GEOSITE 的补充

以下是**精选基础域名集**，不声称与 Mihomo 的完整 `GEOSITE` 等价。

| RAW 文件 | 用途 |
| --- | --- |
| [01-GitHub.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/01-GitHub.list) | GitHub / Copilot 辅助域名 |
| [02-Google.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/02-Google.list) | Google / YouTube 基础补充 |
| [03-Telegram.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/03-Telegram.list) | Telegram 基础补充 |
| [04-Apple.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/04-Apple.list) | Apple 基础补充 |
| [05-Microsoft.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/05-Microsoft.list) | Microsoft 基础补充 |
| [06-Social.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/06-Social.list) | 海外社交网站基础补充 |
| [07-Streaming.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/07-Streaming.list) | 海外流媒体基础补充 |
| [08-LAN.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/08-LAN.list) | 常见内网 IP / ULA |
| [09-CN-GeoIP.list](https://raw.githubusercontent.com/GGforceP/PersonaloonRuleset/main/Rules/Services/09-CN-GeoIP.list) | 中国大陆 GEOIP 兜底 |

## 可莉规则保留情况

原私人配置中的 11 条可莉远程规则（Twitter、SteamCN、Steam、OpenAI、SpeedtestChina、TikTok、AI、SpeedtestInternational、Netflix、LAN、CN REGION）在新配置中全部保留了**原始来源及策略**，排在 GitHub 规则之后。

**目前尚不能声称所有规则都来自 GitHub：** 可莉源站的 `.lsr` 内容无法完整获取核对，因此没有伪造同名镜像，仍直接引用原站；依赖原站可访问性。后续取得可核验的规则原文并确认许可后，才可迁入 `Rules/Kelee/`。

## 目录结构

```text
PersonaloonRuleset/
├── README.md
├── Templates/
│   └── Loon_Advanced_Generic.lcf
└── Rules/
    ├── Personal/        # 10 个本人规则
    ├── Upstream/        # 8 个 Clash 上游兼容规则
    └── Services/        # 9 个服务分类/直连补充规则
```

## 维护要求

1. 改动任何目录、规则文件或配置引用时，同步更新此 README 的结构和 RAW 链接。
2. 增补规则时先查重，检查父域/子域覆盖以及不同策略间的冲突；Linux-Proxy 要排在 Linux-Direct 前。
3. 修改规则后，在 Loon 中刷新远程订阅；GitHub 提交不意味着客户端瞬时更新。
4. 修改私有配置前保留原文件备份，不将订阅、节点凭据、MitM 私钥或口令提交到仓库。
5. 上游目录只允许对公开可核对的 YAML 进行格式转换，不将 Mihomo 专属 GEOSITE/MRS 用作 Loon 原生格式。

## 官方文档

- [Loon · 通用配置 / SSID trigger](https://nsloon.app/docs/General/)
- [Loon · 策略组 / url-test](https://nsloon.app/docs/Policy/policygroup/)
- [Loon · 分流规则匹配优先级](https://nsloon.app/docs/Rule/)
- [Loon · 远程规则](https://nsloon.app/docs/Rule/sub_rule/)
