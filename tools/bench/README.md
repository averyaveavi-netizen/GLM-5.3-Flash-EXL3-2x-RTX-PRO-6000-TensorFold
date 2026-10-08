# Benchmark and exactness tools

Use Python 3.10+, `requests`; Pillow is needed only for the optional image gate. Defaults are `http://127.0.0.1:8022/v1`
and model `glm-5.3-flash`; override with `--base` / `--model` or `TF_BENCH_BASE` /
`TF_BENCH_MODEL`. All scripts support `--help` without an endpoint. Results and
references stay local; do not publish generated prompts, replies or credentials.

## Exactness

Start the known baseline, then run:

```bash
python3 tools/bench/tf_bench.py --id baseline --ref-tag=-tp2-q4 \
  --reference-dir ./bench-references --gate-levels 2,4 \
  --gate-write-ref --no-telemetry gate gatelong
# Start the candidate with the same checkpoint and numerical settings.
python3 tools/bench/tf_bench.py --id candidate --ref-tag=-tp2-q4 \
  --reference-dir ./bench-references --gate-levels 2,4 \
  --no-telemetry gate gatelong
```

`gate` checks drafted against serial replies, concurrent against solo replies,
and five tool calls. Add `--gate-vision` to include an image with the default `VISION=1` server;
the release's image checks passed 3/3. `gatelong` checks fixed 8K/32K/100K
prompts against saved hashes. Missing hashes or reference entries fail. Baseline
creation prints `BASELINE`, not proof of agreement with an absent reference;
failed gates never write reference files. Keep separate references for each
checkpoint and intentional numerical change. Inspect `pass_`, `fails` and
`reference_status` in the result JSON; the runner exit status is not a verdict.
These gates require TensorFold's token-hash extension.

## Speed and latency

```bash
python3 tools/bench/tf_bench.py --id two-gpu-production --no-telemetry \
  --agg-levels 2,4 --speed-variants kk,sampled,nucleus \
  --prefill-sizes 32000,100000,140000 speed3 prefill3 warm
```

`speed3` uses 512-token replies and three fixed seeds. Single-stream throughput
is the mean of code/prose medians; aggregate throughput includes first-token
latency and uses the median of three batches. `sampled` means temperature 1,
top_p 0.95 and omitted top_k, which TensorFold serves as 20; send top_k explicitly
when comparing sampled TensorFold runs. `kk` uses
temperature 0.7/top_p 0.95; `nucleus` explicitly adds top_k -1 to `sampled`.
Aggregate requests always use `sampled`, regardless of `--speed-variants`.
The README's sparkDash results use a separate client and workload.

`prefill3` reports median TTFT for three cold prompts per size with fresh nonces.
`warm` measures turns in a long conversation. Keep client revision, prompts,
settings, cache state and power caps consistent. Optional telemetry is disabled
by the commands above. Performance results have no hardcoded reference measurements or acceptance thresholds.
Results default to `tools/bench/results/<id>/`; `--results-dir` must end in
`/results`. `--bench-client` can select a compatible replacement helper.

## Quality probes

```bash
python3 tools/bench/toolburst.py --out results/production/toolburst.json
python3 tools/bench/agree.py --out results/baseline/agree.json
python3 tools/bench/agree.py --out results/candidate/agree.json \
  --compare results/baseline/agree.json
python3 tools/bench/first_tokens.py --source-tree ./TensorFold --n 100 \
  --out results/baseline/first-tokens.json
python3 tools/bench/first_tokens.py --source-tree ./TensorFold --n 100 \
  --out results/candidate/first-tokens.json --compare results/baseline/first-tokens.json
python3 tools/bench/json_variants.py --contracts-file /path/to/contracts.py \
  --out results/production/json-variants.json
```

`toolburst` checks 30 three-call bursts with default and required tool choice.
`agree` scores 40 fixed answers; `first_tokens` compares greedy token IDs and
common-prefix lengths, not KL divergence. Use the identical source tree in both
arms; corpus changes require new references.
Absent returned IDs make token comparison unavailable.

The original JSON-contract cases and qualification fixtures are not bundled.
`json_variants` requires a trusted Python file with `JSON_CASES` and `check_json`
(`TF_BENCH_CONTRACTS` also works); record which cases were used. Optional phase `qualify` skips when no `--qualifier` is provided.
Its external script must accept `--base URL --model ID --out FILE` and write JSON
with pass-count strings `tools`, `json`, `image`; numeric `tools_share`,
`json_share`, `decode_tok_s`; boolean `effort_ok`; and `context` details.
These small probes do not establish general model quality.
