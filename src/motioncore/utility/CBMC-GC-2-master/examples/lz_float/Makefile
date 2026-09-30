CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/bin/aby-cbmc-gc.exe
CIRCUIT_UTILS=../../bin/circuit-utils

.PHONY: clean run-sim run-aby

# operations:= float64_to_int64_round_to_zero

# =================================================================

# operations:= float32_to_int32  
# operations:= int32_to_float32
# operations:= float32_floor  
# operations:= float32_ceil 
# operations:= float32_to_int64
# operations:= int64_to_float32

# operations:= float64_to_int64 # seems complexer than ESAT
# operations:= int64_to_float64 # time comsuming
# operations:= float64_floor
# operations:= float64_ceil
# operations:= float64_to_int32
# operations:= int32_to_float64
# operations:= int128_to_float64 # need to regenerate 
# operations:= int128_to_float64_towards_zero # need to regenerate 
# operations:= int128_to_float32 # need to regenerate 
# operations:= int128_to_float32_towards_zero # need to regenerate 

# =================================================================
operations:= int128_to_float64_towards_zero int128_to_float32 int128_to_float32_towards_zero# need to regenerate 

# operations:= float32_to_int32 int32_to_float32 float32_floor float32_ceil float64_to_int64 int64_to_float64 float64_floor float64_ceil


all:
	# make clean_all 

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) $(operation).c --low-depth --minimization-time-limit 7200 --unwind 1000000 --outdir ./$(operation)_depth;\
		cp ./$(operation)_depth/output.stats.txt ./$(operation)_depth/$(operation)_depth.stats;\
		cp ./$(operation)_depth/*.txt .;\
		mv output.stats.txt $(operation)_depth.stats;\
		$(CIRCUIT_UTILS) --as-bristol $(operation)_depth.bristol;\
		cp $(operation)_depth.bristol ./$(operation)_depth/$(operation)_depth.bristol;\
    )

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) $(operation).c --minimization-time-limit 1800 --unwind 1000000 --outdir ./$(operation)_size;\
		cp ./$(operation)_size/output.stats.txt ./$(operation)_size/$(operation)_size.stats;\
		cp ./$(operation)_size/*.txt .;\
		mv output.stats.txt $(operation)_size.stats;\
		$(CIRCUIT_UTILS) --as-bristol $(operation)_size.bristol;\
		cp $(operation)_size.bristol ./$(operation)_size/$(operation)_size.bristol;\
    )

	make clean

	# $(CBMC_GC) ltz.c --low-depth --outdir ./ltz_size; \
	# # mv output.gate.txt mul_depth.circ;\
	# $(CIRCUIT_UTILS) --as-bristol mul_depth.bristol;\

# ../../bin/cbmc-gc ltz.c --low-depth --outdir ./ltz_depth

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
