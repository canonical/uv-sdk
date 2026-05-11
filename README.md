# uv SDK for Workshop

A Python package and project manager. It provides uv and uvx, manages a shared
virtual environment via a slot, and aliases pip to uv pip for compatibility.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: python-app
base: ubuntu@24.04
sdks:
  - name: uv
    channel: latest/stable

actions:
  lint: |
    uv run ruff check
    uv run ruff format --diff
  test: uv run pytest "$@"
  format: |
    uv run ruff check --fix
    uv run ruff format
```

---

## Using the SDK

### Prerequisites, project layout

1. No prerequisite SDKs are required.
2. Your Python project (with a `pyproject.toml` or `requirements.txt`) should
   be in your project directory:

   ```bash
   git clone <YOUR_REPO_URL>
   ```

3. On launch, the SDK creates a virtual environment and sets
   `UV_LINK_MODE=copy`.

### Run commands

Once the workshop is ready:

```bash
workshop shell
uv run pytest
```

`uv run` syncs dependencies automatically on the first invocation. Downloaded
packages are stored in the `uv` cache, which is mapped to your host via the
`cache` mount plug. Subsequent runs thus reuse cached packages.

Also, `uv run` and `uv sync` by default use a `.venv` in the project directory,
separate from the shared slot venv at `/home/workshop/uv-venv`.
To use the shared slot venv, activate it in `~/.profile` within the workshop
(for instance, using an in-project SDK beside this one).

### Add dependencies

From within the workshop shell:

```bash
workshop shell
uv add requests
```

When you run `pip install`, it transparently runs `uv pip install`. Use
whichever command you prefer.

### Environment configuration

The SDK sets `UV_LINK_MODE=copy` automatically because the persistent cache
mount does not support hardlinks.

---

## Plugs (resources this SDK consumes)

### `cache`

- Interface: `mount`
- Workshop target: `/home/workshop/.cache/uv`
- Purpose: Persists uv package cache between workshop updates.

## Slots (resources this SDK provides)

### `venv`

- Interface: `mount`
- Workshop source: `/home/workshop/uv-venv`
- Purpose: Other SDKs can plug into this slot to access Python packages
  installed by uv.

---

## Documentation and guidance

- [uv official documentation](https://docs.astral.sh/uv/)
- [Workshop documentation](https://canonical-workshop.readthedocs-hosted.com/latest/)

---

## Community and support

- uv community:
  [GitHub Discussions](https://github.com/astral-sh/uv/discussions)
- Workshop forum:
  [Workshop Discourse](https://discourse.canonical.com/c/engineering/workshops/34)
- Please review our
  [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct) before
  participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- See `CONTRIBUTING.md` for guidelines.
- Open issues or pull requests on the official repository.

---

## License and copyright

Copyright 2025 Canonical Ltd.

This program is free software: you can redistribute it and/or modify it under
the terms of the
[GNU Lesser General Public License version 2.1 (LGPLv2.1)](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.html)
as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details.

[uv](https://github.com/astral-sh/uv) is licensed under the
[MIT License](https://opensource.org/licenses/MIT).
