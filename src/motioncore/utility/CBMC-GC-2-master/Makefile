.PHONY: all clean test create-dirs build-test-default build-test-lowdepth run-test-default run-test-lowdepth

export USE_ABC

all:
	mkdir -p bin

	cd src/cbmc/src && $(MAKE)
	cp src/cbmc/src/cbmc/cbmc bin/cbmc

	cd src/cbmc-gc && $(MAKE)
	cp src/cbmc-gc/cbmc-gc bin/cbmc-gc

	cd src/circuit-utils && $(MAKE)

clean:
	rm -f bin/cbmc
	rm -f bin/cbmc-gc
	cd src/cbmc/src && $(MAKE) clean
	cd src/cbmc-gc && $(MAKE) clean
	cd src/circuit-utils && $(MAKE) clean
	-cd test-default && $(MAKE) clean
	-cd test-lowdepth && $(MAKE) clean


test: build-test-default build-test-lowdepth

run-test: run-test-default run-test-lowdepth

test-default:
	cp -r test-src test-default
	
test-lowdepth:
	cp -r test-src test-lowdepth
	echo "--low-depth" >> test-lowdepth/CBMC_GC_FLAGS

build-test-default: test-default
	cd test-default && $(MAKE)

build-test-lowdepth: test-lowdepth
	cd test-lowdepth && $(MAKE)

run-test-default: test-default
	cd test-default && $(MAKE) run

run-test-lowdepth: test-lowdepth
	cd test-lowdepth && $(MAKE) run


minisat2-download:
	cd src/cbmc/src && $(MAKE) minisat2-download
