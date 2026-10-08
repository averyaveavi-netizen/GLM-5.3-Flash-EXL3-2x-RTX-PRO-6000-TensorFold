# TensorFold patch port history

The current recipe targets TensorFold v0.6.6 at `cb2ebf0540f42604e2759b2ddef497861e928248`.
See [Rebase to v0.6.6](#rebase-to-v066) and [current validation notes](NOTES.md#tensorfold-v066).
The v0.6.5 port and its measurements below are retained as history.

## Historical v0.6.5 port

Historical upstream: `609ca419abecebdc5a059498a613680bd3aa847f`. The 87-patch source series became 86 patches: 63 unchanged, 23 adapted, one supplied upstream. Patch numbers remained stable.

The image still copies runtime C/CUDA headers. Served defaults retain thinking off, the checkpoint's earlier-turn clearing (`TF_GLM_CLEAR_THINKING=1`), and unauthenticated routes unless `TENSORFOLD_API_KEY` is explicitly configured. With a key, `/health` stays open and `/metrics` requires authentication.

| Patch | Status | Reason |
| --- | --- | --- |
| 0000-glm-exl3-route.patch | unchanged | Applies byte-for-byte without offsets. |
| 0001-glm-exl3-prompt-experts.patch | unchanged | Applies byte-for-byte without offsets. |
| 0002-glm-dense-fp8.patch | unchanged | Applies byte-for-byte without offsets. |
| 0003-glm-vision.patch | adapted | Merge GLM image/video frontend with upstream tool-result images (4825c01), video input (6091427), visual-token budgets (362610f), and clear_thinking (ad15127). Keep GLM frame sampling and recipe media byte limits. |
| 0004-glm-prompt-kernels.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0005-glm-dense-q4.patch | unchanged | Applies byte-for-byte without offsets. |
| 0006-cuda-roce-allgather.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0007-glm-copy-drafts.patch | unchanged | Applies byte-for-byte without offsets. |
| 0008-glm-prompt-grid.patch | unchanged | Applies byte-for-byte without offsets. |
| 0009-glm-prefill-kernels.patch | unchanged | Applies byte-for-byte without offsets. |
| 0010-glm-hc-split.patch | adapted | Keep prompt row exchanges, upstream communicator protocol/dtypes, timeout_s and rendezvous key (c3d92bb). Final asymmetric NCCL compatibility is retained by 0087. |
| 0011-glm-draft-sim.patch | unchanged | Applies byte-for-byte without offsets. |
| 0012-glm-kda-chunked.patch | unchanged | Applies byte-for-byte without offsets. |
| 0013-glm-decode-rounds.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0014-glm-kda-chunked-gb10.patch | unchanged | Applies byte-for-byte without offsets. |
| 0015-glm-shared-prefix.patch | unchanged | Applies byte-for-byte without offsets. |
| 0016-glm-decode-kernels.patch | unchanged | Applies byte-for-byte without offsets. |
| 0017-glm-overlap-priority.patch | unchanged | Applies byte-for-byte without offsets. |
| 0018-glm-noise-policies.patch | unchanged | Applies byte-for-byte without offsets. |
| 0019-glm-decode-kernels2.patch | unchanged | Applies byte-for-byte without offsets. |
| 0020-glm-prompt-experts-order.patch | unchanged | Applies byte-for-byte without offsets. |
| 0021-glm-dflash-policy-env.patch | unchanged | Applies byte-for-byte without offsets. |
| 0022-glm-overlap-normal-priority.patch | unchanged | Applies byte-for-byte without offsets. |
| 0024-glm-prompt-select-rows.patch | unchanged | Applies byte-for-byte without offsets. |
| 0025-glm-dflash2-ring.patch | unchanged | Applies byte-for-byte without offsets. |
| 0026-glm-multi-kda.patch | unchanged | Applies byte-for-byte without offsets. |
| 0027-glm-multi-dflash2.patch | unchanged | Applies byte-for-byte without offsets. |
| 0028-glm-lean-prompt-scratch.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0029-glm-multi-dsa.patch | unchanged | Applies byte-for-byte without offsets. |
| 0030-glm-multi-stream-engine.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0031-glm-decode-index-regs.patch | unchanged | Applies byte-for-byte without offsets. |
| 0032-glm-code-copy-drafts.patch | unchanged | Applies byte-for-byte without offsets. |
| 0033-glm-prefill-overlap2.patch | unchanged | Applies byte-for-byte without offsets. |
| 0034-cuda-nucleus-union.patch | unchanged | Applies byte-for-byte without offsets. |
| 0035-glm-multi-rounds.patch | unchanged | Applies byte-for-byte without offsets. |
| 0036-glm-tool-calls.patch | adapted | Combine recipe tool-call streaming/history recovery with upstream clear_thinking (ad15127), stop-sequence reporting (e9fade0), and max_tokens-inside-thinking warning (bcb8f01). |
| 0037-cuda-tokenize.patch | dropped-upstream | 8f5e680 (integrated by 45325f7): CUDA token-id prompts, tokenize/detokenize aliases, special-token and generation flags, vocabulary validation; upstream also supports return_token_strs. Only redundant unused helper/constants remained. |
| 0038-glm-kv-fp8.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0039-glm-kda-chunked-kernel.patch | unchanged | Applies byte-for-byte without offsets. |
| 0040-glm-parallel-deadlocks.patch | unchanged | Applies byte-for-byte without offsets. |
| 0041-glm-parallel-ring-base.patch | unchanged | Applies byte-for-byte without offsets. |
| 0042-glm-prompt-replay.patch | unchanged | Applies byte-for-byte without offsets. |
| 0043-glm-visible-pools.patch | unchanged | Applies byte-for-byte without offsets. |
| 0044-cuda-context-errors.patch | adapted | Upstream 8f5e680 already supplies text context errors; retain the GLM vision context_length_exceeded wording added by this patch. |
| 0045-cuda-metrics.patch | adapted | Keep live GLM pool/health metrics and snapshot guards alongside upstream per-request decode counters (c9259c3), process footprint (0a57d1f), and HTTP/auth counters (bcb8f01). |
| 0046-glm-l2-prefetch.patch | unchanged | Applies byte-for-byte without offsets. |
| 0047-glm-exl3-decode-loads.patch | unchanged | Applies byte-for-byte without offsets. |
| 0048-glm-timing-tokens.patch | unchanged | Applies byte-for-byte without offsets. |
| 0049-glm-multi-prefill.patch | unchanged | Applies byte-for-byte without offsets. |
| 0050-glm-many-media.patch | adapted | Keep custom GLM image/video loaders and 96 MiB request limit; use upstream chunked-body reader (dc8e9eb) with an explicit limit and preserve configurable visual-token budgets. |
| 0051-glm-tool-history-recovery.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0052-cuda-roce-startup.patch | unchanged | Applies byte-for-byte without offsets. |
| 0053-glm-whole-tool-calls.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0054-glm-tp-n.patch | adapted | Pass recipe world size to upstream open_comm (7933ed6); retain arbitrary local rank counts and rank-ordered reductions. |
| 0055-glm-tp3-split-pad.patch | unchanged | Applies byte-for-byte without offsets. |
| 0056-glm-tpn-split-buffer-rows.patch | unchanged | Applies byte-for-byte without offsets. |
| 0057-glm-parallel-cap.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0058-cuda-ipc-allgather.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0059-cuda-nccl-exchange-comm.patch | adapted | Second NCCL exchange communicator remains isolated; preserve upstream timeout_s and key parameters (c3d92bb), derive exchange key from the supplied rendezvous key. |
| 0060-glm-index-split.patch | unchanged | Applies byte-for-byte without offsets. |
| 0061-glm-prefill-digest.patch | unchanged | Applies byte-for-byte without offsets. |
| 0062-glm-hc-exchange-ce.patch | unchanged | Applies byte-for-byte without offsets. |
| 0063-glm-prefill-lanes.patch | unchanged | Applies byte-for-byte without offsets. |
| 0064-glm-launch-tables.patch | unchanged | Applies byte-for-byte without offsets. |
| 0065-gpu-sampling.patch | unchanged | Applies byte-for-byte without offsets. |
| 0066-glm-priority-lanes.patch | adapted | Keep priority scheduling while using upstream token_routes and flag helpers; avoid restoring helpers made redundant by dropped 0037. |
| 0067-glm-segment-grids-by-rows.patch | unchanged | Applies byte-for-byte without offsets. |
| 0068-glm-kept-prompt-arrays.patch | unchanged | Applies byte-for-byte without offsets. |
| 0069-glm-decode-fusions.patch | unchanged | Applies byte-for-byte without offsets. |
| 0070-cuda-ipc-allgather-protocols.patch | unchanged | Applies byte-for-byte without offsets. |
| 0071-glm-prefill-2.patch | adapted | Retain copy-engine exchange and draft-prefill optimization; keep caller-supplied communicator rendezvous key. |
| 0072-glm-kda-split.patch | unchanged | Applies byte-for-byte without offsets. |
| 0073-glm-expert-prompt-kernels.patch | unchanged | Applies byte-for-byte without offsets. |
| 0074-glm-moe-glue.patch | unchanged | Applies byte-for-byte without offsets. |
| 0075-glm-draft-fast.patch | unchanged | Applies byte-for-byte without offsets. |
| 0076-glm-prefill-3.patch | unchanged | Applies byte-for-byte without offsets. |
| 0077-glm-wide-windows.patch | adapted | Rebased hunk context/line positions; three-way merge preserves both changes. |
| 0078-glm-round-cap-twoshot.patch | unchanged | Applies byte-for-byte without offsets. |
| 0079-glm-warm-turn-incremental.patch | adapted | Keep warm-turn encoding reuse with upstream token-route validation helpers, including return_token_strs. |
| 0080-glm-topk-sparse-ws.patch | unchanged | Applies byte-for-byte without offsets. |
| 0081-glm-lane-inputs.patch | unchanged | Applies byte-for-byte without offsets. |
| 0082-glm-lane-partials.patch | unchanged | Applies byte-for-byte without offsets. |
| 0083-glm-draft-ahead-shared-ends.patch | unchanged | Applies byte-for-byte without offsets. |
| 0084-glm-decisions-tp-n.patch | unchanged | Applies byte-for-byte without offsets. |
| 0085-glm-port-settings-check.patch | unchanged | Applies byte-for-byte without offsets. |
| 0086-glm-port-lane-exl3-view.patch | unchanged | Applies byte-for-byte without offsets. |
| 0087-glm-port-stage2-routes.patch | adapted | Preserve GLM route guards and seed salt; retain upstream c3d92bb asymmetric NCCL exchanges, while optional copy-engine exchanges require symmetric buffers. |

## Historical v0.6.5 validation

The fresh-clone preparation/start/smoke/check/stop flow passed on GPUs 0–1 at 300 W each. Drafted/serial and solo/concurrent hashes matched the v0.6.2 references, including sampled and greedy concurrency at two and four requests, tools, image input, and 8K/32K/100K long prompts. A conversation replay after restart matched its cached continuation.

The capacity check accepted 524,280 prompt tokens plus an eight-token reply budget and returned two tokens with the unchanged 1,062,912-token shared pool. The live defaults probe confirmed thinking off and earlier-turn reasoning clearing. Startup logs confirmed CUDA IPC for copy-engine exchanges and index gathers.

The README retains the first user-flow benchmark sweep, including outliers. Matched old/new runs reproduced the cold-prompt latencies. A focused five-repeat comparison reduced initially larger decode gaps to less than one percent in the investigated aggregate-rate medians. Swapping only the saved autotune table did not consistently improve performance; the original table and defaults were restored. Small remaining differences do not establish zero engine overhead at sub-percent precision. Historical quality scores and the earlier simultaneous full-context image stress test were not rerun and remain labeled as historical.

## Rebase to v0.6.6

The current base is TensorFold v0.6.6 at `cb2ebf0540f42604e2759b2ddef497861e928248`.
This is a rebase only. All 86 filenames, header lines and patch order are preserved.
Mia's AI Lab retains credit for her 54 patches. The shared patches were regenerated once and reused
in both recipes. Eighty patch files remain byte-for-byte unchanged.

| Regenerated patch | Rebase change |
| --- | --- |
| `0003-glm-vision.patch` | Hunk line positions only. |
| `0036-glm-tool-calls.patch` | Hunk line positions only. |
| `0053-glm-whole-tool-calls.patch` | Hunk line positions only. |
| `0054-glm-tp-n.patch` | Hunk line positions only. |
| `0066-glm-priority-lanes.patch` | Hunk positions and context, plus the priority merge described below. |
| `0079-glm-warm-turn-incremental.patch` | Hunk positions and context around upstream's new `self.background_ids`. |

The engine adds `--name-priority ID=background`. In `src/tensorfold/cuda/server.py`, the `App.run`
conflict keeps both upstream's model-name background
default and the recipe's priority lanes. It passes `default=prio.BACKGROUND if by_name else None` to
`prio.request_class`. Explicit request priorities and title-request handling retain their precedence.
The later upstream comment change is retained with the same combined behavior.

Neither source revision contains optional patch files. Current digests are in
[the SHA256 table](PATCH-SHA256.md). The release validation above remains historical.

Each recipe's strict replay produced the expected Git tree
`9494ac6d2099ffe7fd9e33bfb412905ea5dc0f08`: its old patched tree plus upstream's three commits.
The recipe replay check, source compilation, all seven shell syntax checks and `DRY_RUN=1 ./start.sh`
passed for each recipe. A separate `App.run` check passed all 31 CPU priority cases for each recipe.
The upstream name-priority pytest module was skipped because `tokenizers`
was unavailable (pytest exit 5). No packages were installed. These checks used CPU only.
The later GPU A/B validation is recorded in [TensorFold v0.6.6 notes](NOTES.md#tensorfold-v066).
