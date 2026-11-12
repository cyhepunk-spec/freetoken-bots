# FreeToken-Bots

保守的 OpenRouter 免费模型发现与扫描工具。每天扫描一次 OpenRouter 公开模型目录（`/models`，无需密钥），记录免费模型清单的变化。

## 脚本

`scripts/radar.py`：

- `scan`：扫描并写入报告 `~/.pi/agent/freetoken-bots.json`
- `status`：打印最新报告
- `probe --model <id>`：对已验证免费的模型做一次真实调用测试（需要 `OPENROUTER_API_KEY` 环境变量）
- `sync`：输出 Pi 的模型同步提案（仅提案，不改配置）

免费判定是保守的：prompt、completion、request、缓存读写价格必须全部为 0；参数量只认 OpenRouter 返回的直接数字，不从名字或上下文长度推断。

## 每日扫描（GitHub Actions）

`.github/workflows/scan.yml` 每天 08:00（新加坡时间，UTC 00:00）运行一次：

1. 用仓库里的 `freetoken-bots.json` 作为上一份报告
2. 跑 `python3 scripts/radar.py scan`
3. 对比新旧报告（忽略 `checked_at` 时间戳）
4. 只有免费模型清单真正变化时才提交新报告并开一个 issue

扫描本身不需要任何密钥，工作流也不用配 secret。

## 本地运行

```bash
python3 scripts/radar.py scan
python3 scripts/radar.py status
```
