# AI使用记录

## 1 工具、模型、日期与小组

DeepSeek Harness 编码代理，deepseek-flash 模型，2026-11-12。小组 A11：吴璠 241250090、邱添 241250091、孙奕珂 241250095。

## 2 输入上下文与问题

输入包括课程 `03_实验包`、`02_操作手册/8_E8_A组.md`、E8 授课 PPT 与云主机实际状态。需要完成 E8 A 轨道的基线建立、BuildChecker 证据采集、未变化复用验证与报告整理。

## 3 模型建议与采纳

模型按手册给出执行序列：`lab_ready`、`lab_new_run e8 A`、`fixture()`、`scan()`、`buildchecker()`、增量扫描与结论写入。新增测试类 `StudentE8A` 由模型生成，检查声明变化时实际图复用与发现重算。

以上建议全部采纳。未采用替代路径或额外脚本。

## 4 人工复核与修改

三人按 E8 分工分别复核基线、BuildChecker 证据与复用验证，并交叉核对代码差异、证据索引与 CI 产物。三人确认 `labs/e8e10/lab.py` 与 `run_lab.py` 未修改，新增内容仅为 `test_profile.py` 中的三个测试类与报告。

## 5 实际验证命令与结果

```bash
python3 -m unittest discover -s labs/e8e10 -p test_profile.py -v
```

`Ran 10 tests`、`OK`、退出码 0。E8 的基线、BuildChecker 与增量断言全部通过，证据目录为 `work/e8-A-manual.D250Zu`。

## 6 贡献边界

`labs/e8e10/lab.py`、`run_lab.py`、`labs/e8e10/environment/` 为课程参考实现，未修改。本次新增内容为 `labs/e8e10/test_profile.py` 中的 `StudentE8A`、`StudentE9A`、`StudentE10A` 测试类及 `e8`、`e9`、`e10` 三份实验报告。
