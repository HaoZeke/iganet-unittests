# IGAnets-UnitTests

[![GitlabSync](https://github.com/iganets/iganet-unittests/actions/workflows/gitlab-sync.yml/badge.svg)](https://github.com/iganets/iganet-unittests/actions/workflows/gitlab-sync.yml)
[![CI](https://github.com/iganets/iganet-unittests/actions/workflows/ci-push-pr.yml/badge.svg)](https://github.com/iganets/iganet-unittests/actions/workflows/ci-push-pr.yml)
[![Documentation](https://img.shields.io/badge/docs-mkdocs-blue.svg)](https://iganets.github.io/iganet/)

[![GitHub Releases](https://img.shields.io/github/release/iganets/iganet-unittests.svg)](https://github.com/iganets/iganet-unittests/releases)
[![GitHub Downloads](https://img.shields.io/github/downloads/iganets/iganet-unittests/total)](https://github.com/iganets/iganet-unittests/releases)
[![GitHub Issues](https://img.shields.io/github/issues/iganets/iganet-unittests.svg)](https://github.com/iganets/iganet-unittests/issues)

This repository contains unit tests for [IGAnets](https://github.com/iganets/iganet), a novel approach to combine the concept of deep operator learning with the mathematical framework of isogeometric analysis.

## Usage instructions

This repository can be used in two modes:

1. As standalone unit tests by running CMake on _this_ repository without flags

2. As optional module in [iganets::core](https://github.com/iganets/iganet) by running CMake on the [iganets::core](https://github.com/iganets/iganet) repository with the flag
   ```
   -DIGANET_OPTIONAL="unittests"
   ```

In both cases, the tests can be run via
```shell
make test
```

## Building with Meson

The unit tests can also be built with [Meson](https://mesonbuild.com) (1.3.0 or newer) and Ninja. The Meson build sits beside the CMake build and does not replace it. It builds one executable per `src/unittest_*.cxx` and registers each as a test.

Dependencies are resolved through wrap files in `subprojects/`:

- `iganet.wrap` fetches [IGAnets](https://github.com/iganets/iganet) from the `build/meson` branch of the HaoZeke fork. Once the Meson build is part of upstream, change `url` to `https://github.com/iganets/iganet.git` and `revision` to `main`.
- `gtest.wrap` is the WrapDB wrap for GoogleTest.
- `bsplinelib.wrap` fetches [BSplineLib](https://github.com/tataratat/BSplineLib), a CMake project, at a fixed revision. Meson configures it through its CMake backend with `BSPLINELIB_SHARED=ON`.

pugixml, nlohmann_json and Boost.Preprocessor come through the wraps of IGAnets. A dependency that the system provides is used instead of its wrap; add `--force-fallback-for=gtest,pugixml,nlohmann_json` to build from the wraps only.

LibTorch is found by IGAnets through Meson's CMake backend. Point `cmake_prefix_path` at the unpacked LibTorch directory (the one that contains `include`, `lib` and `share`):

```shell
meson setup build -Dcmake_prefix_path=/path/to/libtorch
meson compile -C build unittest_options unittest_activation
meson test -C build unittest_options unittest_activation
```

Without target names `meson compile -C build` builds all executables; several of them need minutes and gigabytes of memory each, so build them one at a time with `-j 1`.

- `-Dcoeff_type=float` selects the coefficient type of the splines under test (default `double`, CMake `IGANET_UNITTESTS_COEFF_TYPE`).

Tests carry the suite `unittests`; the tests that compare against BSplineLib reference values also carry the suite `bsplinelib`:

```shell
meson test -C build --suite bsplinelib
```
