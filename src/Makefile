CC=clang
CFLAGS=-Xpreprocessor -fopenmp -I/opt/homebrew/opt/libomp/include -Ofast -march=native -Wall --std=gnu99 -Wno-unused-function -g
LDFLAGS=-Xpreprocessor -fopenmp -L/opt/homebrew/opt/libomp/lib -lm -lomp

# -lrt
INSTALLDIR=/usr/local/bin
PYINSTALLDIR=../python/eflomal/bin

all: eflomal

eflomal: eflomal.o
	$(CC) $(LDFLAGS) $^ -o $@

python-install: eflomal
	mkdir -p $(PYINSTALLDIR)
	cp eflomal $(PYINSTALLDIR)

install: eflomal
	install -t $(INSTALLDIR) eflomal

clean:
	rm -f eflomal eflomal.o
