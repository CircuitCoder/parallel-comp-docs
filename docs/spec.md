# 集群配置

## CPU

以下信息由 `lscpu` 反映。如果想细节查看，或者想了解 CPU 支持的指令集扩展，请 `srun -N1 -n1 lscpu`。

- CPU 型号: 双路 Xeon Gold 6342
  - 每路 CPU 共有 24 物理核心，每个物理核心支持 2 超线程。
  - 每个节点共 2x2x24 = 96 核心，整个集群计算节点拥有 96x4 = 384 核心。
- 主频 2.80GHz，最高 Boost 到 3.5GHz。
- 两路 CPU 各构成一个 NUMA 节点。节点内外通信时间会略有不同，请据此摆放计算任务。具体的核心编号顺序请使用 `numactl -H` 查看。

## GPU

TODO
