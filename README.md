# ExptHEP Docker Environments

Container definitions and helper scripts used for experimental high-energy physics and machine-learning workflows in the NTHU ExptHEP group.

The repository contains several independent TensorFlow, PyTorch, ATLAS/FastFrames, Docker, and Podman environments. Each environment should be built or run from its own directory unless otherwise stated.

## Available Environments

| Directory | Base image or purpose |
|---|---|
| `tf2_13/` | `tensorflow/tensorflow:2.13.0-gpu` |
| `tf2_16_1/` | `tensorflow/tensorflow:2.16.1-gpu` |
| `torch2_5_1/` | `pytorch/pytorch:2.5.1-cuda11.8-cudnn9-devel` |
| `torch2_6_0/` | `pytorch/pytorch:2.6.0-cuda12.4-cudnn9-devel` |
| `local_lxplus/` | Local ATLAS/FastFrames environment based on `gitlab-registry.cern.ch/atlas-amglab/fastframes:v6.4.0` |
| `podman_venv/` | Podman launch and environment-installation helper scripts |

The PyTorch directories contain their own detailed README files:

- [`torch2_5_1/README.md`](torch2_5_1/README.md)
- [`torch2_6_0/README.md`](torch2_6_0/README.md)

## Repository Layout

Most versioned environment directories contain some combination of:

```text
Dockerfile
build.sh
run.sh
bashrc.sh
gitconfig
test.py
*.yml
```

Their purposes are generally:

| File | Purpose |
|---|---|
| `Dockerfile` | Defines the container image |
| `*.yml` | Defines a Conda or Mamba environment |
| `build.sh` | Builds the local image |
| `run.sh` | Starts the container with the configured mounts and resources |
| `bashrc.sh` | Configures the interactive shell |
| `gitconfig` | Provides Git configuration inside the container |
| `test.py` | Checks installed packages and, where applicable, GPU access |

Not every directory contains every file. Inspect the selected directory before building it.

## Quick Start

Choose an environment and enter its directory. For example:

```bash
cd torch2_6_0
```

Inspect the scripts before executing them:

```bash
cat build.sh
cat run.sh
```

Build the image:

```bash
bash build.sh
```

Run the container:

```bash
bash run.sh
```

The exact image name, container name, mounted paths, GPU options, and CPU restrictions are defined by the scripts in that directory.

## GPU Requirements

The TensorFlow and PyTorch environments use GPU-enabled base images.

Running them with GPU access generally requires:

- a compatible NVIDIA GPU and driver;
- Docker or Podman;
- the NVIDIA container runtime configuration appropriate for the selected engine.

The Docker launch scripts use options such as:

```bash
--gpus all
```

The Podman helper uses the CDI device form:

```bash
--device nvidia.com/gpu=all
```

Host compatibility depends on the selected image and the host's NVIDIA driver configuration. Refer to the corresponding Dockerfile and environment-specific README before use.

## Podman Environment

`podman_venv/run_podman.sh` currently:

- pulls `docker.io/yygarypeng/nthu_exphep_torch`;
- creates a container named `${USER}_env`;
- mounts `/data/${USER}` at `/data`;
- allocates configured CPU and memory limits;
- requests all NVIDIA CDI GPUs.

Review and adjust the script for the host system before running it.

## Local ATLAS/FastFrames Environment

`local_lxplus/` contains a container based on:

```text
gitlab-registry.cern.ch/atlas-amglab/fastframes:v6.4.0
```

It also contains scripts related to local CVMFS setup and container startup.

Consult the files in `local_lxplus/` and the external group manual before using this environment.

## Top-Level Script Warning

The two top-level helper scripts do not currently refer to the same TensorFlow version:

- `build.sh` builds `tensorflow-2.16.1-gpu:latest`.
- `run.sh` starts `tensorflow-2.13.0-gpu:latest`.

Use the scripts inside the relevant versioned directory where available, or verify and edit the image tags before running the top-level scripts.

## Internal Manual

The detailed manual is access-restricted.

For access, contact:

Yuan-Yen Peng  
<yygarypeng@gapp.nthu.edu.tw>

NTHU users can find the external manual and latest internal updates here:

[ExptHEP Docker manual](https://docs.google.com/document/d/1HLOuoJij5l0pW-G8gDc7wAotvCSb8Kj8NeDmktPIDi0/edit?usp=sharing)

## License

This repository is distributed under the [MIT License](LICENSE).
