dnl Process this file with autoconf to produce a configure script.

AC_PREREQ([2.64])

m4_define(gitg_major_version, 3)
m4_define(gitg_minor_version, 14)
m4_define(gitg_micro_version, 0)
m4_define(gitg_version, gitg_major_version.gitg_minor_version.gitg_micro_version)

AC_INIT([gitg],
        [gitg_version],
        [https://bugzilla.gnome.org/enter_bug.cgi?product=gitg],
        [gitg],
        [http://wiki.gnome.org/Apps/Gitg])

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_SRCDIR([gitg/gitg.vala])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR([build-aux])

AC_DEFINE(GITG_MAJOR_VERSION, gitg_major_version, [GitG major version])
AC_SUBST(GITG_MAJOR_VERSION, gitg_major_version)
AC_DEFINE(GITG_MINOR_VERSION, gitg_minor_version, [GitG minor version])
AC_SUBST(GITG_MINOR_VERSION, gitg_minor_version)
AC_DEFINE(GITG_MICRO_VERSION, gitg_micro_version, [GitG micro version])
AC_SUBST(GITG_MICRO_VERSION, gitg_micro_version)

AM_INIT_AUTOMAKE([1.11 tar-ustar dist-xz no-dist-gzip -Wno-portability subdir-objects])
AM_MAINTAINER_MODE([enable])

AM_SILENT_RULES([yes])

# Check for programs
AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_MAKE_SET
AC_PATH_PROG(GZIP, gzip)
AC_PROG_MKDIR_P
AM_PROG_VALAC([0.20.1.23])

AC_PATH_PROG(GLIB_MKENUMS, glib-mkenums)
AC_PATH_PROG(GLIB_COMPILE_RESOURCES, glib-compile-resources)
AC_PATH_PROG(GTK_UPDATE_ICON_CACHE, gtk-update-icon-cache)

AC_PATH_PROG(VALADOC, valadoc)

# Initialize libtool
LT_PREREQ([2.2])
LT_INIT(disable-static)

# i18n stuff
IT_PROG_INTLTOOL([0.40.0])

AM_GNU_GETTEXT_VERSION([0.17])
AM_GNU_GETTEXT([external])

GETTEXT_PACKAGE=gitg

dnl adl_RECURSIVE_EVAL(VALUE, RESULT)
dnl =================================
dnl Interpolate the VALUE in loop until it doesn't change,
dnl and set the result to $RESULT.
dnl WARNING: It's easy to get an infinite loop with some unsane input.
AC_DEFUN([adl_RECURSIVE_EVAL],
[_lcl_receval="$1"
$2=`(test "x$prefix" = xNONE && prefix="$ac_default_prefix"
     test "x$exec_prefix" = xNONE && exec_prefix="${prefix}"
     _lcl_receval_old=''
     while test "[$]_lcl_receval_old" != "[$]_lcl_receval"; do
       _lcl_receval_old="[$]_lcl_receval"
       eval _lcl_receval="\"[$]_lcl_receval\""
     done
     echo "[$]_lcl_receval")`])

adl_RECURSIVE_EVAL("$datadir/gitg", [GITG_DATADIR])
adl_RECURSIVE_EVAL("$datadir/locale", [GITG_LOCALEDIR])
adl_RECURSIVE_EVAL("$libdir/gitg", [GITG_LIBDIR])

AC_SUBST(GETTEXT_PACKAGE)

AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE,"$GETTEXT_PACKAGE", [Gettext package])
AC_DEFINE_UNQUOTED(GITG_DATADIR,"$GITG_DATADIR", [data dir])
AC_DEFINE_UNQUOTED(GITG_LOCALEDIR,"$GITG_LOCALEDIR", [locale dir])
AC_DEFINE_UNQUOTED(GITG_LIBDIR,"$GITG_LIBDIR", [lib dir])

AC_CHECK_LIB([m], [sinf])

# Pick from supported API versions of WebKit2
PKG_CHECK_EXISTS([webkit2gtk-4.0], [
	WEBKIT_PKGCONFIG=webkit2gtk-4.0
	AC_SUBST([WEBKIT_PKGCONFIG])
], [
	WEBKIT_PKGCONFIG=webkit2gtk-3.0
	AC_SUBST([WEBKIT_PKGCONFIG])
])

GLIB_REQUIRED_VERSION=2.38
GTK_REQUIRED_VERSION=3.12.0
GTKSOURCEVIEW_REQUIRED_VERSION=3.10
INTROSPECTION_REQUIRED=0.10.1
WEBKITGTK_REQUIRED_VERSION=2.2
LIBGIT2_GLIB_REQUIRED_VERSION=0.0.25

PKG_CHECK_MODULES(LIBGITG, [
	gthread-2.0 >= $GLIB_REQUIRED_VERSION
	glib-2.0 >= $GLIB_REQUIRED_VERSION
	gobject-2.0 >= $GLIB_REQUIRED_VERSION
	gmodule-2.0 >= $GLIB_REQUIRED_VERSION
	gio-2.0 >= $GLIB_REQUIRED_VERSION
	gio-unix-2.0 >= $GLIB_REQUIRED_VERSION
	gobject-introspection-1.0 >= $INTROSPECTION_REQUIRED
	libgit2-glib-1.0 >= $LIBGIT2_GLIB_REQUIRED_VERSION
	gtk+-3.0 >= $GTK_REQUIRED_VERSION
	$WEBKIT_PKGCONFIG >= WEBKITGTK_REQUIRED_VERSION
	gsettings-desktop-schemas
	gee-0.8
	json-glib-1.0
	libsecret-1
])

AC_MSG_CHECKING([for libgit2-glib threading support])

cflags_save="${CFLAGS}"
libs_save="${LIBS}"

CFLAGS="${LIBGITG_CFLAGS}"
LIBS="${LIBGITG_LIBS}"

AC_TRY_RUN([
	#include <libgit2-glib/ggit.h>
	int
	main(int argc, const char *argv[])
	{
		ggit_init ();
		return ((ggit_get_features() & GGIT_FEATURE_THREADS) != 0) ? 0 : 1;
	}
],[AC_MSG_RESULT([yes])],[AC_MSG_ERROR([no, please recompile a threadsafe version of libgit2 (-DTHREADSAFE:BOOL=ON)])])

CFLAGS="${cflags_save}"
LIBS="${libs_save}"

AC_ARG_ENABLE(docs,
              [AS_HELP_STRING([--enable-docs],
                              [enable generating documentation [default=no]])],
              [enable_docs=$enableval])

if test "$enable_docs" = "yes"; then
	if test "$VALADOC" = ""; then
		AC_MSG_ERROR([could not find valadoc])
	fi
fi

AM_CONDITIONAL(ENABLE_DOCS, test "$enable_docs" = "yes")

AC_ARG_ENABLE(deprecations,
              [AS_HELP_STRING([--enable-deprecations],
                              [warn about deprecated usages [default=no]])],
              [enable_deprecations=$enableval])

if test "x$USE_MAINTAINER_MODE" = "xyes"; then
LIBGITG_CFLAGS="$LIBGITG_CFLAGS -Wall -Werror -Wuninitialized -Wmissing-declarations"
fi

if test "$enable_deprecations" = "yes"; then
   LIBGITG_CFLAGS="$LIBGITG_CFLAGS \
-DG_DISABLE_DEPRECATED \
-DGDK_DISABLE_DEPRECATED \
-DGTK_DISABLE_DEPRECATED \
-DGDK_PIXBUF_DISABLE_DEPRECATED \
-DGNOME_DISABLE_DEPRECATED \
-DGSEAL_ENABLE"
fi

AC_ARG_ENABLE(debug,
              [AS_HELP_STRING([--disable-debug],
                              [disable debug information [default=yes]])],
              [enable_debug=$enableval], [enable_debug=yes])

GITG_VALAFLAGS="--target-glib 2.38"

if test "x$enable_debug" = "xyes"; then
	LIBGITG_CFLAGS="$LIBGITG_CFLAGS -g -O0"
	GITG_VALAFLAGS="$GITG_VALAFLAGS -g --save-temps"

	AC_DEFINE([ENABLE_DEBUG],[1],[Whether debugging support is enabled])
fi

LIBGITG_CFLAGS="$LIBGITG_CFLAGS -DLIBSOUP_USE_UNSTABLE_REQUEST_API"

AC_SUBST(LIBGITG_CFLAGS)
AC_SUBST(LIBGITG_LIBS)

AC_SUBST(GITG_VALAFLAGS)

LIBGITG_EXT_CFLAGS="$LIBGITG_CFLAGS"
LIBGITG_EXT_LIBS="$LIBGITG_LIBS"

AC_SUBST(LIBGITG_EXT_CFLAGS)
AC_SUBST(LIBGITG_EXT_LIBS)

LIBPEAS_REQUIRED_VERSION=1.5.0
LIBPEAS_GTK_REQUIRED_VERSION=1.5.0
GTKSPELL_REQUIRED_VERSION=3.0.3

PKG_CHECK_MODULES(GITG, [
	gtksourceview-3.0 >= $GTKSOURCEVIEW_REQUIRED_VERSION
	gsettings-desktop-schemas
	libpeas-1.0 >= $LIBPEAS_REQUIRED_VERSION
	libpeas-gtk-1.0 >= $LIBPEAS_GTK_REQUIRED_VERSION
	gtkspell3-3.0 >= $GTKSPELL_REQUIRED_VERSION
])

GITG_CFLAGS="$LIBGITG_CFLAGS $GITG_CFLAGS"
GITG_LIBS="$LIBGITG_LIBS $GITG_LIBS -lm"

AC_SUBST(GITG_CFLAGS)
AC_SUBST(GITG_LIBS)

GOBJECT_INTROSPECTION_REQUIRE($INTROSPECTION_REQUIRED)
AC_SUBST(INTROSPECTION_REQUIRED)

INTROSPECTION_GIRDIR=`$PKG_CONFIG \
--define-variable="prefix=${prefix}" \
--define-variable="datarootdir=${datarootdir}" \
--define-variable="datadir=${datadir}" \
--variable=girdir gobject-introspection-1.0`

INTROSPECTION_TYPELIBDIR=`$PKG_CONFIG \
--define-variable="prefix=${prefix}" \
--define-variable="libdir=${libdir}" \
--variable=typelibdir gobject-introspection-1.0`

GNOME_COMPILE_WARNINGS([maximum])

GLIB_GSETTINGS

GLADE_UI_REQUIRED=3.2

AC_ARG_ENABLE([glade-catalog],
              [AS_HELP_STRING([--enable-glade-catalog[=@<:@no/auto/yes@:>@]],
                              [Install a glade catalog file [default=no]])],
                              [glade_catalog=$enableval],
                              [glade_catalog="no"])

AM_CONDITIONAL([GLADE_CATALOG], [ test "$glade_catalog" = "yes" ])

AS_IF([ test "$glade_catalog" = "yes" ],
	[PKG_CHECK_EXISTS(GLADE, [gladeui-2.0 >= $GLADE_UI_REQUIRED])
	 AC_MSG_CHECKING([for Glade catalog dir])
	 GLADE_CATALOG_DIR=`$PKG_CONFIG --variable=catalogdir "--define-variable=prefix=${prefix}" gladeui-2.0 2>&AS_MESSAGE_LOG_FD`
	 AC_MSG_RESULT([$GLADE_CATALOG_DIR])
	 AC_SUBST(GLADE_CATALOG_DIR)])

GITG_PLUGIN_DATADIR="$datadir/gitg/plugins"
AC_SUBST(GITG_PLUGIN_DATADIR)

GITG_PLUGIN_LIBDIR="$libdir/gitg/plugins"
AC_SUBST(GITG_PLUGIN_LIBDIR)

GITG_PLUGIN_CFLAGS="$GITG_CFLAGS"
GITG_PLUGIN_LIBS="$GITG_LIBS"

AC_SUBST(GITG_PLUGIN_CFLAGS)
AC_SUBST(GITG_PLUGIN_LIBS)

GITG_PLUGIN_LIBTOOL_FLAGS="-module -avoid-version"
AC_SUBST(GITG_PLUGIN_LIBTOOL_FLAGS)

GITG_PLUGIN_VALAFLAGS="				\
	--pkg ggit-1.0 				\
	--pkg gio-2.0				\
	--pkg gtk+-3.0				\
	--pkg gtksourceview-3.0			\
	--pkg libpeas-1.0			\
	--pkg gee-0.8				\
	--pkg json-glib-1.0			\
	--pkg libsoup-2.4			\
	--pkg libsecret-1			\
	--pkg $WEBKIT_PKGCONFIG			\
	--pkg config				\
	--pkg gd-1.0				\
	\$(GITG_VALAFLAGS)			\
	--vapidir \$(top_srcdir)/vapi		\
	--vapidir \$(top_builddir)/libgitg	\
	--vapidir \$(top_builddir)/libgitg-ext	\
	--vapidir \$(top_builddir)/libgd"

AC_SUBST(GITG_PLUGIN_VALAFLAGS)

dnl ===========================================================================
dnl Check for python
dnl ===========================================================================
PYGOBJECT_REQUIRED=3.0.0

AC_ARG_ENABLE([python],
              AS_HELP_STRING([--enable-python[=@<:@no/auto/yes@:>@]],[Build with python support]),
              [enable_python=$enableval],
              [enable_python="auto"])

if test "x$enable_python" = "xauto"; then
	PKG_CHECK_EXISTS([pygobject-3.0 >= $PYGOBJECT_REQUIRED],
	                 [enable_python=yes],[enable_python=no])
fi

if test "x$enable_python" = "xyes"; then
	AM_PATH_PYTHON([3.2.3])

	PKG_CHECK_MODULES(PYTHON, [pygobject-3.0 >= $PYGOBJECT_REQUIRED])

	pyoverridesdir="\$(pyexecdir)/gi/overrides"
	AC_SUBST(pyoverridesdir)
fi

AM_CONDITIONAL(ENABLE_PYTHON, test x"$enable_python" = "xyes")

dnl libgd
LIBGD_INIT([
	tagged-entry
	_view-common
	static
	vapi
])

AC_CONFIG_FILES([
Makefile
libgd/Makefile
libgitg/libgitg-1.0.pc
libgitg-ext/libgitg-ext-1.0.pc
data/gitg.desktop.in
data/org.gnome.gitg.gschema.xml.in
po/Makefile.in
])

AC_OUTPUT

echo "

Configuration:

	Source code location:	${srcdir}
	Compiler:		${CC}
	Glade catalog:          ${glade_catalog}
	Debug enabled:          ${enable_debug}
	Python support:         ${enable_python}
"
