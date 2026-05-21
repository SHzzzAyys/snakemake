# Fork 研究笔记 — Snakemake

> 本文件是 fork 所有者（@SHzzzAyys）的研究记录，**非上游内容**。上游说明见 [README.md](./README.md)。

## 为什么 fork

评估能否用 Snakemake 重构服务器上的 RNA-seq pipeline（wh3wh6_rnaseq），替代手写 shell 脚本。

## 做了什么

- 独立 venv 装 snakemake 9.21.0。
- 写了一个 Windows 友好的 demo（不在本仓库内）：全用 `run:` Python 块模拟 fastqc→trim→align→count→merge，三样本 WH3/WH6/ctrl。
- 实测三大特性：
  - **DAG 推导**：`--dag` 自动算出 14 job 的 graphviz 依赖图。
  - **并行**：`-j3` 把 14×1s 串行压到 9s。
  - **断点续跑**：删最终表 + 一个样本中间产物，只重算受影响的 4 步，其余 11 步跳过。

## 结论

DAG 驱动的自动最小重算 / 并行 / 断点续跑显著优于 shell 脚本串流程。值得迁移真实 pipeline，但需按真实工具（比对器、计数、参考基因组）重写 Snakefile——demo 只验证了机制。
