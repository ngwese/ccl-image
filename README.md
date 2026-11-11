# Clozure CL 1.14 — linux/arm64 Singularity image

A [Singularity/Apptainer](https://sylabs.io/singularity/) SIF image containing
the official **Clozure Common Lisp 1.14** build for Linux/aarch64 from the
GitHub release:

<https://github.com/Clozure/ccl/releases/tag/v1.14>
(`ccl-1.14-linuxarm64.tar.gz`, sha256
`766c4ef63708ea9152f4565eb59d89a79ab3ddaf852a69c0ef91633f89f61332`)

Running the image starts the CCL REPL.

## Files

| File | Purpose |
|------|---------|
| `ccl-1.14-linuxarm64.sif` | The built Singularity image (arm64/aarch64). |
| `ccl.def`               | Singularity/Apptainer definition (`%post`, `%runscript`, ...); downloads & verifies CCL 1.14. |

## Usage

```sh
# Interactive CCL REPL
singularity run ccl-1.14-linuxarm64.sif

# Pass CCL command-line options through
printf '(print (* 6 7))(quit)\n' | singularity run ccl-1.14-linuxarm64.sif --batch

# Evaluate a form and exit
singularity exec ccl-1.14-linuxarm64.sif ccl --batch -e '(print (* 6 7))'

# Shell into the image (CCL is on PATH as `ccl`)
singularity shell ccl-1.14-linuxarm64.sif
```

The full CCL distribution (kernel, heap image, compiler sources and libraries)
lives at `/opt/ccl`, and `CCL_DEFAULT_DIRECTORY=/opt/ccl` is preset.

## Rebuilding

The image is built directly from `ccl.def`. If unprivileged `--fakeroot` is
available, no elevation is needed:

```sh
singularity build --fakeroot ccl-1.14-linuxarm64.sif ccl.def
```

Some hosts (e.g. Ubuntu 24.04 with the default AppArmor restriction) disallow
unprivileged user namespaces, so `--fakeroot` is unavailable. On those hosts,
build with elevated permissions instead:

```sh
sudo singularity build ccl-1.14-linuxarm64.sif ccl.def
```
