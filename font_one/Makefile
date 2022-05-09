CC = cc
CPPFLAGS = -MMD
CFLAGS = -Wall -Wextra -Werror -std=c99 -g
CFLAGS += -Wno-unused-parameter
CFLAGS += -Wno-missing-field-initializers
LDFLAGS = -mwindows
LDLIBS = -lcomctl32

SRC_DIR = ./
SRC = $(wildcard $(SRC_DIR)/*.c)
RC = $(wildcard $(SRC_DIR)/*.rc)

TARGET = prog.exe
BUILD_DIR = ./build
OBJ = $(SRC:$(SRC_DIR)/%.c=$(BUILD_DIR)/%.o)
OBJ += $(RC:$(SRC_DIR)/%.rc=$(BUILD_DIR)/%.o)
DEP = $(OBJ:%.o=%.d)

.PHONY: all clean run debug disasm

all: $(BUILD_DIR)/$(TARGET)
	$(info $(OBJ))

clean:
	rm -rf $(BUILD_DIR)

ARGS ?=
run: $(BUILD_DIR)/$(TARGET)
	$(BUILD_DIR)/$(TARGET) $(ARGS)

debug: all
	gdb --args $(BUILD_DIR)/$(TARGET) $(ARGS)

disasm: $(BUILD_DIR)/disasm.out

$(BUILD_DIR)/disasm.out: $(BUILD_DIR)/$(TARGET)
	objdump -dS $< > $@

# ==== BUILD ====

$(BUILD_DIR)/%.o: $(SRC_DIR)/%.rc
	mkdir -p $(BUILD_DIR)
	windres -o $@ $<

$(BUILD_DIR)/%.o: $(SRC_DIR)/%.c
	mkdir -p $(BUILD_DIR)
	$(CC) $(CPPFLAGS) $(CFLAGS) -c -o $@ $<

$(BUILD_DIR)/$(TARGET): $(OBJ)
	mkdir -p $(BUILD_DIR)
	$(CC) $(LDFLAGS) -o $@ $^ $(LDLIBS)

-include $(DEP)
