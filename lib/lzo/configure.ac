#
#  configure.ac -- autoconf configuration for the LZO library
#
#  This file is part of the LZO data compression library.
#
#  Copyright (C) 1996-2005 Markus Franz Xaver Johannes Oberhumer
#  All Rights Reserved.
#
#  The LZO library is free software; you can redistribute it and/or
#  modify it under the terms of the GNU General Public License,
#  version 2, as published by the Free Software Foundation.
#
#  The LZO library is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with the LZO library; see the file COPYING.
#  If not, write to the Free Software Foundation, Inc.,
#  51 Franklin Street, Fifth Floor, Boston, MA 02110-1301, USA.
#
#  Markus F.X.J. Oberhumer
#  <markus@oberhumer.com>
#  http://www.oberhumer.com/opensource/lzo/
#


# /***********************************************************************
# // Init
# ************************************************************************/

AC_COPYRIGHT([Copyright (C) 1996-2005 Markus Franz Xaver Johannes Oberhumer.
All Rights Reserved.
This configure script may be copied, distributed and modified under the
terms of the GNU General Public License v2; see COPYING for more details.])

AC_PREREQ(2.59)
AC_INIT([lzo],[2.02],[markus@oberhumer.com])
AC_MSG_NOTICE([Configuring LZO $PACKAGE_VERSION])
AC_CONFIG_SRCDIR(src/lzo_init.c)
AC_CONFIG_AUX_DIR(autoconf)
AC_PREFIX_DEFAULT(/usr/local)
AC_CANONICAL_BUILD
AC_CANONICAL_HOST
AC_CANONICAL_TARGET
AM_MAINTAINER_MODE

if test -z "$ac_abs_top_srcdir"; then
    _AC_SRCPATHS(.)
fi
if test -r .Conf.settings1; then
    . ./.Conf.settings1
fi

AC_PROG_CC
AC_PROG_CPP
mfx_PROG_CPPFLAGS
AC_C_CONST
mfx_LZO_CHECK_ENDIAN
AC_SYS_LARGEFILE

AM_INIT_AUTOMAKE
AC_CONFIG_HEADERS([config.h:config.hin])

AC_ENABLE_STATIC
AC_DISABLE_SHARED
AC_PROG_LIBTOOL


# /***********************************************************************
# // Checks for assembler
# ************************************************************************/

AC_ARG_ENABLE(asm, AS_HELP_STRING(--disable-asm,disable assembly versions))

asm_arch=
asm_dir=
asm_msg_amd64=no
asm_msg_i386=no

case $host_cpu in
    amd64 | x86_64) asm_arch="amd64"; asm_dir="asm/amd64/src_gas/elf64" ;;
    i?86) asm_arch="i386"; asm_dir="asm/i386/src_gas" ;;
    *) enable_asm=no ;;
esac
LZO_ASM_VPATH=

AC_MSG_CHECKING([whether to build assembly versions])
if test "X$enable_asm" != Xno; then
mfx_compile_S='${CC-cc} -c conftest.S 1>&AS_MESSAGE_LOG_FD'
cat > conftest.S <<EOF
#include "$ac_abs_top_srcdir/$asm_dir/lzo1x_f1.S"
EOF
enable_asm=no
if AC_TRY_EVAL(mfx_compile_S); then
    if AC_TRY_COMMAND([test -s conftest.$ac_objext]); then
        enable_asm=yes
        eval asm_msg_$asm_arch="'yes [[$asm_dir]]'"
        LZO_ASM_VPATH=":$ac_abs_top_srcdir/$asm_dir"
    fi
fi
if test "X$enable_asm" = Xno; then
    echo "configure: failed program was:" >&AS_MESSAGE_LOG_FD
    cat conftest.S >&AS_MESSAGE_LOG_FD
fi
rm -rf conftest*
fi
AC_MSG_RESULT([$enable_asm])

AM_CONDITIONAL(LZO_USE_ASM, [test "X$enable_asm" != Xno])
AC_SUBST(LZO_ASM_VPATH)


# /***********************************************************************
# // Checks for header files
# ************************************************************************/

mfx_ACC_CHECK_HEADERS
dnl AC_CHECK_HEADERS([sys/param.h sys/resource.h sys/times.h])
if test "X$ac_cv_header_limits_h" != Xyes; then
    AC_MSG_ERROR([<limits.h> header not found])
fi
mfx_CHECK_HEADER_SANE_LIMITS_H
if test "X$mfx_cv_header_sane_limits_h" != Xyes; then
    AC_MSG_ERROR([your <limits.h> header is broken - for details see config.log])
fi


# /***********************************************************************
# // Checks for typedefs and structures
# ************************************************************************/

AC_TYPE_OFF_T
AC_CHECK_TYPE(ptrdiff_t,long)
AC_TYPE_SIZE_T
AC_TYPE_SIGNAL

mfx_ACC_CHECK_SIZEOF
mfx_CHECK_SIZEOF


# /***********************************************************************
# // Checks for library functions
# ************************************************************************/

mfx_ACC_CHECK_FUNCS
mfx_CHECK_LIB_WINMM


# /***********************************************************************
# // Write output files
# ************************************************************************/

mfx_MINIACC_ACCCHK(["-I$srcdir"],["src/miniacc.h"])

if test -r .Conf.settings2; then
    . ./.Conf.settings2
fi

test "X$CPPFLAGS" != "X" && CPPFLAGS="$CPPFLAGS "
CPPFLAGS="${CPPFLAGS}-DLZO_HAVE_CONFIG_H"
if test "X$enable_asm" != Xno; then
    CPPFLAGS="$CPPFLAGS -DLZO_USE_ASM"
fi
AC_SUBST(LZO_CPPFLAGS)
AC_SUBST(LZO_EXTRA_CPPFLAGS)
AC_SUBST(LZO_CFLAGS)
AC_SUBST(LZO_EXTRA_CFLAGS)

configure_CPPFLAGS=$CPPFLAGS
configure_CFLAGS=$CFLAGS
AC_SUBST(configure_CPPFLAGS)
AC_SUBST(configure_CFLAGS)

AC_CONFIG_FILES([Makefile examples/Makefile include/Makefile include/lzo/Makefile lzotest/Makefile minilzo/Makefile src/Makefile tests/Makefile])
AC_OUTPUT


##   enable AMD64 assembly code : ${asm_msg_amd64}
cat <<EOF

   LZO configuration summary
   -------------------------
   LZO version                : ${PACKAGE_VERSION}
   configured for host        : ${host_cpu}-${host_vendor}-${host_os}
   source code location       : ${srcdir}
   compiler                   : ${CC}
   preprocessor flags         : ${CPPFLAGS}
   compiler flags             : ${CFLAGS}
   build static library       : ${enable_static}
   build shared library       : ${enable_shared}
   enable i386 assembly code  : ${asm_msg_i386}


   LZO ${PACKAGE_VERSION} configured.

   Copyright (C) 1996-2005 Markus Franz Xaver Johannes Oberhumer
   All Rights Reserved.

   The LZO library is free software; you can redistribute it and/or
   modify it under the terms of the GNU General Public License,
   version 2, as published by the Free Software Foundation.

   The LZO library is distributed in the hope that it will be useful,
   but WITHOUT ANY WARRANTY; without even the implied warranty of
   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
   GNU General Public License for more details.

   Markus F.X.J. Oberhumer
   <markus@oberhumer.com>
   http://www.oberhumer.com/opensource/lzo/


Type \`make' to build LZO. Type \`make install' to install LZO.
After installing LZO, please read the accompanied documentation.

EOF

# vi:ts=4:et
