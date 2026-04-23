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

# Run clang-format on all API headers
files_to_format = []
find_sources_py = meson.project_source_root() / 'utils' / 'find_sources.py'
python = import('python').find_installation('python3')
files = run_command([python, find_sources_py, 'api', '.h'], check: true).stdout().split()
if files != ['']
  foreach file : files
    files_to_format += ['@SOURCE_ROOT@/' + file]
  endforeach
endif

clang_format_ok_versions = ['>=18.0.0', '<19.0']
clang_format_dep = find_program('clang-format-18', 'clang-format',
                                required: false,
                                version: clang_format_ok_versions)
if clang_format_dep.found()
    run_target('format', command: [clang_format_dep, '-style=file', '--verbose', '-i'] + files_to_format)
else
    run_target('format', command: [python, '-c', 'import sys; print("`format` is disabled because clang-format %r does not appear to be installed. Consider using the dockerfile to format your code." % sys.argv[1:])'] + clang_format_ok_versions)
endif
