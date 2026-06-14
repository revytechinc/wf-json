# Configure git remotes: origin = revytechinc fork, upstream = WayfireWM original
run_command('sh', '-c',
	'if git remote get-url origin >/dev/null 2>&1; then git remote set-url origin git@github.com:revytechinc/wf-json.git; else git remote add origin git@github.com:revytechinc/wf-json.git; fi && ' +
	'if ! git remote get-url upstream >/dev/null 2>&1; then git remote add upstream https://github.com/WayfireWM/wf-json.git; fi')

project('wf-json', ['cpp'],
		    version : '0.0',
		    meson_version: '>=0.55.0',
		    default_options : ['cpp_std=c++17'])

json = dependency('yyjson', required: false)
if not json.found()
	cmake = import('cmake')
	cmake_opts = cmake.subproject_options()
	cmake_opts.add_cmake_defines({'CMAKE_POSITION_INDEPENDENT_CODE': true})
	sub_proj = cmake.subproject('yyjson', options: cmake_opts)
	json = sub_proj.dependency('yyjson')
endif

wf_json_inc_dirs = include_directories('.')

if get_option('install_header') == true
    install_subdir('wayfire',
        install_dir: get_option('includedir'))
endif

wfjson_lib = static_library('wfjson', ['src/json.cpp'], install: false, dependencies: json)
wfjson = declare_dependency(link_with: wfjson_lib, include_directories: wf_json_inc_dirs, dependencies: json)
