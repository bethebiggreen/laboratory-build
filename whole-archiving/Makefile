CC=g++
CFLAGS=-c -Wall
LDFLAGS=
SRCS=$(wildcard src/*.cc)
OBJS=$(SRCS:.cc=.o)

STATIC_LIB_NAME=liba.a
STATIC_LIB_FILES=src/a.cc
STATIC_LIB_OBJS=$(STATIC_LIB_FILES:.cc=.o)

SHARED_LIB_NAME=libb.so
SHARED_LIB_FILES=src/b.cc
SHARED_LIB_OBJS=$(SHARED_LIB_FILES:.cc=.o)


EXECUTABLE=whole-archiving
AR=ar
RANLIB=ranlib

all: $(STATIC_LIB_NAME) $(SHARED_LIB_NAME) $(EXECUTABLE)

$(STATIC_LIB_NAME): $(STATIC_LIB_OBJS)
	$(AR) rcv $@ $(STATIC_LIB_OBJS)
	$(RANLIB) $@

$(SHARED_LIB_NAME): $(SHARED_LIB_OBJS)
	$(CC) -shared -Wl,-soname,$@ -o $@ $(SHARED_LIB_FILES)

$(EXECUTABLE): src/main.o 
	$(CC) src/main.o $(LDFLAGS) -Wl,--export-dynamic -Wl,-whole-archive $(STATIC_LIB_NAME) -Wl,-no-whole-archive -Wl,-rpath=. -o $@ 

.cc.o: 
	$(CC) $(CFLAGS) -fPIC -DHAPPY $< -o $@ 

.PHONY: clean
clean:
	rm -rf $(OBJS) $(SHARED_LIB_NAME) $(STATIC_LIB_NAME) $(EXECUTABLE)
