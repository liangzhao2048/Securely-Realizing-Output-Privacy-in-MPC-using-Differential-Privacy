CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/bin/aby-cbmc-gc.exe

.PHONY: clean run-sim run-aby

output.gate.txt: main.c
	$(CBMC_GC) $^ --minimization-time-limit 10

run-sim:
	@$(CIRCUIT_SIM) --spec "INPUT_A := {x: 3; y: 9;}; INPUT_B := {x: -4; y: 5;}; print;"

run-aby:
	@$(ABY_CBMC_GC) -r 0 --spec "INPUT_B := {x: -4; y: 5;}; print;" &
	@$(ABY_CBMC_GC) -r 1 --spec "INPUT_A := {x: 3; y: 9;}; print;"

clean:
	rm -f output.*.txt
