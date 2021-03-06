##
# JavaAppLauncher: a simple Java application launcher for Mac OS X.
# GNUmakefile
#
# Paul J. Lucas [paul@lightcrafts.com]
##

ROOT:=		../../..
COMMON_DIR:=	$(ROOT)/lightcrafts
MACOSX_DIR:=	../..
include		$(COMMON_DIR)/mk/platform.mk

# Uncomment to compile in debug mode.
#DEBUG:=		true

ifeq ($(UNIVERSAL),1)
  ##
  # We need to build the PowerPC part with gcc/g++ 3.3 so the launcher will run
  # on older versions of Mac OS X.
  ##
  CC_PPC:=	$(CC) -arch ppc
  CXX_PPC:=	$(CXX) -arch ppc
  CC_X86:=	$(CC) -arch i386
  CXX_X86:=	$(CXX) -arch i386

  #SDKROOT_PPC:=	/Developer/SDKs/MacOSX10.3.9.sdk
  #SDKROOT_X86:=	/Developer/SDKs/MacOSX10.4u.sdk
else
  ifeq ($(PROCESSOR),powerpc)
    ##
    # For PowerPC-only, we want to build against 10.3.9 to support Panther.
    # (Intel-only requires 10.4.)
    ##
    SDKROOT:=	/Developer/SDKs/MacOSX10.3.9.sdk
  endif
endif

CFLAGS:=	-Os -mdynamic-no-pic
INCLUDES=	$(MACOSX_ISYSROOT)
LDFLAGS=	$(PLATFORM_LDFLAGS) -L. -framework Cocoa
LINK:=		-lEWS

TARGET:=	eSellerateActivate

########## You shouldn't have to change anything below this line. #############

export MACOSX_DEPLOYMENT_TARGET_ppc:=	10.3
export MACOSX_DEPLOYMENT_TARGET_i386:=	10.4

ifdef DEBUG
  CFLAGS+=	-g
  DEFINES+=	-DDEBUG
  STRIP:=	echo >/dev/null
else
  STRIP:=	strip
endif

CFLAGS+=	$(DEFINES)

include		$(COMMON_DIR)/mk/sources.mk

##
# Build rules
##

.PHONY: all
all: $(TARGET)

ifeq ($(UNIVERSAL),1)

$(TARGET): $(TARGET)-ppc $(TARGET)-x86
	$(LIPO) -create $(TARGET)-ppc $(TARGET)-x86 -output $@

$(TARGET)-ppc: $(OBJECTS_PPC)
	$(CC_LINK) $(CFLAGS) $(LDFLAGS) -o $@ *-ppc.o $(LINK)
	$(STRIP) $@

$(TARGET)-x86: $(OBJECTS_X86)
	$(CC_LINK) $(CFLAGS) $(LDFLAGS) -o $@ *-x86.o $(LINK)
	$(STRIP) $@

else	# UNIVERSAL

$(TARGET): $(OBJECTS)
	$(CC_LINK) $(CFLAGS) $(LDFLAGS) -o $@ *.o $(LINK)
	$(STRIP) $@

endif	# UNIVERSAL

include		$(COMMON_DIR)/mk/auto_dep.mk

##
# Utility rules
##

.PHONY: clean distclean mostlyclean

clean:
	$(RM) *.o .*.d

distclean: clean
	$(RM) $(TARGET) $(TARGET)-ppc $(TARGET)-x86

mostlyclean:

# vim:set noet sw=8 ts=8:
