AC_INIT([curl-impersonate], [1.0.0], [infinitesheldon@gmail.com])

AC_CANONICAL_BUILD
AC_CANONICAL_HOST

AC_PROG_CC
AC_PROG_CXX

AC_CHECK_TOOL([STRIP], [strip])

AC_ARG_ENABLE([static],
    [AS_HELP_STRING([--enable-static],
        [Build curl-impersonate statically with libcurl-impersonate])],
    [AC_SUBST([static_build], ["$enableval"])],
    [AC_SUBST([static_build], ["no"])])

# Path to CA certificates.
# These options will be passed as-is to curl's configure script.
# Useful when cross compiling, since curl's configure script doesn't know
# where to look for these files in that case.
AC_ARG_WITH([ca-bundle],
    [AS_HELP_STRING([--with-ca-bundle=FILE],
        [Path to be passed to curl's --with-ca-bundle configure option. \
         Useful when cross compiling. \
         Relevant only for the Chrome build.])],
    [AC_SUBST([with_ca_bundle], ["$withval"])],
    [AC_SUBST([with_ca_bundle], [""])])
AC_ARG_WITH([ca-path],
    [AS_HELP_STRING([--with-ca-path=DIRECTORY],
        [Path to be passed to curl's --with-ca-path configure option. \
         Useful when cross compiling. \
         Relevant only for the chrome build.])],
    [AC_SUBST([with_ca_path], ["$withval"])],
    [AC_SUBST([with_ca_path], [""])])

# BoringSSL requires cmake 3.5+, which is sometimes available under
# "cmake3" instead of "cmake"
AC_CHECK_PROGS([cmake], [cmake3 cmake])

AC_CHECK_PROGS([ninja], [ninja ninja-build])

# Convert autoconf style os name to CMake style os name.
case $host_os in
    linux*)
        system_name=Linux
        ;;
    *android*)
        system_name=Android
        ;;
    ios*)
        system_name=iOS
        ;;
    darwin*)
        system_name=Darwin
        ;;
    *)
        system_name=$host_os
        ;;
esac
AC_SUBST([system_name])

# CMake flags for Apple platforms
cmake_osx_flags=""
case $host_os in
    ios*)
        if test -n "$SDKROOT"; then
            cmake_osx_flags="$cmake_osx_flags -DCMAKE_OSX_SYSROOT=$SDKROOT"
        elif test "$host_cpu" = "x86_64"; then
            cmake_osx_flags="$cmake_osx_flags -DCMAKE_OSX_SYSROOT=iphonesimulator"
        else
            cmake_osx_flags="$cmake_osx_flags -DCMAKE_OSX_SYSROOT=iphoneos"
        fi
        if test -n "$IOS_MIN_VERSION"; then
            cmake_osx_flags="$cmake_osx_flags -DCMAKE_OSX_DEPLOYMENT_TARGET=$IOS_MIN_VERSION"
        fi
        ;;
esac
AC_SUBST([cmake_osx_flags])

AC_CONFIG_FILES([Makefile])

AC_OUTPUT
