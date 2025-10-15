# TrendRadar 本地部署计划

## 项目概述
TrendRadar 是一个热点新闻聚合工具，可以监控11个主流平台（今日头条、百度热搜、微博、抖音、知乎、B站等）的热点新闻，支持关键词过滤和多渠道推送通知。

## 部署待办事项

### 环境准备
- [x] 检查 Python 环境（需要 Python 3.x）
- [x] 安装 Python 依赖包
- [x] 安装 Playwright 浏览器驱动

### 配置文件
- [x] 查看并理解 config/config.yaml 配置文件
- [ ] 根据需要修改配置（可选）
  - 运行模式（daily/current/incremental）
  - 监控平台列表
  - 关键词配置（config/frequency_words.txt）

### 本地测试
- [x] 运行一次程序测试是否正常工作
- [x] 检查输出结果（output 目录和控制台输出）
- [x] 验证生成的 HTML 文件

### 可选配置
- [ ] 配置推送通知（企业微信/飞书/钉钉/Telegram）

## 部署方式选择

### 方案一：直接运行 Python（推荐新手）
- 简单直接，适合快速测试
- 需要手动安装依赖

### 方案二：Docker 部署
- 环境隔离，更专业
- 需要安装 Docker

## 注意事项
- 首次运行需要下载 Playwright 浏览器驱动（约 300MB）
- 运行一次大约需要几分钟时间
- 输出结果保存在 output 目录

---

## 审查总结

### 部署完成情况 ✅
已成功完成 TrendRadar 本地部署并测试运行。

### 执行步骤
1. **环境检查**：Python 3.10.7 环境确认正常
2. **依赖安装**：成功安装 requests、pytz、PyYAML、playwright 等依赖包
3. **浏览器驱动**：成功安装 Playwright Chromium 浏览器（136.9 MB）
4. **程序运行**：首次运行成功，程序运行约 2 分钟

### 运行结果
- ✅ 成功爬取 11 个平台的热点新闻（今日头条、百度、微博、抖音、知乎、B站等）
- ✅ 生成当日汇总 HTML 报告（857 条新闻）
- ✅ 生成 API JSON 数据文件（api/trends.json）
- ✅ 生成新闻图片（img/news.jpg，1.7 MB）
- ✅ 生成按时间段的 HTML 报告（16 个时间段）

### 输出文件位置
- **汇总报告**：`/Users/apple/work/ai/playground/TrendRadar_WX/output/2025年10月14日/html/当日汇总.html`
- **API 数据**：`api/trends.json`
- **图片文件**：`img/news.jpg`
- **分时报告**：`output/2025年10月14日/html/` 目录下的各时段文件

### 后续可选操作
1. **自定义关键词**：编辑 `config/frequency_words.txt` 添加你关心的关键词（如：AI、比亚迪等）
2. **调整运行模式**：在 `config/config.yaml` 中修改 `report.mode` 为 daily/current/incremental
3. **配置推送通知**：如需手机推送，可配置企业微信/飞书/钉钉/Telegram webhook
4. **定时运行**：可使用 cron 或 launchd 设置定时任务自动运行

### 使用建议
- 直接在浏览器打开 `当日汇总.html` 文件查看今日热点新闻
- 每次运行 `python3 main.py` 即可获取最新热点
- ✅ 已配置定时任务，每小时自动运行一次

### 自动运转配置（已完成）
- **定时任务类型**：macOS launchd
- **配置文件**：`~/Library/LaunchAgents/com.trendradar.crawler.plist`
- **运行频率**：每小时自动执行一次
- **日志位置**：`logs/trendradar.log` 和 `logs/trendradar.error.log`
- **任务状态**：✅ 已加载并运行（进程 ID: 19019）

### 管理定时任务
```bash
# 查看任务状态
launchctl list | grep trendradar

# 停止任务
launchctl unload ~/Library/LaunchAgents/com.trendradar.crawler.plist

# 启动任务
launchctl load ~/Library/LaunchAgents/com.trendradar.crawler.plist

# 立即运行一次
launchctl start com.trendradar.crawler

# 查看运行日志
tail -f logs/trendradar.log
```
