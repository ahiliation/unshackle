AUTOMAKE_OPTIONS= dist-shar

#config.h.in and acconfig.h to avoid a bug in automake that prevent them to be included in the
#dist...

EXTRA_DIST=oRTP.prj   ortp-config.h.in	TODO pkg.list autogen.sh ortp.pc.in

SUBDIRS=src docs build include

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = ortp.pc

## oRTP packaging : `make package'

if WITH_EPM

.PHONY: package

PKG_NAME = $(PACKAGE)-$(VERSION)-$(RELEASE)

package: $(srcdir)/pkg.list $(srcdir)/configure
	[ -n "$(BUILDROOT)" ] && rm -rf $(BUILDROOT)/*
	[ -n "$(PKG_NAME).*" ] && rm -f $(PKG_NAME).*
	$(MAKE) install DESTDIR=$(BUILDROOT)
	$(MKEPMLIST) -u $(SYS_USER) -g $(SYS_GROUP) --prefix $(prefix) \
		$(BUILDROOT)/$(prefix) > files.list
	$(EPM) -v -f native -n -a $(ARCH) --keep-files --output-dir pkg \
		srcdir=$(srcdir) \
		top_srcdir=$(top_srcdir) \
		top_builddir=$(top_builddir) \
		PACKAGE=$(PACKAGE) \
		SUMMARY="$(SUMMARY)" \
		VERSION=$(VERSION) \
		RELEASE=$(RELEASE) \
		LICENSE="$(LICENSE)" \
		VENDOR="$(VENDOR)" \
		PACKAGER="$(PACKAGER)" \
		$(PACKAGE) $(srcdir)/pkg.list
	mv -f pkg/$(PKG_NAME).$(EPM_PKG_EXT) $(PKG_NAME).$(ARCH).$(EPM_PKG_EXT)

clean-local:
	rm -rf pkg $(BUILDROOT)
	rm -f files.list

endif WITH_EPM
