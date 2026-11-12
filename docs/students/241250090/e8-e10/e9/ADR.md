# 决策记录

## 1 背景与约束

E9 需要判断跨提交扫描何时复用历史实际图、何时重新采集。判断必须同时覆盖普通代码、预处理指令、Makefile 配方与编译配置，并且不能把声明变化误判为实际读取变化。

## 2 选择与替代方案

选择：以 `scan.key` 作为缓存身份，key 包含项目内 `.c` 与 `.h` 文件的预处理行、Makefile 中以制表符开头的配方行、`CFLAGS` 与 `gcc --version` 首行。key 相同则复用历史实际图，key 不同则刷新 `main.o` 与 `helper.o` 两个目标。

替代方案：按目标建立更细的失效范围，只刷新受影响的翻译单元。该方案更接近论文的增量分析，但需要更完整的依赖图与命令建模，超出当前教学实现范围。

## 3 代价与覆盖范围

保守刷新会在仅一个目标受影响时重采两个目标，降低复用收益；C2、`flags`、`header` 场景的增量耗时与全量接近。声明变化不改变 key，因此只重算声明图与发现集合。

## 4 验证命令与实际证据

```bash
python3 labs/e8e10/lab.py e9 --track A --out work/e9-A-manual.KufTDW
```

C1 的 `reused` 为两个目标、`refreshed` 为空；C2 的 `refreshed` 为两个目标，`added` 含 `MISSING(main.o, extra.h)`；C3 的 `resolved` 含两项 MD，`findings` 只剩 `REDUNDANT(main.o, unused.h)`。C1 与 C2 的构建日志显示只有 `main.o` 重新编译并重新链接。
