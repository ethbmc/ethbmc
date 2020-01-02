# EVM automatic exploit generation

### Requirements

- Latest Rust nightly verison (can be built without nightly, this will disable benchmarking)
- the latest version of the (go-ethereum)[https://github.com/ethereum/go-ethereum] stand alone evm; make sure it is in your PATH
- we offer support for three different smt solvers:
	- (Z3)[https://github.com/Z3Prover/z3]
	- (Yices2)[https://github.com/SRI-CSL/yices2]
	- (Boolector)[https://github.com/Boolector/boolector]
- The serivce is expected to be used with a rocket.toml file
- When using the timeout mode the service expects a ethaeg binary in the path

### Building

```
	cargo build --release
```

### Binaries

- ethbmc: Simple stand alone binary
- ethbmc-service: Webservice which accepts accounts and returns the result as json
- ethbmc-scheduler: Accepts two lists, an account list and a server list, then distributes the accounts between the backing server running the service

### Benchmarks

Executing benchmarks takes around 2 hours on a Ubuntu 16.04 machine with Intel(R) Core(TM) i7-6700 CPU @ 3.40GHz and 24GB RAM.

#### Current Benchmark 

```
	test benchmark_tests::control_flow_hijack            ... bench: 199,200,421 ns/iter (+/- 7,172,259)
	test benchmark_tests::delegate_call                  ... bench: 3,943,478,623 ns/iter (+/- 945,873,681)
	test benchmark_tests::proxy_call                     ... bench: 1,605,270,864 ns/iter (+/- 343,546,710)
	test benchmark_tests::unprotected_function_call      ... bench: 1,018,462,696 ns/iter (+/- 35,504,563)
	test benchmark_tests::unprotected_function_call_args ... bench: 1,073,372,483 ns/iter (+/- 96,758,747)
	test benchmark_tests::unprotected_function_suicide   ... bench: 1,069,503,964 ns/iter (+/- 184,508,410)
```

### Tests

- test can only be run one at a time at the moment (cargo test -- --test-threads=1)
- Integration tests take a long time and should allways be run with optimizations (cargo test integra --release -- --ignored)
- Some tests need PARITY_IP env var set, naming starts with parity (PARITY_IP=127.0.0.1 cargo test parity -- --test-threads=1 --ignored)

You can run all test with the supplied shell script (requires parity backend):
```
	PARITY_IP=127.0.0.1 ./run_all_test.sh
```
