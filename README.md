# Print Handy regression corpus

Public-source 3D models used for regression testing in
[`catduckgnaf/print-handy`](https://github.com/catduckgnaf/print-handy). Model
binaries are distributed **only as release assets**; they are not committed to
this repository, so the archive is small (manifest + README) and the models are
downloaded on demand by the `model-corpus` workflow.

## Versions

Two GitHub Releases carry the corpus, tagged to keep the download commands stable
across the `print-buddy` → `print-handy` rename:

| Release tag | What it is | Models | Asset |
| :--- | :--- | :---: | :--- |
| `corpus-v1` | The core regression set — 171 public-source models. | 171 | [`print-handy-corpus-v1.tar.gz`](https://github.com/catduckgnaf/print-handy-corpus/releases/download/corpus-v1/print-handy-corpus-v1.tar.gz) |
| `corpus-v2` | The [#458](https://github.com/catduckgnaf/print-handy/issues/458) complex cases (7–69 MB each) for slicer diversity. | complex | [`print-handy-corpus-v2.tar.gz`](https://github.com/catduckgnaf/print-handy-corpus/releases/download/corpus-v2/print-handy-corpus-v2.tar.gz) |

The core archive extracts to a top-level `models/<case-id>/<file>` tree with one
folder per case; `corpus-v2` layers additional cases onto the same `models/`
directory.

## Download and verify

Each release ships its tarball plus a `SHA256SUMS` for that tarball:

```bash
gh release download corpus-v1 --repo catduckgnaf/print-handy-corpus --dir corpus
cd corpus && sha256sum -c SHA256SUMS && tar -xzf print-handy-corpus-v1.tar.gz
```

Re-run `sha256sum -c` before trusting the archive.

## Reproduce the complex cases

Public cases can be reproduced from their sources (Printables, NASA) through the
same eligible-site paths production intake uses, pinned by SHA-256 and byte size,
so a changed upstream file is refused, not silently accepted:

```bash
cd ~/projects/print-handy
python tests/corpus/fetch_complex_corpus.py            # -> ~/print-handy-complex-corpus
COMPLEX_CORPUS_ROOT=~/print-handy-complex-corpus \
  python -m pytest tests/corpus/test_complex_corpus.py # manifest contract + hash check
```

## Attribution

The core models are public-source; each retains its author's original license
(see the manifest in the archive, and
[`tests/corpus/complex_cases.toml`](https://github.com/catduckgnaf/print-handy/blob/main/tests/corpus/complex_cases.toml)
for the complex cases). Redistribution here does not change those licenses.
