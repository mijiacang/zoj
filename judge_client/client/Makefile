CC=$(CXX)
CXXFLAGS=-I ../kernel_module -L ../kernel_module -Wall -Werror -Wno-sign-compare -ansi -O3 -MD
VPATH=../kernel_module
ALL_SOURCES:=$(wildcard *.cc)
UNITTEST_SOURCES:=$(wildcard *_unittest.cc)
SOURCES=$(filter-out $(UNITTEST_SOURCES),$(ALL_SOURCES))
ALL_OBJS:=$(patsubst %.cc,%.o,$(ALL_SOURCES))
OBJS:=$(patsubst %.cc,%.o,$(SOURCES))
UNITTESTS:=$(patsubst %.cc,%,$(UNITTEST_SOURCES))
UNITTEST_RUNS:=$(patsubst %.cc,%.run,$(UNITTEST_SOURCES))

.PHONY: all clean rebuild $(UNITTEST_RUNS) test

all: judge_client

clean:
	@rm -f $(ALL_OBJS) $(ALL_DEPS) judge_client $(UNITTESTS)

rebuild: clean all

test: $(UNITTEST_RUNS)

ALL_DEPS:=$(patsubst %.cc,%.d,$(ALL_SOURCES))
MISSING_DEPS:=$(filter-out $(wildcard $(ALL_DEPS)),$(ALL_DEPS))
ifneq ($(MISSING_DEPS),)
$(MISSING_DEPS) :
	@rm -f $(patsubst %.d,%.o,$@)
endif

-include $(ALL_DEPS)

judge_client: $(OBJS) -lkmmon
	$(CXX) $(CXXFLAGS) -o $@ $^

$(UNITTESTS): %_unittest : %.o %_unittest.o -lcppunit -lkmmon
util_unittest: args.o util.o trace.o logging.o

$(UNITTEST_RUNS): %.run : %
	@echo -n "Running $<: "
	@-./$<
	@echo "******************************************************************************"
	@echo