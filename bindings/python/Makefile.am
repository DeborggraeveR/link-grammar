#
# Makefile for generating python bindings
#
# Created by Linas Vepstas February 2014
#

SWIG_SOURCES = ../swig/link_grammar.i
SWIG_INCLUDES = ../../link-grammar/link-includes.h
built_c_sources = lg_python_wrap.cc
built_py_sources = $(top_builddir)/bindings/python/clinkgrammar.py

# Over-ride the install location, so as to remove the dash in the
# directory "link-grammar".  Turns out python cannot tolerate dashes.
# Thus, typically, it will install into
# /usr/local/lib/python2.7/dist-packages/linkgrammar (without the dash)
pkgpythondir=$(python2dir)/linkgrammar
pkgpyexecdir=$(python2dir)/linkgrammar

# Files that get installed in $pkgpythondir
pkgpython_PYTHON =                                 \
   linkgrammar.py                                  \
   $(top_builddir)/bindings/python/__init__.py     \
   $(top_builddir)/bindings/python/clinkgrammar.py

# The make uninstall target should remove directories we created.
uninstall-hook:
	-rmdir $(pkgpythondir)

# These are packaged in the tarball; make clean should not remove them.
maintainer-clean-local:
	-rm -f $(built_c_sources)
	-rm -f $(built_py_sources)

# Don't remove __init__.py; it is built by configure!
DISTCLEANFILES =                                   \
   $(top_builddir)/bindings/python/__init__.py

if HAVE_SWIG
# Swig builds these ....
$(built_c_sources) $(built_py_sources): $(SWIG_INCLUDES)
$(built_c_sources) $(built_py_sources): $(SWIG_SOURCES)
	$(SWIG) -python -module clinkgrammar -I$(top_srcdir)/link-grammar -o $@ $<
else
$(built_c_sources) $(built_py_sources):
	touch $(built_c_sources) $(built_py_sources)
endif

# The la MUST have the same name as the pm,
# which MUST be the swig -module name!
# We're calling this clinkgrammar, since its a direct map of the C
# interfaces.
pkgpyexec_LTLIBRARIES = _clinkgrammar.la

_clinkgrammar_la_SOURCES = $(built_c_sources) $(SWIG_SOURCES)

# $(top_builddir) to pick up autogen'ed link-grammar/link-features.h
_clinkgrammar_la_CPPFLAGS =       \
   $(SWIG_PYTHON_CPPFLAGS)        \
   $(PYTHON2_CPPFLAGS)            \
   -I$(top_srcdir)                \
   -I$(top_builddir)

# On Cygwin, a DLL with version is named name-major.dll with no symlink to
# it of an unversioned name (at least up and including libtool 2.4.6).
# This is bad for Python modules, as they must have an identifier name.
if OS_CYGWIN
AVOID_VERSION = -avoid-version
endif
_clinkgrammar_la_LDFLAGS =                        \
    -version-info @VERSION_INFO@ $(AVOID_VERSION) \
    $(PYTHON2_LDFLAGS) -module -no-undefined
_clinkgrammar_la_LIBADD =                         \
    $(top_builddir)/link-grammar/liblink-grammar.la $(PYTHON2_LIBS)


EXTRA_DIST =         \
   AUTHORS           \
   LICENSE           \
   README.md         \
   __init__.py.in    \
   linkgrammar.py
