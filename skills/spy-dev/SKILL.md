---
name: spy-dev
description: Useful for SPy development and design.
---

# SPy development

## Installing

From the root of the checkout:

```bash
uv sync
. .venv/bin/activate   # NOT `source`: it doesn't work in some sandboxes
make -C spy/libspy
```

Use `. .venv/bin/activate` rather than `source`, because some sandboxes use a shell where `source` fails. Run the `make -C spy/libspy` step after `uv sync`: it builds the C support library that the C backend needs, so C tests fail without it.

## Running tests

Never run the full test suite. It is slow (it compiles a lot of C), and the output is rarely useful for a focused change. Target what you touched, and filter by backend marker:

```bash
pytest spy/tests/path/to/test_file.py -m interp
pytest spy/tests/path/to/test_file.py -m C
```

`-m interp` runs only the interpreter variant of the tests and is the fastest feedback loop, so start there. Add `-m C` once the interpreter variant passes and the change could affect the C backend. Narrow further with `-k <name>` when you're iterating on a single test.
