CPP = g++
CPP_FLAGS = -std=c++14 -Wall -Wextra -Wpedantic -Iinclude -g

BUILD_DIR = build
BIN_DIR = ../../bin

# Flags required for automatic dependency genneration.
# See http://make.mad-scientist.net/papers/advanced-auto-dependency-generation
DEPS_FLAGS = -MT $@ -MMD -MP -MF $(BUILD_DIR)/$*.Td
# Rename temporary dependency file to real dependency file.
POST_COMPILE = mv -f $(BUILD_DIR)/$*.Td $(BUILD_DIR)/$*.d


.PHONY: all clean

all: $(BUILD_DIR) $(BIN_DIR) test/test $(BIN_DIR)/circuit-sim $(BIN_DIR)/circuit-utils $(BIN_DIR)/libcircuit-utils.a

clean:
	rm -f build/*
	rm -f test/test
	rm -f $(BIN_DIR)/circuit-sim
	rm -f $(BIN_DIR)/circuit-utils
	rm -f $(BIN_DIR)/libcircuit-utils.a

$(BIN_DIR) $(BUILD_DIR):
	@mkdir $@


$(BIN_DIR)/circuit-sim: \
 $(BUILD_DIR)/sim_main.o \
 $(BUILD_DIR)/circuit.o \
 $(BUILD_DIR)/spec.o \
 $(BUILD_DIR)/bristol_file_format.o \
 $(BUILD_DIR)/circuit_simulation.o
	$(CPP) $(CPP_FLAGS) $^ -o $@

$(BIN_DIR)/circuit-utils: \
 $(BUILD_DIR)/utils_main.o \
 $(BUILD_DIR)/circuit.o \
 $(BUILD_DIR)/spec.o \
 $(BUILD_DIR)/bristol_file_format.o \
 $(BUILD_DIR)/scd_file_format.o \
 $(BUILD_DIR)/shdl_file_format.o
	$(CPP) $(CPP_FLAGS) $^ -o $@

$(BIN_DIR)/libcircuit-utils.a: \
 $(BUILD_DIR)/circuit.o \
 $(BUILD_DIR)/spec.o \
 $(BUILD_DIR)/circuit_simulation.o \
 $(BUILD_DIR)/bristol_file_format.o \
 $(BUILD_DIR)/scd_file_format.o \
 $(BUILD_DIR)/shdl_file_format.o
	ar rcs $@ $^

test/test: \
 $(BUILD_DIR)/test.o \
 $(BUILD_DIR)/circuit.o \
 $(BUILD_DIR)/spec.o
	$(CPP) $(CPP_FLAGS) $^ -o $@


$(BUILD_DIR)/%.o: src/%.cpp $(BUILD_DIR)/%.d
	$(CPP) $(CPP_FLAGS) $(DEPS_FLAGS) -c  $< -o $@
	$(POST_COMPILE)

$(BUILD_DIR)/%.o: test/%.cpp $(BUILD_DIR)/%.d
	$(CPP) $(CPP_FLAGS) $(DEPS_FLAGS) -c  $< -o $@
	$(POST_COMPILE)

$(BUILD_DIR)/%.d: ;

# Prevent make from deleting the dependency files.
.PRECIOUS: $(BUILD_DIR)/%.d

include $(BUILD_DIR)/*.d
