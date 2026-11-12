# AI使用记录

## 1 工具、模型、日期与小组

DeepSeek Harness 编码代理，deepseek-flash 模型，2026-11-12。小组 A11：吴璠 241250090、邱添 241250091、孙奕珂 241250095。

## 2 输入上下文与问题

输入包括课程 `03_实验包`、`02_操作手册/10_E10_A组.md`、E10 授课 PPT 与云主机实际状态。需要完成 E10 A 轨道的六类场景矩阵、30 行完整集合对照、逐场景计时、最终 BuildChecker 重检与 CI 证据整理。

## 3 模型建议与采纳

模型按手册给出执行序列：建立矩阵样例与基线、依次测量 `unchanged`、`body`、`include`、`declaration`、`flags`、`header`、生成 `results.csv`、`performance-summary.csv` 与 `gate.json`、运行最终 BuildChecker。新增测试类 `StudentE10A` 由模型生成，检查传递 include 下增量与全量集合一致。

以上建议全部采纳。未采用替代测量方法。

## 4 人工复核与修改

吴璠复核 E8 执行记录与报告字段，邱添复核代码差异与提交内容，孙奕珂复核 E10 矩阵、计时数据与 CI 产物。三人确认 `labs/e8e10/lab.py` 与 `run_lab.py` 未修改，新增内容仅为 `test_profile.py` 中的三个测试类与报告。

## 5 实际验证命令与结果

```bash
python3 -m unittest discover -s labs/e8e10 -p test_profile.py -v
```

`Ran 10 tests`、`OK`、退出码 0。`gate.json` 为 `passed=true`、`rows=30`，`results.csv` 的 `equal` 全部为 `True`，证据目录为 `work/e10-A-manual.Jp4epm`。

## 6 贡献边界

`labs/e8e10/lab.py`、`run_lab.py`、`labs/e8e10/environment/` 为课程参考实现，未修改。本次新增内容为 `labs/e8e10/test_profile.py` 中的 `StudentE8A`、`StudentE9A`、`StudentE10A` 测试类及 `e8`、`e9`、`e10` 三份实验报告。
