# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

烙馍网在线工具（toolboxy.luomor.com）—— 约 48 个独立在线工具（时间戳、JSON 格式化、Base64、加密解密、计算器、汇率、万年历、各类查询等）组成的纯静态 + 少量 PHP 的站点。本目录嵌在主站字帖项目（luomor-zt，有独立 CLAUDE.md）内部，共享其 `../Pinyin.php` 与 `bishun_data/` 笔顺数据。

**无构建系统、无测试、无 lint**——纯 HTML/CSS/JS/PHP，部署即用。本地开发可用 `php -S localhost:8000` 在本目录起服务（注意 pinyin 工具依赖父目录的 `../Pinyin.php`，从 toolboxy 目录起服务时该相对路径恰好成立）。

## 架构：三种工具页模式

每个工具是一个自包含的 `<分类>/<name>.html`（内联 CSS/JS + 引用 `/form.css`），按后端需求分三类：

1. **纯前端**（大多数，如 dev/json.html、dev/base64.html、dev/aes.html）——全部逻辑在页尾内联 `<script>`，本地运行不上传数据。
2. **前端 + 静态 JSON**（query/ 下的 youbian/diqu/jichang/guojia/sfz/shouji/tld/chepai 与 convert/rili）——JS `fetch('/data/xxx.json')` 加载 `data/` 下手工整理的数据文件，数据变更无需改代码。
3. **PHP 后端**（4 个端点都在**根目录**）：
   - `convert/pinyin.html → /pinyin.php`、`query/zidian.html → /zidian.php`：表单 POST 到 PHP，PHP 直接渲染结果页（同一文件既处理请求又输出 HTML）。
   - `query/dns.html → /dns.php`：JS fetch JSON API（PHP 原生 `dns_get_record`）。
   - `convert/rili.html → /proxy.php?url=`：远程 .ics/JSON 代理，带 SSRF 防护（拒绝内网 IP）。

**站内引用一律用站点根绝对路径**（`/form.css`、`/js/nav.js`、`/dev/xxx.html`、`/data/xxx.json`），因为页面分布在分类子目录中。

**PHP 关键坑（proxy.php/dns.php 头部注释均有强调）：本机 PHP 构建 `exit()`/`die()` 无效，错误分支必须用 if/else 结构，不能提前 exit。**

## 共享代码

- **`form.css`** — 所有表单页公共样式（`.container`/`.header`/`.nav-bar`/`.card`/`.submit-btn` 等）。新页面直接 `<link rel="stylesheet" href="form.css">`。
- **`js/nav.js`** — 每页底部 `defer` 加载：自动判断导航 active 状态、移动端抽屉菜单、**自动向 `.nav-bar` 注入语言切换器**。勿在页面里手写语言切换按钮。
- **`js/site-i18n.js`** — 多语言引擎（见下文）。
- **`lib.php`** — 从主站复制的 `xx_*` 函数库（笔顺加载、田字格渲染等），目前仅 `zidian.php` 使用。顶部 Pinyin 类加载逻辑：用 `scandir` 大小写敏感地判断同目录是否存在真正的 `Pinyin.php`（部署时从主站复制），否则回退 `../Pinyin.php`——**不能用 `file_exists`/`include 'Pinyin.php'` 直接判断**，大小写不敏感文件系统会误命中 `pinyin.php` 工具页导致整页 HTML 混入输出。
- **`data/*.json`** — 手工整理的权威数据：`airport.json`（机场三字码）、`country.json`（国家编码）、`zipcode.json`（邮编/区号）、`chepai.json`（车牌归属地）、`holiday.json`（法定节假日调休）、`chengyu.json`/`zuci.json`（字典页用）。

## 多语言 (i18n)

- 方案：`js/site-i18n.js` 源文本字典——`lang/site-en.json` / `lang/site-zh-TW.json`（各约 1700+ 条，扁平 `中文原文→译文`），遍历文本节点/title/meta/placeholder 命中即替换，**无需 data-i18n 标记**。
- 语言解析：`?lang=` > `localStorage('tzg-lang')` > `navigator.language`，默认 `zh`（简体为原文不处理）。
- **新页面/新文案**：页面写简体中文原文即可；要让繁体/英文生效，必须把新出现的可见文案加入两个 `lang/site-*.json`，未命中保持中文。

## 新增一个工具的清单

工具页按 6 个分类放在子目录：`dev/` 开发、`calc/` 计算、`query/` 查询、`convert/` 换算、`image/` 图片、`other/` 其它，**每个分类目录有自己的 `index.html` 分类首页**（卡片列表，canonical 为 `/dev/` 目录形式）。导航一级分类是可点击链接（指向分类首页），其下悬挂该分类的工具菜单；`烙馍网首页` 旁有 `工具首页`（→ `/index.html`，全工具总览）。页面结构高度模板化，复制同分类现有工具页（如 `dev/json.html`）最快。需要同步修改的位置：

1. 新建 `<分类>/<name>.html`（含 canonical `https://toolboxy.luomor.com/<分类>/<name>.html`、keywords/description meta、AdSense 脚本块原样保留；资源引用全部用绝对路径 `/form.css`、`/js/...`）
2. `index.html`（总览）和 `<分类>/index.html`（分类首页）的 `.cards` 里都加工具卡片
3. **每个页面的 `.nav-bar` 对应分类下拉都要加链接**（导航 HTML 在全部 48 工具页 + 6 分类首页 + 根 index.html + pinyin.php/zidian.php 中逐页重复，需批量同步；可用 sed 批量插入）
4. `sitemap.xml` 加条目
5. 新文案补进 `lang/site-en.json` 和 `lang/site-zh-TW.json`（分类首页的 title/subtitle/description 整串也是字典条目）

## 编辑注意事项

- 每页含大段 Google AdSense/funding choices 压缩 JS——编辑时**不要动这块**，定位正文请跳过 `<head>` 中的混淆脚本。
- 页面面向 Edge/Chrome；SEO 字段（title/keywords/description/canonical）每页独立维护。
- 外部依赖：Google AdSense、fanyi.html 用 api.mymemory.translated.net 翻译接口；其余无 CDN 依赖（jQuery/html2canvas 仅主站打印页用，本站未用）。
