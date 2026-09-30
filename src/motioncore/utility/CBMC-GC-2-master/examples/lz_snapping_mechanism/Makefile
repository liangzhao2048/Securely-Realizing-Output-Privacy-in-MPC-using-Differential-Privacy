CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/bin/aby-cbmc-gc.exe
CIRCUIT_UTILS=../../bin/circuit-utils

.PHONY: clean run-sim run-aby


operations:= round_to_nearest_integer_CBMC # size-optimized circuit takes many rounds to finish, need improvement?

all:
	# make clean_all

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) sm64_$(operation).c --low-depth --minimization-time-limit 7200 --unwind 1000000 --outdir ./sm64_$(operation)_depth;\
		cp ./sm64_$(operation)_depth/output.stats.txt ./sm64_$(operation)_depth/sm64_$(operation)_depth.stats;\
		cp ./sm64_$(operation)_depth/*.txt .;\
		mv output.stats.txt sm64_$(operation)_depth.stats;\
		$(CIRCUIT_UTILS) --as-bristol sm64_$(operation)_depth.bristol;\
		cp sm64_$(operation)_depth.bristol ./sm64_$(operation)_depth/sm64_$(operation)_depth.bristol;\
    )

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) sm64_$(operation).c --minimization-time-limit 7200 --unwind 1000000 --outdir ./sm64_$(operation)_size;\
		cp ./sm64_$(operation)_size/output.stats.txt ./sm64_$(operation)_size/sm64_$(operation)_size.stats;\
		cp ./sm64_$(operation)_size/*.txt .;\
		mv output.stats.txt sm64_$(operation)_size.stats;\
		$(CIRCUIT_UTILS) --as-bristol sm64_$(operation)_size.bristol;\
		cp sm64_$(operation)_size.bristol ./sm64_$(operation)_size/sm64_$(operation)_size.bristol;\
    )

	make clean

	# $(CBMC_GC) sm64_ltz.c --low-depth --outdir ./sm64_ltz_size; \
	# # mv output.gate.txt sm64_mul_depth.circ;\
	# $(CIRCUIT_UTILS) --as-bristol sm64_mul_depth.bristol;\

# ../../bin/cbmc-gc sm64_ltz.c --low-depth --outdir ./sm64_ltz_depth

run-sim:
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 30000; INPUT_B_x := -30000; print;"
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 50; INPUT_B_x := -30; print;"
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 50; INPUT_B_x := -30; print;"
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 50; INPUT_B_x := -30; print;"
	# @$(CIRCUIT_SIM) --spec "INPUT_A_x := 50; INPUT_B_x := -30; print;"

	# test in separate directory
	../../../bin/circuit-sim  --spec "INPUT_A_x :=50000; INPUT_B_x := 0; print;"

# run-aby:
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 0 --spec "INPUT_B := 35; print;" &
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 1 --spec "INPUT_A := 25; print;"

clean:
	rm -f output-constants.txt output.gate.txt output.inputs.partyA.txt output.inputs.partyB.txt output.inputs.txt output.mapping.txt output.numberofgates.txt output.spec.txt output.stats.txt

clean_all:
	rm -f *.circ *.stats *.txt *.bristol
