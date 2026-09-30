CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/bin/aby-cbmc-gc.exe
CIRCUIT_UTILS=../../bin/circuit-utils

.PHONY: clean run-sim run-aby


# operations:= add sub mul gt
# operations:= div # very time comsuming
operations:=  exp2
all:
	# make clean_all

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) fix128_$(operation).c --low-depth --minimization-time-limit 360000 --unwind 1000000 --outdir ./fix128_$(operation)_depth;\
		cp ./fix128_$(operation)_depth/output.stats.txt ./fix128_$(operation)_depth/fix128_$(operation)_depth.stats;\
		cp ./fix128_$(operation)_depth/*.txt .;\
		mv output.stats.txt fix128_$(operation)_depth.stats;\
		$(CIRCUIT_UTILS) --as-bristol fix128_$(operation)_depth.bristol;\
		cp fix128_$(operation)_depth.bristol ./fix128_$(operation)_depth/fix128_$(operation)_depth.bristol;\
    )

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) fix128_$(operation).c --minimization-time-limit 360000 --unwind 1000000 --outdir ./fix128_$(operation)_size;\
		cp ./fix128_$(operation)_size/output.stats.txt ./fix128_$(operation)_size/fix128_$(operation)_size.stats;\
		cp ./fix128_$(operation)_size/*.txt .;\
		mv output.stats.txt fix128_$(operation)_size.stats;\
		$(CIRCUIT_UTILS) --as-bristol fix128_$(operation)_size.bristol;\
		cp fix128_$(operation)_size.bristol ./fix128_$(operation)_size/fix128_$(operation)_size.bristol;\
    )
    
	make clean

	# $(CBMC_GC) fix128_add.c --low-depth --outdir ./fix128_add_size; \
	# # mv output.gate.txt fix128_mul_depth.circ;\
	# $(CIRCUIT_UTILS) --as-bristol fix128_mul_depth.bristol;\


run-sim:
	@$(CIRCUIT_SIM) fix128_add_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"
	@$(CIRCUIT_SIM) fix128_mul_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"
	@$(CIRCUIT_SIM) fix128_sub_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"
	@$(CIRCUIT_SIM) fix128_div_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"
	@$(CIRCUIT_SIM) fix128_gt_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"

# run-aby:
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 0 --spec "INPUT_B := 35; print;" &
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 1 --spec "INPUT_A := 25; print;"

clean:
	rm -f output-constants.txt output.gate.txt output.inputs.partyA.txt output.inputs.partyB.txt output.inputs.txt output.mapping.txt output.numberofgates.txt output.spec.txt output.stats.txt

clean_all:
	rm -f *.circ *.stats *.txt *.bristol
