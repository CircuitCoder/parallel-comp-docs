# 集群使用初步

以下是关于集群的基本信息，以及初次登录所需要进行的设置。

课程集群目前由五台机器组成：

- `admin`: 登录节点，对外开放，IP `166.111.226.100`
- `node1-4`: 四个计算节点。

关于计算节点的详细配置，见 [集群配置](spec.md)。

## SSH

同学从外界可以使用 ssh 连接登录集群 `admin` 节点，需要使用端口 22222，用户名是你的学生证号，例如：

```bash
$ ssh -p 22222 学生证号@166.111.226.100
[学生证号@admin ~]
```

为了方便后续登录以及使用 scp 传输文件，推荐在 `~/.ssh/config` 文件末尾添加以下配置（如目录、文件先前不存在，从空文件开始）：

```
# ... 之前的内容

Host parallel-comp
  User 学生证号
  Hostname 166.111.226.100
  Port 22222
```

之后即可使用 `ssh parallel-comp` 登录集群。

## 修改密码

为避免密码泄露，请第一时间使用 `passwd` 将登录密码修改为一个更强的密码。

你也可以选择[添加 ssh key](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)。

## Spack 初始化

常用的库和软件，例如 CUDA, OpenMPI 等已经在集群中安装好了，但是默认是不可见的。这些软件使用 Spack 管理，用户可以动态地“显示”、“隐藏”这些软件包。

Spack 在初次登录后需要进行一些设置：

```bash
[____@admin ~] source /home/software/spack/share/spack/setup-env.sh
[____@admin ~] spack load gcc@10.4.0
[____@admin ~] spack compiler find
```

## Spack 加载软件

初始化之后，可以使用 `spack load` 和 `spack unload` 指令动态加载、卸载软件。例如如果你需要使用 OpenMPI 的相关库和可执行文件，可以运行：

```bash
$ spack load openmpi
```

之后可以在 Shell 中看到 OpenMPI 提供的 mpicxx、mpirun 等可执行文件。尝试：

```bash
$ which mpicxx
$ mpicxx
```

可以使用 `spack unload ...` 隐藏软件包，使用 `spack list` 列出所有软件包。详细的说明请见 [spack 文档](https://spack.readthedocs.io/en/latest/command_index.html)。

## 提交任务

集群使用 SLURM 管理任务，在计算节点上执行任务无需登录到计算节点上。`osu_bw` 是俄亥俄州立大学的 Micro Benchmark 中测试带宽的程序，以它为例：

```
# 加载 osu_bw 所在的软件包
# TODO: 目前有命名冲突（IMPI OMPI 各一个）。需要看老师上课讲哪个版本的 MPI
$ spack load osu-micro-benchmark

# 提交任务
# TODO: 目前不工作，见 internal/todo.md 中关于 PMIx 的说明
$ srun -n2 -N2 osu_bw
```

调用 `squeue` 可以看到等待中的任务，调用 `sinfo` 可以看到节点的。详细的说明请见 [SLURM 文档](https://slurm.schedmd.com/quickstart.html)。

## VSCode

如果希望使用 VSCode 直接编辑集群上的文件，请安装 VSCode "Remote - SSH" 扩展。在安装完成之后，在 ++ctrl+shift+p++ 的选单中运行 `Remote-SSH: Connect to Host...`。如果你已经配置了 `~/.ssh/config`，那么应该可以看到集群显示在下一步选单中了。如果没有出现，请选择 `Configure SSH Hosts...`，并和 `~/.ssh/config` 写入同样的内容，再重新执行 `Remote-SSH: Connect to Host...`。

使用 VSCode 登录集群后即可远程编辑，并可以使用内置的集成终端在集群上执行命令。

!!! warning "如果你在 Windows 上使用 WSL"

    请注意：WSL 的 `~/.ssh/config` 不在 Windows 的文件系统内。通常你的 VSCode 安装在 Windows 上，因此会使用两套 SSH 配置。公钥登录时也是一样：两边使用的密钥在不同的路径上。

## Jupyter

TODO
