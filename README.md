# claude-rpa

RPA 自动化录制工具，基于 [laziobird/openclaw-rpa](https://github.com/laziobird/openclaw-rpa) 修改。

录制浏览器操作和本机文件操作，生成可复用的 Playwright 脚本。回放时**不调大模型**，省费用、更快、无幻觉。

## 快速开始

```bash
cd /Users/liyuhao/Documents/Aitrash/claude-rpa
source .venv/bin/activate

# 环境检查
python3 rpa_manager.py env-check

# 查看已录制的任务
python3 rpa_manager.py list
```

## 录制新流程

```
record-start <任务名>            启动浏览器开始录制
record-step '<json>'             发送单步操作指令
record-status                    查看录制状态
record-end                       结束录制，生成 RPA 脚本
record-end --abort               放弃录制
```

**示例：**

```bash
# 启动录制
python3 rpa_manager.py record-start 我的任务

# 录制完成后生成脚本到 rpa/ 目录
# 之后可直接回放
python3 rpa_manager.py run 我的任务
```

## CDP 模式（使用已有 Chrome）

如果你的 Chrome 已开启远程调试（`--remote-debugging-port=9222`），可以直接连接，不启动新浏览器：

```bash
# 录制时连接已有 Chrome（默认新建标签页）
python3 rpa_manager.py record-start 我的任务 --cdp-url http://127.0.0.1:9222

# 录制时复用当前已激活的标签页（不新建）
python3 rpa_manager.py record-start 我的任务 --cdp-url http://127.0.0.1:9222 --use-active-tab

# 登录时连接已有 Chrome
python3 rpa_manager.py login-start https://example.com/login --cdp-url http://127.0.0.1:9222

# 回放时连接已有 Chrome
python3 rpa_manager.py run 我的任务 --cdp-url http://127.0.0.1:9222
```

**参数说明：**

| 参数 | 适用命令 | 说明 |
|------|---------|------|
| `--cdp-url http://ip:port` | `record-start`, `login-start`, `run` | 连接已有 Chrome，不启动新浏览器 |
| `--use-active-tab` | `record-start`（需配合 `--cdp-url`） | 复用当前激活的标签页而非新建 |

## 登录会话管理

适用于需要验证码、短信、滑块等复杂登录的网站——只需登录一次，Cookie 重复复用：

```bash
# 1. 打开登录页
python3 rpa_manager.py login-start https://example.com/login

# 2. 在浏览器里手动完成登录
# 3. 导出 Cookie
python3 rpa_manager.py login-done

# 4. 录制时自动注入 Cookie（跳过登录）
python3 rpa_manager.py record-start 我的任务 --autologin example.com
```

## 能力码（Capability）

录制任务时通过 `--profile` 指定任务类型：

| 能力码 | 说明 |
|--------|------|
| A | 仅浏览器自动化 |
| B | 仅 Excel |
| C | 仅 Word |
| D | 浏览器 + Excel |
| E | 浏览器 + Word |
| F | Excel + Word（无浏览器）|
| G | 浏览器 + Excel + Word |
| N | 以上都不需要（仅 API/文件操作）|

## 依赖

- Python 3.8+
- `playwright` — 浏览器自动化
- `httpx` — HTTP API 调用
- `openpyxl` — Excel `.xlsx` 操作
- `python-docx` — Word `.docx` 操作

## 与上游的差异

- 新增 `--use-active-tab` 参数：CDP 连接时复用当前已激活的标签页而非新建

## 协议

本项目基于 Apache License 2.0 发布。原始项目 [openclaw-rpa](https://github.com/laziobird/openclaw-rpa) 版权所有 2026 openclaw-rpa contributors。
