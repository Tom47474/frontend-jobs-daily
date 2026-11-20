# 2027届前端校招飞书日报

云端 GitHub Actions 每天 UTC 00:00（北京时间08:00）启动。GitHub 可能延迟调度，不保证8点整送达。电脑无需开机。支持手动 Run workflow。

## 启用前配置

在 Settings → Secrets and variables → Actions → Secrets 中添加：

- `FEISHU_WEBHOOK`：飞书自定义机器人地址，沿用已测试的地址；仅支持无签名校验的机器人。
- `OPENAI_API_KEY`：具有联网搜索模型权限和可用额度的 OpenAI API Key。

在 Variables 中设置：

- `OPENAI_MODEL`：你同意使用的、支持 Responses API web_search 的模型标识。
- `DAILY_ENABLED`：确认费用并准备好测试后设置为 `true`。未设置时工作流跳过，不产生API请求。

API 模型及联网搜索按 OpenAI API 价格计费；不要假定 ChatGPT 订阅包含这些调用。私有仓库的 Actions 使用量还取决于 GitHub 计划。请先检查各自账户额度。

## 验证和故障处理

配置后在 Actions → Frontend campus jobs daily → Run workflow 手动运行。应同时看到绿色成功记录、飞书日报和 state.json 的成功日期，再停用旧本机任务。

Secrets 不写入仓库。仓库若公开，报告和去重记录也是公开的公开招聘资料；建议设为私有。

只有飞书返回code=0才记为成功。11232等明确限流最多重试3次；网络超时不自动重发，避免重复。发送前保存pending_day；如有中断，下次运行会停止，需核对飞书是否收到后人工清除pending_day。Actions红色失败记录可查看错误；API错误不得当作“没有新增”。成功后提交state.json去重，提交失败也要检查后再重跑。

迁移保留此前已推送企业记录。网页或公众号无法访问时如实注明。AI检索有覆盖限制，投递资格与截止时间以企业原文为准。

参考：
- https://developers.openai.com/api/docs/guides/tools-web-search
- https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule
