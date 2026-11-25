# Git Bash rewrites Linux paths in `docker run`

On Windows, in Git Bash, this fails:

```sh
docker run --rm image cat /root/benchmark.txt
# cat: 'C:/Program Files/Git/root/benchmark.txt': No such file or directory
```

MSYS sees an argument that starts with `/` and converts it to a Windows path before Docker gets it. The path is meant for inside the container, so the conversion breaks it.

Turn it off for one command:

```sh
MSYS_NO_PATHCONV=1 docker run --rm image cat /root/benchmark.txt
```

Or start the path with two slashes (`//root/benchmark.txt`), which MSYS leaves alone.
