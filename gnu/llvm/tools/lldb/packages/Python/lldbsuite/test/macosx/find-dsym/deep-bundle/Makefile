CC ?= clang

ifeq "$(ARCH)" ""
	ARCH = x86_64
endif

CFLAGS ?= -g -O0 -arch $(ARCH)

all: clean
	$(CC) $(CFLAGS) -install_name $(PWD)/MyFramework.framework/Versions/A/MyFramework -dynamiclib -o MyFramework myframework.c
	mkdir -p MyFramework.framework/Versions/A/Headers
	mkdir -p MyFramework.framework/Versions/A/Resources
	cp MyFramework MyFramework.framework/Versions/A
	cp MyFramework.h MyFramework.framework/Versions/A/Headers
	cp Info.plist MyFramework.framework/Versions/A/Resources
	( cd MyFramework.framework/Versions ; ln -s A Current )
	( cd MyFramework.framework/ ; ln -s Versions/Current/Headers . )
	( cd MyFramework.framework/ ; ln -s Versions/Current/MyFramework . )
	( cd MyFramework.framework/ ; ln -s Versions/Current/Resources . )
	mv MyFramework.dSYM MyFramework.framework.dSYM
	mkdir hide.app
	rm -f MyFramework
	tar cf - MyFramework.framework MyFramework.framework.dSYM | ( cd hide.app;tar xBpf -)
	$(CC) $(CFLAGS) -o deep-bundle main.c -F. -framework MyFramework
	

clean:
	rm -rf a.out a.out.dSYM deep-bundle deep-bundle.dSYM MyFramework.framework MyFramework.framework.dSYM MyFramework MyFramework.dSYM hide.app
