
vmoddir = $(VMOD_DIR)
vmod_LTLIBRARIES = libvmod_leveldb.la

libvmod_leveldb_la_CFLAGS = $(VMOD_INCLUDES)
libvmod_leveldb_la_LDFLAGS = -module -export-dynamic -avoid-version -shared \
	$(LIBS)

libvmod_leveldb_la_SOURCES = \
	vmod_leveldb.c
nodist_libvmod_leveldb_la_SOURCES = \
	vcc_if.c \
	vcc_if.h

dist_man_MANS = vmod_leveldb.3

VTC_LOG_COMPILER = $(VARNISHTEST) -v \
	-Dvarnishd=$(VARNISHD) -Dvmod_topbuild=$(abs_top_builddir)
TEST_EXTENSIONS = .vtc
TESTS = @VMOD_TESTS@

$(libvmod_leveldb_la_OBJECTS): vcc_if.h

vcc_if.c vcc_if.h vmod_leveldb.man.rst: $(VMODTOOL) $(top_srcdir)/src/vmod.vcc
	$(PYTHON) $(VMODTOOL) $(top_srcdir)/src/vmod.vcc

vmod_leveldb.3: vmod_leveldb.man.rst
	$(RST2MAN) $? $@

EXTRA_DIST = vmod.vcc \
	$(TESTS)

CLEANFILES = $(builddir)/vcc_if.c \
	$(builddir)/vcc_if.h \
	$(builddir)/vmod_leveldb.man.rst \
	$(builddir)/vmod_leveldb.rst \
	$(builddir)/$(dist_man_MANS)

if USE_LCOV
check-local:
	$(LCOV) -c -d .libs -o .libs/$(PACKAGE).info
	$(GENHTML) -o lcov .libs/$(PACKAGE).info

clean-local:
	rm -rf lcov
endif
