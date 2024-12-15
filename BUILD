
load('@bazel_skylib//rules:expand_template.bzl', 'expand_template')
load('@module_versions//:versions.bzl', 'LIBODBCXX_VERSION')

expand_template(
    name = 'config_header',
    out = 'include/config.h',
    substitutions = {
        '@MODULE_VERSION@': LIBODBCXX_VERSION
    },
    template = 'bazel/config.h.linux',
)

expand_template(
    name = 'lib_config_header',
    out = 'include/odbc++/config.h',
    substitutions = {
        '@MODULE_VERSION@': LIBODBCXX_VERSION
    },
    template = 'bazel/odbcxx.config.h.linux',
)

cc_library(
    name = 'libodbcxx',
    visibility = ['//visibility:public'],
    srcs = [
        # headers
        ':config_header',
        'src/dtconv.h',
        'src/driverinfo.h',
        'src/datahandler.h',
        'src/datastream.h',
        # implementations
        'src/databasemetadata.cpp',
        'src/resultsetmetadata.cpp',
        'src/datastream.cpp',
        'src/threads.cpp',
        'src/datetime.cpp',
        'src/connection.cpp',
        'src/errorhandler.cpp',
        'src/preparedstatement.cpp',
        'src/driverinfo.cpp',
        'src/resultset.cpp',
        'src/drivermanager.cpp',
        'src/statement.cpp',
        'src/callablestatement.cpp',
        'src/datahandler.cpp',
    ],
    # NOTE: seems to fix a bug in src/dtconv.h
    copts = ['-DODBCXX_STRING_PERCENT='],
    includes = ['include'],
    hdrs = [
        ':lib_config_header',
        'include/odbc++/types.h',
        'include/odbc++/resultsetmetadata.h',
        'include/odbc++/errorhandler.h',
        'include/odbc++/preparedstatement.h',
        'include/odbc++/threads.h',
        'include/odbc++/setup.h',
        'include/odbc++/callablestatement.h',
        'include/odbc++/drivermanager.h',
        'include/odbc++/connection.h',
        'include/odbc++/databasemetadata.h',
        'include/odbc++/statement.h',
        'include/odbc++/resultset.h',
    ],
    deps = [
        '@github_unixodbc//:libodbc',
    ],
    strip_include_prefix = 'include',
)

cc_library(
    name = 'dtconv',
    hdrs = [
        'src/dtconv.h',
    ],
    includes = ['src'],
    strip_include_prefix = 'src',
)

# NOTE: isql++ marked as manual becuase it needs support for
# libreadline to build successfully
cc_binary(
    name = 'isql++',
    srcs = [
        'isql++/isql++.cpp',
        'isql++/isql++.h',
    ],
    copts = ['-DODBCXX_STRING_PERCENT='],
    deps = [
        ':dtconv',
        ':libodbcxx',
    ],
    tags = ['manual'],
)
