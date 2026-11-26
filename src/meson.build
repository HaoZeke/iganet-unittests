unittest_conf = configuration_data()
unittest_conf.set('IGANET_UNITTESTS_COEFF_TYPE', get_option('coeff_type'))
configure_file(
  input: 'unittest_config.hpp.in',
  output: 'unittest_config.hpp',
  configuration: unittest_conf,
)

unittest_inc = include_directories('.')

test_env = environment()
if torch_libdir != ''
  test_env.prepend('LD_LIBRARY_PATH', torch_libdir)
endif

# One executable per source file. The sources listed second compare against
# BSplineLib reference values.
unittests = [
  'unittest_activation',
  'unittest_blocktensor',
  'unittest_boundary',
  'unittest_bspline_conversion',
  'unittest_collocation',
  'unittest_container',
  'unittest_core',
  'unittest_creator',
  'unittest_fqn',
  'unittest_functionspace',
  'unittest_getenv',
  'unittest_igabase',
  'unittest_iganet',
  'unittest_index_sequence',
  'unittest_integer_pow',
  'unittest_linalg',
  'unittest_matrix',
  'unittest_memory',
  'unittest_multipatch',
  'unittest_nonuniform_bspline',
  'unittest_nurbs',
  'unittest_optimizer',
  'unittest_options',
  'unittest_serialize',
  'unittest_solver',
  'unittest_sysinfo',
  'unittest_tensorarray',
  'unittest_tuple',
  'unittest_uniform_bspline',
  'unittest_uuid',
  'unittest_vslice',
  'unittest_zip',
]

bsplinelib_unittests = [
  'unittest_nonuniform_bspline_eval1',
  'unittest_nonuniform_bspline_eval2',
  'unittest_nonuniform_bspline_eval3',
  'unittest_nonuniform_bspline_eval4',
  'unittest_uniform_bspline_eval1',
  'unittest_uniform_bspline_eval2',
  'unittest_uniform_bspline_eval3',
  'unittest_uniform_bspline_eval4',
]

foreach name : unittests
  exe = executable(
    name,
    name + '.cxx',
    include_directories: unittest_inc,
    dependencies: [iganet_dep, gtest_main_dep],
    cpp_args: unittest_args,
    install: true,
  )
  test(
    name,
    exe,
    suite: ['unittests'],
    protocol: 'gtest',
    env: test_env,
    timeout: 300,
  )
endforeach

foreach name : bsplinelib_unittests
  exe = executable(
    name,
    name + '.cxx',
    include_directories: unittest_inc,
    dependencies: [iganet_dep, gtest_main_dep, bsplinelib_dep],
    cpp_args: unittest_args,
    install: true,
  )
  test(
    name,
    exe,
    suite: ['unittests', 'bsplinelib'],
    protocol: 'gtest',
    env: test_env,
    timeout: 900,
  )
endforeach
