CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/bin/aby-cbmc-gc.exe
CIRCUIT_UTILS=../../bin/circuit-utils

.PHONY: clean run-sim run-aby


# =================================================================
# finished operations low depth, size
# operations:= add sub gt eqz
# operations:= mul
# operations:= ceil
# operations:= to_int64
# operations:= to_float32
# operations:= to_float64
# operations:= exp2_P1045_neg_0_1
# operations:= sqrt_P0132 # --minimization-time-limit 72000 time out, generate depth-optimized circuit and size-optimized circuit
# operations:= sqr 
# =================================================================
# testing
# operations:= exp2_P1045_overflow # with overflow multiplication
# operations:= log2_P2508_overflow # with overflow multiplication
# operations:= sqrt_P0132_overflow # with overflow multiplication
# operations:= exp2_P1045_neg_0_1_overflow # with overflow multiplication

# operations:= div_Goldschmidt # very time comsuming
operations:= div # very time comsuming, generate not optimized circuit

# =================================================================
# operations:= div # very time comsuming
# operations:= div_Goldschmidt
# operations:= mul_overflowfree div_overflowfree
# operations:= exp2_P1045 log2_P2508 log2_PQ2524 sqrt sqrt_P0132 sqrt_PQ0371 exp ln

# operations:= sqrt # inaccurate method

# operations:= exp2_P1045_with_div_Goldschmidt 
# operations:= log2_PQ2524
# operations:= sqrt_PQ0371 # seems to have problem
# operations:= exp ln
# operations:= int2fx # Bristol doesn't support direct connections between inputs and outputs
# operations:= sqr # Bristol doesn't support direct connections between inputs and outputs

# operations:= exp2_P1045
# operations:= exp
# operations:= div

# operations:= add sub mul gt eqz ceil floor fx2int fx2fl 
# operations:= exp2_P1045
# operations:= exp
# operations:= exp2_P1045_neg_0_1 exp2_P1045_neg_0_1_low_depth exp_neg_0_1 


# operations:= exp2_P1045 exp exp2_P1045_neg_0_1 exp_neg_0_1 log2_P2508 ln sqrt sqrt_P0132 div div_Goldschmidt
# operations:=  log2_P2508 ln sqrt sqrt_P0132 div div_Goldschmidt

# operations:= log2_P2508 ln # --minimization-time-limit 3600 time out
# operations:= ln # --minimization-time-limit 3600 time out


all:
	# make clean_all

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) fix64_$(operation).c --low-depth --minimization-time-limit 72000 --unwind 1000000 --outdir ./fix64_$(operation)_depth;\
		cp ./fix64_$(operation)_depth/output.stats.txt ./fix64_$(operation)_depth/fix64_$(operation)_depth.stats;\
		cp ./fix64_$(operation)_depth/*.txt .;\
		mv output.stats.txt fix64_$(operation)_depth.stats;\
		$(CIRCUIT_UTILS) --as-bristol fix64_$(operation)_depth.bristol;\
		cp fix64_$(operation)_depth.bristol ./fix64_$(operation)_depth/fix64_$(operation)_depth.bristol;\
    )

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) fix64_$(operation).c --minimization-time-limit 72000 --unwind 1000000 --outdir ./fix64_$(operation)_size;\
		cp ./fix64_$(operation)_size/output.stats.txt ./fix64_$(operation)_size/fix64_$(operation)_size.stats;\
		cp ./fix64_$(operation)_size/*.txt .;\
		mv output.stats.txt fix64_$(operation)_size.stats;\
		$(CIRCUIT_UTILS) --as-bristol fix64_$(operation)_size.bristol;\
		cp fix64_$(operation)_size.bristol ./fix64_$(operation)_size/fix64_$(operation)_size.bristol;\
    )

	make clean

	# $(CBMC_GC) fix64_ltz.c --low-depth --outdir ./fix64_ltz_size; \
	# # mv output.gate.txt fix64_mul_depth.circ;\
	# $(CIRCUIT_UTILS) --as-bristol fix64_mul_depth.bristol;\

# ../../bin/cbmc-gc fix64_ltz.c --low-depth --outdir ./fix64_ltz_depth

run-sim:
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 30000; INPUT_B_x := -30000; print;"
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 50; INPUT_B_x := -30; print;"
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 50; INPUT_B_x := -30; print;"
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 50; INPUT_B_x := -30; print;"
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 50; INPUT_B_x := -30; print;"

	# test in separate directory
	../../../bin/circuit-sim  --spec "INPUT_A_x :=-50000; INPUT_B_x := 30000; print;"

# run-aby:
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 0 --spec "INPUT_B := 35; print;" &
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 1 --spec "INPUT_A := 25; print;"

clean:
	rm -f output-constants.txt output.gate.txt output.inputs.partyA.txt output.inputs.partyB.txt output.inputs.txt output.mapping.txt output.numberofgates.txt output.spec.txt output.stats.txt

clean_all:
	rm -f *.circ *.stats *.txt *.bristol
