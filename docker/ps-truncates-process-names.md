# `ps` cuts process names at 15 characters

A process started as `egglog-experiment` shows up in `ps -o comm` as `egglog-experime`.

Linux keeps the command name in a 16-byte field (`TASK_COMM_LEN`), so 15 characters plus the terminator. Anything longer is cut.

So when a script looks for leftover processes by name, match on the first 15 characters:

```python
def is_tolerated(comm: str, names=("egglog-experime",)) -> bool:
    return comm.startswith(names)
```

`ps -o args` shows the full command line if you need it.
