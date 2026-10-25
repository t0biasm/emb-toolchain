# emb-toolchain

Bazel module `emb_toolchain`: compiler toolchains and host tooling shared by all `prj-*` projects.

| Path | Content |
|---|---|
| `platforms/avr8/toolchain` | AVR-GCC `cc_toolchain`, `cc_binary_avr8` |
| `platforms/c2000/toolchain` | TI C2000 CGT `cc_toolchain`, `cc_binary_c2000` |
| `platforms/gcc/toolchain` | GCC unit-test toolchain (Windows/MSYS2) |
| `platforms/BUILD` | Linux/Windows execution platforms |
| `tools/llvm` | clang-format / clang-tidy (Windows, Linux x86_64) and check scripts |
| `shared.bazelrc` | `--config` definitions (`atmega32u4`, `c2000`, `gcc`) |

Toolchains, execution platforms and registrations are declared in this module's `MODULE.bazel`; projects only need a `bazel_dep` plus a `local_path_override` to the submodule checkout.

## Use in a project
```
git submodule add https://github.com/t0biasm/emb-toolchain toolchain/emb-toolchain
```
MODULE.bazel:
```
bazel_dep(name = "emb_toolchain", version = "0.1.0")
local_path_override(module_name = "emb_toolchain", path = "toolchain/emb-toolchain")
```
.bazelrc:
```
import %workspace%/toolchain/emb-toolchain/shared.bazelrc
try-import %workspace%/user.bazelrc
```

## Artifactory
Compilers are downloaded from an Artifactory instance whose URL is **not** stored in this repository. Set it per machine in the gitignored `user.bazelrc`:
```
common --repo_env=EMB_ARTIFACTORY_URL=https://<host>/artifactory
```
Credentials are read from `~/.netrc`. In CI the URL comes from the `ARTIFACTORY_URL` secret.

## Tooling
Needs Bazelisk (Bazel version in `.bazelversion`) and Python 3 for the check scripts.
