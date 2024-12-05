# Default compiler flags
CFLAGS=-Ofast -Wall --std=gnu99 -Wno-unused-function -g
LDFLAGS=-lm

# Detect the platform
UNAME_S := $(shell uname -s)

# Compiler and OpenMP detection
ifeq ($(UNAME_S), Darwin)
    # macOS-specific logic
    HOMEBREW_GCC_PATH=$(shell brew --prefix gcc 2>/dev/null)/bin
    ifneq ($(wildcard $(HOMEBREW_GCC_PATH)/gcc-*),)
        CC=$(shell ls $(HOMEBREW_GCC_PATH)/gcc-* | head -n 1)
        CXX=$(shell ls $(HOMEBREW_GCC_PATH)/g++-* | head -n 1)
        $(info macOS: GCC detected at $(CC). Using GCC and G++.)
        CFLAGS += -fopenmp -march=native
        LDFLAGS += -lgomp -fopenmp
    else
        CC=clang
        CXX=clang++
        $(info macOS: GCC not found. Falling back to Clang.)
        ifneq ($(shell $(CC) -fopenmp -dM -E - < /dev/null 2>/dev/null | grep -c '__OPENMP__'),1)
            $(info macOS: OpenMP not supported by Clang. Skipping -fopenmp.)
        else
            CFLAGS += -fopenmp
            LDFLAGS += -fopenmp
        endif
    endif
else ifeq ($(UNAME_S), Linux)
    # Linux-specific logic
    ifneq ($(shell which gcc 2>/dev/null),)
        CC=gcc
        CXX=g++
        $(info Linux: System GCC detected. Using GCC and G++.)
        CFLAGS += -fopenmp -march=native
        LDFLAGS += -lgomp -fopenmp
    else
        $(error Linux: GCC is required but not found. Please install GCC.)
    endif
else ifeq ($(UNAME_S), Windows_NT)
    # Windows-specific logic
    ifneq ($(shell which gcc 2>/dev/null),)
        CC=gcc
        CXX=g++
        $(info Windows: GCC detected. Using GCC and G++.)
        CFLAGS += -fopenmp -march=native
        LDFLAGS += -lgomp -fopenmp
    else
        $(error Windows: GCC is required but not found. Please install GCC (e.g., via MinGW or WSL).)
    endif
else
    $(error Unsupported platform: $(UNAME_S).)
endif

# Installation paths
INSTALLDIR=/usr/local/bin
PYINSTALLDIR=../python/eflomal/bin

all: eflomal

eflomal.o: eflomal.c natmap.c hash.c random.c simd_math_prims.h
	$(CC) $(CFLAGS) -c eflomal.c

eflomal: eflomal.o
	$(CC) -o $@ $^ $(LDFLAGS)

python-install: eflomal
	mkdir -p $(PYINSTALLDIR)
	cp eflomal $(PYINSTALLDIR)

install: eflomal
	install -t $(INSTALLDIR) eflomal

clean:
	rm -f eflomal eflomal.o
