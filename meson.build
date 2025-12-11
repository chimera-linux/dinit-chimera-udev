project(
    'dinit-chimera',
    ['cpp'],
    version: '0.1.0',
    default_options: [
        'prefix=/usr',
        'cpp_std=c++17',
        'cpp_eh=none', 'cpp_rtti=false',
        'warning_level=3',
        'buildtype=debugoptimized',
    ],
    license: 'BSD-2-Clause',
)

pfx = get_option('prefix')
sbindir = get_option('sbindir')
dlibdir = get_option('libdir') / 'dinit'
tmpfdir = get_option('libdir') / 'tmpfiles.d'
srvdir = get_option('libdir') / 'dinit.d'
earlydir = srvdir / 'early'

cpp = meson.get_compiler('cpp')

libudev_dep = dependency('libudev', required: true)
dinitctl_dep = cpp.find_library('dinitctl', required: true)

subdir('helpers')
