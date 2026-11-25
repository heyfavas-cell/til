# `python -I` ignores `PYTHONIOENCODING`

On Windows I ran a script with `python -I` and set `PYTHONIOENCODING=utf-8` so it could print non-ASCII text. It still crashed with `UnicodeEncodeError: 'charmap' codec can't encode...`.

`-I` (isolated mode) implies `-E`, which ignores every `PYTHON*` environment variable, including `PYTHONIOENCODING`. The console stays on the code page (cp1252 here).

```sh
PYTHONIOENCODING=utf-8 python -I -c "import sys; print(sys.stdout.encoding)"   # cp1252
PYTHONIOENCODING=utf-8 python    -c "import sys; print(sys.stdout.encoding)"   # utf-8
```

Keep `-I` (it stops the script's folder from shadowing modules) and set the encoding in the script instead:

```python
import sys
sys.stdout.reconfigure(encoding="utf-8")
```
