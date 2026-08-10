# IP 查询 Max · 全接口

整合 **20 个公开 IP 接口** + **4 个多地网络出口视角**，纯前端运行的 IP 查询工具。支持查询本机公网 IP 及任意 IPv4/IPv6 地址，通过智能多数投票共识得出最可信的归属地信息。页面配有 **Three.js 彩色粒子跟随背景**，明暗主题跟随系统自动切换。

## 特性

- **20 个数据源交叉验证**：同时调用 cz88、ip.sb、ipapi.co、ip-api.com、ipinfo.io 等 20 个 IP 地理信息服务
- **IPv4 / IPv6 分开投票**：IPv4 与 IPv6 独立做多数投票共识，顶部并列展示，各自带置信度，不再因地址族不同误报「不一致」
- **多地出口视角**：从中国大陆、中国香港、美国等不同地区服务器探测你的出口 IP，辅助判断代理/VPN/CDN 链路
- **查询任意 IP**：支持输入 IPv4 / IPv6 地址查询归属地（7 个接口交叉验证）
- **彩色粒子背景**：基于 Three.js 的鼠标跟随粒子光晕（源自 Google Antigravity 首页风格），跟随光标流动
- **跟随系统深浅色主题**：浅色模式白底 + 深色文字，深色模式深灰底 + 浅色文字，自动随系统切换
- **毛玻璃卡片**：半透明 + backdrop-filter 模糊，粒子光晕在卡片与文字下方
- **纯前端运行**：零构建，双击 `index.html` 即可使用

## 快速开始

### 本地使用

双击 `index.html` 用浏览器打开即可，无需安装任何环境。请保持 `particles.bundle.js` 与 `index.html` 同目录。

> 粒子背景已打包为经典脚本（无 `import/export`），因此 `file://` 直接双击也能正常运行。

### 部署到 Netlify

1. Fork 或克隆本仓库

2. 在 [Netlify](https://app.netlify.com/) 中导入项目

3. 无需额外配置，`netlify.toml` 已包含部署设置

   [![Netlify Status](https://api.netlify.com/api/v1/badges/4ea68c14-2815-476f-9edb-763d62ac15ff/deploy-status)](https://app.netlify.com/projects/wzlniptest/deploys)

## 文件结构

| 文件 | 说明 |
|------|------|
| `index.html` | 主页面（查询逻辑 + 双主题样式 + 粒子挂载） |
| `particles.bundle.js` | 粒子背景打包产物（组件 + Three.js，esbuild 生成） |
| `Mouse.ZrlRGzn3.js` | Three.js 打包（粒子组件依赖） |
| `谷歌agy背景彩色.js` | 粒子组件源码（含主题自动切换逻辑） |

> 重新打包粒子背景（改完源码后）：`npx esbuild 谷歌agy背景彩色.js --bundle --format=iife --platform=browser --outfile=particles.bundle.js`

## 数据源

### 本机 IP（20 个接口）

| 接口 | 类型 | 说明 |
|------|------|------|
| cz88.net | JSON | 纯真 IP 数据库 |
| ipip.net | Text | 中文归属地 |
| ipinfo.io | JSON | 地理信息 + ASN |
| ipwhois.app | JSON | WHOIS 信息 |
| ipwho.is | JSON | WHOIS 信息 |
| ipapi.is | JSON | ASN + 位置 |
| ifconfig.me | JSON | 网络信息 |
| IP.CN | Text | 中文归属地 |
| ip138.com | Text | 中文归属地 |
| ipify.org | JSON | 纯 IP（IPv4） |
| ipify.org (v6) | JSON | 纯 IP（IPv6 优先） |
| httpbin.org | JSON | 纯 IP |
| icanhazip.com | Text | 纯 IP |
| ip.sb | JSONP | GeoIP 查询 |
| ipapi.co | JSON | 地理信息 |
| freeipapi.com | JSON | 免费 IP API |
| db-ip.com | JSON | 免费 IP 查询 |
| api.myip.com | JSON | 简单 IP 查询 |
| ip-api.com | JSON | 地理信息 + ISP |
| Cloudflare Trace | Text | Cloudflare 检测 |

### 任意 IP 查询（7 个接口）

cz88、ip.sb、ipapi.co、ip-api.com、ipinfo.io、ipwhois.app

### 多地出口（4 个视角）

中国大陆（cn.ipcelou.com）、中国香港（hk.ipcelou.com）、美国（us.ipcelou.com）、AppSpot

## 技术实现

- 纯原生 JavaScript，无框架依赖
- 粒子背景：Three.js WebGL + GPU 粒子模拟（GPGPU），鼠标光环跟随光标，明暗主题监听系统 `prefers-color-scheme` 自动切换并重建
- 页面主题：CSS 变量 + `prefers-color-scheme` 媒体查询，浅色/深色双套配色
- JSONP 跨域请求支持（ip.sb 等接口）
- 并发限流执行器，避免同时大量请求
- IPv4 / IPv6 按地址族分别做多数投票，从多数派 IP 中选信息最全的结果作为 Hero 展示
- 置信度评级：高置信（≥80%一致）/ 中置信（≥50%）/ 低置信

## 浏览器兼容

现代浏览器均支持（Chrome / Firefox / Safari / Edge），建议使用最新版本。粒子背景需要 WebGL 支持。

## 限制说明

- 部分接口存在限流，少数卡片可能请求失败，可点击卡片内「重试」按钮重新请求
- 不同接口的归属地数据库不同，城市/运营商显示略有差异属正常现象
- 接口均为免费公开 API，服务稳定性由各接口提供商保障
- `file://` 直接打开时部分接口会因 CORS 被浏览器拦截，联网部署后即正常

## 免责声明

本项目仅聚合公开可用的 IP 查询接口，不对查询结果的准确性、可用性作任何保证。所有接口的版权和商标归各自所有者拥有。

## License

MIT
