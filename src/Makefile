CC       = gcc
SOURCES  = $(filter-out Test.c,$(wildcard *.c))
OBJECTS_RELEASE  = $(SOURCES:.c=.o)
OBJECTS_DEBUG   = $(SOURCES:.c=_d.o)
BIN_RELEASE     = libTilengine.so
BIN_DEBUG       = libTilengine_d.so
LD_LIB_RELEASE     = Tilengine
LD_LIB_DEBUG       = Tilengine_d
INCPATH  = ../include

DEBUG    ?= 0

OBJECTS = $(OBJECTS_RELEASE)
BIN = $(BIN_RELEASE)
LD_LIB = $(LD_LIB_RELEASE)
ifeq ($(DEBUG), 1)
	OBJECTS = $(OBJECTS_DEBUG)
	BIN = $(BIN_DEBUG)
	LD_LIB = $(LD_LIB_DEBUG)
endif

# Windows specific flags
ifeq ($(OS),Windows_NT)
$(error This Makefile is not intended for Windows! Please use Visual Studio instead)
else
	RM = rm -f
	name := $(shell uname -s)
	arch := $(shell uname -m)
	
	# Linux specific flags (i686, x64 and arm)
	ifeq ($(name),Linux)
		LIBS = -lSDL2 -lc -lz -lpng
		LDFLAGS = -shared -s
		LIBPATH = ../lib/linux_$(arch)
		ifeq ($(arch),i686)
			CFLAGS = -m32 -msse2
		endif
		ifeq ($(arch),x86_64)
			CFLAGS = -m64 -msse2
		endif
	endif
	
	# OSX specific flags
	ifeq ($(name),Darwin)
		LIBS = -lpng -lz -framework SDL2 -lc
		ifeq ($(DEBUG), 1)
		    BIN = Tilengine_d.dylib
		    LD_LIB = Tilengine_d
		else
		    BIN = Tilengine.dylib
		    LD_LIB = Tilengine
		endif
		LDFLAGS = -dynamiclib
		LIBPATH = ../lib/darwin_x86_64
	endif
endif

# common flags
CFLAGS += -I$(INCPATH) -std=c99 -O2 -fpic -DLIB_EXPORTS

ifeq ($(DEBUG), 1)
    CFLAGS +=-D_DEBUG
else
    CFLAGS +=-DNDEBUG
endif

.PHONY: all all-before all-after clean clean-custom

all: all-before $(BIN) all-after

clean: clean-custom
	${RM} $(OBJECTS) $(BIN) Test Test_d

$(BIN): $(OBJECTS)
	$(CC) -shared $(OBJECTS) -o $(BIN) $(LIBS) -fvisibility=hidden
	mkdir -p $(LIBPATH)
	cp $(BIN) $(LIBPATH)

Test: Test.c
	$(RM) Test.o Test
	$(CC) Test.c -o Test -I$(INCPATH) -L$(LIBPATH) -l$(LD_LIB) -Wl,-rpath,$(LIBPATH)

%.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@

%_d.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@
