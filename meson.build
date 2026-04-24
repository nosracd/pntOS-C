project('pntos', 'c',
  default_options: ['c_std=gnu11',
                    'warning_level=3',
                    'buildtype=debug',
                    'backend_max_links=4',
                    'b_ndebug=if-release',
                    'c_args=-Wformat -Wformat-security -Werror=format-security'],
  version : '2.1.0-0.dev',
  meson_version: '>=1.8.4')

if get_option('default_library') == 'static'
  add_project_arguments('-DPNTOS_STATIC_BUILD', language: 'c')
endif

subdir('docs')

aspn_c_dep = dependency('aspn23', required: false)
if not aspn_c_dep.found()
  aspn_generated = subproject('aspn-generated',
                                required: true,
                                default_options: ['aspn-cpp=disabled',
                                                  'dds-generated=disabled',
                                                  'lcm-generated=disabled'])
  aspn_c_dep = dependency('aspn23', required: true)
endif

subdir('api')
