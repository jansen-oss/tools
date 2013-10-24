#####################################################################
# Makefile for small to medium sized C++ applications               #
# Copyright 2013 Jan Nikolas Jansen <jannikolas.jansen@gmail.com>   #
#                                                                   #
# Designed to build applications with as little effort as possible  #
# in release and debug modes.                                       #
#####################################################################

# Application name
EXECUTABLE=
# Additional compiler flags, e.g. -DSOME_MACRO
CXXFLAGS=
# Additional linker flags, e.g. -lpthread
LDFLAGS=

#####################################################################
# You should not need to modify below this line                     #
#####################################################################

# Compiler settings
CXXFLAGS+=-std=c++11 -Wall -Werror -pedantic-errors 
CXXFLAGS+=-Iinclude -MMD -MP
RELEASE_FLAGS=-O2 -DNDEBUG
DEBUG_FLAGS=-g -g3 -O0 -DDEBUG

# Project settings
SOURCES=$(wildcard src/*.cpp)
RELEASE_OBJ_DIR=release_obj
RELEASE_OBJ=$(addprefix $(RELEASE_OBJ_DIR)/,$(notdir $(SOURCES:.cpp=.o)))
RELEASE_DEP=$(addprefix $(RELEASE_OBJ_DIR)/,$(notdir $(SOURCES:.cpp=.d)))
DEBUG_OBJ_DIR=debug_obj
DEBUG_OBJ=$(addprefix $(DEBUG_OBJ_DIR)/,$(notdir $(SOURCES:.cpp=.o)))
DEBUG_DEP=$(addprefix $(DEBUG_OBJ_DIR)/,$(notdir $(SOURCES:.cpp=.d)))

# Release build
.PHONY: all
all: CXXFLAGS+=$(RELEASE_FLAGS)
all: LDFLAGS+=$(RELEASE_FLAGS)
all: $(SOURCES) $(EXECUTABLE)

$(EXECUTABLE): $(RELEASE_OBJ_DIR) $(RELEASE_OBJ)
	$(CXX) $(LDFLAGS) $(RELEASE_OBJ) -o $@

$(RELEASE_OBJ_DIR)/%.o: src/%.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@
-include $(RELEASE_DEP)

$(RELEASE_OBJ_DIR):
	mkdir $(RELEASE_OBJ_DIR)

# Debug build
.PHONY: debug
debug: CXXFLAGS+=$(DEBUG_FLAGS)
debug: LDFLAGS+=$(DEBUG_FLAGS)
debug: $(SOURCES) $(EXECUTABLE)_d

$(EXECUTABLE)_d: $(DEBUG_OBJ_DIR) $(DEBUG_OBJ)
	$(CXX) $(LDFLAGS) $(DEBUG_OBJ) -o $@

$(DEBUG_OBJ_DIR)/%.o: src/%.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@
-include $(DEBUG_DEP)

$(DEBUG_OBJ_DIR):
	mkdir $(DEBUG_OBJ_DIR)

# Clean up
.PHONY: clean
clean:
	$(RM) $(RELEASE_OBJ)
	$(RM) $(RELEASE_DEP)
	-! test -d $(RELEASE_OBJ_DIR) || rmdir $(RELEASE_OBJ_DIR)
	$(RM) $(EXECUTABLE)
	$(RM) $(DEBUG_OBJ)
	$(RM) $(DEBUG_DEP)
	-! test -d $(DEBUG_OBJ_DIR) || rmdir $(DEBUG_OBJ_DIR)
	$(RM) $(EXECUTABLE)_d
