# Qwen3-8B HBM Supplementary Experiments

This repository organizes five selected Qwen3-8B experiment groups and their benchmark results: an FP16 reference, clean and fault-injected INT8 baselines, an INT8 adaptation of SPECC (Sparrow ECC), and SRLR.

## Selected results

Accuracy values from the selected result table:

| Experiment | MathQA | MMLU | HumanEval |
|---|---:|---:|---:|
| `fp16-clean` | 84.90% | 72.30% | 82.32% |
| `int8-clean` | 84.90% | 72.20% | 83.54% |
| `int8-ber003` | 24.41% | 43.99% | 37.81% |
| `specc` | 84.39% | 72.36% | 82.87% |
| `srlr` | 80.40% | 71.13% | 78.17% |

## Repository structure

- `fp16-clean/`: FP16 reference experiment.
- `int8-clean/`: clean INT8 baseline.
- `int8-ber003/`: unprotected INT8 baseline with BER 0.003 fault injection.
- `specc/`: SPECC (Sparrow ECC), adapted for this INT8 experiment. Each high nibble is encoded with Hamming(7,4); encoded high-nibble bits are excluded from fault injection. BER 0.003 one-to-zero faults are injected only into eligible raw low-nibble bits. Since high-nibble codeword bits are not faulted, this run does not measure ECC recovery from high-nibble errors.
- `srlr/`: SRLR experiment, including the MathQA replay worker and configuration used for the three-benchmark result set.

## Experiment configuration

The INT8 experiments use symmetric W8A16 RTN quantization with group size 128 and BF16 compute. Protection and fault injection cover the 252 non-`lm_head` linear weight matrices specified in the experiment configurations; embeddings, `lm_head`, scales, and biases are excluded.

For the SPECC INT8 adaptation, eligible raw low-nibble bits use a one-to-zero fault model at BER 0.003. Hamming(7,4)-encoded high-nibble bits are excluded from injection. SRLR uses the BER and payload definition recorded in its own configuration.
