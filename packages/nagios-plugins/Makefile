include ../../Makefile.omd

NAME = nagios-plugins
VERSION = 1.4.16
DIR = $(NAME)-$(VERSION)

CONFIGUREOPTS = \
    --prefix=$(OMD_ROOT) \
    --libexecdir=$(OMD_ROOT)/lib/nagios/plugins

build:
	tar xzf $(DIR).tar.gz
	set -e ; for p in patches/*.dif ; do \
	    echo "applying $$p..." ; \
	    ( cd $(DIR) ; patch -p1 -b ) < $$p ; \
	done
	cd $(DIR) ; ./configure $(CONFIGUREOPTS)
	$(MAKE) -C $(DIR) all

install:
	$(MAKE) DESTDIR=$(DESTDIR) -C $(DIR) install
	# FIXME: pack these with SUID root
	install -m 755 $(DIR)/plugins-root/check_icmp $(DESTDIR)$(OMD_ROOT)/lib/nagios/plugins
	install -m 755 $(DIR)/plugins-root/check_dhcp $(DESTDIR)$(OMD_ROOT)/lib/nagios/plugins
	ln -s check_icmp $(DESTDIR)$(OMD_ROOT)/lib/nagios/plugins/check_host

	# Copy package documentations to have these information in the binary packages
	mkdir -p $(DESTDIR)$(OMD_ROOT)/share/doc/$(NAME)
	for file in ACKNOWLEDGEMENTS AUTHORS BUGS CODING COPYING FAQ LEGAL NEWS README REQUIREMENTS SUPPORT THANKS ; do \
	   install -m 644 $(DIR)/$$file $(DESTDIR)$(OMD_ROOT)/share/doc/$(NAME); \
	done

	# link lib/nagios/plugins to lib/icinga/plugins
	if [ -d "$(DESTDIR)$(OMD_ROOT)/lib/icinga" ] ; then \
            cd $(DESTDIR)$(OMD_ROOT)/lib/icinga && ln -s ../nagios/plugins plugins ; \
	fi

skel:

clean:
	rm -rf $(DIR)
