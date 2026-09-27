# Qwen3-8B selected experiments

This folder contains only the five code groups behind the selected result table. It is intended as a source handoff and GitHub-ready code archive; model weights, datasets, masks, logs, and full benchmark outputs are not included.

## Selected results

Percent accuracy as shown in the final table supplied for this archive:

| Code folder | MathQA | MMLU | HumanEval |
|---|---:|---:|---:|
| `fp16-clean` | 84.90% | 72.30% | 82.32% |
| `int8-clean` | 84.90% | 72.20% | 83.54% |
| `int8-ber003` | 24.41% | 43.99% | 37.81% |
| `specc` | 84.39% | 72.36% | 82.87% |
| `srlr` | 80.40% | 71.13% | 78.17% |

## Folders

- `fp16-clean`: FP16 reference worker and its run configuration.
- `int8-clean`: symmetric INT8 W8A16 RTN, group size 128, BF16 compute.
- `int8-ber003`: INT8 BER 0.003 one-to-zero fault injection without protection.
- `specc`: high-nibble Hamming(7,4) encoding and low-nibble-only fault injection, using the selected teacher-protocol worker.
- `srlr`: INT8 SRLR worker plus the MathQA replay worker/config used for the three-task result set.

The INT8 protection runs use the Qwen3-8B snapshot and the 252 non-`lm_head` linear weight matrices recorded in their configs. Embeddings, `lm_head`, scales, and biases are outside the protected/injected payload. The SPECC config uses BER 0.003 over eligible raw low-nibble bits; its high-nibble codeword bits are excluded from injection. SRLR uses the BER and payload definition recorded in its config.

## Running and required inputs

Each worker expects to be launched with its group folder as the working directory and its JSON config as the first argument, for example:

```bash
cd int8-clean
python worker.py config.json
```

The configured model and fixed benchmark datasets must be made available in the relative locations expected by the worker/config. They are not copied into this source archive. The SRLR MathQA replay expects the SRLR run's frozen artifacts to exist before replaying MathQA.

## Privacy and provenance

The packaged source contains no account credentials or personal machine profile paths. Server-root literals were changed to folder-relative roots where needed, and identity-like shorthand in code identifiers/log labels was replaced with neutral protocol wording; these changes do not alter sampling or evaluation logic. `provenance.json` in each folder records source and packaged SHA-256 hashes and those portability edits. Runtime outputs are directed to a relative `results_int8/` folder under the selected code group.
