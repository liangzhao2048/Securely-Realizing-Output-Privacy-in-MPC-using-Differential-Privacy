SRC = cbmc_gc_main.cpp \
      cbmc_gc_parse_options.cpp \
      cbmc_languages.cpp \
      bv_cbmc_gc.cpp \
      circuit_creator_default.cpp \
      building_blocks.cpp \
      simple_circuit.cpp \
      simple_circuit_clustering.cpp \
      simple_circuit_converting.cpp \
      simple_circuit_dot.cpp \
      simple_circuit_evaluate.cpp \
      simple_circuit_framework_adapter.cpp \
      simple_circuit_gate.cpp \
      simple_circuit_rewriting.cpp \
      simple_circuit_rewriting_old.cpp \
      simple_circuit_structurizing.cpp \
      simple_circuit_sweep.cpp \
      simple_circuit_theorems.cpp

OBJ += ../cbmc/src/ansi-c/ansi-c$(LIBEXT) \
      ../cbmc/src/cpp/cpp$(LIBEXT) \
      ../cbmc/src/linking/linking$(LIBEXT) \
      ../cbmc/src/big-int/big-int$(LIBEXT) \
      ../cbmc/src/goto-programs/goto-programs$(LIBEXT) \
      ../cbmc/src/goto-symex/goto-symex$(LIBEXT) \
      ../cbmc/src/pointer-analysis/value_set$(OBJEXT) \
      ../cbmc/src/pointer-analysis/value_set_analysis_fi$(OBJEXT) \
      ../cbmc/src/pointer-analysis/value_set_domain_fi$(OBJEXT) \
      ../cbmc/src/pointer-analysis/value_set_fi$(OBJEXT) \
      ../cbmc/src/pointer-analysis/value_set_dereference$(OBJEXT) \
      ../cbmc/src/pointer-analysis/dereference_callback$(OBJEXT) \
      ../cbmc/src/pointer-analysis/add_failed_symbols$(OBJEXT) \
      ../cbmc/src/pointer-analysis/rewrite_index$(OBJEXT) \
      ../cbmc/src/pointer-analysis/goto_program_dereference$(OBJEXT) \
      ../cbmc/src/goto-instrument/full_slicer$(OBJEXT) \
      ../cbmc/src/goto-instrument/nondet_static$(OBJEXT) \
      ../cbmc/src/goto-instrument/cover$(OBJEXT) \
      ../cbmc/src/analyses/analyses$(LIBEXT) \
      ../cbmc/src/langapi/langapi$(LIBEXT) \
      ../cbmc/src/xmllang/xmllang$(LIBEXT) \
      ../cbmc/src/assembler/assembler$(LIBEXT) \
      ../cbmc/src/solvers/solvers$(LIBEXT) \
      ../cbmc/src/util/util$(LIBEXT) \
      ../cbmc/src/cbmc/xml_interface$(OBJEXT) \
      ../cbmc/src/cbmc/symex_bmc$(OBJEXT) \
      ../cbmc/src/cbmc/bmc$(OBJEXT) \
      ../cbmc/src/cbmc/symex_coverage$(OBJEXT) \
      ../cbmc/src/cbmc/fault_localization$(OBJEXT) \
      ../cbmc/src/cbmc/all_properties$(OBJEXT) \
      ../cbmc/src/cbmc/show_vcc$(OBJEXT) \
      ../cbmc/src/cbmc/bv_cbmc$(OBJEXT) \
      ../cbmc/src/cbmc/counterexample_beautification$(OBJEXT) \
      ../cbmc/src/cbmc/bmc_cover$(OBJEXT)


include ../cbmc/src/config.inc
include ../cbmc/src/common

INCLUDES= -I../cbmc/src

ifeq ($(USE_ABC),yes)
  LIBS = -L../abc -labc -ldl -lpthread -lreadline
  INCLUDES += -I../abc/src
  CP_CXXFLAGS += -DCBMC_GC_USE_ABC $(shell ../abc/arch_flags)
endif


CLEANFILES = cbmc-gc$(EXEEXT)

all: cbmc-gc$(EXEEXT)

ifneq ($(wildcard ../bv_refinement/Makefile),)
  OBJ += ../bv_refinement/bv_refinement$(LIBEXT)
  CP_CXXFLAGS += -DHAVE_BV_REFINEMENT
endif

ifneq ($(wildcard ../java_bytecode/Makefile),)
  OBJ += ../java_bytecode/java_bytecode$(LIBEXT)
  CP_CXXFLAGS += -DHAVE_JAVA_BYTECODE
  ifneq ($(wildcard $(LIBZIPINC)),)
    LIBS += $(LIBZIPLIB)
  endif
endif

ifneq ($(wildcard ../jsil/Makefile),)
  OBJ += ../jsil/jsil$(LIBEXT)
  CP_CXXFLAGS += -DHAVE_JSIL
endif

ifneq ($(wildcard ../specc/Makefile),)
  OBJ += ../specc/specc$(LIBEXT)
  CP_CXXFLAGS += -DHAVE_SPECC
endif

ifneq ($(wildcard ../php/Makefile),)
  OBJ += ../php/php$(LIBEXT)
  CP_CXXFLAGS += -DHAVE_PHP
endif

###############################################################################

cbmc-gc$(EXEEXT): $(OBJ)
	$(LINKBIN)

