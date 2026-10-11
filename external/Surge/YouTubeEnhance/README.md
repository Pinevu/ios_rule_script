# YouTube (Music) Enhance Plus

适用于 Surge（iOS）的 YouTube / YouTube Music 增强模块：**去广告 · 后台播放 · 画中画 · 界面净化**。

基于 [Maasea/sgmodule](https://github.com/Maasea/sgmodule) 的脚本整理优化；响应脚本自持于本仓库（固定构建版本），不随上游变动。

## 安装

Surge → 首页 → 模块 → 从 URL 安装（或添加到配置的 `[Module]` 段）：

```
https://raw.githubusercontent.com/Pinevu/ios_rule_script/master/external/Surge/YouTubeEnhance/YouTubeEnhance.sgmodule
```

> 需开启 MITM 并信任证书；请勿与其它 YouTube 去广告 / 增强模块混用，避免规则与脚本重复处理。

## 功能

- 去除视频广告（贴片 / 中插）、首页瀑布流广告、搜索与播放页广告、Shorts 广告
- 解锁后台播放、画中画（PiP）
- 可选屏蔽：上传按钮 / 选段按钮 / Shorts 入口
- 可选字幕翻译
- 拦截广告统计接口（stats/ads、pagead、ptracking、qoe adcontext）

## 参数

| 参数 | 默认 | 说明 |
| --- | --- | --- |
| 屏蔽上传按钮 | true | 隐藏底部「上传」按钮 |
| 屏蔽选段按钮 | true | 隐藏 YouTube Music「选段」入口 |
| 屏蔽Shorts按钮 | false | 隐藏 Shorts 入口 |
| 字幕翻译语言 | off | Google Translate 语言代码，如 zh-Hans；off 关闭 |
| 启用调试模式 | false | 输出更多日志 |

## 相对原版的增强

1. **脚本自持**：响应脚本固定在本仓库（`Script/youtube.response.js`，向上游 2026-07-19 构建），不再实时跟随上游，避免上游重构（如 2026-07 的请求脚本 + Cloudflare Worker 迁移）导致模块行为突变
2. **视频广告请求**：新增 `ctier` 分层标记重写（URL Rewrite），处理带分层标记的广告视频请求
3. **拦截范围扩展**：`&oad` 广告流拦截从仅 `initplayback` 扩展至全部非播放路径（保留 `dclk_video_ads`、`videoplayback` 正常路径不受影响）
4. **广告统计接口**：新增 `stats/ads`、`pagead`、`ptracking`、`qoe adcontext` 拦截
5. **MITM 优化**：补充 `www.youtube.com`、`s.youtube.com`；排除 redirector 主机，减少无谓解密开销
6. **QUIC 阻断**：保留 UDP 阻断规则，确保流量回落 TCP 以参与 MITM

## 文件结构

```
external/Surge/YouTubeEnhance/
├── YouTubeEnhance.sgmodule      # 模块主文件
├── README.md                    # 本说明
└── Script/
    └── youtube.response.js      # 响应脚本（自持，源自 Maasea/sgmodule master）
```

## 脚本更新（维护用）

如需同步上游最新脚本：

```bash
curl -fsSL 'https://raw.githubusercontent.com/Maasea/sgmodule/master/Script/Youtube/youtube.response.js' \
  -o Script/youtube.response.js
```

> 注意：上游 2026-07 起引入「请求脚本 + CF Worker」新架构，直接替换新版需要同步调整模块结构；如遇播放问题可回退到本仓库历史版本。

## 已知说明

- 采用本地脚本 + 规则的经典方案，不依赖任何第三方服务
- 新型服务端拼流广告的拦截能力有限，如遇广告残留可反馈
- 画中画 / 后台播放在 YouTube 客户端内也可开关（设置 → 播放 → 画中画；设置 → 后台播放和下载）

## 致谢

- [Maasea/sgmodule](https://github.com/Maasea/sgmodule) — 响应脚本
- [Aioneas/Surge](https://github.com/Aioneas/Surge)、[DivineEngine](https://github.com/DivineEngine)、[eslco](https://github.com/eslco/base) — 规则参考

## 免责声明

仅供学习研究使用。
