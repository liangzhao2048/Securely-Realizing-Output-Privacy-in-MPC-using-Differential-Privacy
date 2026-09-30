CBMC_GC = ../../bin/cbmc-gc
CIRCUIT_SIM = ../../bin/circuit-sim
ABY_CBMC_GC = ../../../ABY/bin/aby-cbmc-gc.exe

.PHONY: clean run-sim run-aby

output.gate.txt: main.c
	$(CBMC_GC) $^

run-sim:
	@$(CIRCUIT_SIM) --spec "INPUT_A := 24; INPUT_B := 42; print;"

run-aby:
	@$(ABY_CBMC_GC) -r 0 --spec "INPUT_B := 35; print;" &
	@$(ABY_CBMC_GC) -r 1 --spec "INPUT_A := 25; print;"

clean:
	rm -f output.*.txt
