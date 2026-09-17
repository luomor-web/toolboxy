# 烙馍网在线工具（toolboxy）

> 在线访问：<https://toolboxy.luomor.com/>

烙馍网旗下的免费在线实用工具集，共 48 个工具。无需注册、开箱即用，绝大多数工具在浏览器本地运行，数据不上传服务器。

## 工具一览

### 🛠️ 开发工具（`dev/`）
| 工具 | 页面 | 说明 |
|------|------|------|
| 时间戳转换 | `dev/timestamp.html` | Unix 时间戳与日期互转 |
| JSON 格式化 | `dev/json.html` | 格式化 / 压缩 / 校验，本地运行 |
| 格式化代码 | `dev/codefmt.html` | 代码美化 |
| Markdown 编辑器 | `dev/markdown.html` | 在线编辑预览 |
| HTML 编辑器 | `dev/htmledit.html` | 在线编辑预览 |
| Base64 编解码 | `dev/base64.html` | 编码 / 解码 |
| 加密解密 | `dev/aes.html` | AES 加解密 |
| 密码生成器 | `dev/password.html` | 随机密码生成 |

### 🧮 计算工具（`calc/`）
| 工具 | 页面 | 说明 |
|------|------|------|
| 在线计算器 | `calc/jisuan.html` | 四则/科学计算 |
| 房贷计算 | `calc/fangdai.html` | 等额本息/本金 |
| 养老金计算 | `calc/yanglao.html` | 养老金测算 |
| 个税计算 | `calc/geshui.html` | 个人所得税 |
| 企业所得税 | `calc/qiyeshui.html` | 企业所得税 |
| 增值税计算 | `calc/zengzhishui.html` | 增值税 |
| 商品税计算 | `calc/shangpin.html` | 商品税费 |
| BMI 计算器 | `calc/bmi.html` | 身体质量指数 |

### 🔍 查询工具（`query/`）
| 工具 | 页面 | 说明 | 数据/后端 |
|------|------|------|-----------|
| 邮编查询 | `query/youbian.html` | 邮编/区号 | `data/zipcode.json` |
| 地区编码查询 | `query/diqu.html` | 行政区划代码 | `data/zipcode.json` |
| 机场三字码 | `query/jichang.html` | IATA 三字码 | `data/airport.json` |
| 国家编码查询 | `query/guojia.html` | 国家代码/域名/区号 | `data/country.json` |
| 身份证号解析 | `query/sfz.html` | 归属地/出生日期/校验 | 本地算法 |
| 手机号验证 | `query/shouji.html` | 号段归属 | 本地算法 |
| 车牌查询 | `query/chepai.html` | 车牌归属地 | `data/chepai.json` |
| 域名后缀查询 | `query/tld.html` | TLD 列表 | 内嵌数据 |
| 域名查询 | `query/whois.html` | Whois 查询 | 远程接口 |
| 域名解析查询 | `query/dns.html` | DNS 记录 | `dns.php` |
| 本机 IP 查询 | `query/myip.html` | 当前公网 IP | 远程接口 |
| IP 查询 | `query/ipcha.html` | IP 归属地 | 远程接口 |
| 网络链路查询 | `query/lianlu.html` | 链路诊断 | 远程接口 |
| 汉字字典 | `query/zidian.html` | 拼音/笔画/笔顺/组词/成语 | `zidian.php` + `data/` + `bishun_data/` |

### 🔄 换算工具（`convert/`）
| 工具 | 页面 | 说明 |
|------|------|------|
| 币种转换 | `convert/huobi.html` | 汇率换算 |
| 世界时钟 | `convert/shizhong.html` | 多时区时间 |
| 万年历 | `convert/rili.html` | 日历/节假日/订阅 .ics（经 `proxy.php`） |
| 重量转换 | `convert/zhongliang.html` | 单位换算 |
| 温度转换 | `convert/wendu.html` | 单位换算 |
| 风速转换 | `convert/fengsu.html` | 单位换算 |
| 大写数字 | `convert/daxie.html` | 金额大写 |
| 简繁转换 | `convert/jianfan.html` | 简体 ↔ 繁体 |
| 汉字转拼音 | `convert/pinyin.html` | 带声调/无声调/首字母（`pinyin.php`） |

### 🖼️ 图片工具（`image/`）
| 工具 | 页面 | 说明 |
|------|------|------|
| 图片压缩 | `image/imgzip.html` | 本地压缩 |
| 图片格式转换 | `image/imgconv.html` | 本地转换 |

### ✨ 其它工具（`other/`）
| 工具 | 页面 | 说明 |
|------|------|------|
| 数学符号 | `other/shuxue.html` | 符号查询复制 |
| 特殊符号 | `other/teshu.html` | 符号查询复制 |
| 交通标志 | `other/biaozhi.html` | 标志图例 |
| Emoji 符号 | `other/emoji.html` | Emoji 查询复制 |
| 字数统计 | `other/zishu.html` | 文本统计 |
| 色盲测试 | `other/semang.html` | 色觉检查图 |
| 在线翻译 | `other/fanyi.html` | 调用 MyMemory API |

## 技术栈

- **纯静态 + 少量 PHP**：无构建系统、无框架、无数据库，部署到支持 PHP 的 Web 服务器即可
- 每个工具是一个自包含的 `<category>/<name>.html`（内联 CSS/JS + 公共 `/form.css`），站内引用一律用站点根绝对路径
- PHP 仅 4 个根目录端点：`pinyin.php`、`zidian.php`（表单渲染结果页）、`dns.php`（JSON API）、`proxy.php`（带 SSRF 防护的远程代理）
- 查询类工具由前端 `fetch /data/*.json` 加载手工整理的数据，数据更新无需改代码
- 多语言（简体/繁體/EN）：`js/site-i18n.js` 源文本字典方案 + `lang/site-*.json`；语言切换器由 `js/nav.js` 自动注入

## 目录结构

```
├── index.html       # 工具总览首页（按 6 个分类分组展示卡片）
├── dev/             # 开发工具（8），含 index.html 分类首页
├── calc/            # 计算工具（8），含 index.html 分类首页
├── query/           # 查询工具（14），含 index.html 分类首页
├── convert/         # 换算工具（9），含 index.html 分类首页
├── image/           # 图片工具（2），含 index.html 分类首页
├── other/           # 其它工具（7），含 index.html 分类首页
├── pinyin.php / zidian.php / dns.php / proxy.php  # PHP 端点（根目录）
├── lib.php          # 共享 PHP 函数（笔顺/田字格渲染，仅 zidian.php 用）
├── form.css         # 表单页公共样式
├── js/
│   ├── nav.js       # 导航 active 状态、移动端抽屉、语言切换器注入
│   └── site-i18n.js # 多语言引擎（中文原文 → 译文 字典替换）
├── lang/            # site-en.json / site-zh-TW.json 语言包
├── data/            # 手工整理的查询数据（机场/国家/邮编/车牌/节假日/成语/组词）
├── bishun_data/     # 汉字笔顺 SVG 数据（字典页用）
└── img/             # 图片资源
```

## 本地开发

需要 PHP 环境（供 4 个 PHP 端点），在**本目录**起服务（页面使用站点根绝对路径）：

```bash
php -S localhost:8000
```

## 相关项目

- 主站：[烙馍网](https://www.luomor.com/)
- 字帖生成器：[烙馍网字帖](https://zzzt.luomor.com/)
