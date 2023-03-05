# 并行计算基础 - 集群文档

你好！这里是并行计算基础 2023 年春季学期的集群文档。

关于初次登陆集群需要进行的设置，见 [集群使用初步](intro.md)。

在初次登陆之后，再次登陆并加载 Spack 请使用

```bash
# 登录集群
$ ssh -p 22222 1145141919@166.111.226.100
# 或者如果你设置了 config: `ssh parallel-comp`

# 加载 Spack
[__@admin ~] source /home/software/spack/share/spack/setup-env.sh

# 加载其他软件包
spack load openmpi
# ...
```
