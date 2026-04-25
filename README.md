# claude-rpa

RPA 自动化录制工具，基于 [laziobird/openclaw-rpa](https://github.com/laziobird/openclaw-rpa) 修改。

记录浏览器操作和本机文件操作，生成可复用的 Playwright 脚本。

## 与上游的差异

- 新增 `--use-active-tab` 参数：CDP 连接时复用当前已激活的标签页而非新建

## 协议

本项目基于 Apache License 2.0 发布。原始项目 [openclaw-rpa](https://github.com/laziobird/openclaw-rpa) 版权所有 2026 openclaw-rpa contributors。
