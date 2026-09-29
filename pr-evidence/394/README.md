# PR 394 comparison evidence

Built release clients at master `300ded8d6` and PR head `8efcd5047` on macOS arm64 with `CCACHE=1 scons -j6 release=1 server=0`. The two saved per-tick traces match byte for byte in each scenario. These scenarios do not exercise every Numbi upgrade decision; the `syncRand()` correction still needs independent human review.

- `cross-replay`: `python3 test/run-browser-determinism.py build/darwin/client/release/src/glob2 <output-dir>` on each commit, using the committed `games/cross-replay.game.gz` fixture, seed 42, 1,500 ticks. Both raw traces SHA-256: `cd2530894aa9cb3b1c1b96f6946b50bd61e1a2ad40cbfbc6b5308e95448638d6`. Fixture SHA-256: `28e1b5b701b877d13e02547c4d874712e1973c2457210915c83c5f4cf376e09f`.
- `numbi`: `build/darwin/client/release/src/glob2 --run-game --generator 15 --map-seed 42 --param teams=2 --game-seed 19 --player numbi --player castor --ticks 3000 --telemetry checksums --save initial --save final --output-dir <absolute-output-dir>`. Both raw traces SHA-256: `09908226ec259ce52c8a7b19596463c2257c2644d75d64281f26f0d385c89e9e`. The initial and final save files also match byte for byte between commits, with SHA-256 `5db4d833aa3a0058eca2b76633fc050c10c51bbaec7e2d4e244ecc8d34a90c15` and `a58e204130de1e880dcf17c7b92896af647f12539962b404eb7bd90b2d466935`.

The `.checksums.gz` files are gzip-compressed raw binary sidecars. `manifest.json` records the compressed and raw hashes and sizes. Decompress both files for a scenario and compare them byte for byte.
