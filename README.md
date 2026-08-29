# TB2.1 Penguin DeepSeek-V4-Flash

Immutable Harbor Terminal-Bench 2.1 delivery bundle.

**Official score: 75.28%** (mean of two attempts, then mean over 89 tasks).

Required files: `manifest.json`, `environment.json`, `preflight.json`, `timeout-calibration.json`, `oracle-smoke/`, `jobs/scored-full89/`, `traces/`, `server-telemetry/`, `incidents.jsonl`, `attempt-ledger.csv`, `summary.json`, `REPORT.md`.

See [`REPORT.md`](REPORT.md).

The `sanitize-git-repo` traces contain Terminal-Bench task fixtures (fake `ghp_` / `AKIA` strings). One DCLM fixture Hugging Face token was replaced with `hf_REDACTED-TB21-SANITIZE-GIT-REPO-FIXTURE` so GitHub secret scanning does not treat the published bundle as a credential leak. It is not a user token.
