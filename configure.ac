#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.71])
AC_INIT([lib_ifxos],[1.7.1],[www.maxlinear.com])
AC_CONFIG_SRCDIR([config.h.in])
AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE([-Wno-portability tar-pax])
AC_CONFIG_MACRO_DIR([m4])

# Checks for programs.
AC_PROG_CC
AM_PROG_CC_C_O
LT_INIT

# save the configure arguments
CONFIGURE_OPTIONS="$ac_configure_args"
AC_SUBST(CONFIGURE_OPTIONS)

dnl enable linux kernel 2.6.x support
AM_CONDITIONAL(KERNEL_2_6, false)
AC_ARG_ENABLE(linux-26,
	AS_HELP_STRING(--enable-linux-26,Enable support for linux kernel 2.6.x),
	[
		AM_CONDITIONAL(KERNEL_2_6, true)
		if test -z "$ARCH" ; then
			[ARCH=`$CC -dumpmachine | sed -e s'/-.*//' \
			-e 's/i[3-9]86/i386/' \
			-e 's/mipsel/mips/' \
			-e 's/powerpc/ppc/' \
			-e 's/sh[234]/sh/' \
			`]
		fi
		if test -n "$ARCH" ; then
			echo "Set the kernel architecture to $ARCH"
			AC_SUBST([KERNEL_ARCH],[$ARCH])
		else
			AC_MSG_ERROR([Kernel architecture not set!])
		fi
	]
)

dnl Set kernel include path (Linux, eCos, ...)
DEFAULT_KERNEL_INCL_PATH='.'
AC_ARG_ENABLE(kernelincl,
	AS_HELP_STRING([--enable-kernelincl=x],[Set the target kernel include path
	]),
	[
	if test -n $enableval; then
		echo Set the kernel include path $enableval
		AC_SUBST([KERNEL_INCL_PATH],[$enableval])
	else
		echo Set the default kernel include path $DEFAULT_KERNEL_INCL_PATH
		AC_SUBST([KERNEL_INCL_PATH],[$DEFAULT_KERNEL_INCL_PATH])
	fi
	],
	[
		echo Set the default kernel include path $DEFAULT_KERNEL_INCL_PATH
		AC_SUBST([KERNEL_INCL_PATH],[$DEFAULT_KERNEL_INCL_PATH])
	]
)

dnl Set kernel build path
AC_ARG_ENABLE(kernelbuild,
	AS_HELP_STRING(--enable-kernelbuild=x,Set the target kernel build path (only for kernel 2.6.x)),
	[
		if test -e $enableval/include/linux/autoconf.h -o -e $enableval/include/generated/autoconf.h; then
			AC_SUBST([KERNEL_BUILD_PATH],[$enableval])
		else
			AC_MSG_ERROR([The kernel build directory is not valid or not configured!])
		fi
	],
	[
		if test -z $KERNEL_BUILD_PATH; then
			# assume kernel was build in source dir...
			AC_SUBST([KERNEL_BUILD_PATH],[$KERNEL_INCL_PATH/..])
		fi
	]
)

dnl Build of the Kernel modules under LINUX
AM_CONDITIONAL(WITH_KERNEL_MODULE, true)
AC_ARG_WITH(kernel-module,
	AS_HELP_STRING(
		[--with-kernel-module],
		[Build with or without Kernel Module]
	),
	[
		echo Build with or without Kernel Module = $withval

		case $withval in
		0|no)
			AM_CONDITIONAL(WITH_KERNEL_MODULE, false)
			AC_SUBST([WITH_KERNEL_MODULE],[no])
			;;
		1|yes)
			AM_CONDITIONAL(WITH_KERNEL_MODULE, true)
			AC_SUBST([WITH_KERNEL_MODULE],[yes])
			;;
		*) AC_MSG_ERROR([ With Kernel Module, use with or without ]);
			;;
		esac
	],
	[
		AM_CONDITIONAL(WITH_KERNEL_MODULE, true)
		AC_SUBST([WITH_KERNEL_MODULE],[yes])
	]
)

dnl Set target driver cflags
AM_CONDITIONAL(ADD_DRV_CFLAGS_SET, false)
AC_ARG_ENABLE(add_drv_cflags,
	AS_HELP_STRING([--enable-add_drv_cflags=x],[Set add driver cflags]),
	[
		if test -n "$enableval"; then
			echo Set add drv cflags = $enableval
			AM_CONDITIONAL(ADD_DRV_CFLAGS_SET, true)
			AC_SUBST([ADD_DRV_CFLAGS],[$enableval])
		fi
	]
)

dnl Set target application cflags
AM_CONDITIONAL(ADD_APPL_CFLAGS_SET, false)
AC_ARG_ENABLE(add_appl_cflags,
	AS_HELP_STRING([--enable-add_appl_cflags=x],[Set add appl cflags]),
	[
		if test -n "$enableval"; then
			echo Set add appl compile flags = $enableval
			AM_CONDITIONAL(ADD_APPL_CFLAGS_SET, true)
			AC_SUBST([ADD_APPL_CFLAGS],[$enableval])
		fi
	]
)

dnl Set the target library include path
if test -z "$TARGET_INCLUDE"; then
	DEFAULT_TARGET_INCL_PATH='.'
else
	DEFAULT_TARGET_INCL_PATH=$TARGET_INCLUDE
fi
AC_ARG_ENABLE(targetincl,
	AS_HELP_STRING([--enable-targetincl=x],[Set the target image include path
	]),
	[
	if test -n $enableval; then
		echo Set the target image include path $enableval
		AC_SUBST([TARGET_INCL_PATH],[$enableval])
	else
		echo Set the default target image include path $DEFAULT_TARGET_INCL_PATH
		AC_SUBST([TARGET_INCL_PATH],[$DEFAULT_TARGET_INCL_PATH])
	fi
	],
	[
		echo Set the default target image include path $DEFAULT_TARGET_INCL_PATH
		AC_SUBST([TARGET_INCL_PATH],[$DEFAULT_TARGET_INCL_PATH])
	]
)

AM_CONDITIONAL(HAVE_GCC, true)
AM_CONDITIONAL(ENABLE_ECOS, false)
AM_CONDITIONAL(ENABLE_NUCLEUS, false)
AM_CONDITIONAL(ENABLE_LINUX, true)
AM_CONDITIONAL(ENABLE_SUN_OS, false)
AM_CONDITIONAL(ENABLE_RTEMS, false)
AM_CONDITIONAL(ENABLE_GENERIC, false)
AM_CONDITIONAL(ENABLE_LINT, false)
AM_CONDITIONAL(HAVE_IFXOS_SYSOBJ_SUPPORT, false)
AM_CONDITIONAL(ENABLE_DEBUG, false)

dnl Enable debugging mode
AC_ARG_ENABLE(debug,
	AS_HELP_STRING(
		[--enable-debug],
		[Build in debugging symbols and enable debugging messages]
	),
	[
		if test "$enableval" = yes; then
			AM_CONDITIONAL(ENABLE_DEBUG, true)
			AC_SUBST([ENABLE_DEBUG],[yes])
		else
			AM_CONDITIONAL(ENABLE_DEBUG, false)
			AC_SUBST([ENABLE_DEBUG],[no])
		fi
	],
	[
		AM_CONDITIONAL(ENABLE_DEBUG, false)
		AC_SUBST([ENABLE_DEBUG],[no])
	]
)

dnl Enable the cc compiler
AM_CONDITIONAL(HAVE_GCC, true)
AC_ARG_ENABLE(have-cc,
	AS_HELP_STRING(
		[--enable-have-cc],
		[Use the cc compiler]
	),
	[
		if test "$enableval" = yes; then
			echo Use the cc compiler
			AM_CONDITIONAL(HAVE_GCC, false)
			AC_SUBST([HAVE_GCC],[no])
		else
			echo Use the gcc compiler
			AM_CONDITIONAL(HAVE_GCC, true)
			AC_SUBST([HAVE_GCC],[yes])
		fi
	],
	[
		AM_CONDITIONAL(HAVE_GCC, true)
		AC_SUBST([HAVE_GCC],[yes])
	]
)

dnl Enable the Sun OS operating system
AC_ARG_ENABLE(sunos,
	AS_HELP_STRING(
		[--enable-sunos],
		[Enable Sun OS]
	),
	[
		if test "$enableval" = yes; then
			echo Enable Sun OS operating system
			AM_CONDITIONAL(ENABLE_LINUX, false)
			AM_CONDITIONAL(ENABLE_SUN_OS, true)
			AM_CONDITIONAL(ENABLE_ECOS, false)
			AM_CONDITIONAL(ENABLE_GENERIC, false)
		fi
	]
)

dnl Enable the eCos operating system
AC_ARG_ENABLE(ecos,
	AS_HELP_STRING(
		[--enable-ecos],
		[Enable eCos]
	),
	[
		if test "$enableval" = yes; then
			echo Enable eCos operating system
			AM_CONDITIONAL(ENABLE_ECOS, true)
			AM_CONDITIONAL(ENABLE_LINUX, false)
			AM_CONDITIONAL(ENABLE_SUN_OS, false)
			AM_CONDITIONAL(ENABLE_GENERIC, false)
		fi
	]
)

dnl Enable the Nucleus operating system
AC_ARG_ENABLE(nucleus,
	AS_HELP_STRING(
		[--enable-nucleus],
		[Enable Nucleus]
	),
	[
		if test "$enableval" = yes; then
			echo Enable Nucleus operating system
			AM_CONDITIONAL(ENABLE_NUCLEUS, true)
			AM_CONDITIONAL(ENABLE_LINUX, false)
			AM_CONDITIONAL(ENABLE_SUN_OS, false)
			AM_CONDITIONAL(ENABLE_GENERIC, false)
		fi
	]
)

dnl Enable the generic operating system
AC_ARG_ENABLE(generic,
	AS_HELP_STRING(
		[--enable-generic],
		[Enable generic OS]
	),
	[
		if test "$enableval" = yes; then
			echo Enable generic operating system
			AM_CONDITIONAL(ENABLE_ECOS, false)
			AM_CONDITIONAL(ENABLE_LINUX, false)
			AM_CONDITIONAL(ENABLE_SUN_OS, false)
			AM_CONDITIONAL(ENABLE_GENERIC, true)
		fi
	]
)

AC_ARG_ENABLE(lint,
	AS_HELP_STRING(
		[--enable-lint],[enable automatic checks with (Flexe)Lint.]
	),
	[
		if test "$enableval" = yes; then
			AM_CONDITIONAL(ENABLE_LINT, true)
			AC_SUBST([ENABLE_LINT],[yes])
		else
			AM_CONDITIONAL(ENABLE_LINT, false)
			AC_SUBST([ENABLE_LINT],[no])
		fi
	],
	[
		AM_CONDITIONAL(ENABLE_LINT, false)
		AC_SUBST([ENABLE_LINT],[no])
	]
)

dnl enable IFXOS System Object support
AC_ARG_ENABLE(ifxos-sys-obj-support,
	AS_HELP_STRING(
		[--enable-ifxos-sys-obj-support],[Support IFXOS System Objects.]
	),
	[
		if test "$enableval" = yes; then
			AM_CONDITIONAL(HAVE_IFXOS_SYSOBJ_SUPPORT, true)
			AC_SUBST([HAVE_IFXOS_SYSOBJ_SUPPORT],[yes])
		else
			AM_CONDITIONAL(HAVE_IFXOS_SYSOBJ_SUPPORT, false)
			AC_SUBST([HAVE_IFXOS_SYSOBJ_SUPPORT],[no])
		fi
	],
	[
		AM_CONDITIONAL(HAVE_IFXOS_SYSOBJ_SUPPORT, false)
		AC_SUBST([HAVE_IFXOS_SYSOBJ_SUPPORT],[no])
	]
)

dnl enable warnings as error
AC_ARG_ENABLE(werror,
	AS_HELP_STRING([--enable-werror],[enable warnings as error
	]),
	[
		if test $enableval = 'yes'; then
				AM_CONDITIONAL(WERROR, true)
				CFLAGS="$CFLAGS -Werror"
		else
				AM_CONDITIONAL(WERROR, false)
		fi
	],
	[
		AM_CONDITIONAL(WERROR, false)
	]
)

AM_CONDITIONAL(IFXOS_HAVE_IPV6_DRV, false)
AC_ARG_ENABLE(ipv6-drv,
	AS_HELP_STRING(
		[--enable-ipv6-drv],
		[enable IFXOS ipv6 support for kernel modules]
	),
	[
		case $enableval in
		0|no|NO)
			AM_CONDITIONAL(IFXOS_HAVE_IPV6_DRV, false)
			;;
		1|yes|YES)
			AM_CONDITIONAL(IFXOS_HAVE_IPV6_DRV, true)
			;;
		*)
			AM_CONDITIONAL(IFXOS_HAVE_IPV6_DRV, false)
			;;
		esac
	],
	[
		AM_CONDITIONAL(IFXOS_HAVE_IPV6_DRV, false)
	]
)

AM_CONDITIONAL(IFXOS_HAVE_IPV6_APPL, false)
AC_ARG_ENABLE(ipv6-appl,
	AS_HELP_STRING(
		[--enable-ipv6-appl],
		[enable IFXOS ipv6 support for applications]
	),
	[
		case $enableval in
		0|no|NO)
			AM_CONDITIONAL(IFXOS_HAVE_IPV6_APPL, false)
			;;
		1|yes|YES)
			AM_CONDITIONAL(IFXOS_HAVE_IPV6_APPL, true)
			;;
		*)
			AM_CONDITIONAL(IFXOS_HAVE_IPV6_APPL, false)
			;;
		esac
	],
	[
		AM_CONDITIONAL(IFXOS_HAVE_IPV6_APPL, false)
	]
)

AC_SUBST([DISTCHECK_CONFIGURE_PARAMS],[$CONFIGURE_OPTIONS])

echo \
"------------------------------------------------------------------------
Configuration:

  Source code location: ${srcdir}
  Compiler:             ${CC}
  Compiler flags:       ${CFLAGS}
  Host System Type:     ${host}
  Install path:         ${prefix}
  IFXOS Object Support: $HAVE_IFXOS_SYSOBJ_SUPPORT
  With Kernel Module:   $WITH_KERNEL_MODULE

Settings:
  Configure options:    ${CONFIGURE_OPTIONS}
------------------------------------------------------------------------"

AC_CONFIG_FILES([Makefile src/Makefile])
AC_OUTPUT

