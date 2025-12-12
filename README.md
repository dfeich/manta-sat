# manta-sat

SAT (System Admin Toolkit) configuration files for HPE Cray on the ALPS/Merlin7 system at CSCS.

## Overview

This repository contains SAT bootprep configuration files for building and configuring compute nodes. The configurations define CFS layers, IMS images, and BOS session templates used to provision compute nodes.

## Structure

```
├── burst/                    # Configuration for 'burst' HSM group
│   ├── burst-values.yaml     # Shared values (versions, branches, commits)
│   ├── burst-gh.yaml         # Grace Hopper (ARM) compute nodes
│   ├── burst-gpu.yaml        # GPU (x86_64) compute nodes
│   └── burst-mc.yaml         # Multicore/CPU-only (x86_64) compute nodes
│
└── sichle/                   # Configuration for 'sichle' HSM group
    ├── sichle-values.yaml    # Shared values (versions, branches, commits)
    ├── sichle-gh.yaml        # Grace Hopper (ARM) compute nodes
    ├── sichle-gpu.yaml       # GPU (x86_64) compute nodes
    └── sichle-mc.yaml        # Multicore/CPU-only (x86_64) compute nodes
```

## Workflow

### Option 1: CSCS-initiated changes

1. CSCS opens PR and tags PSI for review
2. PSI reviews and merges
3. PSI pulls to castaneda and runs manta

### Option 2: PSI-initiated changes

1. PSI opens PR and tags CSCS for review
2. CSCS reviews and approves
3. PSI merges
4. PSI pulls to castaneda and runs manta

## Usage

```console
manta-cli apply sat-file -t <node-type>.yaml -f <values>.yaml
```

Example for sichle multicore nodes:

```console
manta-cli apply sat-file -t sichle-mc.yaml -f sichle-values.yaml
```

### What Gets Created

When applied, these sat files create the following resources on the system:

| Resource | Description |
|----------|-------------|
| **CFS Configurations** | Two configurations per node type: a `tmp-*` configuration for initial image customization and a runtime configuration for post-boot node personalization |
| **IMS Image** | Customized compute node image built from CSCS base image using the `tmp-*` CFS configuration |
| **BOS Session Template** | Boot orchestration template that define kernel parameters, root filesystem provider, and target node groups |
