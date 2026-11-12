# AI使用记录

## 1 工具、模型与日期

DeepSeek Harness 编码代理，deepseek-flash 模型，2026-11-12。

## 2 输入上下文与问题

输入包括课程 `03_实验包`、`02_操作手册/8_E8_A组.md`、E8 授课 PPT 与云主机实际状态。需要完成 E8 A 轨道的基线建立、BuildChecker 证据采集、未变化复用验证与报告整理。

## 3 模型建议与采纳

模型按手册给出执行序列：`lab_ready`、`lab_new_run e8 A`、`fixture()`、`scan()`、`buildchecker()`、增量扫描与结论写入。新增测试类 `StudentE8A` 由模型生成，检查声明变化时实际图复用与发现重算。

以上建议全部采纳。未采用替代路径或额外脚本。

## 4 人工修改

学生复核命令输出、报告字段与提交内容后确认提交。（人工填写具体复核修改）

## 5 实际验证命令与结果

```bash
python3 -m unittest discover -s labs/e8e10 -p test_profile.py -v
```

`Ran 10 tests`、`OK`、退出码 0。E8 的基线、BuildChecker 与增量断言全部通过，证据目录为 `work/e8-A-manual.D250Zu`。

## 6 贡献边界

`labs/e8e10/lab.py`、`run_lab.py`、`labs/e8e10/environment/` 为课程参考实现，未修改。本次新增内容为 `labs/e8e10/test_profile.py` 中的 `StudentE8A` 测试类及本报告。
