#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.64)
AC_INIT(dbarts, 0.9.9, vdorie@gmail.com)
AC_CONFIG_SRCDIR([src/R_interface.cpp])
AC_CONFIG_AUX_DIR([tools/build-aux])
AC_CONFIG_HEADER([src/config.hpp])
AC_CONFIG_HEADER([src/dbarts/config.hpp])
AC_CONFIG_HEADER([src/misc/config.h])
AC_CONFIG_HEADER([src/external/config.h])
AC_CONFIG_HEADER([inst/include/dbarts/types.hpp])
AC_CONFIG_HEADER([src/include/misc/types.h])

m4_include([tools/m4/ax_check_compile_flag.m4])
m4_include([tools/m4/ax_pthread.m4])
#m4_include([tools/m4/ax_gcc_x86_cpuid.m4])
#m4_include([tools/m4/ax_gcc_x86_avx_xgetbv.m4])
#m4_include([tools/m4/ax_ext.m4])
m4_include([tools/m4/ax_cxx_namespace_std.m4])
m4_include([tools/m4/ax_snprintf_in_namespace_std.m4])
m4_include([tools/m4/ax_log1p_in_namespace_std.m4])
m4_include([tools/m4/ax_func_posix_memalign.m4])

AC_ARG_ENABLE([match-bayes-tree],
              [AS_HELP_STRING([--enable-match-bayes-tree],
                              [get results identical to BayesTree])],
              [match_bayes_tree=-DMATCH_BAYES_TREE],
              [unset match_bayes_tree])
AC_SUBST(match_bayes_tree)

AC_ARG_ENABLE([thread-safe-unload],
              [AS_HELP_STRING([--enable-thread-safe-unload],
                              [block when adding/deleting fits for the purposes of package reloading])],
              [thread_safe_unload=-DTHREAD_SAFE_UNLOAD],
              [unset thread_safe_unload])
AC_SUBST(thread_safe_unload)

AC_ARG_WITH([xint-size],
            [AS_HELP_STRING([--with-xint-size=16],
                            [constrains the number of cutpoints to 2^x-size - 1])],
             ,
             [with_xint_size=no])

case "x$with_xint_size" in
  x8)
    xint_size=uint8_t
  ;;
  x32)
    xint_size=uint32_t
  ;;
  x64)
    xint_size=uint64_t
  ;;
  xyes|xno|x16)
    xint_size=uint16_t
  ;;
esac

AC_DEFINE_UNQUOTED(XINT_TYPE, $xint_size, [Define to the integer type used to represent feature map.])

# Checks for programs.
: ${R_HOME=`R RHOME`}
if test -z "${R_HOME}"; then
  echo "could not determine R_HOME"
  exit 1
fi
CC=`"${R_HOME}/bin/R" CMD config CC`
CPP=`"${R_HOME}/bin/R" CMD config CPP`
CXX=`"${R_HOME}/bin/R" CMD config CXX`
CFLAGS=`"${R_HOME}/bin/R" CMD config CFLAGS`
CPICFLAGS=`"${R_HOME}/bin/R" CMD config CPICFLAGS`
CPPFLAGS=`"${R_HOME}/bin/R" CMD config CPPFLAGS`
CXXFLAGS=`"${R_HOME}/bin/R" CMD config CXXFLAGS`
CXXPICFLAGS=`"${R_HOME}/bin/R" CMD config CXXPICFLAGS`

AC_PROG_CC
AC_PROG_CC_C99
AC_PROG_CPP
AC_PROG_CXX
AC_PROG_CXXCPP

AC_LANG(C)
AC_LANG_PUSH(C++)
AC_C_RESTRICT
AC_LANG_POP(C++)

AC_C_BIGENDIAN

#AX_EXT
AX_CHECK_COMPILE_FLAG(-mmmx, ax_cv_support_mmx_ext=yes, [])
if test x"$ax_cv_support_mmx_ext" = x"yes"; then
  SIMD_FLAGS="$SIMD_FLAGS -mmmx"
  AC_DEFINE(HAVE_MMX,,[Support mmx instructions])
fi
AX_CHECK_COMPILE_FLAG(-msse, ax_cv_support_sse_ext=yes, [])
if test x"$ax_cv_support_sse_ext" = x"yes"; then
  SIMD_FLAGS="$SIMD_FLAGS -msse"
fi

AX_CHECK_COMPILE_FLAG(-msse2, ax_cv_support_sse2_ext=yes, [])
if test x"$ax_cv_support_sse2_ext" = x"yes"; then
  SIMD_FLAGS="$SIMD_FLAGS -msse2"
  AC_DEFINE(HAVE_SSE2,,[Support SSE2 (Streaming SIMD Extensions 2) instructions])
fi

AX_CHECK_COMPILE_FLAG(-msse3, ax_cv_support_sse3_ext=yes, [])
if test x"$ax_cv_support_sse3_ext" = x"yes"; then
  SIMD_FLAGS="$SIMD_FLAGS -msse3"
  AC_DEFINE(HAVE_SSE3,,[Support SSE3 (Streaming SIMD Extensions 3) instructions])
fi

AX_CHECK_COMPILE_FLAG(-msse4.1, ax_cv_support_sse41_ext=yes, [])
if test x"$ax_cv_support_sse41_ext" = x"yes"; then
  SIMD_FLAGS="$SIMD_FLAGS -msse4.1"
  AC_DEFINE(HAVE_SSE4_1,,[Support SSSE4.1 (Streaming SIMD Extensions 4.1) instructions])
fi

AX_CHECK_COMPILE_FLAG(-msse4.2, ax_cv_support_sse42_ext=yes, [])
if test x"$ax_cv_support_sse42_ext" = x"yes"; then
  SIMD_FLAGS="$SIMD_FLAGS -msse4.2"
  AC_DEFINE(HAVE_SSE4_2,,[Support SSSE4.2 (Streaming SIMD Extensions 4.2) instructions])
fi

AX_CHECK_COMPILE_FLAG(-mavx, ax_cv_support_avx_ext=yes, [])
if test x"$ax_cv_support_avx_ext" = x"yes"; then
  SIMD_FLAGS="$SIMD_FLAGS -mavx"
  AC_DEFINE(HAVE_AVX,,[Support AVX (Advanced Vector Extensions) instructions])
fi

AX_CHECK_COMPILE_FLAG(-mavx2, ax_cv_support_avx2_ext=yes, [])
if test x"$ax_cv_support_avx2_ext" = x"yes"; then
  SIMD_FLAGS="$SIMD_FLAGS -mavx2"
  AC_DEFINE(HAVE_AVX2,,[Support AVX2 (Advanced Vector Extensions 2) instructions])
fi

AC_SUBST(SIMD_FLAGS)

# Checks for libraries.
AX_PTHREAD
AC_SUBST(PTHREAD_CFLAGS)
AC_SUBST(PTHREAD_LDFLAGS)
AC_SUBST(PTHREAD_LIBS)

# Checks for header files.
AC_LANG_PUSH(C++)
AC_CHECK_HEADERS([cstdint])
AC_LANG_POP(C++)

if test "$ac_cv_header_cstdint" == no
then
  AC_CHECK_HEADER([stdint.h])
fi

AC_CHECK_HEADERS([sys/time.h])
AC_CHECK_HEADERS([unistd.h])
AC_CHECK_HEADERS([malloc.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_INT64_T
AC_TYPE_UINT64_T
AC_CHECK_SIZEOF([size_t])
AC_CHECK_ALIGNOF([void*])

# Checks for library functions.
AC_CHECK_FUNCS([gettimeofday])
AC_CHECK_FUNCS([clock_gettime])
AC_CHECK_FUNCS([ffs])
AC_FUNC_ALLOCA

AX_FUNC_POSIX_MEMALIGN

AC_LANG_PUSH(C++)
AC_CHECK_FUNCS([snprintf])
AX_CXX_SNPRINTF_IN_NAMESPACE_STD
AC_CHECK_FUNCS([log1p])
AX_CXX_LOG1P_IN_NAMESPACE_STD
AC_LANG_POP


AC_CONFIG_FILES([src/Makevars])
AC_OUTPUT
