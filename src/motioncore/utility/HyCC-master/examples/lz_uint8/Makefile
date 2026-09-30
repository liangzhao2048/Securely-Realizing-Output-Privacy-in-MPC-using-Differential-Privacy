CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/build/bin/aby-hycc
CIRCUIT_UTILS=../../bin/circuit-utils

.PHONY: clean run-sim run-aby

# operations:= add mul sub div gt eqz mod

# operations:= neg
operations:= geq
all:
	make clean_all
	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) uint8_$(operation).c --low-depth --outline;\
		mv mpc_main.circ uint8_$(operation)_depth.circ;\
		mv mpc_main.stats uint8_$(operation)_depth.stats;\
		$(CIRCUIT_UTILS) uint8_$(operation)_depth.circ --as-bristol uint8_$(operation)_depth.bristol;\
    )

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) uint8_$(operation).c --outline;\
		mv mpc_main.circ uint8_$(operation)_size.circ;\
		mv mpc_main.stats uint8_$(operation)_size.stats;\
		$(CIRCUIT_UTILS) uint8_$(operation)_size.circ --as-bristol uint8_$(operation)_size.bristol;\
    )

run-sim:
	@$(CIRCUIT_SIM) uint8_add_wrap_depth.circ --spec "INPUT_A_x := 8; INPUT_B_x := 127; print;"

# run-aby:
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 0 --spec "INPUT_B := 35; print;" &
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 1 --spec "INPUT_A := 25; print;"


clean:
	rm -f mpc_main.circ mpc_main.stats

clean_all:
	rm -f *.circ mpc_main.stats *.txt *.dot