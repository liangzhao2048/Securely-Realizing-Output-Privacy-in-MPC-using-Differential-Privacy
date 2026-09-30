CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/build/bin/aby-hycc
CIRCUIT_UTILS=../../bin/circuit-utils

.PHONY: clean run-sim run-aby

all: clean uint8ToInt8

uint8ToInt8: uint8ToInt8.c
	$(CBMC_GC) $^ --low-depth --outline
	# $(CIRCUIT_UTILS) mpc_main.circ --as-bristol uint8ToInt8_depth.txt
	# make clean

run-sim:
	@$(CIRCUIT_SIM) mpc_main.circ --spec "INPUT_A_x := 3; INPUT_B_x := 5; print;"

# run-aby:
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 0 --spec "INPUT_B := 35; print;" &
# 	@$(ABY_CBMC_GC) mpc_main.circ -r 1 --spec "INPUT_A := 25; print;"

clean:
	rm -f mpc_main.circ mpc_main.stats
