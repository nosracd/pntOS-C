cc = meson.get_compiler('c')
if not cc.check_header('stdint.h')
    args = ['-DPNTOS_NO_STDINT']
else
    args = []
endif

# array of compiler args that force all warnings to be errors on known compilers
force_werror = cc.get_supported_arguments(
'-Werror', '-Wextra', '-Wall', '-pedantic', '/WX', '/Wall')

stdbool_src = '#include <stdbool.h>\nbool sample_func(void);\n'

if not cc.compiles(stdbool_src, args: force_werror)
    args += ['-DPNTOS_NO_BOOL']
endif

inc_dir = get_option('includedir')

install_subdir('include/pntos',
            install_dir: inc_dir)

install_headers('include/pntos.h')

pntos_api_dep = declare_dependency(
    include_directories : include_directories('include'),
    dependencies : [aspn_c_dep],
    compile_args: args)

pkg = import('pkgconfig')
pkg.generate(filebase: 'pntos',
            name: 'pntOS API',
            description: 'pnt Operating System plugin API',
            version: meson.project_version(),
            subdirs: [''])