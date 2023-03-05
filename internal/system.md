# Filesystem

All user accessible storage is located on `admin`. Directories are bind-mounted under `/srv/nfs` and exported to computing nodes.

- `/home/software` -> `/srv/nfs/software` (RO)
- `/home/2023-spring` -> `/srv/nfs/users` (RW)

# User

User `x`'s home should be placed at `/home/2023-spring/x`. This is to make sure all computing nodes see identical files that are written by users.

UID / Home GID begins at 2000
