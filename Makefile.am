AM_DEFAULT_SOURCE_EXT = .cpp

sbin_PROGRAMS = \
	ipmid

ipmid_SOURCES = \
	ipmid.cpp \
	settings.cpp \
	host-cmd-manager.cpp \
	timer.cpp \
	utils.cpp \
	oemrouter.cpp
nodist_ipmid_SOURCES = ipmiwhitelist.cpp

libipmi20_BUILT_LIST = \
	sensor-gen.cpp \
	inventory-sensor-gen.cpp \
	fru-read-gen.cpp \
	channel-gen.cpp

BUILT_SOURCES = \
               ipmiwhitelist.cpp \
               $(libipmi20_BUILT_LIST)


CLEANFILES = $(BUILT_SOURCES)

BOOST_CXX = -DBOOST_ERROR_CODE_HEADER_ONLY \
	    -DBOOST_SYSTEM_NO_DEPRECATED \
	    -DBOOST_ALL_NO_LIB
#TODO - Make this path a configure option (bitbake parameter)
ipmid_CXXFLAGS = -DHOST_IPMI_LIB_PATH=\"/usr/lib/host-ipmid/\" \
		 $(BOOST_CXX) \
                 $(PHOSPHOR_LOGGING_CFLAGS) \
                 $(PHOSPHOR_DBUS_INTERFACES_CFLAGS)
ipmid_LDFLAGS = \
	$(SYSTEMD_LIBS) \
	$(libmapper_LIBS) \
	$(LIBADD_DLOPEN) \
	$(PHOSPHOR_LOGGING_LIBS) \
	$(PHOSPHOR_DBUS_INTERFACES_LIBS) \
	-lstdc++fs \
	-pthread \
	-export-dynamic

# TODO: Rather than use -export-dynamic, we should use -export-symbol to have a
#       selective list of symbols.

ipmiwhitelist.cpp: ${srcdir}/generate_whitelist.sh $(WHITELIST_CONF)
	$(SHELL) $^ > $@

sensor-gen.cpp:
	$(AM_V_GEN)@SENSORGEN@ -o $(top_builddir) generate-cpp

inventory-sensor-gen.cpp:
	$(AM_V_GEN)@INVSENSORGEN@ -o $(top_builddir) generate-cpp

fru-read-gen.cpp:
	$(AM_V_GEN)@FRUGEN@ -o $(top_builddir) generate-cpp

channel-gen.cpp:
	$(AM_V_GEN)@CHANNELGEN@ -o $(top_builddir) generate-cpp

libipmi20dir = ${libdir}/ipmid-providers
libipmi20_LTLIBRARIES = libipmi20.la
libipmi20_la_SOURCES = \
	net.cpp \
	app/channel.cpp \
	app/watchdog.cpp \
	app/watchdog_service.cpp \
	apphandler.cpp \
	sensorhandler.cpp \
	storagehandler.cpp \
	chassishandler.cpp \
	dcmihandler.cpp \
	ipmisensor.cpp \
	storageaddsel.cpp \
	transporthandler.cpp \
	globalhandler.cpp \
	groupext.cpp \
	utils.cpp \
	selutility.cpp \
	ipmi_fru_info_area.cpp \
	read_fru_data.cpp \
	sensordatahandler.cpp \
	$(libipmi20_BUILT_LIST)

@CODE_COVERAGE_RULES@

check_PROGRAMS =
XFAIL_TESTS =

TESTS = $(check_PROGRAMS)

libipmi20_la_LDFLAGS = $(SYSTEMD_LIBS) $(libmapper_LIBS) \
	$(PHOSPHOR_LOGGING_LIBS) $(PHOSPHOR_DBUS_INTERFACES_LIBS) -lstdc++fs \
	-version-info 0:0:0 -shared
libipmi20_la_CXXFLAGS = $(SYSTEMD_CFLAGS) $(libmapper_CFLAGS) \
	$(BOOST_CXX) $(PHOSPHOR_LOGGING_CFLAGS) \
	$(PHOSPHOR_DBUS_INTERFACES_CFLAGS)

libsysintfcmdsdir = ${libdir}/ipmid-providers
libsysintfcmds_LTLIBRARIES = libsysintfcmds.la
libsysintfcmds_la_SOURCES = \
	systemintfcmds.cpp \
	host-interface.cpp
libsysintfcmds_la_LDFLAGS = $(SYSTEMD_LIBS) \
                            $(libmapper_LIBS) \
                            $(PHOSPHOR_DBUS_INTERFACES_LIBS) \
                            $(PHOSPHOR_LOGGING_LIBS) \
                            $(SDBUSPLUS_LIBS) \
                            -version-info 0:0:0 -shared
libsysintfcmds_la_CXXFLAGS = $(SYSTEMD_CFLAGS) \
			     $(BOOST_CXX) \
                             $(libmapper_CFLAGS) \
                             $(PHOSPHOR_DBUS_INTERFACES_CFLAGS) \
                             $(PHOSPHOR_LOGGING_CFLAGS) \
                             $(SDBUSPLUS_CFLAGS)

nobase_include_HEADERS = \
	host-ipmid/ipmid-api.h \
	host-ipmid/ipmid-host-cmd.hpp \
	host-ipmid/ipmid-host-cmd-utils.hpp \
	host-ipmid/oemopenbmc.hpp \
	host-ipmid/oemrouter.hpp

# Forcing the build of self and then subdir
SUBDIRS = . test softoff
