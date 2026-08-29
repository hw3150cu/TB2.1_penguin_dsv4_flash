# Terminal-Bench 2.1 Report — DeepSeek-V4-Flash One DGX Spark

## Official score

**0.752809** under the predeclared aggregation: mean of the two scoreable attempt rewards per task, then mean over all 89 tasks (equivalent to 67.00 / 89). Harbor `pass@2` = 0.7978.

All published pass/fail results are verifier-backed, trace-backed, and free of unresolved timeout or inference-error outcomes.

## Aggregation

- Rule (locked before the scored run): mean of two attempt rewards per task; official score = mean over 89 tasks.
- Both attempts reported individually in `attempt-ledger.csv`. No cherry-picking.
- Calibrated `AgentTimeoutError` with trajectory + verifier is **model_fail** (allowance was validated). Uncalibrated/too-short timeouts would be `infra_invalid`.

## Per-attempt counts

| class | n |
|---|---|
| pass (scoreable) | 134 |
| model_fail (scoreable) | 44 |
| infra_invalid | 2 |
| scoreable total | 178 |
| tasks with exactly two valid attempts | **89 / 89** |

The two `infra_invalid` rows are extra attempts that were retried. Each of those tasks still has two scoreable, trace-backed attempts. Raw invalid artifacts are kept.

## Timeout calibration

- `--timeout-multiplier`: **3.172** (locked for the entire scored run)
- `--agent-setup-timeout-multiplier`: 4
- Method: `slowest_pilot_wall / base_agent_timeout * 1.25` headroom; any `AgentTimeout` under the provisional 2.5 multiplier raised the floor to `2.5 * 1.25`; clamp [, 6]
- Conservative stable decode: 0.8738 tok/s
- Calculation: `timeout-calibration.json`

## Provenance

| | |
|---|---|
| Harbor | 0.21.0 |
| Dataset | `terminal-bench/terminal-bench-2-1` (`sha256:7d7bdc1cbedad549fc1140404bd4dc45e5fd0ea7c4186773687d177ad3a0699a`) |
| Agent | `harbor_penguin:PenguinCli` (Penguin 0.2.2) |
| Harbor model | `openai/deepseek-v4-flash-0731` |
| Served ID | `deepseek-v4-flash-0731` |
| Image | `ghcr.io/0xsero/deepseek-v4-flash-0731-spark-sparkinfer@sha256:2e077489a83a0360952828051fe7f7a32c1801e5ce8436d85f7267583d614ff4` |
| Weights rev | `22f28d32b9b29b4352eaa380ff8c2c170b2847ab` |
| Endpoint | `http://192.168.100.11:8888/v1` (DGX2 One-DGX; Harbor on DGX1) |
| Hardware | NVIDIA GB10 ×1, 121 GiB UMA, aarch64 |
| Concurrency / k | n=1, n-attempts=2 |
| Agent params | thinking_level=xhigh; skills=false; memory=false; PENGUIN_MAX_TOKENS=256000; PENGUIN_CONTEXT_WINDOW=128000 |

## Oracle smoke

Harbor `oracle` on the first five TB2.1 tasks: **5/5**, mean_reward=1.0, verifier artifacts in `oracle-smoke/`.

## Incident table (infra_invalid)

| task | attempt | run_id | classification | resolution |
|---|---|---|---|---|
| protein-assembly | 2 | `protein-assembly__3zYVGsL` | `ApiRateLimitError` (infra) | valid retries `qHHQCmX` + `AjmtJxx` (both scoreable model_fail) |
| torch-pipeline-parallelism | 1 | `torch-pipeline-parallelism__Gzyj4D2` | missing trajectory; verifier incomplete | valid retries `piR2RR6` + `bSYyUjB` (both scoreable model_fail) |

Thermal/host pauses are logged in `incidents.jsonl` and did not leave any task without two valid attempts.

## Integrity statement

Terminal-Bench 2.1 was run through Harbor on the official dataset under one predeclared timeout policy. Every published pass/fail has completed verifier evidence and a healthy inference trajectory. Every harness/inference fault is preserved as `infra_invalid` and was resolved by a valid retry. The benchmark is complete; a score is published.
