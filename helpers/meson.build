devsock = ['-DDEVMON_SOCKET="' + get_option('devmon-socket') + '"']

helpers = [
    ['devclient', ['devclient.cc'], [], [devsock]],
    [
        'devmon',
        ['devmon.cc'],
        [dinitctl_dep, libudev_dep],
        ['-DHAVE_UDEV'] + devsock
    ],
]

foreach helper: helpers
    executable(
        helper[0], helper[1],
        dependencies: helper[2],
        cpp_args: helper[3],
        install: true,
        install_dir: earlydir / 'helpers'
    )
endforeach
