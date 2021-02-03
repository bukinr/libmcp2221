
PROJECT=libmcp2221

SOURCES= \
	hid.c \
	libmcp2221.c

CFLAGS= \
	-c \
	-Wall \
	-Wextra \
	-Wstrict-prototypes \
	-Wunused-result \
	-O3 \
	-std=c99 \
	-fmessage-length=0 \
	-fPIC \
	-I./

CXXFLAGS= \
	-c \
	-Wall \
	-Wextra \
	-Wunused-result \
	-O3 \
	-fmessage-length=0 \
	-fPIC \
	-I./

LDFLAGS= \
	-s \
	-static-libgcc \
	-static-libstdc++ \
	-shared

LDLIBS=

ifeq ($(OS),Windows_NT)
	SOURCES += win/resource.rc
	LDLIBS += -lsetupapi
	EXECUTABLE=$(PROJECT).dll
	NULLOUT=nul
else
	# udev is for the HIDRAW version of HIDAPI and usb-1.0 is for the libusb version
	LDLIBS += -L/usr/local/lib -lusb
	EXECUTABLE=$(PROJECT).so
	NULLOUT=/dev/null
endif

ARCHIVE=$(PROJECT).a

OBJ_DIR=obj
BIN_DIR=bin

CC=cc
CXX=cc
WINDRES=windres

WINOBJS=$(SOURCES:win/%.rc=$(OBJ_DIR)/%.o)
CPPOBJS=$(WINOBJS:%.cpp=$(OBJ_DIR)/%.o)
OBJECTS=$(CPPOBJS:%.c=$(OBJ_DIR)/%.o)

install:
	cp bin/libmcp2221.a /usr/local/lib/
	cp bin/libmcp2221.so /usr/local/lib/
	mkdir -p /usr/local/include/libmcp2221/
	cp libmcp2221.h /usr/local/include/libmcp2221/

all: dirs $(SOURCES) $(EXECUTABLE)

$(EXECUTABLE): $(OBJECTS)
	$(CC) $(LDFLAGS) $(OBJECTS) -o $(BIN_DIR)/$@ $(LDLIBS)
	ar rcs $(BIN_DIR)/$(ARCHIVE) $(OBJECTS)

$(OBJ_DIR)/%.o: win/%.rc
	windres $< -o $@

$(OBJ_DIR)/%.o: %.c
	$(CC) $(CFLAGS) $< -o $@

$(OBJ_DIR)/%.o: %.cpp
	$(CXX) $(CXXFLAGS) $< -o $@

clean:
	rm -rf $(OBJ_DIR)/*.o $(BIN_DIR)/$(EXECUTABLE) $(BIN_DIR)/$(ARCHIVE)

dirs:
	mkdir $(BIN_DIR) 2>$(NULLOUT) || true
	mkdir $(OBJ_DIR) 2>$(NULLOUT) || true

.PHONY: clean all
