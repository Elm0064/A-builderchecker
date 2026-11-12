# AI使用记录

## 1 工具、模型与日期

DeepSeek Harness 编码代理，deepseek-flash 模型，2026-11-12。

## 2 输入上下文与问题

输入包括课程 `03_实验包`、`02_操作手册/10_E10_A组.md`、E10 授课 PPT 与云主机实际状态。需要完成 E10 A 轨道的六类场景矩阵、30 行完整集合对照、逐场景计时、最终 BuildChecker 重检与 CI 证据整理。

## 3 模型建议与采纳

模型按手册给出执行序列：建立矩阵样例与基线、依次测量 `unchanged`、`body`、`include`、`declaration`、`flags`、`header`、生成 `results.csv`、`performance-summary.csv` 与 `gate.json`、运行最终 BuildChecker。新增测试类 `StudentE10A` 由模型生成，检查传递 include 下增量与全量集合一致。

以上建议全部采纳。未采用替代测量方法。

## 4 人工修改

学生复核 30 行矩阵、计时数据、报告字段与提交内容后确认提交。（人工填写具体复核修改）

## 5 实际验证命令与结果

```bash
python3 -m unittest discover -s labs/e8e10 -p test_profile.py -v
```

`Ran 10 tests`、`OK`、退出码 0。`gate.json` 为 `passed=true`、`rows=30`，`results.csv` 的 `equal` 全部为 `True`，证据目录为 `work/e10-A-manual.Jp4epm`。

## 6 贡献边界

`labs/e8e10/lab.py`、`run_lab.py`、`labs/e8e10/environment/` 为课程参考实现，未修改。本次新增内容为 `labs/e8e10/test_profile.py` 中的 `StudentE10A` 测试类及本报告。
