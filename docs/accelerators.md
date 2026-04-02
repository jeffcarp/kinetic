# Accelerator Support

Each accelerator and topology requires setting up its own node pool as a prerequisite.

## TPUs

| Type           | Configurations                                                                                                                |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| TPU tpu-v6e        | `tpu-v6e-8`, `tpu-v6e-16`                                                                                                             |
| TPU tpu-v5p        | `tpu-v5p-8`, `tpu-v5p-16`, `tpu-v5p-32`                                                                                                   |
| TPU v5 Litepod | `tpu-v5e-1`, `tpu-v5e-4`, `tpu-v5e-8`, `tpu-v5e-16`, `tpu-v5e-32`, `tpu-v5e-64`, `tpu-v5e-128`, `tpu-v5e-256` |
| TPU tpu-v4         | `tpu-v4-4`, `tpu-v4-8`, `tpu-v4-16`, `tpu-v4-32`, `tpu-v4-64`, `tpu-v4-128`, `tpu-v4-256`, `tpu-v4-512`, `tpu-v4-1024`, `tpu-v4-2048`, `tpu-v4-4096`                      |
| TPU tpu-v3         | `tpu-v3-4`, `tpu-v3-16`, `tpu-v3-32`, `tpu-v3-64`, `tpu-v3-128`, `tpu-v3-256`, `tpu-v3-512`, `tpu-v3-1024`, `tpu-v3-2048`                                         |

## GPUs

| Type             | Aliases                         | Multi-GPU Counts |
| ---------------- | ------------------------------- | ---------------- |
| NVIDIA H100      | `gpu-h100`, `gpu-nvidia-h100-80gb`      | 1, 2, 4, 8       |
| NVIDIA A100 80GB | `gpu-a100-80gb`, `gpu-nvidia-a100-80gb` | 1, 2, 4, 8, 16   |
| NVIDIA A100      | `gpu-a100`, `gpu-nvidia-tesla-a100`     | 1, 2, 4, 8, 16   |
| NVIDIA L4        | `gpu-l4`, `gpu-nvidia-l4`               | 1, 2, 4, 8       |
| NVIDIA V100      | `gpu-v100`, `gpu-nvidia-tesla-v100`     | 1, 2, 4, 8       |
| NVIDIA T4        | `gpu-t4`, `gpu-nvidia-tesla-t4`         | 1, 2, 4          |
| NVIDIA P100      | `gpu-p100`, `gpu-nvidia-tesla-p100`     | 1, 2, 4          |
| NVIDIA P4        | `gpu-p4`, `gpu-nvidia-tesla-p4`         | 1, 2, 4          |

For multi-GPU configurations on GKE, append the count: `gpu-a100x4`, `gpu-l4x2`, etc.

## CPU

Use `accelerator="cpu"` to run on a CPU-only node (no accelerator attached).
