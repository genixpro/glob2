# PR 394 comparison on master 1ef59aab3

Built release clients at master `1ef59aab36ef764b45289fe271bab1b12ecd82c8` and PR `726497d9e8c278a33015b59cc7cc101523faf50f` using `CCACHE=1 scons -j6 release=1 server=0` on macOS arm64. The same seeded scenarios produced byte-identical raw per-tick checksums on both builds. Numbi initial and final saves also matched byte for byte. Master already contains the separate Numbi saved-RNG correction (#401). These scenarios do not exercise every gameplay path.

- Cross replay: `python3 test/run-browser-determinism.py build/darwin/client/release/src/glob2 <output-dir>` on each commit, using `games/cross-replay.game.gz`, seed 42 and 1,500 ticks. Raw trace SHA-256: `a1fa3456711d78346f13bb85582224231060faed2b93a428c8e390e5a7ba94b8`.
- Numbi/Castor: `build/darwin/client/release/src/glob2 --run-game --generator 15 --map-seed 42 --param teams=2 --game-seed 19 --player numbi --player castor --ticks 3000 --telemetry checksums --save initial --save final --output-dir <absolute-output-dir>`. Raw trace SHA-256: `d38e424e503157c57f83cde19bd0e158ae4c1a03a99da280a8c3dde360749c3a`. Initial save SHA-256: `8b3eefa69015c9ced3edfe6ed42860403053f6f006a50fbbfb32fdeb51610d50`. Final save SHA-256: `dba47a37934655ace13d8346370f25656b34f0f6fe12b3de37ffd2c6e439e4f5`.

The `.checksums.gz` files are gzip-compressed raw binary sidecars. `manifest.json` records compressed and raw hashes and sizes. Decompress both files for a scenario and compare them byte for byte.

`python3 test/check_parallel_compute.py <PR binary> --baseline <master binary> --output <artifact-dir>` also passed: exact traces, replay bytes, final saves, and shared-runtime/Castor continuation at 1, 2, 4 and 8 threads. Its full local output is generated on rerun and is not on this branch; rerun the command for those artifacts.
