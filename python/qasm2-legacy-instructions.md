# Old QASM files need `LEGACY_CUSTOM_INSTRUCTIONS`

Qiskit's `qasm2.loads` reads `qelib1.inc` as the OpenQASM 2 paper defines it. Many files in the wild use gates that only Qiskit's old version of that file had: `sx`, `rxx`, `rzz`, `u`. Those fail with `QASM2ParseError`, while `u3` and `cx` load fine.

Pass the legacy set to get the old behaviour:

```python
from qiskit import qasm2

circuit = qasm2.loads(
    text,
    custom_instructions=qasm2.LEGACY_CUSTOM_INSTRUCTIONS,
)
```

If you're checking a whole folder of benchmarks, load every file this way before trusting any result. One file that doesn't parse can quietly drop out of a table.
