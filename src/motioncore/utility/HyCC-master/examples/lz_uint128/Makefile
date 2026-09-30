CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/build/bin/aby-hycc
CIRCUIT_UTILS=../../bin/circuit-utils

.PHONY: clean run-sim run-aby

operations:= add
# operations:= sub
# operations:= gt
# operations:= geq
# operations:= eqz
# operations:= mul
# operations:= div # div take very long time, get killed in VMWare
# operations:= div
all:
	make clean_all
	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) uint128_$(operation).c --low-depth --minimization-time-limit 72000 --unwind 1000000 --outline;\
		mv mpc_main.circ uint128_$(operation)_depth.circ;\
		mv mpc_main.stats uint128_$(operation)_depth.stats;\
		$(CIRCUIT_UTILS) uint128_$(operation)_depth.circ --as-bristol uint128_$(operation)_depth.bristol;\
    )

	$(foreach operation,$(operations),\
        echo $(operation); \
		$(CBMC_GC) uint128_$(operation).c --minimization-time-limit 72000 --unwind 1000000 --outline;\
		mv mpc_main.circ uint128_$(operation)_size.circ;\
		mv mpc_main.stats uint128_$(operation)_size.stats;\
		$(CIRCUIT_UTILS) uint128_$(operation)_size.circ --as-bristol uint128_$(operation)_size.bristol;\
    )

run-sim:
	@$(CIRCUIT_SIM) uint128_add_wrap_depth.circ --spec "INPUT_A_x := 128; INPUT_B_x := 127; print;"

# run-aby:
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 0 --spec "INPUT_B := 35; print;" &
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 1 --spec "INPUT_A := 25; print;"


clean:
	rm -f *.circ *.stats *.bristol

clean_all:
	rm -f mpc_main.circ mpc_main.stats *.txt *.dot