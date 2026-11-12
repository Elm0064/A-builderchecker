# AI使用记录

## 1 工具、模型与日期

DeepSeek Harness 编码代理，deepseek-flash 模型，2026-11-12。

## 2 输入上下文与问题

输入包括课程 `03_实验包`、`02_操作手册/9_E9_A组.md`、E9 授课 PPT 与云主机实际状态。需要完成 E9 A 轨道的 C0 至 C3 逐提交更新、复用与刷新判定、发现新增与消除验证。

## 3 模型建议与采纳

模型按手册给出执行序列：创建 C0 并构建、制造 C1 函数体变化、C2 新增 include、C3 补齐声明，逐步保存 `baseline.json`、`C1.json`、`C2.json`、`C3.json` 与构建日志。新增测试类 `StudentE9A` 由模型生成，覆盖头文件内部 include 的缓存失效。

以上建议全部采纳。未修改 `lab.py` 的扫描逻辑。

## 4 人工修改

学生复核各提交差异、报告字段与提交内容后确认提交。（人工填写具体复核修改）

## 5 实际验证命令与结果

```bash
python3 -m unittest discover -s labs/e8e10 -p test_profile.py -v
```

`Ran 10 tests`、`OK`、退出码 0。E9 的四份报告与内部 Git 历史一致，证据目录为 `work/e9-A-manual.KufTDW`。

## 6 贡献边界

`labs/e8e10/lab.py`、`run_lab.py`、`labs/e8e10/environment/` 为课程参考实现，未修改。本次新增内容为 `labs/e8e10/test_profile.py` 中的 `StudentE9A` 测试类及本报告。
