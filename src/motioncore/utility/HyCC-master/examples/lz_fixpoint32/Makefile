CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/build/bin/aby-hycc
CIRCUIT_UTILS=../../bin/circuit-utils

.PHONY: clean run-sim run-aby


operations:= add mul sub div gt mul_overflow_free
# operations:= div_overflow_free

all:
	make clean_all
	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) fix32_$(operation).c --low-depth --outline --minimization-time-limit 360000 --unwind 1000000;\
		mv mpc_main.circ fix32_$(operation)_depth.circ;\
		$(CIRCUIT_UTILS) fix32_$(operation)_depth.circ --as-bristol fix32_$(operation)_depth.bristol;\
    )

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) fix32_$(operation).c --outline --minimization-time-limit 360000 --unwind 1000000;\
		mv mpc_main.circ fix32_$(operation)_size.circ;\
		$(CIRCUIT_UTILS) fix32_$(operation)_size.circ --as-bristol fix32_$(operation)_size.bristol;\
    )

run-sim:
	@$(CIRCUIT_SIM) fix32_add_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"
	@$(CIRCUIT_SIM) fix32_mul_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"
	@$(CIRCUIT_SIM) fix32_sub_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"
	@$(CIRCUIT_SIM) fix32_div_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"
	@$(CIRCUIT_SIM) fix32_gt_depth.circ --spec "INPUT_A_x := 500; INPUT_B_x := -30; print;"

# run-aby:
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 0 --spec "INPUT_B := 35; print;" &
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 1 --spec "INPUT_A := 25; print;"

clean:
	rm -f mpc_main.circ mpc_main.stats

clean_all:
	rm -f *.circ *.stats *.txt *.bristol
