project(
  'iganet-unittests',
  'cpp',
  version: '25.06.0',
  meson_version: '>=1.3.0',
  default_options: [
    'buildtype=release',
    'cpp_std=c++20',
    'warning_level=1',
    'b_ndebug=if-release',
  ],
  license: 'MPL-2.0',
)

fs = import('fs')
cmake = import('cmake')

# IgANet core: system install, a parent project, or subprojects/iganet.wrap.
# LibTorch is found by the core; pass -Dcmake_prefix_path=<libtorch>.
iganet_dep = dependency('iganet', fallback: ['iganet', 'iganet_dep'])
torch_libdir = iganet_dep.get_variable(
  internal: 'torch_libdir',
  pkgconfig: 'torch_libdir',
  default_value: '',
)

# GoogleTest from WrapDB.
gtest_main_dep = dependency('gtest_main', fallback: ['gtest', 'gtest_main_dep'])

# BSplineLib has no Meson build; the wrap fetches the CMake project and Meson
# configures it through its CMake backend. The shared library matches the
# CMake build of the unit tests.
bsplinelib_options = cmake.subproject_options()
bsplinelib_options.add_cmake_defines({'BSPLINELIB_SHARED': true})
bsplinelib_proj = cmake.subproject('bsplinelib', options: bsplinelib_options)
bsplinelib_dep = bsplinelib_proj.dependency('splines')

unittest_args = ['-DUNITTEST_DISABLE_SHORT_MACROS']

# Data files are found relative to the executable: ../share/iganet/unittests/filedata
_py = find_program('python3', 'python')
run_command(
  _py,
  '-c',
  'import os, sys; d = os.path.join(sys.argv[2], "share", "iganet", "unittests"); os.makedirs(d, exist_ok=True); l = os.path.join(d, "filedata"); (os.path.lexists(l) and os.remove(l)); os.symlink(sys.argv[1], l)',
  meson.project_source_root() / 'filedata',
  meson.project_build_root(),
  check: true,
)
install_subdir(
  'filedata',
  install_dir: get_option('datadir') / 'iganet' / 'unittests',
)

subdir('src')
