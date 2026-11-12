# 决策记录

## 1 背景与约束

E8 需要为固定提交与编译配置建立历史实际依赖图，并把结论回溯到原始证据。样例只包含 `main.o` 与 `helper.o` 两个对象目标，构建环境固定为 `course-e8e10:241250090`。

## 2 选择与替代方案

选择：使用 BuildChecker `full_check` 产生的 `actual-graph.json`，投影到 `main.o` 与 `helper.o` 后写入 `baseline.actual`，`baseline_source` 记录投影来源。

替代方案：只用 `gcc -MM` 结果作为基线。该方案速度快，但无法提供构建期系统调用证据，也无法验证声明遗漏在真实构建中的表现。

## 3 代价与覆盖范围

基线依赖一次完整 clean build 与 `strace` 跟踪，耗时高于纯 `gcc -MM`。投影只覆盖两个对象目标，`app` 链接目标不纳入对象目标比较；系统头文件被过滤。

## 4 验证命令与实际证据

```bash
python3 labs/e8e10/lab.py e8 --track A --out work/e8-A-manual.D250Zu
```

`baseline.json` 的 `actual` 与 `declared` 各含两个目标；`findings` 精确为 `MISSING(main.o, config.h)` 与 `REDUNDANT(main.o, unused.h)`；`incremental.json` 的 `reused` 为两个目标、`refreshed` 为空。BuildChecker 的 `findings.json` 状态为 `SUCCEEDED`，读取证据位于 `trace/t.14:579` 与 `trace/t.15:23`。
