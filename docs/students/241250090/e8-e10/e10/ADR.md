# 决策记录

## 1 背景与约束

E10 需要判定增量扫描与全量扫描是否等价，并给出可核对的耗时数据。只比较发现数量无法发现目标或依赖不同但数量相同的情况，因此必须比较完整集合。

## 2 选择与替代方案

选择：对 `actual`、`declared`、`findings` 三个集合逐项比较，六类场景各重复五次，共 30 行；每类五次都从该类变化前的同一历史图出发，末次结果作为下一场景的历史图。

替代方案：只比较 `findings` 数量或只比较 `passed` 布尔值。该方案实现简单，但会漏掉目标替换、依赖替换和声明图错误。

## 3 代价与覆盖范围

每类场景执行五次增量与五次全量扫描，输出 60 份原始 JSON 与汇总 CSV。计时范围是 `scan()` 路径，包含 Git 提交读取、声明解析、key 计算与 `gcc -MM`，不包含 clean build。小样例中固定开销占比较高，复用场景的加速比不能代表论文规模。

## 4 验证命令与实际证据

```bash
python3 labs/e8e10/lab.py e10 --track A --out work/e10-A-manual.Jp4epm
```

`gate.json` 为 `passed=true`、`rows=30`；`results.csv` 的 `equal` 全部为 `True`；`performance-summary.csv` 记录六类中位数与极值。最终 BuildChecker 重检只剩 `REDUNDANT(main.o, unused.h)`，两项 MD 已消除。
