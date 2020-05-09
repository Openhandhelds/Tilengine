CC       = gcc
SOURCES  = $(wildcard *.c)
OBJECTS_RELEASE    = $(SOURCES:.c=.o)
OBJECTS_DEBUG      = $(SOURCES:.c=_d.o)
LD_LIB_RELEASE     = Tilengine
LD_LIB_DEBUG       = Tilengine_d
TARGETS_RELEASE    = barrel mode7 platformer racer scaling shadow shooter tutorial wobble colorcycle benchmark supermarioclone test_mouse forest
TARGETS_DEBUG      = barrel_d mode7_d platformer_d racer_d scaling_d shadow_d shooter_d tutorial_d wobble_d colorcycle_d benchmark_d supermarioclone_d test_mouse_d forest_d

OBJECTS  = $(OBJECTS_RELEASE)
TARGETS  = $(TARGETS_RELEASE)
LD_LIB   = $(LD_LIB_RELEASE)

DEBUG    ?= 0

ifeq ($(DEBUG), 1)
	TARGETS = $(TARGETS_DEBUG)
        OBJECTS = $(OBJECTS_DEBUG)
        LD_LIB = $(LD_LIB_DEBUG)
endif

# Windows specific flags
ifeq ($(OS),Windows_NT)
	RM = del /Q *.exe
	CFLAGS  = -m32 -msse2
	LDFLAGS = ../lib/win32/Tilengine.lib -s
else
	RM = rm
	name := $(shell uname -s)
	arch := $(shell uname -m)
	
	# Linux specific flags (i686, x64 and arm)
	ifeq ($(name),Linux)
		ifeq ($(arch),i686)
			CFLAGS = -m32 -msse2
		endif
		ifeq ($(arch),x86_64)
			CFLAGS = -m64 -msse2
		endif
		LIBPATH = ../lib/linux_$(arch)
		LDFLAGS = -L$(LIBPATH) -l$(LD_LIB) -lm -s -Wl,-rpath,$(LIBPATH)
	endif
	
	# OSX specific flags
	ifeq ($(name),Darwin)
                ifeq ($(DEBUG), 1)
			LDFLAGS = "/usr/local/lib/Tilengine_d.dylib" -lm
                else
			LDFLAGS = "/usr/local/lib/Tilengine.dylib" -lm
                endif
	endif
endif

# common C flags
CFLAGS += -I"../include" -std=c99 -O2

ifeq ($(DEBUG), 1)
    CFLAGS +=-D_DEBUG
else
    CFLAGS +=-DNDEBUG
endif

.PHONY: all all-before all-after clean clean-custom

all: $(TARGETS)

barrel: Barrel.o Simon.o
	$(CC) Barrel.o Simon.o -o barrel $(LDFLAGS)
barrel_d: Barrel_d.o Simon_d.o
	$(CC) Barrel_d.o Simon_d.o -o barrel_d $(LDFLAGS)
	
mode7: Mode7.o Sin.o
	$(CC) Mode7.o Sin.o -o mode7 $(LDFLAGS)
mode7_d: Mode7_d.o Sin_d.o
	$(CC) Mode7_d.o Sin_d.o -o mode7_d $(LDFLAGS)
	
platformer: Platformer.o
	$(CC) Platformer.o -o platformer $(LDFLAGS)
platformer_d: Platformer_d.o
	$(CC) Platformer_d.o -o platformer_d $(LDFLAGS)
	
racer: Racer.o Tree.o Actor.o
	$(CC) Racer.o Tree.o Actor.o -o racer $(LDFLAGS)
racer_d: Racer_d.o Tree_d.o Actor_d.o
	$(CC) Racer_d.o Tree_d.o Actor_d.o -o racer_d $(LDFLAGS)
	
scaling: Scaling.o
	$(CC) Scaling.o -o scaling $(LDFLAGS)
scaling_d: Scaling_d.o
	$(CC) Scaling_d.o -o scaling_d $(LDFLAGS)
	
shadow: Shadow.o
	$(CC) Shadow.o -o shadow $(LDFLAGS)
shadow_d: Shadow_d.o
	$(CC) Shadow_d.o -o shadow_d $(LDFLAGS)

shooter: Shooter.o Actor.o Boss.o Enemy.o Explosion.o Ship.o Sin.o
	$(CC) Shooter.o Actor.o Boss.o Enemy.o Explosion.o Ship.o Sin.o -o shooter $(LDFLAGS)
shooter_d: Shooter_d.o Actor_d.o Boss_d.o Enemy_d.o Explosion_d.o Ship_d.o Sin_d.o
	$(CC) Shooter_d.o Actor_d.o Boss_d.o Enemy_d.o Explosion_d.o Ship_d.o Sin_d.o -o shooter_d $(LDFLAGS)
	
tutorial: Tutorial.o
	$(CC) Tutorial.o -o tutorial $(LDFLAGS)
tutorial_d: Tutorial_d.o
	$(CC) Tutorial_d.o -o tutorial_d $(LDFLAGS)

wobble: Wobble.o Sin.o
	$(CC) Wobble.o Sin.o -o wobble $(LDFLAGS)
wobble_d: Wobble_d.o Sin_d.o
	$(CC) Wobble_d.o Sin_d.o -o wobble_d $(LDFLAGS)
	
colorcycle: ColorCycle.o
	$(CC) ColorCycle.o -o colorcycle $(LDFLAGS)
colorcycle_d: ColorCycle_d.o
	$(CC) ColorCycle_d.o -o colorcycle_d $(LDFLAGS)

benchmark: Benchmark.o
	$(CC) Benchmark.o -o benchmark $(LDFLAGS)
benchmark_d: Benchmark_d.o
	$(CC) Benchmark_d.o -o benchmark_d $(LDFLAGS)
	
supermarioclone: SuperMarioClone.o
	$(CC) SuperMarioClone.o -o supermarioclone $(LDFLAGS)
supermarioclone_d: SuperMarioClone_d.o
	$(CC) SuperMarioClone_d.o -o supermarioclone_d $(LDFLAGS)
	
test_mouse: TestMouse.o
	$(CC) TestMouse.o -o test_mouse $(LDFLAGS)
test_mouse_d: TestMouse_d.o
	$(CC) TestMouse_d.o -o test_mouse_d $(LDFLAGS)
	
forest: Forest.o
	$(CC) Forest.o -o forest $(LDFLAGS)
forest_d: Forest_d.o
	$(CC) Forest_d.o -o forest_d $(LDFLAGS)
	
%.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@

%_d.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@

# clean
clean: clean-custom
	$(RM) $(OBJECTS) $(TARGETS)
	
