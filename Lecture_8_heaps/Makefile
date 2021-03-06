#Variables
MODULES=main


SOURCES=$(MODULES:=.cpp)
OBJECTS=$(MODULES:=.o)
DEPS=$(MODULES:=.d)
EXE=main
CXXFLAGS=-I -std=c++11 -Wall -pedantic -ansi -pthread
INCLUDES=-L./lib -lpthread
BUILD_DIR=build
BIN_DIR=bin

#Making for host > make ARCH=host
ifeq (${ARCH}, host)
CXX=g++
BUILD_DIR=build/host
BIN_DIR=bin/host
endif

#Making for target > make ARCH=target
ifeq (${ARCH}, target)
CXX=arm-devkit-g++
BUILD_DIR=build/target
BIN_DIR=bin/target
endif

CONTROL_OBJECTS=$(addprefix ${BUILD_DIR}/, $(OBJECTS))
DEP_DIR=$(addprefix ${BUILD_DIR}/,$(DEPS))

all:${BIN_DIR}/$(EXE)

#Here we put the executables file into the bin dir
$(BIN_DIR)/$(EXE): ${DEP_DIR} $(CONTROL_OBJECTS)
	@mkdir -p ${BIN_DIR}
	$(CXX) -o $@ $(CONTROL_OBJECTS) $(INCLUDES)
	@echo "Compiling complete"

#Building .o (object) files
${BUILD_DIR}/%.o: %.cpp
	@mkdir -p ${BUILD_DIR}
	$(CXX) $(CXXFLAGS) -c $^ -o $@


#Rule that descibes how a .d (dependency) file is crated from a .cpp file
${BUILD_DIR}/%.d: %.cpp
	@mkdir -p ${BUILD_DIR}
	$(CXX) -MT${BUILD_DIR}/$(@:.d=.o) -MM $(CXXFLAGS) $^ > $@

ifneq ($(MAKECMDGOALS), clean)
-include $(DEP_DIR)
endif

.PHONY: clean
clean:
	@rm -rf ${BUILD_DIR}
	@rm -rf ${BIN_DIR}
