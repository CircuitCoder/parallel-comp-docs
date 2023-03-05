# OpenMPI IB support

OpenMPI 4 has two way of supporting IB:

- `openib`, which is deprecated in 4.0.x, and is known to have some issue when working with later OpenMPI releases.
- Using UCX. This can be achieved with selecting UCX as PML.

Right now, UCX is not compiled with IB support (verbs, dc, dm, rc, ud, etc.). So we first need to enable those.

run `ucx_info -v` to see configure flags, and `ucx_info -d` to see detected devices.

Also, OpenMPI itself seems to have explicit configure switches mentioning IB features (verbs, tm, cma, etc. Check `ompi_info -v`) Do we need to turn them on too?

To test OMPI with UCX with a specific transport on a specific device, run:

```bash
mpirun \
  --host node1:1,node2:1 --np 2 \
  --mca pml_ucx_verbose 100 --mca pml_base_verbose 100 \
  --mca pml ucx --mca pml_ucx_tls any -mca pml_ucx_devices ib0 \
  --prefix $(dirname $(dirname $(which mpirun))) \
  $(which osu_bw)
```

The above example uses tcp on `ib0` (netdev on `mlx5_0`). If we're going to use IB, it's likely that we have to use the name `mlx5_0` with a port (e.g. `mlx5_0:1`). Check `ucx_info -d`


# slurm PMIx support

Currently, slurm is not compiled with PMIx. So we cannot run `srun ompi_software` and expect it to JUST WORK (TM).

# mpirun prefix

Right now mpirun doesn't have prefixes. It also cannot automatically find orted.

It's likely we'll need these configure flags: `--enable-orterun-prefix-by-default --enable-mpirun-prefix-by-default`

It's currently unknown that after these flags are added, if we'll able to specify different prefixes for orted and user programs.
