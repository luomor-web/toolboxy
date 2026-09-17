# 烙馍网工具专区（toolboxy）

> 在线访问：<https://toolboxy.luomor.com/>

烙馍网旗下的免费在线实用工具集，共 48 个工具。无需注册、开箱即用，绝大多数工具在浏览器本地运行，数据不上传服务器。

## 工具一览

### 开发工具
| 工具 | 页面 | 说明 |
|------|------|------|
| 时间戳转换 | `timestamp.html` | Unix 时间戳与日期互转 |
| JSON 格式化 | `json.html` | 格式化 / 压缩 / 校验，本地运行 |
| 格式化代码 | `codefmt.html` | 代码美化 |
| Markdown 编辑器 | `markdown.html` | 在线编辑预览 |
| HTML 编辑器 | `htmledit.html` | 在线编辑预览 |
| Base64 编解码 | `base64.html` | 编码 / 解码 |
| 加密解密 | `aes.html` | AES 加解密 |
| 密码生成器 | `password.html` | 随机密码生成 |

### 计算工具
| 工具 | 页面 | 说明 |
|------|------|------|
| 在线计算器 | `jisuan.html` | 四则/科学计算 |
| 房贷计算 | `fangdai.html` | 等额本息/本金 |
| 养老金计算 | `yanglao.html` | 养老金测算 |
| 个税计算 | `geshui.html` | 个人所得税 |
| 企业所得税 | `qiyeshui.html` | 企业所得税 |
| 增值税计算 | `zengzhishui.html` | 增值税 |
| 商品税计算 | `shangpin.html` | 商品税费 |
| BMI 计算器 | `bmi.html` | 身体质量指数 |

### 查询工具
| 工具 | 页面 | 说明 | 数据/后端 |
|------|------|------|-----------|
| 邮编查询 | `youbian.html` | 邮编/区号 | `data/zipcode.json` |
| 地区编码查询 | `diqu.html` | 行政区划代码 | `data/zipcode.json` |
| 机场三字码 | `jichang.html` | IATA 三字码 | `data/airport.json` |
| 国家编码查询 | `guojia.html` | 国家代码/域名/区号 | `data/country.json` |
| 身份证号解析 | `sfz.html` | 归属地/出生日期/校验 | 本地算法 |
| 手机号验证 | `shouji.html` | 号段归属 | 本地算法 |
| 车牌查询 | `chepai.html` | 车牌归属地 | `data/chepai.json` |
| 域名后缀查询 | `tld.html` | TLD 列表 | 内嵌数据 |
| 域名查询 | `whois.html` | Whois 查询 | 远程接口 |
| 域名解析查询 | `dns.html` | DNS 记录 | `dns.php` |
| 本机 IP 查询 | `myip.html` | 当前公网 IP | 远程接口 |
| IP 查询 | `ipcha.html` | IP 归属地 | 远程接口 |
| 网络链路查询 | `lianlu.html` | 链路诊断 | 远程接口 |
| 汉字字典 | `zidian.html` | 拼音/笔画/笔顺/组词/成语 | `zidian.php` + `data/` + `bishun_data/` |

### 换算与其他
| 工具 | 页面 | 说明 |
|------|------|------|
| 币种转换 | `huobi.html` | 汇率换算 |
| 世界时钟 | `shizhong.html` | 多时区时间 |
| 万年历 | `rili.html` | 日历/节假日/订阅 .ics（经 `proxy.php`） |
| 汉字转拼音 | `pinyin.html` | 带声调/无声调/首字母（`pinyin.php`） |
| 简繁转换 | `jianfan.html` | 简体 ↔ 繁体 |
| 大写数字 | `daxie.html` | 金额大写 |
| 数学符号 | `shuxue.html` | 符号查询复制 |
| 特殊符号 | `teshu.html` | 符号查询复制 |
| Emoji 符号 | `emoji.html` | Emoji 查询复制 |
| 交通标志 | `biaozhi.html` | 标志图例 |
| 重量转换 | `zhongliang.html` | 单位换算 |
| 温度转换 | `wendu.html` | 单位换算 |
| 风速转换 | `fengsu.html` | 单位换算 |
| 字数统计 | `zishu.html` | 文本统计 |
| 色盲测试 | `semang.html` | 色觉检查图 |
| 在线翻译 | `fanyi.html` | 调用 MyMemory API |

### 图片工具
| 工具 | 页面 | 说明 |
|------|------|------|
| 图片压缩 | `imgzip.html` | 本地压缩 |
| 图片格式转换 | `imgconv.html` | 本地转换 |

## 技术栈

- **纯静态 + 少量 PHP**：无构建系统、无框架、无数据库，部署到支持 PHP 的 Web 服务器即可
- 每个工具是一个自包含的 `<name>.html`（内联 CSS/JS + 公共 `form.css`）
- PHP 仅用于 4 个端点：`pinyin.php`、`zidian.php`（表单渲染结果页）、`dns.php`（JSON API）、`proxy.php`（带 SSRF 防护的远程代理）
- 查询类工具由前端 `fetch data/*.json` 加载手工整理的数据，数据更新无需改代码
- 多语言（简体/繁體/EN）：`js/site-i18n.js` 源文本字典方案 + `lang/site-*.json`；语言切换器由 `js/nav.js` 自动注入

## 目录结构

```
├── index.html       # 工具导航首页
├── <name>.html      # 各工具页（48 个，自包含）
├── pinyin.php / zidian.php / dns.php / proxy.php  # PHP 端点
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

需要 PHP 环境（供 4 个 PHP 端点）：

```bash
php -S localhost:8000
```

纯前端页面也可以直接用浏览器打开 HTML 文件预览。

## 相关项目

- 主站：[烙馍网字帖生成器](https://www.luomor.com/)（本目录嵌套于其仓库中，共享 `Pinyin.php` 拼音字典与笔顺数据）
