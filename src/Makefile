DESTDIR        = /usr/local

CXX            = g++
CXXFLAGS       = $(shell pkg-config --cflags luajit) -fPIC -I. -Wall -O3
CXXFLAGS      += -DUDF_LUA_PATH=\"$(DESTDIR)/share/hdf5-udf/udf.lua\"
LDFLAGS        = -ldl -lm -lhdf5 -Wl,--no-undefined

FILTER_TARGET  = libhdf5-udf.so
FILTER_SOURCES = hdf5-udf.cpp
FILTER_OBJS    = $(patsubst %.cpp,%.o, $(FILTER_SOURCES))
FILTER_LDFLAGS = $(shell pkg-config --libs luajit) -shared

BIN_TARGETS    = hdf5-udf
BIN_SOURCES    = main.cpp lua_parser.cpp
BIN_OBJS       = $(patsubst %.cpp,%.o, $(BIN_SOURCES))
BIN_CXXFLAGS   = -Wall


all: $(FILTER_TARGET) $(BIN_TARGETS)

clean:
	rm -f $(BIN_TARGETS) $(FILTER_TARGET) *.o

install:
	@install -v -d $(DESTDIR)/bin $(DESTDIR)/share/hdf5-udf $(DESTDIR)/hdf5/lib/plugin
	@install -v -t $(DESTDIR)/bin $(BIN_TARGETS)
	@install -v -t $(DESTDIR)/share/hdf5-udf udf.lua
	@install -v -t $(DESTDIR)/hdf5/lib/plugin $(FILTER_TARGET)

$(FILTER_TARGET): $(FILTER_OBJS)
	$(CXX) $^ -C -o $@ $(FILTER_LDFLAGS) $(LDFLAGS)

hdf5-udf: $(BIN_OBJS)
	$(CXX) $^ -o $@ $(LDFLAGS)

$(BIN_OBJS) $(FILTER_OBJS):
