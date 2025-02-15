# Compare Substrate Weight Files

Compares weight files that where generated by Substrate.  
It currently only analyzes the constant factor of the weight. Parsing the linear aspect is missing.

## Example: Web Interface

```sh
git submodule update --init # This takes a while
cargo r --bin web --release
```

## Example: Weight files

Suppose you have some weight files in `old_weights` and `new_weights`. Compare them with:

```sh
cargo r --bin cli -- --old old_weights/* --new new_weights/*
```

## Example: Polkadot Commits

Compare arbitrary Polkadot commits with the [compare.sh](compare.sh) script.   It has the [Polkadot](https://github.com/paritytech/polkadot) as submodule twice. 
Cloning will take a while. It then checks out the two commits and compares the weights of the Polkadot runtime.

```sh
# Set the threshold to 30% for less output.
$ git clone https://github.com/ggwpez/substrate-weight-compare
cd substrate-weight-compare
./compare.sh 20467ccea1ae ef922a7110eb --threshold 30

pallet_scheduler.rs::on_initialize_named_aborted 4957 -> 3406 ns (-31.29 %)
pallet_election_provider_multi_phase.rs::finalize_signed_phase_reject_solution 33389 -> 19348 ns (-42.05 %)
pallet_election_provider_multi_phase.rs::submit 77368 -> 42754 ns (-44.74 %)
pallet_election_provider_multi_phase.rs::on_initialize_nothing 23878 -> 12324 ns (-48.39 %)
pallet_election_provider_multi_phase.rs::finalize_signed_phase_accept_solution 50596 -> 25888 ns (-48.83 %)
pallet_scheduler.rs::on_initialize_resolved 3886 -> 1701 ns (-56.23 %)
pallet_election_provider_multi_phase.rs::on_initialize_open_unsigned 33568 -> 12320 ns (-63.30 %)
pallet_election_provider_multi_phase.rs::on_initialize_open_signed 34547 -> 12500 ns (-63.82 %)
pallet_election_provider_multi_phase.rs::create_snapshot_internal 8835233 -> 47360 ns (-99.46 %)
pallet_scheduler.rs::on_initialize_periodic_resolved 33 -> 0 ns (-100.00 %)
frame_benchmarking_baseline.rs::subtraction 100 -> 131 ns (+31.00 %)
frame_benchmarking_baseline.rs::addition 96 -> 126 ns (+31.25 %)
pallet_scheduler.rs::on_initialize_periodic 5139 -> 7913 ns (+53.98 %)
runtime_common_crowdloan.rs::on_initialize 0 -> 4293 ns (+100.00 %)
```
It prints first the ones that decreased (good) and then the ones that increased (bad) sorted by ascending absolute value.
