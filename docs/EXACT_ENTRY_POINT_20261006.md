# 完整精确入口消去检查点（2026-10-06）

已验证 Lean 源码：`6ab64bc33deb2209c27ecf43a807d8e38d963351`。此发布分支仅增加检查点文档，Lean 源码、验证脚本、工具链及依赖保持该提交内容。

## 完整电路与六阶段资源

当前入口是 `controlledPointAdd`，不是单独的 GCD、求逆或平方子电路。有限经典加数时的完整受控点加资源如下。所有 Toffoli 和测量数均为同一已证明程序的静态指令数，测量数不是测试输入数量或抽样次数。

| 阶段 | 逻辑分配位置上界（含常驻位置） | 静态 Toffolis | 测量指令 |
| --- | ---: | ---: | ---: |
| 1. Coordinate differences | ≤1,036 | 2,046 | 2,046 |
| 2. Skywalk-GCD division | ≤1,899 | 1,055,297 | 724,551 |
| 3. Prepare X workspace | ≤1,036 | 1,023 | 1,023 |
| 4. Measured streamed modular square | ≤1,297 | 99,902 | 99,382 |
| 5. Forward multiplication | ≤1,899 | 1,055,297 | 724,551 |
| 6. Recover output | ≤1,036 | 2,301 | 2,301 |
| 六阶段小计 | ≤1,899 | 2,215,866 | 1,553,854 |
| 额外输入/角落分类 | ≤1,034 | 4,104 | 4,104 |
| 完整受控有限加数点加 | ≤1,899 | **2,219,970** | **1,557,958** |

位置上界不是精确 peak-live 测量，也不声称最优分配。六阶段表采用概念顺序，源码先平方减法后加 `3x_A`，两者的域更新可交换。无穷远经典加数对应空程序。Stage 2/5 的 ≤1,297 Q / <600K Toffoli 目标仍未达到。

## 此次精确变化

相对完整验证的 `adcdfef`，Stage 2 入口消去省 1,535 Toffolis/测量，Stage 5 入口消去省 1,536 Toffolis/测量。合计 3,071，已包含在上述总数。先前末轮消去的收益不重复计数。整数记录/反记录仍保留 512 轮，字段回放保留已证明的末轮省略。没有近似窗口、概率域限制或未证明的高位截断。

公开字段契约的假设与结论不变，控制、输入符号、每个独立测量记录和工作区清理保持。`ControlledPointAddSpec.lean`、`PointAddSpec.lean`、`AffineFormula.lean` 与 `adcdfef` 逐字一致。证明范围仍是仓库的带符号基态/固定测量记录模型，见 [PROOF_SCOPE](PROOF_SCOPE.md)，没有增加完整量子信道证明。

相对 7,207,866-Toffoli 原始基线，省 **4,987,896（69.20%）**。

## 验证证据

- 用户 CPU pod 上完整 build：3,939 jobs，695s（11m35s）。
- 传递公理审计：1,466 queries / 1,465 distinct declarations，205s（3m25s）。
- 合计 900s（15m），验证器 setup 和 queue 各 0s。源码准备/传输未单独计时。
- 833 个源码哈希在验证前后及发布源码中一致。
- 公理仅允许 `propext`、`Classical.choice`、`Quot.sound`。
- 验证脚本显式构建 audit-only RecordedRailApply 依赖，再检查所有入口，避免缺失对象文件。

可移植证据：[计时/资源回执](verification/entry-exact-20261006/timing.json)、[源码哈希](verification/entry-exact-20261006/source-manifest.json)、[完整公理输出](verification/entry-exact-20261006/axioms.log)。验证脚本为 [verify.sh](../scripts/verify.sh) 与 [verify_timed.sh](../scripts/verify_timed.sh)。资源入口为 [ControlledPointResources](../ECDSAAdd/Arithmetic/ControlledPointResources.lean) 与 [MappedCompressedStageCounts](../ECDSAAdd/Arithmetic/MappedCompressedStageCounts.lean)。

发布不包含尚未接入/验证的 258 位 signed/streamed 候选或两步融合实验。已随验证源码保存的 Recorded 框架组件不会被计为当前点加的额外资源收益。
